version: "3.8"
services:
  superset:
    build:
      context: .
      dockerfile: Dockerfile
    volumes:
      - .:/app  # This maps your current directory to /app in the container
    ports:
      - "8088:8088"
    environment:
      - SUPERSET_SECRET_KEY=your_secret_key_here
    depends_on:
      - superset_db
      - superset_cache
      - superset_worker
      - superset_websocket
      - superset_nginx
    # add other environment variables or services if needed
