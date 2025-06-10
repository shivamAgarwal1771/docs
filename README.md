shivam220802@EC03-B17-UBAPP2:~/superset-project-final/superset/bu-digital-insightshub-backend$ sudo docker exec -it superset_nginx bash
root@f7877ab978ab:/# apt update && apt install nano -y
Ign:1 http://deb.debian.org/debian bookworm InRelease
Ign:2 http://deb.debian.org/debian bookworm-updates InRelease
Ign:3 http://deb.debian.org/debian-security bookworm-security InRelease
Ign:1 http://deb.debian.org/debian bookworm InRelease
Ign:2 http://deb.debian.org/debian bookworm-updates InRelease
Ign:3 http://deb.debian.org/debian-security bookworm-security InRelease
Ign:1 http://deb.debian.org/debian bookworm InRelease
Ign:2 http://deb.debian.org/debian bookworm-updates InRelease
Ign:3 http://deb.debian.org/debian-security bookworm-security InRelease
Err:1 http://deb.debian.org/debian bookworm InRelease
  Connection failed [IP: 199.232.30.132 80]
Err:2 http://deb.debian.org/debian bookworm-updates InRelease
  Connection failed [IP: 199.232.30.132 80]
Err:3 http://deb.debian.org/debian-security bookworm-security InRelease
  Connection failed [IP: 199.232.30.132 80]
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
All packages are up to date.
W: Failed to fetch http://deb.debian.org/debian/dists/bookworm/InRelease  Connection failed [IP: 199.232.30.132 80]
W: Failed to fetch http://deb.debian.org/debian/dists/bookworm-updates/InRelease  Connection failed [IP: 199.232.30.132 80]
W: Failed to fetch http://deb.debian.org/debian-security/dists/bookworm-security/InRelease  Connection failed [IP: 199.232.30.132 80]
W: Some index files failed to download. They have been ignored, or old ones used instead.
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
Package nano is not available, but is referred to by another package.
This may mean that the package is missing, has been obsoleted, or
is only available from another source

E: Package 'nano' has no installation candidate
root@f7877ab978ab:/# cat /etc/nginx/conf.d/superset.conf
# Licensed to the Apache Software Foundation (ASF) under one
# or more contributor license agreements.  See the NOTICE file
# distributed with this work for additional information
# regarding copyright ownership.  The ASF licenses this file
# to you under the Apache License, Version 2.0 (the
# "License"); you may not use this file except in compliance
# with the License.  You may obtain a copy of the License at
#
#   http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing,
# software distributed under the License is distributed on an
# "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
# KIND, either express or implied.  See the License for the
# specific language governing permissions and limitations
# under the License.

upstream superset_app {
    server host.docker.internal:8088;
    keepalive 100;
}

upstream superset_websocket {
    server host.docker.internal:8080;
    keepalive 100;
}

server {
    listen 80 default_server;
    server_name  _;

    location /ws {
        proxy_pass http://superset_websocket;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "Upgrade";
        proxy_set_header Host $host;
    }

    location //static {
        proxy_pass http://host.docker.internal:9000;  # Proxy to superset-node
        proxy_http_version 1.1;
        proxy_set_header Host $host;
    }

    location / {
        proxy_pass http://superset_app;
        proxy_set_header Host $http_host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_http_version 1.1;
        port_in_redirect off;
        proxy_connect_timeout 300;
    }
