Dockerfile:82
--------------------
  81 |     # Build the frontend if not in dev mode
  82 | >>> RUN --mount=type=cache,target=/root/.npm \
  83 | >>>     if [ "$DEV_MODE" = "false" ]; then \
  84 | >>>         echo "Running 'npm run ${BUILD_CMD}'"; \
  85 | >>>         npm run ${BUILD_CMD}; \
  86 | >>>     else \
  87 | >>>         echo "Skipping 'npm run ${BUILD_CMD}' in dev mode"; \
  88 | >>>     fi;
  89 |
--------------------
ERROR: failed to solve: process "/bin/sh -c if [ \"$DEV_MODE\" = \"false\" ]; then         echo \"Running 'npm run ${BUILD_CMD}'\";         
npm run ${BUILD_CMD};     else         echo \"Skipping 'npm run ${BUILD_CMD}' in dev mode\";     fi;" did not complete successfully: exit code: 1
