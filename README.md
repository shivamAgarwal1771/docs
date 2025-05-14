 => ERROR [python-common 12/17] COPY superset/static/assets/images/ /app/superset/static/assets/images/                          0.0s 
------
 > [python-common 12/17] COPY superset/static/assets/images/ /app/superset/static/assets/images/:
------
Dockerfile:206
--------------------
 204 |
 205 |     # Custom static assets (logo, favicon, etc.)
 206 | >>> COPY superset/static/assets/images/ /app/superset/static/assets/images/
 207 |
 208 |     # Fix permissions
--------------------
ERROR: failed to solve: failed to compute cache key: failed to calculate checksum of ref 07f9e593-4e55-445e-b4f5-e160fb15ec36::g7qgdjbdrymgbw8g152yoxk0m: "/superset/static/assets/images": not found
