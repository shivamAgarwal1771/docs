services:
  superset:
    image: apache/superset:latest
    container_name: superset
    ports:
      - "8088:8088"
    environment:
      SUPERSET_ENV: development
      SUPERSET_LOAD_EXAMPLES: "yes"
      SUPERSET_ADMIN_USERNAME: admin
      SUPERSET_ADMIN_PASSWORD: admin
      SUPERSET_ADMIN_FIRST_NAME: Admin
      SUPERSET_ADMIN_LAST_NAME: User
      SUPERSET_ADMIN_EMAIL: admin@example.com
      SUPERSET_WEBSERVER_PREFIX: /dash
    volumes:
      - ./superset:/app/superset_home
      - ./superset_config.py:/app/pythonpath/superset_config.py
      - ./init_superset.sh:/app/init_superset.sh
    command: ["/bin/sh", "-c", "/app/init_superset.sh && superset run -h 0.0.0.0 -p 8088 --with-threads --reload --debugger"]

  postgres:
    image: postgres:12
    environment:
      POSTGRES_DB: superset
      POSTGRES_USER: superset
      POSTGRES_PASSWORD: superset
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data:






# Add the following import at the top of the file
# from flask import Flask

# # Modify the configuration to add the response headers
# def add_headers(response):
#     response.headers['X-Frame-Options'] = 'ALLOWALL'
#     return response

# # Apply the header modification to the Flask app
# app = Flask(__name__)
# app.after_request(add_headers)

TALISMAN_ENABLED = False
# ENABLE_CORS = True
HTTP_HEADERS={"X-Frame-Options":"ALLOWALL"}

# import os
# from flask_appbuilder.security.manager import AUTH_DB
# from superset.security import SupersetSecurityManager


# # class CustomSecurityManager(SupersetSecurityManager):
# #     def is_item_public(self, view_name, item):
# #         return True

# #     def auth_user_oauth(self, userinfo):
# #         # Implement your token authentication logic here
# #         token = userinfo.get('token')
# #         if token == '3DeyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJleHAiOjE3MTYyMzcyOTd9':
# #             return self.find_user('admin')
# #         return None
FEATURE_FLAGS = {"EMBEDDED_SUPERSET": True}

# AUTH_TYPE = AUTH_DB
# # CUSTOM_SECURITY_MANAGER = CustomSecurityManager

# Your generated SECRET_KEY
SECRET_KEY = 'y6PCtXU-R0HXTZXuwehepBPdj3Hg_WXNN7Wu_19yJMQ'


# # Enable CORS and set the allowed origins
# ENABLE_CORS = True
# CORS_OPTIONS = {
#     'origins': ['http://localhost:3000', 'http://your-app.com']
# }

# # Allow Superset to be embedded in an iframe
# HTTP_HEADERS = {
#     'X-Frame-Options': 'ALLOWALL',
#     'Access-Control-Allow-Origin': '*',
#     'Access-Control-Allow-Methods': 'GET, POST, OPTIONS',
#     'Access-Control-Allow-Headers': 'DNT,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Range',
#     'Access-Control-Expose-Headers': 'Content-Length,Content-Range',
# }



# FEATURE_FLAGS = {
#     "ENABLE_TEMPLATE_PROCESSING": True,
# }

# # BASE_URL = 'http://localhost/dash'
# # ENABLE_PROXY_FIX = True
# # SUPERSET_WEB_SERVER_PORT = 8088

# # ENABLE_CORS = True
# # HTTP_HEADERS = {
# #     'X-Frame-Options': 'ALLOWALL',
# #     'Access-Control-Allow-Origin': '*',
# #     'Access-Control-Allow-Methods': 'GET, POST, OPTIONS',
# #     'Access-Control-Allow-Headers': 'DNT,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Range',
# #     'Access-Control-Expose-Headers': 'Content-Length,Content-Range',
# # }

# # SUPERSET_WEBSERVER_PREFIX = '/dash'

# # # Define the ReverseProxied middleware
# # class ReverseProxied(object):
# #     def __init__(self, app):
# #         self.app = app

# #     def __call__(self, environ, start_response):
# #         script_name = environ.get('HTTP_X_SCRIPT_NAME', '')
# #         if script_name:
# #             environ['SCRIPT_NAME'] = script_name
# #             path_info = environ['PATH_INFO']
# #             if path_info.startswith(script_name):
# #                 environ['PATH_INFO'] = path_info[len(script_name):]

# #         scheme = environ.get('HTTP_X_SCHEME', '')
# #         if scheme:
# #             environ['wsgi.url_scheme'] = scheme
# #         return self.app(environ, start_response)

# # ADDITIONAL_MIDDLEWARE = [ReverseProxied, ]
