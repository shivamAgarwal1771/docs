Dockerfile:50
--------------------
  48 |     # Runs the webpack build process
  49 |     COPY superset-frontend /app/superset-frontend
  50 | >>> RUN npm run ${BUILD_CMD}
  51 |
  52 |     # This copies the .po files needed for translation
--------------------
ERROR: failed to solve: process "/bin/sh -c npm run ${BUILD_CMD}" did not complete successfully: exit code: 1  
