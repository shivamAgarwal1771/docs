# Custom Superset config
COPY custom/superset_config.py /app/pythonpath/

# Custom static assets (logo, favicon, etc.)
COPY custom/assets/images/ /app/superset/static/assets/images/
COPY custom/assets/styles/ /app/superset/static/assets/styles/

# Fix permissions
USER root
RUN chown -R superset:superset /app/superset/static/assets
USER superset
