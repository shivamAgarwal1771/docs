PS C:\Users\Shivam220802\OneDrive - EXLService.com (I) Pvt. Ltd\Desktop\superset\superset-1> docker inspect superset_app 
[
    {
        "Id": "386e12284617b60ca99ba0f97d8b3eeea89091d6bf4fd9a8a80132359813d2e7",
        "Created": "2025-04-28T15:32:53.84496807Z",
        "Path": "/app/docker/docker-bootstrap.sh",
        "Args": [
            "app"
        ],
        "State": {
            "Status": "created",
            "Running": false,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 0,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "0001-01-01T00:00:00Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        "Image": "sha256:ae1478f8d470f0297170f275d533b32d9d504942fe5a5a702f0b78896e32121e",
        "ResolvConfPath": "",
        "HostnamePath": "",
        "HostsPath": "",
        "LogPath": "/var/lib/docker/containers/386e12284617b60ca99ba0f97d8b3eeea89091d6bf4fd9a8a80132359813d2e7/386e12284617b60ca99ba0f97d8b3eeea89091d6bf4fd9a8a80132359813d2e7-json.log",
        "Name": "/superset_app",
        "RestartCount": 0,
        "Driver": "overlay2",
        "Platform": "linux",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "",
        "ExecIDs": null,
        "HostConfig": {
            "Binds": [
                "C:\\Users\\Shivam220802\\OneDrive - EXLService.com (I) Pvt. Ltd\\Desktop\\superset\\superset-1\\tests:/app/tests:rw",
                "C:\\Users\\Shivam220802\\OneDrive - EXLService.com (I) Pvt. Ltd\\Desktop\\superset\\superset-1\\docker:/app/docker:rw",
                "C:\\Users\\Shivam220802\\OneDrive - EXLService.com (I) Pvt. Ltd\\Desktop\\superset\\superset-1\\superset:/app/superset:rw",
                "C:\\Users\\Shivam220802\\OneDrive - EXLService.com (I) Pvt. Ltd\\Desktop\\superset\\superset-1\\superset-frontend:/app/superset-frontend:rw"
            ],
            "ContainerIDFile": "",
            "LogConfig": {
                "Type": "json-file",
                "Config": {}
            },
            "NetworkMode": "superset-1_default",
            "PortBindings": {
                "8081/tcp": [
                    {
                        "HostIp": "",
                        "HostPort": "8081"
                    }
                ],
                "8088/tcp": [
                    {
                        "HostIp": "",
                        "HostPort": "8088"
                    }
                ]
            },
            "RestartPolicy": {
                "Name": "unless-stopped",
                "MaximumRetryCount": 0
            },
            "AutoRemove": false,
            "VolumeDriver": "",
            "VolumesFrom": null,
            "ConsoleSize": [
                0,
                0
            ],
            "CapAdd": null,
            "CapDrop": null,
            "CgroupnsMode": "host",
            "Dns": null,
            "DnsOptions": null,
            "DnsSearch": null,
            "ExtraHosts": [
                "host.docker.internal:host-gateway"
            ],
            "GroupAdd": null,
            "IpcMode": "private",
            "Cgroup": "",
            "Links": null,
            "OomScoreAdj": 0,
            "PidMode": "",
            "Privileged": false,
            "PublishAllPorts": false,
            "ReadonlyRootfs": false,
            "SecurityOpt": null,
            "UTSMode": "",
            "UsernsMode": "",
            "ShmSize": 67108864,
            "Runtime": "runc",
            "Isolation": "",
            "CpuShares": 0,
            "Memory": 0,
            "NanoCpus": 0,
            "CgroupParent": "",
            "BlkioWeight": 0,
            "BlkioWeightDevice": null,
            "BlkioDeviceReadBps": null,
            "BlkioDeviceWriteBps": null,
            "BlkioDeviceReadIOps": null,
            "BlkioDeviceWriteIOps": null,
            "CpuPeriod": 0,
            "CpuQuota": 0,
            "CpuRealtimePeriod": 0,
            "CpuRealtimeRuntime": 0,
            "CpusetCpus": "",
            "CpusetMems": "",
            "Devices": null,
            "DeviceCgroupRules": null,
            "DeviceRequests": null,
            "MemoryReservation": 0,
            "MemorySwap": 0,
            "MemorySwappiness": null,
            "OomKillDisable": false,
            "PidsLimit": null,
            "Ulimits": null,
            "CpuCount": 0,
            "CpuPercent": 0,
            "IOMaximumIOps": 0,
            "IOMaximumBandwidth": 0,
            "Mounts": [
                {
                    "Type": "volume",
                    "Source": "superset-1_superset_home",
                    "Target": "/app/superset_home",
                    "VolumeOptions": {}
                }
            ],
            "MaskedPaths": [
                "/proc/asound",
                "/proc/acpi",
                "/proc/kcore",
                "/proc/keys",
                "/proc/latency_stats",
                "/proc/timer_list",
                "/proc/timer_stats",
                "/proc/sched_debug",
                "/proc/scsi",
                "/sys/firmware"
            ],
            "ReadonlyPaths": [
                "/proc/bus",
                "/proc/fs",
                "/proc/irq",
                "/proc/sys",
                "/proc/sysrq-trigger"
            ]
        },
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/d1b406985d4444d6f2e54244da81c40000210b5a64cfca9e9e4e94b69dd3dc8d-init/diff:/var/lib/docker/overlay2/hz8xz9boriw56e9nsetcqmqtf/diff:/var/lib/docker/overlay2/r5voth8qg5dtvnd4208097s9z/diff:/var/lib/docker/overlay2/pefof9243slb30v1rmxzy5ld0/diff:/var/lib/docker/overlay2/i37g64tj730f3038mwf45ncig/diff:/var/lib/docker/overlay2/fm5p7pwn05sx0vlk0x7jaizsa/diff:/var/lib/docker/overlay2/tbvqq173ri6b5dpwc1ihx2m3c/diff:/var/lib/docker/overlay2/ryopudcn5osiyuutmsggb5k83/diff:/var/lib/docker/overlay2/aowilxujlw8sxq0yhejxc89xt/diff:/var/lib/docker/overlay2/gzjd8824neajkx0melhdzycy5/diff:/var/lib/docker/overlay2/jwdu2rm38pxy2ustx9vc9n485/diff:/var/lib/docker/overlay2/xu05nycajn3bxzwlrbwjop75l/diff:/var/lib/docker/overlay2/m2w2j6x3tt5kkn11abwg0k9t2/diff:/var/lib/docker/overlay2/na7mkoq0yplyytdffqzk7k1fd/diff:/var/lib/docker/overlay2/y9d8bo9whjxybhria6w9gw8j4/diff:/var/lib/docker/overlay2/qh1e8l90zkhaxprks2fpza52h/diff:/var/lib/docker/overlay2/sdwyuq9gxj2l351ilzzr0l4sd/diff:/var/lib/docker/overlay2/4mldzwzk291wuw8rmzwfqfmnt/diff:/var/lib/docker/overlay2/y7flciao08vy9fgiocuhjzdbg/diff:/var/lib/docker/overlay2/f0bu67rmeppdcd672gkkjtdjh/diff:/var/lib/docker/overlay2/hw1ndcs5b3pyprywak9cqvsuw/diff:/var/lib/docker/overlay2/r2cawevdl6czu1c8kn7vjjrhb/diff:/var/lib/docker/overlay2/p8bsz9n4btiqgu2hy7n2sgqe7/diff:/var/lib/docker/overlay2/h1lso0nwez46bwvaj1giime83/diff:/var/lib/docker/overlay2/yk7hd4y2hehs4q0zbyjo4yncc/diff:/var/lib/docker/overlay2/nm6ve0m0wm45r7w582t7621so/diff:/var/lib/docker/overlay2/58d4785b0403dd09cf61ff281e8f0a84f07fd6af288b8513dd69e2ce61348b9b/diff:/var/lib/docker/overlay2/be32be6aba86913ec6a40a4fddc37c87d8bebe7bf6b543c9008ffc13b365d4a4/diff:/var/lib/docker/overlay2/0297df963845ace487d2f2d736259477d60b310fa5f8cd91d181a9348cd4a1a7/diff:/var/lib/docker/overlay2/e55ac0ddd43d7220b4320f7dcb99c6f8c55d501052067973ea2999f871e6954a/diff",
                "MergedDir": "/var/lib/docker/overlay2/d1b406985d4444d6f2e54244da81c40000210b5a64cfca9e9e4e94b69dd3dc8d/merged",
                "UpperDir": "/var/lib/docker/overlay2/d1b406985d4444d6f2e54244da81c40000210b5a64cfca9e9e4e94b69dd3dc8d/diff",
                "WorkDir": "/var/lib/docker/overlay2/d1b406985d4444d6f2e54244da81c40000210b5a64cfca9e9e4e94b69dd3dc8d/work"
            },
            "Name": "overlay2"
        },
        "Mounts": [
            {
                "Type": "bind",
                "Source": "C:\\Users\\Shivam220802\\OneDrive - EXLService.com (I) Pvt. Ltd\\Desktop\\superset\\superset-1\\tests",
                "Destination": "/app/tests",
                "Mode": "rw",
                "RW": true,
                "Propagation": "rprivate"
            },
            {
                "Type": "bind",
                "Source": "C:\\Users\\Shivam220802\\OneDrive - EXLService.com (I) Pvt. Ltd\\Desktop\\superset\\superset-1\\docker",
                "Destination": "/app/docker",
                "Mode": "rw",
                "RW": true,
                "Propagation": "rprivate"
            },
            {
                "Type": "bind",
                "Source": "C:\\Users\\Shivam220802\\OneDrive - EXLService.com (I) Pvt. Ltd\\Desktop\\superset\\superset-1\\superset",
                "Destination": "/app/superset",
                "Mode": "rw",
                "RW": true,
                "Propagation": "rprivate"
            },
            {
                "Type": "bind",
                "Source": "C:\\Users\\Shivam220802\\OneDrive - EXLService.com (I) Pvt. Ltd\\Desktop\\superset\\superset-1\\superset-frontend",
                "Destination": "/app/superset-frontend",
                "Mode": "rw",
                "RW": true,
                "Propagation": "rprivate"
            },
            {
                "Type": "volume",
                "Name": "superset-1_superset_home",
                "Source": "/var/lib/docker/volumes/superset-1_superset_home/_data",
                "Destination": "/app/superset_home",
                "Driver": "local",
                "Mode": "z",
                "RW": true,
                "Propagation": ""
            }
        ],
        "Config": {
            "Hostname": "386e12284617",
            "Domainname": "",
            "User": "root",
            "AttachStdin": false,
            "AttachStdout": true,
            "AttachStderr": true,
            "ExposedPorts": {
                "8081/tcp": {},
                "8088/tcp": {}
            },
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "DATABASE_PORT=5432",
                "POSTGRES_USER=superset",
                "DATABASE_DB=superset",
                "SUPERSET_APP_ROOT=/",
                "DEV_MODE=true",
                "REDIS_PORT=6379",
                "EXAMPLES_HOST=db",
                "REDIS_HOST=redis",
                "DATABASE_DIALECT=postgresql",
                "CYPRESS_CONFIG=",
                "DATABASE_PASSWORD=superset",
                "EXAMPLES_USER=examples",
                "EXAMPLES_PORT=5432",
                "SUPERSET_ENV=development",
                "ENABLE_PLAYWRIGHT=false",
                "SUPERSET_LOAD_EXAMPLES=yes",
                "PYTHONUNBUFFERED=1",
                "FLASK_DEBUG=true",
                "BUILD_SUPERSET_FRONTEND_IN_DOCKER=true",
                "PUPPETEER_SKIP_CHROMIUM_DOWNLOAD=true",
                "EXAMPLES_DB=examples",
                "SUPERSET_PORT=8088",
                "POSTGRES_PASSWORD=superset",
                "MAPBOX_API_KEY=",
                "DATABASE_USER=superset",
                "EXAMPLES_PASSWORD=examples",
                "DATABASE_HOST=db",
                "SUPERSET_LOG_LEVEL=info",
                "POSTGRES_DB=superset",
                "COMPOSE_PROJECT_NAME=superset",
                "SUPERSET_SECRET_KEY=TEST_NON_DEV_SECRET",
                "PYTHONPATH=/app/pythonpath:/app/docker/pythonpath_dev",
                "PATH=/app/.venv/bin:/usr/local/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",   
                "LANG=C.UTF-8",
                "GPG_KEY=A035C8C19219BA821ECEA86B64E628F8D684696D",
                "PYTHON_VERSION=3.11.11",
                "PYTHON_SHA256=2a9920c7a0cd236de33644ed980a13cbbc21058bfdc528febb6081575ed73be3",
                "SUPERSET_HOME=/app/superset_home",
                "HOME=/app/superset_home",
                "FLASK_APP=superset.app:create_app()"
            ],
            "Cmd": [
                "/app/docker/docker-bootstrap.sh",
                "app"
            ],
            "Healthcheck": {
                "Test": [
                    "CMD-SHELL",
                    "/app/docker/docker-healthcheck.sh"
                ]
            },
            "Image": "superset-1-superset",
            "Volumes": null,
            "WorkingDir": "/app",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": {
                "com.docker.compose.config-hash": "ed1e4bc9eb301870227efdfa8cd9a33a77fd218a06a89fe45494c7145e62551d",
                "com.docker.compose.container-number": "1",
                "com.docker.compose.depends_on": "superset-init:service_completed_successfully:false",
                "com.docker.compose.image": "sha256:ae1478f8d470f0297170f275d533b32d9d504942fe5a5a702f0b78896e32121e",
                "com.docker.compose.oneoff": "False",
                "com.docker.compose.project": "superset-1",
                "com.docker.compose.project.config_files": "C:\\Users\\Shivam220802\\OneDrive - EXLService.com (I) Pvt. Ltd\\Desktop\\superset\\superset-1\\docker-compose.yml",
                "com.docker.compose.project.working_dir": "C:\\Users\\Shivam220802\\OneDrive - EXLService.com (I) Pvt. Ltd\\Desktop\\superset\\superset-1",
                "com.docker.compose.service": "superset",
                "com.docker.compose.version": "2.18.1"
            }
        },
        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {},
            "SandboxKey": "",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "",
            "Gateway": "",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "",
            "IPPrefixLen": 0,
            "IPv6Gateway": "",
            "MacAddress": "",
            "Networks": {
                "superset-1_default": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": [
                        "superset_app",
                        "superset",
                        "386e12284617"
                    ],
                    "NetworkID": "",
                    "EndpointID": "",
                    "Gateway": "",
                    "IPAddress": "",
                    "IPPrefixLen": 0,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "",
                    "DriverOpts": null
                }
            }
        }
    }
]
