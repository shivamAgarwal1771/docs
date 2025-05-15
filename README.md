 => ERROR [lean 2/4] RUN --mount=type=cache,target=/app/superset_home/.cache/uv     /app/docker/pip-install.sh --requires-build  0.8s 
------
 > [lean 2/4] RUN --mount=type=cache,target=/app/superset_home/.cache/uv     /app/docker/pip-install.sh --requires-build-essential -r requirements/base.txt:
0.594 Installing build-essential for package builds...
0.663 E: List directory /var/lib/apt/lists/partial is missing. - Acquire (13: Permission denied)
0.666 Using pip cache...
0.702 error: failed to open file `/app/superset_home/.cache/uv/sdists-v9/.git`: Permission denied (os error 13)
------
Dockerfile:229
--------------------
 228 |     COPY requirements/base.txt requirements/
 229 | >>> RUN --mount=type=cache,target=${SUPERSET_HOME}/.cache/uv \
 230 | >>>     /app/docker/pip-install.sh --requires-build-essential -r requirements/base.txt
 231 |     # Install the superset package
--------------------
ERROR: failed to solve: process "/bin/sh -c /app/docker/pip-install.sh --requires-build-essential -r requirements/base.txt" did not complete successfully: exit code: 2
