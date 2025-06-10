shivam220802@EC03-B17-UBAPP2:~/superset-project-final/superset/bu-digital-insightshub-backend$ sudo docker logs superset_init
Reinstalling the app in editable mode
Resolved 147 packages in 2.92s
   Building apache-superset @ file:///app
      Built apache-superset @ file:///app
Prepared 1 package in 2.09s
Uninstalled 1 package in 559ms
warning: Failed to hardlink files; falling back to full copy. This may lead to degraded performance.
         If the cache and target directories are on different filesystems, hardlinking may not be supported.
         If this is intentional, set `export UV_LINK_MODE=copy` or use `--link-mode=copy` to suppress this warning.
Installed 1 package in 6ms
 ~ apache-superset==0.0.0.dev0 (from file:///app)
Installing postgres requirements
Resolved 148 packages in 1.88s
   Building apache-superset @ file:///app
      Built apache-superset @ file:///app
Prepared 1 package in 2.61s
Uninstalled 1 package in 1ms
warning: Failed to hardlink files; falling back to full copy. This may lead to degraded performance.
         If the cache and target directories are on different filesystems, hardlinking may not be supported.
         If this is intentional, set `export UV_LINK_MODE=copy` or use `--link-mode=copy` to suppress this warning.
Installed 1 package in 3ms
 ~ apache-superset==0.0.0.dev0 (from file:///app)
Skipping local overrides
Unknown Operation!!!
######################################################################
Init Step 1/3 [Starting] -- Applying DB migrations
######################################################################
Loaded your LOCAL configuration at [/app/docker/pythonpath_dev/superset_config.py]
2025-06-10 11:01:30,411:INFO:superset.initialization:Setting database isolation level to READ COMMITTED
INFO  [alembic.env] Starting the migration scripts.
INFO  [alembic.runtime.migration] Context impl PostgresqlImpl.
INFO  [alembic.runtime.migration] Will assume transactional DDL.
INFO  [alembic.env] Migration scripts completed. Duration: 00:00:01
######################################################################
Init Step 1/3 [Complete] -- Applying DB migrations
######################################################################
######################################################################
Init Step 2/3 [Starting] -- Setting up admin user ( admin / admin )
######################################################################
Loaded your LOCAL configuration at [/app/docker/pythonpath_dev/superset_config.py]
2025-06-10 11:01:38,429:INFO:superset.initialization:Setting database isolation level to READ COMMITTED
Recognized Database Authentications.
Error! User already exists admin
######################################################################
Init Step 2/3 [Complete] -- Setting up admin user
######################################################################
######################################################################
Init Step 3/3 [Starting] -- Setting up roles and perms
######################################################################
Loaded your LOCAL configuration at [/app/docker/pythonpath_dev/superset_config.py]
2025-06-10 11:01:44,123:INFO:superset.initialization:Setting database isolation level to READ COMMITTED
2025-06-10 11:01:47,807:INFO:superset.security.manager:Syncing role definition
2025-06-10 11:01:47,848:INFO:superset.security.manager:Syncing Admin perms
2025-06-10 11:01:47,857:INFO:superset.security.manager:Syncing Alpha perms
2025-06-10 11:01:47,864:INFO:superset.security.manager:Syncing Gamma perms
2025-06-10 11:01:47,869:INFO:superset.security.manager:Syncing sql_lab perms
2025-06-10 11:01:47,873:INFO:superset.security.manager:Fetching a set of all perms to lookup which ones are missing
2025-06-10 11:01:47,878:INFO:superset.security.manager:Creating missing datasource permissions.
2025-06-10 11:01:47,900:INFO:superset.security.manager:Creating missing database permissions.
2025-06-10 11:01:47,911:INFO:superset.security.manager:Cleaning faulty perms
######################################################################
Init Step 3/3 [Complete] -- Setting up roles and perms
######################################################################
shivam220802@EC03-B17-UBAPP2:~/superset-project-final/superset/bu-digital-insightshub-backend$ sudo docker ps
CONTAINER ID   IMAGE                                                 COMMAND                  CREATED         STATUS                   PORTS                                                                                      NAMES
6d932233f552   bu-digital-insightshub-backend-superset-worker-beat   "/app/docker/docker-…"   4 minutes ago   Up 4 minutes             8088/tcp                                                                                   superset_worker_beat
436bb29db712   bu-digital-insightshub-backend-superset-worker        "/app/docker/docker-…"   4 minutes ago   Up 4 minutes (healthy)   8088/tcp                                                                                   superset_worker
30a949006d32   bu-digital-insightshub-backend-superset               "/app/docker/docker-…"   4 minutes ago   Up 4 minutes (healthy)   0.0.0.0:8081->8081/tcp, [::]:8081->8081/tcp, 0.0.0.0:8088->8088/tcp, [::]:8088->8088/tcp   superset_app
7af9b15f4b47   bu-digital-insightshub-backend-superset-websocket     "docker-entrypoint.s…"   4 minutes ago   Up 4 minutes             0.0.0.0:8080->8080/tcp, [::]:8080->8080/tcp                                                superset_websocket
c1247f0821aa   redis:7                                               "docker-entrypoint.s…"   4 minutes ago   Up 4 minutes             127.0.0.1:6379->6379/tcp                                                                   superset_cache
f7877ab978ab   nginx:latest                                          "/docker-entrypoint.…"   4 minutes ago   Up 4 minutes             0.0.0.0:80->80/tcp, [::]:80->80/tcp                                                        superset_nginx
664e8a228aa3   postgres:16                                           "docker-entrypoint.s…"   4 minutes ago   Up 4 minutes  
