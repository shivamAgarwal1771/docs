#
# Licensed to the Apache Software Foundation (ASF) under one or more
# contributor license agreements.  See the NOTICE file distributed with
# this work for additional information regarding copyright ownership.
# The ASF licenses this file to You under the Apache License, Version 2.0
# (the "License"); you may not use this file except in compliance with
# the License.  You may obtain a copy of the License at
#
#    http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#

# -----------------------------------------------------------------------
# We don't support docker-compose for production environments.
# If you choose to use this type of deployment make sure to
# create you own docker environment file (docker/.env) with your own
# unique random secure passwords and SECRET_KEY.
# -----------------------------------------------------------------------
x-superset-user: &superset-user root
x-superset-depends-on: &superset-depends-on
  - db
  - redis
x-superset-volumes: &superset-volumes
  # /app/pythonpath_docker will be appended to the PYTHONPATH in the final container
  - ./docker:/app/docker
  - ./superset:/app/superset
  - ./superset-frontend:/app/superset-frontend
  - superset_home:/app/superset_home
  - ./tests:/app/tests

x-common-build: &common-build
  context: .
  target: dev
  cache_from:
    - apache/superset-cache:3.10-slim-bookworm

services:
  nginx:
    image: nginx:latest
    container_name: superset_nginx
    restart: unless-stopped
    ports:
      - "80:80"
    extra_hosts:
      - "host.docker.internal:host-gateway"
    volumes:
      - ./docker/nginx/nginx.conf:/etc/nginx/nginx.conf:ro
    networks:
      - superset
  redis:
    image: redis:7
    container_name: superset_cache
    restart: unless-stopped
    ports:
      - "127.0.0.1:6379:6379"
    volumes:
      - redis:/data
    networks:
      - superset

  db:
    env_file:
      - path: docker/.env # default
        required: true
      - path: docker/.env-local # optional override
        required: false
    image: postgres:16
    container_name: superset_db
    restart: unless-stopped
    # ports:
    #  - "127.0.0.1:5432:5432"
    volumes:
      - db_home:/var/lib/postgresql/data
      - ./docker/docker-entrypoint-initdb.d:/docker-entrypoint-initdb.d
    networks:
      - superset

  superset:
    image: saaacr.azurecr.io/saa-superset-superset:0.0.1
    env_file:
      - path: docker/.env # default
        required: true
      - path: docker/.env-local # optional override
        required: false
    # build:
    #   <<: *common-build
    container_name: superset_app
    command: ["/app/docker/docker-bootstrap.sh", "app"]
    restart: unless-stopped
    ports:
      - 8088:8088
    extra_hosts:
      - "host.docker.internal:host-gateway"
    user: *superset-user
    depends_on: *superset-depends-on
    volumes: *superset-volumes
    environment:
      CYPRESS_CONFIG: "${CYPRESS_CONFIG:-}"
    networks:
      - superset

  superset-websocket:
    container_name: superset_websocket
    # build: ./superset-websocket
    image: saaacr.azurecr.io/saa-superset-superset-websocket:0.0.1
    ports:
      - 8080:8080
    extra_hosts:
      - "host.docker.internal:host-gateway"
    depends_on:
      - redis
    # Mount everything in superset-websocket into container and
    # then exclude node_modules and dist with bogus volume mount.
    # This is necessary because host and container need to have
    # their own, separate versions of these files. .dockerignore
    # does not seem to work when starting the service through
    # docker compose.
    #
    # For example, node_modules may contain libs with native bindings.
    # Those bindings need to be compiled for each OS and the container
    # OS is not necessarily the same as host OS.
    volumes:
      - ./superset-websocket:/home/superset-websocket
      - /home/superset-websocket/node_modules
      - /home/superset-websocket/dist

      # Mounting a config file that contains a dummy secret required to boot up.
      # do not use this docker-compose in production
      - ./docker/superset-websocket/config.json:/home/superset-websocket/config.json
    environment:
      - PORT=8080
      - REDIS_HOST=redis
      - REDIS_PORT=6379
      - REDIS_SSL=false
    networks:
      - superset

  superset-init:
    # build:
    #   <<: *common-build
    image: saaacr.azurecr.io/saa-superset-superset-init:0.0.1
    container_name: superset_init
    command: ["/app/docker/docker-init.sh"]
    env_file:
      - path: docker/.env # default
        required: true
      - path: docker/.env-local # optional override
        required: false
    depends_on: *superset-depends-on
    user: *superset-user
    volumes: *superset-volumes
    environment:
      CYPRESS_CONFIG: "${CYPRESS_CONFIG:-}"
    healthcheck:
      disable: true
    networks:
      - superset

  superset-node:
    image: node:18
    environment:
      # set this to false if you have perf issues running the npm i; npm run dev in-docker
      # if you do so, you have to run this manually on the host, which should perform better!
      BUILD_SUPERSET_FRONTEND_IN_DOCKER: true
      SCARF_ANALYTICS: "${SCARF_ANALYTICS:-}"
    container_name: superset_node
    command: ["/app/docker/docker-frontend.sh"]
    env_file:
      - path: docker/.env # default
        required: true
      - path: docker/.env-local # optional override
        required: false
    depends_on: *superset-depends-on
    volumes: *superset-volumes
    networks:
      - superset

  superset-worker:
    # build:
    #   <<: *common-build
    image: saaacr.azurecr.io/saa-superset-superset-worker:0.0.1
    container_name: superset_worker
    command: ["/app/docker/docker-bootstrap.sh", "worker"]
    env_file:
      - path: docker/.env # default
        required: true
      - path: docker/.env-local # optional override
        required: false
    environment:
      CELERYD_CONCURRENCY: 2
    restart: unless-stopped
    depends_on: *superset-depends-on
    user: *superset-user
    volumes: *superset-volumes
    extra_hosts:
      - "host.docker.internal:host-gateway"
    healthcheck:
      test: ["CMD-SHELL", "celery -A superset.tasks.celery_app:app inspect ping -d celery@$$HOSTNAME"]
    # Bump memory limit if processing selenium / thumbnails on superset-worker
    # mem_limit: 2038m
    # mem_reservation: 128M
    networks:
      - superset

  superset-worker-beat:
    # build:
    #   <<: *common-build
    image: saaacr.azurecr.io/saa-superset-superset-worker-beat:0.0.1
    container_name: superset_worker_beat
    command: ["/app/docker/docker-bootstrap.sh", "beat"]
    env_file:
      - path: docker/.env # default
        required: true
      - path: docker/.env-local # optional override
        required: false
    restart: unless-stopped
    depends_on: *superset-depends-on
    user: *superset-user
    volumes: *superset-volumes
    healthcheck:
      disable: true
    networks:
      - superset

  # superset-tests-worker:
    # build:
    #   <<: *common-build
    # container_name: superset_tests_worker
    # command: ["/app/docker/docker-bootstrap.sh", "worker"]
    # env_file:
    #   - path: docker/.env # default
    #     required: true
    #   - path: docker/.env-local # optional override
    #     required: false
    # profiles:
    #   - optional
    # environment:
    #   DATABASE_HOST: localhost
    #   DATABASE_DB: test
    #   REDIS_CELERY_DB: 2
    #   REDIS_RESULTS_DB: 3
    #   REDIS_HOST: localhost
    #   CELERYD_CONCURRENCY: 8
    # network_mode: host
    # depends_on: *superset-depends-on
    # user: *superset-user
    # volumes: *superset-volumes
    # healthcheck:
    #   test: ["CMD-SHELL", "celery inspect ping -A superset.tasks.celery_app:app -d celery@$$HOSTNAME"]
    # networks:
    #   - superset


