services:
  redis:
    image: redis:7
    container_name: superset_cache
    restart: unless-stopped
    volumes:
      - redis:/data
    networks:
      - superset_network

  db:
    env_file:
      - "docker/.env"
    image: postgres:16
    container_name: superset_db
    restart: unless-stopped
    volumes:
      - db_home:/var/lib/postgresql/data
      - ./docker/docker-entrypoint-initdb.d:/docker-entrypoint-initdb.d
    networks:
      - superset_network

  # Main Superset service (app)
  superset:
    image: apachesuperset.docker.scarf.sh/apache/superset:latest-dev
    container_name: superset_app
    command: ["/app/docker/docker-bootstrap.sh", "app-gunicorn"]
    user: "root"
    restart: unless-stopped
    ports:
      - 8088:8088
    depends_on:
      superset-init:
        condition: service_completed_successfully
    volumes:
      - ./docker:/app/docker
      - superset_home:/app/superset_home
    environment:
      SUPERSET_LOG_LEVEL: "${SUPERSET_LOG_LEVEL:-info}"
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:8088"]
      interval: 30s
      retries: 3
      start_period: 10s
      timeout: 10s
    networks:
      - superset_network

  # Superset Initialization (run initial setup)
  superset-init:
    image: apachesuperset.docker.scarf.sh/apache/superset:latest-dev
    container_name: superset_init
    command: ["/app/docker/docker-init.sh"]
    env_file:
      - "docker/.env"
    depends_on:
      db:
        condition: service_started
      redis:
        condition: service_started
    user: "root"
    volumes:
      - ./docker:/app/docker
      - superset_home:/app/superset_home
    healthcheck:
      disable: true
    environment:
      SUPERSET_LOAD_EXAMPLES: "${SUPERSET_LOAD_EXAMPLES:-yes}"
      SUPERSET_LOG_LEVEL: "${SUPERSET_LOG_LEVEL:-info}"
    networks:
      - superset_network

  # Superset Worker (Celery Worker)
  superset-worker:
    image: apachesuperset.docker.scarf.sh/apache/superset:latest-dev
    container_name: superset_worker
    command: ["/app/docker/docker-bootstrap.sh", "worker"]
    env_file:
      - "docker/.env"
    restart: unless-stopped
    depends_on:
      superset-init:
        condition: service_completed_successfully
    user: "root"
    volumes:
      - ./docker:/app/docker
      - superset_home:/app/superset_home
    healthcheck:
      test:
        [
          "CMD-SHELL",
          "celery -A superset.tasks.celery_app:app inspect ping -d celery@$$HOSTNAME",
        ]
    environment:
      SUPERSET_LOG_LEVEL: "${SUPERSET_LOG_LEVEL:-info}"
    networks:
      - superset_network

  # Superset Beat (Celery Beat)
  superset-worker-beat:
    image: apachesuperset.docker.scarf.sh/apache/superset:latest-dev
    container_name: superset_worker_beat
    command: ["/app/docker/docker-bootstrap.sh", "beat"]
    env_file:
      - "docker/.env"
    restart: unless-stopped
    depends_on:
      superset-init:
        condition: service_completed_successfully
    user: "root"
    volumes:
      - ./docker:/app/docker
      - superset_home:/app/superset_home
    healthcheck:
      disable: true
    environment:
      SUPERSET_LOG_LEVEL: "${SUPERSET_LOG_LEVEL:-info}"
    networks:
      - superset_network

volumes:
  superset_home:
    external: false
  db_home:
    external: false
  redis:
    external: false

networks:
  superset_network:
    driver: bridge
