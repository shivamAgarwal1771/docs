 => [superset-websocket internal] load metadata for docker.io/library/node:16-alpine                                            0.7s
 => [superset-websocket internal] load .dockerignore                                                                            0.0s
 => => transferring context: 842B                                                                                               0.0s
 => [superset-websocket internal] load build context                                                                            0.0s
 => => transferring context: 1.77kB                                                                                             0.0s
 => [superset-websocket build 1/4] FROM docker.io/library/node:16-alpine@sha256:a1f9d027912b58a7c75be7716c97cfbc6d3099f3a97ed8  0.0s
 => CACHED [superset-websocket build 2/4] WORKDIR /home/superset-websocket                                                      0.0s
 => CACHED [superset-websocket build 3/4] COPY . ./                                                                             0.0s
 => [superset-websocket build 4/4] RUN npm ci &&   npm run build                                                               21.6s
 => => # npm WARN EBADENGINE }
 => => # npm WARN EBADENGINE Unsupported engine {
 => => # npm WARN EBADENGINE   package: '@typescript-eslint/type-utils@8.19.0',
 => => # npm WARN EBADENGINE   required: { node: '^18.18.0 || ^20.9.0 || >=21.1.0' },
 => => # npm WARN EBADENGINE   current: { node: 'v16.20.2', npm: '8.19.4' }
 => => # npm WARN EBADENGINE }
