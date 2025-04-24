2025/04/24 10:41:41 [error] 28#28: *1 connect() failed (111: Connection refused) while connecting to upstream, client: 172.21.0.1, server: _, request: "GET /favicon.ico HTTP/1.1", upstream: "http://192.168.65.254:8088/favicon.ico", host: "localhost", referrer: "http://localhost/"  

only these services running

              PORTS                      NAMES
c9c2e251594d   insighthub-custom-superset:latest   "/app/docker/docker-…"   18 minutes ago   Up About a minute   8088/tcp                   superset_init
f2c10ace1d38   superset-superset-websocket         "docker-entrypoint.s…"   18 minutes ago   Up About a minute   0.0.0.0:8080->8080/tcp     superset_websocket
2db72090aedb   postgres:16                         "docker-entrypoint.s…"   18 minutes ago   Up About a minute   127.0.0.1:5432->5432/tcp   superset_db
24a8500159c4   nginx:latest                        "/docker-entrypoint.…"   18 minutes ago   Up About a minute   0.0.0.0:80->80/tcp         superset_nginx
ff1611a5fd71   redis:7                             "docker-entrypoint.s…"   18 minutes ago   Up About a minute   127.0.0.1:6379->6379/tcp   superset_cache
