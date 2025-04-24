824bd006d070   insighthub-custom-superset:latest   "/app/docker/docker-…"   56 seconds ago   Up 52 seconds   8088/tcp            
       superset_init
d13665731f51   superset-superset-websocket         "docker-entrypoint.s…"   56 seconds ago   Up 52 seconds   0.0.0.0:8080->8080/tcp     superset_websocket
8e8995614b6a   nginx:latest                        "/docker-entrypoint.…"   56 seconds ago   Up 53 seconds   0.0.0.0:80->80/tcp         superset_nginx
09bffe5e3544   postgres:16                         "docker-entrypoint.s…"   56 seconds ago   Up 53 seconds   127.0.0.1:5432->5432/tcp   superset_db
a9a441ef3967   redis:7                             "docker-entrypoint.s…"   56 seconds ago   Up 53 seconds   127.0.0.1:6379->6379/tcp   superset_cache
45838a3cd83f   superset-superset-node              "docker-entrypoint.s…"   28 minutes ago   Up 53 seconds   127.0.0.1:9000->9000/tcp   superset_node
