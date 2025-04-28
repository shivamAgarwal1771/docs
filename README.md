version: "3.8"

services:
  superset_db:
    image: postgres:16
    restart: always
    environment:
      POSTGRES_DB: superset
      POSTGRES_USER: superset
      POSTGRES_PASSWORD: superset
    ports:
      - "5432:5432"
    volumes:
      - superset_db_data:/var/lib/postgresql/data

  superset_cache:
    image: redis:7
    restart: always
    ports:
      - "6379:6379"

  superset_app:
    build:
      context: .
      dockerfile: Dockerfile
    container_name: superset_app
    command: >
      bash -c "
      pip install -e . &&
      pip install psycopg2-binary &&
      superset db upgrade &&
      superset fab create-admin --username admin --firstname admin --lastname admin --email admin@admin.com --password admin123 &&
      superset init &&
      gunicorn --bind 0.0.0.0:8088 --workers 4 --timeout 120 --log-level info --access-logfile - 'superset.app:create_app()'
      "
    ports:
      - "8088:8088"
    environment:
      - SUPERSET_ENV=production
      - PYTHONUNBUFFERED=1
      - UV_LINK_MODE=copy
    volumes:
      - ./superset:/app
    depends_on:
      - superset_db
      - superset_cache

volumes:
  superset_db_data:












FROM python:3.10-slim

# System dependencies
RUN apt-get update && apt-get install -y \
    build-essential \
    libssl-dev \
    libffi-dev \
    libpq-dev \
    libsasl2-dev \
    gcc \
    curl \
    && rm -rf /var/lib/apt/lists/*

# Install pip-tools to manage dependencies faster
RUN pip install --upgrade pip setuptools wheel pip-tools

WORKDIR /app

# Environment to avoid hardlink issues
ENV UV_LINK_MODE=copy

# Copy your codebase (it should have setup.py)
COPY ./superset /app

# Default port
EXPOSE 8088

# Entrypoint will be defined inside docker-compose
