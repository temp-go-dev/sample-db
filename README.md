# sample-db
サンプルアプリ用のRDB（MySQL）  

## ローカル開発用RDB起動

- 前提
  - Docker for Windowsがインストールされていること
    - Docker
    - Docker-Compose

- DB起動

```bash
$ cd localhost
$ docker-compose up -d

// omitted

$ Creating localhost_mysql_1 ... done
$ docker-compose ps
docker-compose ps
      Name                    Command                State               Ports
---------------------------------------------------------------------------------------
localhost_mysql_1   docker-entrypoint.sh mysqld   Up (healthy)   0.0.0.0:3306->3306/tcp
$
```

- マイグレーション

```bash
$ docker run -v ./script:/migrations --network localhost_default migrate/migrate -path=/migrations/ -database mysql://user:password@localhost:3306/sampledb up
```
これでマイグレーションを実行して終わりたいのだが、うまくいかない。

```bash
error: default addr for network 'localhost:3306' unknown
```

docker-composeで立ち上げているDockerに対して、
localhostでは参照できないからだと思うが、一応networkは合わせているつもり・・・

```bash
docker inspect a32b4cf0ad7a
[
    {
        "Id": "a32b4cf0ad7a25b39cf5e9f095cd774614a358ad3cbe87e18a42ad84f14532f2",
        "Created": "2019-05-28T08:39:51.3861365Z",
        "Path": "/migrate",
        "Args": [
            "-path=/migrations/",
            "-database",
            "mysql://user:password@localhost:3306/sampledb",
            "up"
        ],
        "State": {
            "Status": "exited",
            "Running": false,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 0,
            "ExitCode": 1,
            "Error": "",
            "StartedAt": "2019-05-28T08:39:52.1667646Z",
            "FinishedAt": "2019-05-28T08:39:52.2299369Z"
        },
        "Image": "sha256:d0e78ee30440b68d3e9299271c9ab43613e8458835a697a6e19d7a4b94cd644b",
        "ResolvConfPath": "/var/lib/docker/containers/a32b4cf0ad7a25b39cf5e9f095cd774614a358ad3cbe87e18a42ad84f14532f2/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/a32b4cf0ad7a25b39cf5e9f095cd774614a358ad3cbe87e18a42ad84f14532f2/hostname",
        "HostsPath": "/var/lib/docker/containers/a32b4cf0ad7a25b39cf5e9f095cd774614a358ad3cbe87e18a42ad84f14532f2/hosts",
        "LogPath": "/var/lib/docker/containers/a32b4cf0ad7a25b39cf5e9f095cd774614a358ad3cbe87e18a42ad84f14532f2/a32b4cf0ad7a25b39cf5e9f095cd774614a358ad3cbe87e18a42ad84f14532f2-json.log",
        "Name": "/friendly_varahamihira",
        "RestartCount": 0,
        "Driver": "overlay2",
        "Platform": "linux",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "",
        "ExecIDs": null,
        "HostConfig": {
            "Binds": [
                "script:/migrations"
            ],
            "ContainerIDFile": "",
            "LogConfig": {
                "Type": "json-file",
                "Config": {}
            },
            "NetworkMode": "bridge",
            "PortBindings": {},
            "RestartPolicy": {
                "Name": "no",
                "MaximumRetryCount": 0
            },
            "AutoRemove": false,
            "VolumeDriver": "",
            "VolumesFrom": null,
            "CapAdd": null,
            "CapDrop": null,
            "Dns": [],
            "DnsOptions": [],
            "DnsSearch": [],
            "ExtraHosts": null,
            "GroupAdd": null,
            "IpcMode": "shareable",
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
            "ConsoleSize": [
                12,
                139
            ],
            "Isolation": "",
            "CpuShares": 0,
            "Memory": 0,
            "NanoCpus": 0,
            "CgroupParent": "",
            "BlkioWeight": 0,
            "BlkioWeightDevice": [],
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
            "Devices": [],
            "DeviceCgroupRules": null,
            "DiskQuota": 0,
            "KernelMemory": 0,
            "MemoryReservation": 0,
            "MemorySwap": 0,
            "MemorySwappiness": null,
            "OomKillDisable": false,
            "PidsLimit": 0,
            "Ulimits": null,
            "CpuCount": 0,
            "CpuPercent": 0,
            "IOMaximumIOps": 0,
            "IOMaximumBandwidth": 0,
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
                "LowerDir": "/var/lib/docker/overlay2/d30e1fa2d5273a4f1a66ebacc48df15b634649cb7e589261d47cbd3f358cb5d4-init/diff:/var/lib/docker/overlay2/8f2f133befeb17d38a7dcadbb31d79d5b5120b7256a9373f4a75e3a5fb7a011c/diff:/var/lib/docker/overlay2/1815d4979b47fd71dbd7c510c87913a07c8691db26b035ea7b188076aef74c83/diff:/var/lib/docker/overlay2/bc4bdef742ebc41ae23678018913c95e8ee0edfd0e553339b7dbae4280882e27/diff",
                "MergedDir": "/var/lib/docker/overlay2/d30e1fa2d5273a4f1a66ebacc48df15b634649cb7e589261d47cbd3f358cb5d4/merged",
                "UpperDir": "/var/lib/docker/overlay2/d30e1fa2d5273a4f1a66ebacc48df15b634649cb7e589261d47cbd3f358cb5d4/diff",
                "WorkDir": "/var/lib/docker/overlay2/d30e1fa2d5273a4f1a66ebacc48df15b634649cb7e589261d47cbd3f358cb5d4/work"
            },
            "Name": "overlay2"
        },
        "Mounts": [
            {
                "Type": "volume",
                "Name": "script",
                "Source": "/var/lib/docker/volumes/script/_data",
                "Destination": "/migrations",
                "Driver": "local",
                "Mode": "z",
                "RW": true,
                "Propagation": ""
            }
        ],
        "Config": {
            "Hostname": "a32b4cf0ad7a",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": true,
            "AttachStderr": true,
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
            ],
            "Cmd": [
                "-path=/migrations/",
                "-database",
                "mysql://user:password@localhost:3306/sampledb",
                "up"
            ],
            "Image": "migrate/migrate",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": [
                "/migrate"
            ],
            "OnBuild": null,
            "Labels": {}
        },
        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "2f4c470d3190522d5875380e0e76bb8d17a627418842806621e4e4961c188f4c",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {},
            "SandboxKey": "/var/run/docker/netns/2f4c470d3190",
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
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "2cadf902c8bfdb9a5f60eb5395d94b06f1ca8448173a92851b835b3f781298ae",
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
```

- network

```bash
docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
2cadf902c8bf        bridge              bridge              local
9f8d560daded        host                host                local
7424a5b0645a        localhost_default   bridge              local
59b8a06ecf1e        none                null                local

```
