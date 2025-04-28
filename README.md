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

  superset_db:
    image: postgres:16  # Ensure you're using the correct version of postgres
    environment:
      POSTGRES_USER: superset
      POSTGRES_PASSWORD: superset
      POSTGRES_DB: superset
    ports:
      - "5432:5432"
    volumes:
      - superset_db_data:/var/lib/postgresql/data

  superset_cache:
    image: redis:7
    ports:
      - "6379:6379"
  
  superset_worker:
    image: superset-1-superset-worker
    environment:
      - SUPERSET_SECRET_KEY=your_secret_key_here
    depends_on:
      - superset_db
      - superset_cache

  superset_websocket:
    image: superset-1-superset-websocket
    ports:
      - "8080:8080"
  
  superset_nginx:
    image: nginx:latest
    ports:
      - "80:80"
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf

volumes:
  superset_db_data:
