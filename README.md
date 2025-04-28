RUN --mount=type=cache,target=/root/.npm \
    if [ -z "$BUILD_CMD" ]; then \
        echo "BUILD_CMD is not set, skipping npm run"; \
    elif [ "$DEV_MODE" = "false" ]; then \
        echo "Running 'npm run ${BUILD_CMD}'"; \
        npm run ${BUILD_CMD}; \
    else \
        echo "Skipping 'npm run ${BUILD_CMD}' in dev mode"; \
    fi;
