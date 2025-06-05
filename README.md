 > [superset-node-ci 7/8] RUN --mount=type=bind,source=./superset-frontend/package.json,target=./package.json     --mount=type=bind,source=./superset-frontend/package-lock.json,target=./package-lock.json     --mount=type=cache,target=/root/.cache     --mount=type=cache,target=/root/.npm     if [ "false" = "false" ]; then         npm ci;     else         echo "Skipping 'npm ci' in dev mode";     fi:
20.79 npm warn deprecated viewport-mercator-project@7.0.4: Package no longer supported. Contact Support at https://www.npmjs.com/support for more info.
26.28 npm warn deprecated querystring@0.2.1: The querystring API is considered Legacy. new code should use the URLSearchParams API instead.
27.69 npm warn deprecated topojson@1.6.27: Use topojson-client, topojson-server or topojson-simplify directly.
29.52 npm warn deprecated nomnom@1.8.1: Package no longer supported. Contact support@npmjs.com for more info.
33.49 npm warn deprecated inflight@1.0.6: This module is not supported, and leaks memory. Do not use it. Check out lru-cache if you want a good and tested way to coalesce async requests by a key value, which is much more comprehensive and powerful.
34.42 npm warn deprecated glob@7.2.3: Glob versions prior to v9 are no longer supported
40.87 npm warn deprecated abab@2.0.6: Use your platform's native atob() and btoa() methods instead
48.04 npm warn deprecated @humanwhocodes/config-array@0.13.0: Use @eslint/config-array instead
48.07 npm warn deprecated @humanwhocodes/object-schema@2.0.3: Use @eslint/object-schema instead
50.68 npm warn deprecated @babel/plugin-proposal-nullish-coalescing-operator@7.18.6: This proposal has been merged to the ECMAScript standard and thus this plugin is no longer maintained. Please use @babel/plugin-transform-nullish-coalescing-operator instead.
50.79 npm warn deprecated @babel/plugin-proposal-class-properties@7.18.6: This proposal has been merged to the ECMAScript standard and thus this plugin is no longer maintained. Please use @babel/plugin-transform-class-properties instead.
50.79 npm warn deprecated @babel/plugin-proposal-private-methods@7.18.6: This proposal has been merged to the ECMAScript standard and thus this plugin is no longer maintained. Please use @babel/plugin-transform-private-methods instead.
50.80 npm warn deprecated @babel/plugin-proposal-optional-chaining@7.21.0: This proposal has been merged to the ECMAScript standard and thus this plugin is no longer maintained. Please use @babel/plugin-transform-optional-chaining instead.
52.33 npm warn deprecated @babel/polyfill@7.12.1: 🚨 This package has been deprecated in favor of separate inclusion of a polyfill and regenerator-runtime (when needed). See the @babel/polyfill docs (https://babeljs.io/docs/en/babel-polyfill) for more information.
52.93 npm warn deprecated rimraf@3.0.2: Rimraf versions prior to v4 are no longer supported
53.61 npm warn deprecated rimraf@2.6.3: Rimraf versions prior to v4 are no longer supported
54.25 npm warn deprecated rimraf@3.0.2: Rimraf versions prior to v4 are no longer supported
57.31 npm warn deprecated rimraf@3.0.2: Rimraf versions prior to v4 are no longer supported
59.45 npm warn deprecated domexception@4.0.0: Use your platform's native DOMException instead
59.52 npm warn deprecated rimraf@3.0.2: Rimraf versions prior to v4 are no longer supported
61.00 npm warn deprecated rimraf@3.0.2: Rimraf versions prior to v4 are no longer supported
61.92 npm warn deprecated lodash.isequal@3.0.4: This package is deprecated. Use require('node:util').isDeepStrictEqual instead.
63.95 npm warn deprecated glob@8.1.0: Glob versions prior to v9 are no longer supported
75.24 npm warn deprecated eslint@8.57.1: This version is no longer supported. Please see https://eslint.org/version-support for other options.
215.7 npm error code 1
215.7 npm error path /app/superset-frontend/node_modules/puppeteer
215.7 npm error command failed
215.7 npm error command sh -c node install.mjs
215.7 npm error Error: ERROR: Failed to set up chrome-headless-shell v127.0.6533.88! Set "PUPPETEER_SKIP_DOWNLOAD" env variable to skip download.
215.7 npm error     at file:///app/superset-frontend/node_modules/puppeteer/lib/esm/puppeteer/node/install.js:84:27
215.7 npm error     at process.processTicksAndRejections (node:internal/process/task_queues:95:5)
215.7 npm error     at async Promise.all (index 1)
215.7 npm error     at async downloadBrowser (file:///app/superset-frontend/node_modules/puppeteer/lib/esm/puppeteer/node/install.js:90:9) {
215.7 npm error   [cause]: Error: self-signed certificate in certificate chain
215.7 npm error       at TLSSocket.onConnectSecure (node:_tls_wrap:1677:34)
215.7 npm error       at TLSSocket.emit (node:events:524:28)
215.7 npm error       at TLSSocket._finishInit (node:_tls_wrap:1076:8)
215.7 npm error       at ssl.onhandshakedone (node:_tls_wrap:862:12) {
215.7 npm error     code: 'SELF_SIGNED_CERT_IN_CHAIN'
215.7 npm error   }
215.7 npm error }
215.7 npm notice
215.7 npm notice New major version of npm available! 10.8.2 -> 11.4.1
215.7 npm notice Changelog: https://github.com/npm/cli/releases/tag/v11.4.1
215.7 npm notice To update run: npm install -g npm@11.4.1
215.7 npm notice
215.7 npm error A complete log of this run can be found in: /root/.npm/_logs/2025-06-05T12_04_48_850Z-debug-0.log
------
Dockerfile:66
--------------------
  65 |     # Note that's it's not possible selectively COPY of mount using blobs.
  66 | >>> RUN --mount=type=bind,source=./superset-frontend/package.json,target=./package.json \
  67 | >>>     --mount=type=bind,source=./superset-frontend/package-lock.json,target=./package-lock.json \
  68 | >>>     --mount=type=cache,target=/root/.cache \
  69 | >>>     --mount=type=cache,target=/root/.npm \
  70 | >>>     if [ "$DEV_MODE" = "false" ]; then \
  71 | >>>         npm ci; \
  72 | >>>     else \
  73 | >>>         echo "Skipping 'npm ci' in dev mode"; \
  74 | >>>     fi
  75 |
--------------------
ERROR: failed to solve: process "/bin/sh -c if [ \"$DEV_MODE\" = \"false\" ]; then         npm ci;     else         echo \"Skipping 'npm ci' in dev mode\";     fi" did not complete successfully: exit code: 1
