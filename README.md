 => CANCELED [superset-node-ci 3/8] RUN /app/docker/apt-install.sh build-essential python3 zstd                                13.1s
 => [python-base 4/8] COPY --chmod=755 docker/*.sh /app/docker/                                                                 0.1s
 => ERROR [python-base 5/8] RUN apt-get update && apt-get install -y curl build-essential pkg-config libssl-dev                12.8s
------
 > [python-base 5/8] RUN apt-get update && apt-get install -y curl build-essential pkg-config libssl-dev:
5.391 Ign:1 http://deb.debian.org/debian bookworm InRelease
5.456 Ign:2 http://deb.debian.org/debian bookworm-updates InRelease
5.521 Ign:3 http://deb.debian.org/debian-security bookworm-security InRelease
6.455 Ign:1 http://deb.debian.org/debian bookworm InRelease
6.525 Ign:2 http://deb.debian.org/debian bookworm-updates InRelease
6.589 Ign:3 http://deb.debian.org/debian-security bookworm-security InRelease
8.522 Ign:1 http://deb.debian.org/debian bookworm InRelease
8.588 Ign:2 http://deb.debian.org/debian bookworm-updates InRelease
8.657 Ign:3 http://deb.debian.org/debian-security bookworm-security InRelease
12.58 Err:1 http://deb.debian.org/debian bookworm InRelease
12.58   Connection failed [IP: 199.232.30.132 80]
12.65 Err:2 http://deb.debian.org/debian bookworm-updates InRelease
12.65   Connection failed [IP: 199.232.30.132 80]
12.72 Err:3 http://deb.debian.org/debian-security bookworm-security InRelease
12.72   Connection failed [IP: 199.232.30.132 80]
12.72 Reading package lists...
12.74 W: Failed to fetch http://deb.debian.org/debian/dists/bookworm/InRelease  Connection failed [IP: 199.232.30.132 80]
12.74 W: Failed to fetch http://deb.debian.org/debian/dists/bookworm-updates/InRelease  Connection failed [IP: 199.232.30.132 80]
12.74 W: Failed to fetch http://deb.debian.org/debian-security/dists/bookworm-security/InRelease  Connection failed [IP: 199.232.30.132 80]
12.74 W: Some index files failed to download. They have been ignored, or old ones used instead.
12.76 Reading package lists...
12.77 Building dependency tree...
12.77 Reading state information...
12.77 E: Unable to locate package curl
12.77 E: Unable to locate package build-essential
12.77 E: Unable to locate package pkg-config
12.77 E: Unable to locate package libssl-dev
------
Dockerfile:121
--------------------
 119 |
 120 |     # Install curl, required build tools
 121 | >>> RUN apt-get update && apt-get install -y curl build-essential pkg-config libssl-dev
 122 |
 123 |     # Install uv using Astral's install script
--------------------
ERROR: failed to solve: process "/bin/sh -c apt-get update && apt-get install -y curl build-essential pkg-config libssl-dev" did not complete successfully: exit code: 100
