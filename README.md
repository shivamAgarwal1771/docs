 => ERROR [superset-init python-common  9/14] RUN /app/docker/apt-install.sh       curl       libsasl2-dev       libsasl2-mod  12.2s
------
 > [superset-init python-common  9/14] RUN /app/docker/apt-install.sh       curl       libsasl2-dev       libsasl2-modules-gssapi-mit       libpq-dev       libecpg-dev       libldap2-dev:
0.264 Updating package lists...
12.03 W: Failed to fetch http://deb.debian.org/debian/dists/bookworm/InRelease  Connection failed [IP: 151.101.154.132 80]
12.03 W: Failed to fetch http://deb.debian.org/debian/dists/bookworm-updates/InRelease  Connection failed [IP: 151.101.154.132 80]
12.03 W: Failed to fetch http://deb.debian.org/debian-security/dists/bookworm-security/InRelease  Connection failed [IP: 151.101.154.132 80]
12.03 W: Some index files failed to download. They have been ignored, or old ones used instead.
12.03 Installing packages: curl libsasl2-dev libsasl2-modules-gssapi-mit libpq-dev libecpg-dev libldap2-dev
12.06 E: Unable to locate package curl
12.06 E: Unable to locate package libsasl2-dev
12.06 E: Unable to locate package libsasl2-modules-gssapi-mit
12.06 E: Unable to locate package libpq-dev
12.06 E: Unable to locate package libecpg-dev
12.06 E: Unable to locate package libldap2-dev
------
failed to solve: process "/bin/sh -c /app/docker/apt-install.sh       curl       libsasl2-dev       libsasl2-modules-gssapi-mit       libpq-dev       libecpg-dev       libldap2-dev" did not complete successfully: exit code: 100
