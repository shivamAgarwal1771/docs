C:\Users\Shivam220802\superset\superset-frontend>npm run build

> superset@0.0.0-dev build
> cross-env NODE_OPTIONS=--max_old_space_size=8192 NODE_ENV=production BABEL_ENV="${BABEL_ENV:=production}" webpack --color --mode production

INFO: Could not find files for the given pattern(s).
[webpack-cli] Failed to load 'C:\Users\Shivam220802\superset\superset-frontend\webpack.config.js' config
[webpack-cli] Error: Can not access zstd! Is it installed?
    at Object.<anonymous> (C:\Users\Shivam220802\superset\superset-frontend\node_modules\simple-zstd\index.js:15:9)  
    at Module._compile (node:internal/modules/cjs/loader:1529:14)
les/cjs/loader:1096:12)
    at Module.require (node:internal/modules/cjs/loader:1298:19)
    at require (node:internal/modules/helpers:182:18)
    at Object.<anonymous> (C:\Users\Shivam220802\superset\superset-frontend\webpack.proxy-config.js:20:28)

    at Module._compile (node:internal/modules/cjs/loader:1529:14)
    at Module._extensions..js (node:internal/modules/cjs/loader:1613:10)
