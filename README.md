x-superset-image: &superset-image apachesuperset.docker.scarf.sh/apache/superset:${TAG:-latest}
x-superset-depends-on: &superset-depends-on
  - db
  - redis
x-superset-volumes:
  &superset-volumes # /app/pythonpath_docker will be appended to the PYTHONPATH in the final container
  - ./docker:/app/docker
  - superset_home:/app/superset_home

version: "3.7"
services:
  redis:
    image: redis:7
    container_name: superset_cache
    restart: unless-stopped
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
    image: postgres:latest
    container_name: superset_db
    restart: unless-stopped
    volumes:
      - db_home:/var/lib/postgresql/data
      - ./docker/docker-entrypoint-initdb.d:/docker-entrypoint-initdb.d
    networks:
      - superset

  superset:
    env_file:
      - path: docker/.env # default
        required: true
    image: apachesuperset.docker.scarf.sh/apache/superset
    container_name: superset_app
    command: ["/app/docker/docker-bootstrap.sh", "app-gunicorn"]
    user: "root"
    restart: unless-stopped
    ports:
      - 8088:8088
    depends_on: *superset-depends-on
    volumes: *superset-volumes
    networks:
      - superset

  superset-init:
    image: apachesuperset.docker.scarf.sh/apache/superset
    container_name: superset_init
    command: ["/app/docker/docker-init.sh"]
    env_file:
      - path: docker/.env # default
        required: true
      - path: docker/.env-local # optional override
        required: false
    depends_on: *superset-depends-on
    user: "root"
    volumes: *superset-volumes
    healthcheck:
      disable: true
    networks:
      - superset

  superset-worker:
    image: apachesuperset.docker.scarf.sh/apache/superset
    container_name: superset_worker
    command: ["/app/docker/docker-bootstrap.sh", "worker"]
    env_file:
      - path: docker/.env # default
        required: true
      - path: docker/.env-local # optional override
        required: false
    restart: unless-stopped
    depends_on: *superset-depends-on
    user: "root"
    volumes: *superset-volumes
    networks:
      - superset
    healthcheck:
      test:
        [
          "CMD-SHELL",
          "celery -A superset.tasks.celery_app:app inspect ping -d celery@$$HOSTNAME",
        ]

  superset-worker-beat:
    image: apachesuperset.docker.scarf.sh/apache/superset
    container_name: superset_worker_beat
    command: ["/app/docker/docker-bootstrap.sh", "beat"]
    env_file:
      - path: docker/.env # default
        required: true
      - path: docker/.env-local # optional override
        required: false
    restart: unless-stopped
    networks:
      - superset
    depends_on: *superset-depends-on
    user: "root"
    volumes: *superset-volumes
    healthcheck:
      disable: true
      
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
    build:
        context: ./druid
        dockerfile: Dockerfile
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
    build:
        context: ./druid
        dockerfile: Dockerfile
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
    build:
        context: ./druid
        dockerfile: Dockerfile
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
    build:
       context: ./druid
       dockerfile: Dockerfile
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
          #image: apache/druid:29.0.1
    build:
        context: ./druid
        dockerfile: Dockerfile
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
