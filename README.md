345.4 WARNING in ./src/components/Tooltip/index.tsx 23:0-42
345.4 export 'TooltipProps' (reexported as 'TooltipProps') was not found in 'antd-v5/lib/tooltip' (possible exports: __esModule, default)
345.4
345.4 WARNING in ./src/components/Tooltip/index.tsx 23:0-42
345.4 export 'TooltipPlacement' (reexported as 'TooltipPlacement') was not found in 'antd-v5/lib/tooltip' (possible exports: __esModule, default)
345.4
345.4 ERROR in ./src/dashboard/components/nativeFilters/FilterBar/index.tsx:66:1
345.4 TS6133: 'Vertical' is declared but its value is never read.
345.4     64 | import ActionButtons from './ActionButtons';
345.4     65 | import Horizontal from './Horizontal';
345.4   > 66 | import Vertical from './Vertical';
345.4        | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
345.4     67 | import { useSelectFiltersInScope } from '../state';
345.4     68 |
345.4     69 | // FilterBar is just being hidden as it must still
345.4
345.4 webpack 5.98.0 compiled with 1 error and 2 warnings in 315169 ms
------
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
ERROR: failed to solve: process "/bin/sh -c if [ \"$DEV_MODE\" = \"false\" ]; then         echo \"Running 'npm run ${BUILD_CMD}'\";         npm run ${BUILD_CMD};     else         echo \"Skipping 'npm run ${BUILD_CMD}' in dev mode\";     fi;" did not complete successfully: exit code: 1