# Druid
  postgres:
    container_name: postgres
    image: postgres:latest
    restart: always
    ports:
      - "5432:5432"
    volumes:
      - metadata_data:/var/lib/postgresql/data
    environment:
      - POSTGRES_PASSWORD=FoolishPassword
      - POSTGRES_USER=druid
      - POSTGRES_DB=druid
    networks:
       - superset

  # Need 3.5 or later for container nodes
  zookeeper:
    container_name: zookeeper
    image: zookeeper:3.5.10
    restart: always
    ports:
      - "2181:2181"
    environment:
      - ZOO_MY_ID=1
    networks:
      - superset

  coordinator:
          #image: apache/druid:29.0.1
    # build:
    #     context: ./druid
    #     dockerfile: Dockerfile
    image: saaacr.azurecr.io/saa-superset-coordinator:0.0.1
    container_name: coordinator
    restart: always
    volumes:
      - druid_shared:/opt/shared
      - coordinator_var:/opt/druid/var
    depends_on:
      - zookeeper
      - postgres
    ports:
      - "8081:8081"
    command:
      - coordinator
    env_file:
      - environment
    networks:
      - superset

  broker:
          #image: apache/druid:29.0.1
    # build:
    #     context: ./druid
    #     dockerfile: Dockerfile
    image: saaacr.azurecr.io/saa-superset-broker:0.0.1
    container_name: broker
    restart: always
    volumes:
      - broker_var:/opt/druid/var
    depends_on:
      - zookeeper
      - postgres
      - coordinator
    ports:
      - "8082:8082"
    command:
      - broker
    env_file:
      - environment
    networks:
      - superset

  historical:
          #image: apache/druid:29.0.1
    # build:
    #     context: ./druid
    #     dockerfile: Dockerfile
    image: saaacr.azurecr.io/saa-superset-historical:0.0.1
    container_name: historical
    restart: always
    volumes:
      - druid_shared:/opt/shared
      - historical_var:/opt/druid/var
    depends_on:
      - zookeeper
      - postgres
      - coordinator
    ports:
      - "8083:8083"
    command:
      - historical
    env_file:
      - environment
    networks:
      - superset

  middlemanager:
          #image: apache/druid:29.0.1
    # build:
    #    context: ./druid
    #    dockerfile: Dockerfile
    image: saaacr.azurecr.io/saa-superset-middlemanager:0.0.1
    user: root
    restart: always
    container_name: middlemanager
    volumes:
      - druid_shared:/opt/shared
      - ./logs:/opt/shared/indexing-logs
      - middle_var:/opt/druid/var
    depends_on:
      - zookeeper
      - postgres
      - coordinator
    ports:
      - "8091:8091"
      - "8100-8105:8100-8105"
    command:
      - middleManager
    env_file:
      - environment
    networks:
      - superset

  router:
    # image: apache/druid:29.0.1
    image: saaacr.azurecr.io/saa-superset-router:0.0.1
    # build:
    #     context: ./druid
    #     dockerfile: Dockerfile
    container_name: router
    user: druid
    restart: always
    volumes:
      - router_var:/opt/druid/var
        # - ./data:/opt/druid/data
    depends_on:
      - zookeeper
      - postgres
      - coordinator
    ports:
      - "8888:8888"
    command:
      - router
    env_file:
      - environment
    networks:
      - superset


volumes:
  superset_home:
    external: false
  db_home:
    external: false
  redis:
    external: false
  metadata_data: {}
  middle_var: {}
  historical_var: {}
  broker_var: {}
  coordinator_var: {}
  router_var: {}
  druid_shared: {}
  druid_shared_logs: {}


networks:
    superset:
