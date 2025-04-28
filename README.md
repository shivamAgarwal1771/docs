PS C:\Users\Shivam220802\OneDrive - EXLService.com (I) Pvt. Ltd\Desktop\superset\superset-1> docker ps -a
CONTAINER ID   IMAGE                             COMMAND                  CREATED         STATUS                     PORTS                      NAMES
3056e88dfe44   superset-1-superset-worker-beat   "/app/docker/docker-…"   4 minutes ago   Up 4 minutes               8088/tcp                   superset_worker_beat
386e12284617   superset-1-superset               "/app/docker/docker-…"   4 minutes ago   Created                                               superset_app
47893f5e2f2e   superset-1-superset-worker        "/app/docker/docker-…"   4 minutes ago   Up 4 minutes (unhealthy)   8088/tcp                   superset_worker
b5a641e0a2ce   superset-1-superset-init          "/app/docker/docker-…"   4 minutes ago   Up 4 minutes               
8088/tcp                   superset_init
1a157bebfe7b   superset-1-superset-websocket     "docker-entrypoint.s…"   4 minutes ago   Up 4 minutes               
0.0.0.0:8080->8080/tcp     superset_websocket
4d7d3a6706e6   redis:7                           "docker-entrypoint.s…"   4 minutes ago   Up 4 minutes               
127.0.0.1:6379->6379/tcp   superset_cache
6784d4ce8339   nginx:latest                      "/docker-entrypoint.…"   4 minutes ago   Up 4 minutes               
0.0.0.0:80->80/tcp         superset_nginx
29f2a0e0e79e   superset-1-superset-node          "docker-entrypoint.s…"   4 minutes ago   Up 4 minutes               
127.0.0.1:9000->9000/tcp   superset_node
c32837bac702   postgres:16                       "docker-entrypoint.s…"   4 minutes ago   Up 4 minutes               
127.0.0.1:5432->5432/tcp   superset_db
