C:\Users\Shivam220802\superset>docker ps
CONTAINER ID   IMAGE                           COMMAND                  CREATED          STATUS                    PORTS                                            NAMES
a6f390651c12   superset-superset-worker-beat   "/app/docker/docker-…"   55 minutes ago   Up 43 minutes             8088/tcp                                         superset_worker_beat
c07d784eab90   superset-superset-worker        "/app/docker/docker-…"   55 minutes ago   Up 43 minutes (healthy)   8088/tcp                                         superset_worker
b293645197a7   superset-superset               "/app/docker/docker-…"   55 minutes ago   Up 43 minutes (healthy)   0.0.0.0:8081->8081/tcp, 0.0.0.0:8088->8088/tcp   superset_app
550dde9cb009   superset-superset-websocket     "docker-entrypoint.s…"   55 minutes ago   Up 55 minutes          
   0.0.0.0:8080->8080/tcp                           superset_websocket
4e78d841a612   postgres:16                     "docker-entrypoint.s…"   55 minutes ago   Up 55 minutes          
   127.0.0.1:5432->5432/tcp                         superset_db
abb1e159a086   redis:7                         "docker-entrypoint.s…"   55 minutes ago   Up 55 minutes          
   127.0.0.1:6379->6379/tcp                         superset_cache
e0d5623c6422   nginx:latest                    "/docker-entrypoint.…"   55 minutes ago   Up 55 minutes          
   0.0.0.0:80->80/tcp                               superset_nginx
710d420b068e   superset-superset-node          "docker-entrypoint.s…"   55 minutes ago   Up 55 minutes          
   127.0.0.1:9000->9000/tcp                         superset_node
