PS C:\Users\Shivam220802\OneDrive - EXLService.com (I) Pvt. Ltd\Desktop\superset\superset-1> docker ps -a            
CONTAINER ID   IMAGE                           COMMAND                  CREATED          STATUS                      PORTS                    NAMES
c5a42bd8ea7b   superset-1-superset             "python3"                55 seconds ago   Created                                              superset-1-superset-1
801eba13c0de   superset-1-superset-worker      "/app/docker/entrypo…"   55 seconds ago   Up 53 seconds (healthy)     8088/tcp                 superset-1-superset_worker-1
2a655d791517   superset-1-superset-websocket   "docker-entrypoint.s…"   56 seconds ago   Exited (1) 51 seconds ago                            superset-1-superset_websocket-1
c1703a2b0b7d   postgres:16                     "docker-entrypoint.s…"   56 seconds ago   Up 53 seconds               0.0.0.0:5432->5432/tcp   superset-1-superset_db-1
34152d7ac369   redis:7                         "docker-entrypoint.s…"   56 seconds ago   Up 53 seconds               0.0.0.0:6379->6379/tcp   superset-1-superset_cache-1
ed0dbd3270de   nginx:latest                    "/docker-entrypoint.…"   56 seconds ago   Created                     0.0.0.0:80->80/tcp       superset-1-superset_nginx-1
a5e9a353e068   superset-1-superset_app         "bash -c ' pip insta…"   11 minutes ago   Exited (1) 11 minutes ago 
