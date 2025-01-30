CONTAINER ID   IMAGE                                           COMMAND                  CREATED      STATUS                 PORTS                                                      NAMES
aa722d89c92d   saa-superset-1-historical                       "/druid.sh historical"   2 days ago   Up 6 hours             0.0.0.0:8083->8083/tcp                                     historical
a8f35ee45eb8   saa-superset-1-middlemanager                    "/druid.sh middleMan…"   2 days ago   Up 6 hours             0.0.0.0:8091->8091/tcp, 0.0.0.0:8100-8105->8100-8105/tcp   middlemanager
240c2ac7aebf   saa-superset-1-router                           "/druid.sh router"       2 days ago   Up 6 hours             0.0.0.0:8888->8888/tcp                                     router
847aa57d6772   saa-superset-1-broker                           "/druid.sh broker"       2 days ago   Up 6 hours             0.0.0.0:8082->8082/tcp                                     broker
74dabf7bef78   saa-superset-1-coordinator                      "/druid.sh coordinat…"   2 days ago   Up 6 hours             0.0.0.0:8081->8081/tcp                                     coordinator
415bf26b1d91   saaacr.azurecr.io/saa-superset-superset:0.0.4   "/app/docker/docker-…"   2 days ago   Up 6 hours (healthy)   0.0.0.0:8088->8088/tcp                                     superset_app
f3332cc775e2   saaacr.azurecr.io/saa-superset-superset:0.0.4   "/app/docker/docker-…"   2 days ago   Up 6 hours (healthy)   8088/tcp                                                   superset_worker
c3440b567ec7   saaacr.azurecr.io/saa-superset-superset:0.0.4   "/app/docker/docker-…"   2 days ago   Up 6 hours             8088/tcp                                                   superset_worker_beat
c4e239d00c26   zookeeper:3.5.10                                "/docker-entrypoint.…"   2 days ago   Up 6 hours             2888/tcp, 3888/tcp, 0.0.0.0:2181->2181/tcp, 8080/tcp       zookeeper
67f33c916975   postgres:15                                     "docker-entrypoint.s…"   2 days ago   Up 6 hours             5432/tcp                                                   superset_db
fb68ac29b64c   postgres:latest                                 "docker-entrypoint.s…"   2 days ago   Up 6 hours             0.0.0.0:5432->5432/tcp                                     postgres
015d114587cf   redis:7                                         "docker-entrypoint.s…"   2 days ago   Up 6 hours             2888/tcp, 3888/tcp, 0.0.0.0:2181->2181/tcp, 8080/tcp       zookeeper
67f33c916975   postgres:15                                     "docker-entrypoint.s…"   2 days ago   Up 6 hours             5432/tcp                                                   superset_db
fb68ac29b64c   postgres:latest                                 "docker-entrypoint.s…"   2 days ago   Up 6 hours             0.0.0.0:5432->5432/tcp                                     postgres
015d114587cf   redis:7                                         "docker-entrypoint.s…"   2 days ago   Up 6 hours             6379/tcp                                                   superset_cache
PS C:\Users\Shivam220802\OneDrive - EXLService.com (I) Pvt. Ltd\Desktop\superset\saa-superset-1\superset-frontend67f33c916975   postgres:15                                     "docker-entrypoint.s…"   2 days ago   Up 6 hours             5432/tcp                                                   superset_db
fb68ac29b64c   postgres:latest                                 "docker-entrypoint.s…"   2 days ago   Up 6 hours             0.0.0.0:5432->5432/tcp                                     postgres
015d114587cf   redis:7                                         "docker-entrypoint.s…"   2 days ago   Up 6 hours  fb68ac29b64c   postgres:latest                                 "docker-entrypoint.s…"   2 days ago   Up 6 hours             0.0.0.0:5432->5432/tcp                                     postgres
015d114587cf   redis:7                                         "docker-entrypoint.s…"   2 days ago   Up 6 hours  015d114587cf   redis:7                                         "docker-entrypoint.s…"   2 days ago   Up 6 hours             6379/tcp                                                   superset_cache
