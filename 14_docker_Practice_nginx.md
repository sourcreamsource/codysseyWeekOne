# 🟩 14. Practice - nginx 사용해보기  

## 📋 Contents  
1. 실습1 - trouble shooting  
   1-1. nginx 실행하기  
   1-2. nginx 동작 중 자세히보기  
   1-3. [Trouble Shooting] curl은 서버에 HTTP 요청을 보내서 응답이 오는지 확인하기  
   1-4. 문제 파악 및 해결 방법  
2. 실습2 - 포트매핑 추가  
   2-1. nginx `백그라운드(-d)`로 `포트매핑(-p)`하여 재실행하기  
   2-2. 다시 nginx 동작 중 자세히보기  
   2-3. curl은 서버에 HTTP 요청을 보내서 응답이 오는지 확인하기  
   2-4. nginx를 하나 더 실행해보자!!!  
   2-5. nginx webserver2에 대한 자세히보기  
   2-6. curl로도 응답이 오는지 확인해보기  
   2-7. 배운 내용!  
   2-8. webserver, webserver2 중지 및 삭제, 강제 삭제하기  
      2-8-1. webserver 중지  
      2-8-2. webserver, webserver2 삭제  
      2-8-3. webserver2 강제 삭제  
      2-8-4. docker container 전체 확인  

<br>

## 🟢 1. 실습1 - trouble shooting  

### 🟡 1-1. nginx 실행하기  
> docker run --name webserver nginx:latest  
```bash
docker run --name webserver nginx:latest  

/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration  
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/  
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh  
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf  
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf  
/docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh  
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh  
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh  
/docker-entrypoint.sh: Configuration complete; ready for start up  
2026/04/02 02:32:35 [notice] 1#1: using the "epoll" event method  
2026/04/02 02:32:35 [notice] 1#1: nginx/1.29.7  
2026/04/02 02:32:35 [notice] 1#1: built by gcc 14.2.0 (Debian 14.2.0-19)  
2026/04/02 02:32:35 [notice] 1#1: OS: Linux 6.12.76-linuxkit  
2026/04/02 02:32:35 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576  
2026/04/02 02:32:35 [notice] 1#1: start worker processes  
2026/04/02 02:32:35 [notice] 1#1: start worker process 29  
2026/04/02 02:32:35 [notice] 1#1: start worker process 30  
2026/04/02 02:32:35 [notice] 1#1: start worker process 31  
2026/04/02 02:32:35 [notice] 1#1: start worker process 32  
2026/04/02 02:32:35 [notice] 1#1: start worker process 33  
2026/04/02 02:32:35 [notice] 1#1: start worker process 34  
2026/04/02 02:32:35 [notice] 1#1: start worker process 35  
2026/04/02 02:32:35 [notice] 1#1: start worker process 36  
```



<br>

### 🟡 1-2. nginx 동작 중 자세히보기  
> docker inspect webserver  
```bash
docker inspect webserver  
[  
    {  
        "Created": "2026-04-02T02:32:35.167703001Z",  
        "Path": "/docker-entrypoint.sh",  
        "Args": [  
            "nginx",  
            "-g",  
            "daemon off;"  
        ],  
        "State": {  
            "Status": "running",  
            "Running": true,  
            "Paused": false,  
            "Restarting": false,  
            "OOMKilled": false,  
            "Dead": false,  
            "Pid": 634,  
            "ExitCode": 0,  
            "Error": "",  
            "StartedAt": "2026-04-02T02:32:35.203382917Z",  
            "FinishedAt": "0001-01-01T00:00:00Z"  
        },  
        "Image": "sha256:7150b3a39203c",  
        "Name": "/webserver",  
        "RestartCount": 0,  
        "Driver": "overlayfs",  
        "Platform": "linux",  
        "MountLabel": "",  
        "ProcessLabel": "",  
        "ExecIDs": null,  
        "HostConfig": {  
            "Binds": null,  
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
            "ConsoleSize": [  
                41,  
                120  
            ],  
            "CapAdd": null,  
            "CapDrop": null,  
            "CgroupnsMode": "private",  
            "Dns": null,  
            "DnsOptions": [],  
            "DnsSearch": [],  
            "ExtraHosts": null,  
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
            "BlkioWeightDevice": [],  
            "BlkioDeviceReadBps": [],  
            "BlkioDeviceWriteBps": [],  
            "BlkioDeviceReadIOps": [],  
            "BlkioDeviceWriteIOps": [],  
            "CpuPeriod": 0,  
            "CpuQuota": 0,  
            "CpuRealtimePeriod": 0,  
            "CpuRealtimeRuntime": 0,  
            "CpusetCpus": "",  
            "CpusetMems": "",  
            "Devices": [],  
            "DeviceCgroupRules": null,  
            "DeviceRequests": null,  
            "MemoryReservation": 0,  
            "MemorySwap": 0,  
            "MemorySwappiness": null,  
            "OomKillDisable": null,  
            "PidsLimit": null,  
            "Ulimits": [],  
            "CpuCount": 0,  
            "CpuPercent": 0,  
            "IOMaximumIOps": 0,  
            "IOMaximumBandwidth": 0,  
            "MaskedPaths": [  
                "/proc/acpi",  
                "/proc/asound",  
                "/proc/interrupts",  
                "/proc/kcore",  
                "/proc/keys",  
                "/proc/latency_stats",  
                "/proc/sched_debug",  
                "/proc/scsi",  
                "/proc/timer_list",  
                "/proc/timer_stats",  
                "/sys/devices/virtual/powercap",  
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
        "Storage": {  
            "RootFS": {  
                "Snapshot": {  
                    "Name": "overlayfs"  
                }  
            }  
        },  
        "Mounts": [],  
        "Config": {  
            "Domainname": "",  
            "User": "",  
            "AttachStdin": false,  
            "AttachStdout": true,  
            "AttachStderr": true,  
            "ExposedPorts": {  
                "80/tcp": {}  
            },  
            "Tty": false,  
            "OpenStdin": false,  
            "StdinOnce": false,  
            "Env": [  
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",  
                "NGINX_VERSION=1.29.7",  
                "NJS_VERSION=0.9.6",  
                "NJS_RELEASE=1~trixie",  
                "ACME_VERSION=0.3.1",  
                "PKG_RELEASE=1~trixie",  
                "DYNPKG_RELEASE=1~trixie"  
            ],  
            "Cmd": [  
                "nginx",  
                "-g",  
                "daemon off;"  
            ],  
            "Image": "nginx:latest",  
            "Volumes": null,  
            "WorkingDir": "",  
            "Entrypoint": [  
                "/docker-entrypoint.sh"  
            ],  
            "Labels": {  
                "maintainer": "NGINX Docker Maintainers \u003cdocker-maint@nginx.com\u003e"  
            },  
            "StopSignal": "SIGQUIT",  
            "StopTimeout": 1  
        },  
        "NetworkSettings": {  
            "Ports": {  
                "80/tcp": null  
            },  
            "Networks": {  
                "bridge": {  
                    "IPAMConfig": null,  
                    "Links": null,  
                    "Aliases": null,  
                    "DriverOpts": null,  
                    "GwPriority": 0,  
                    "Gateway": "172.17.0.1",  
                    "🔥🔥🔥🔥🔥 IPAddress": "172.17.0.2",  
                    "IPPrefixLen": 16,  
                    "IPv6Gateway": "",  
                    "GlobalIPv6Address": "",  
                    "GlobalIPv6PrefixLen": 0,  
                    "DNSNames": null  
                }  
            }  
        },  
        "ImageManifestDescriptor": {  
            "mediaType": "application/vnd.oci.image.manifest.v1+json",  
            "size": 2222,  
            "annotations": {  
                "com.docker.official-images.bashbrew.arch": "arm64v8",  
                "org.opencontainers.image.base.name": "debian:trixie-slim",  
                "org.opencontainers.image.created": "2026-03-24T22:11:13Z",  
                "org.opencontainers.image.url": "https://hub.docker.com/_/nginx",  
                "org.opencontainers.image.version": "1.29.7"  
            },  
            "platform": {  
                "architecture": "arm",  
                "os": "linux",  
                "variant": "v8"  
            }  
        }  
    }  
]  
```

<br>

### 🟡 1-3. [Trouble Shooting] curl은 서버에 HTTP 요청을 보내서 응답이 오는지 확인하기  
> curl 172.17.0.2  
curl은 서버에 HTTP 요청을 보내서 응답이 오는지 확인하는 도구  
```bash
curl 172.17.0.2  

    curl: (28) Failed to connect to 172.17.0.2 port 80 after 75002 ms: Couldn't connect to server  
```

#### 1-4. 문제 파악 및 해결 방법  
- 나의 개발환경은 mac(macOS + Docker Desktop)이다.  
- Mac에서는 Docker가 네이티브로 직접 도는 게 아니라, 작은 Linux VM 안에서 돈다.  
- 172.17.0.2는 컨테이너 내부 브리지 네트워크 IP라서, Mac에서 바로 curl 172.17.0.2 하면 보통 안 된다.  
- 포트 publish를 해야한다.  
- `-p 8080:80 nginx` 이런 식으로 포트매핑 필요  



## 🟢 2. 실습2 - 포트매핑 추가  

<br>

### 🟡 2-1. nginx `백그라운드(-d)`로 `포트매핑(-p)`하여 재실행하기  
> docker run -d --name webserver -p 8080:80 nginx:latest  

- -d (detached mode)  
    - 백그라운드 실행  
    - 현재 사용중인 터미널을 붙잡아놓지 않고 백그라운드에서 실행이 된다.  

- -p 8080:80  
    - 내 Mac의 `8080 포트`로 들어온 요청을 컨테이너의 `80 포트`로 전달하라  

```bash
docker run -d --name webserver -p 8080:80 nginx:latest  
    # Contianer ID가 정상 출력되며 백그라운드 실행 완료  
    2565bf85778a650415bdfc15db6403ca584b9a21ebe6a9ff6ea53a04fe7ea228  

# 실행중인 것을 확인!!!  
docker ps  
    CONTAINER ID   IMAGE          COMMAND                  CREATED          STATUS          PORTS                                     NAMES  
    704889035d1a   nginx:latest   "/docker-entrypoint.…"   11 seconds ago   Up 11 seconds   0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   webserver  
```

<br>

### 🟡 2-2. 다시 nginx 동작 중 자세히보기  
> docker inspect webserver  
```bash
docker inspect webserver  
[  
    {  
        "Created": "2026-04-02T02:32:35.167703001Z",  
        "Path": "/docker-entrypoint.sh",  
        "Args": [  
            "nginx",  
            "-g",  
            "daemon off;"  
        ],  
        "State": {  
            "Status": "running",  
            "Running": true,  
            "Paused": false,  
            "Restarting": false,  
            "OOMKilled": false,  
            "Dead": false,  
            "Pid": 634,  
            "ExitCode": 0,  
            "Error": "",  
            "StartedAt": "2026-04-02T02:32:35.203382917Z",  
            "FinishedAt": "0001-01-01T00:00:00Z"  
        },  
        "Image": "sha256:7150b3a39203c",  
        "Name": "/webserver",  
        "RestartCount": 0,  
        "Driver": "overlayfs",  
        "Platform": "linux",  
        "MountLabel": "",  
        "ProcessLabel": "",  
        "ExecIDs": null,  
        "HostConfig": {  
            "Binds": null,  
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
            "ConsoleSize": [  
                41,  
                120  
            ],  
            "CapAdd": null,  
            "CapDrop": null,  
            "CgroupnsMode": "private",  
            "Dns": null,  
            "DnsOptions": [],  
            "DnsSearch": [],  
            "ExtraHosts": null,  
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
            "BlkioWeightDevice": [],  
            "BlkioDeviceReadBps": [],  
            "BlkioDeviceWriteBps": [],  
            "BlkioDeviceReadIOps": [],  
            "BlkioDeviceWriteIOps": [],  
            "CpuPeriod": 0,  
            "CpuQuota": 0,  
            "CpuRealtimePeriod": 0,  
            "CpuRealtimeRuntime": 0,  
            "CpusetCpus": "",  
            "CpusetMems": "",  
            "Devices": [],  
            "DeviceCgroupRules": null,  
            "DeviceRequests": null,  
            "MemoryReservation": 0,  
            "MemorySwap": 0,  
            "MemorySwappiness": null,  
            "OomKillDisable": null,  
            "PidsLimit": null,  
            "Ulimits": [],  
            "CpuCount": 0,  
            "CpuPercent": 0,  
            "IOMaximumIOps": 0,  
            "IOMaximumBandwidth": 0,  
            "MaskedPaths": [  
                "/proc/acpi",  
                "/proc/asound",  
                "/proc/interrupts",  
                "/proc/kcore",  
                "/proc/keys",  
                "/proc/latency_stats",  
                "/proc/sched_debug",  
                "/proc/scsi",  
                "/proc/timer_list",  
                "/proc/timer_stats",  
                "/sys/devices/virtual/powercap",  
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
        "Storage": {  
            "RootFS": {  
                "Snapshot": {  
                    "Name": "overlayfs"  
                }  
            }  
        },  
        "Mounts": [],  
        "Config": {  
            "Domainname": "",  
            "User": "",  
            "AttachStdin": false,  
            "AttachStdout": true,  
            "AttachStderr": true,  
            "ExposedPorts": {  
                "80/tcp": {}  
            },  
            "Tty": false,  
            "OpenStdin": false,  
            "StdinOnce": false,  
            "Env": [  
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",  
                "NGINX_VERSION=1.29.7",  
                "NJS_VERSION=0.9.6",  
                "NJS_RELEASE=1~trixie",  
                "ACME_VERSION=0.3.1",  
                "PKG_RELEASE=1~trixie",  
                "DYNPKG_RELEASE=1~trixie"  
            ],  
            "Cmd": [  
                "nginx",  
                "-g",  
                "daemon off;"  
            ],  
            "Image": "nginx:latest",  
            "Volumes": null,  
            "WorkingDir": "",  
            "Entrypoint": [  
                "/docker-entrypoint.sh"  
            ],  
            "Labels": {  
                "maintainer": "NGINX Docker Maintainers \u003cdocker-maint@nginx.com\u003e"  
            },  
            "StopSignal": "SIGQUIT",  
            "StopTimeout": 1  
        },  
        "NetworkSettings": {  
            # ✅ 여기서 Port Mapping을 확인할 수 있습니다.  
            🔥🔥🔥🔥🔥 "Ports": {  
                "80/tcp": [  
                    {  
                        "HostIp": "0.0.0.0",  
                        "HostPort": "8080"  
                    },  
                    {  
                        "HostIp": "::",  
                        "HostPort": "8080"  
                    }  
                ]  
            },  
            "Networks": {  
                "bridge": {  
                    "IPAMConfig": null,  
                    "Links": null,  
                    "Aliases": null,  
                    "DriverOpts": null,  
                    "GwPriority": 0,  
                    "Gateway": "172.17.0.1",  
                    "IPAddress": "172.17.0.2",  
                    "IPPrefixLen": 16,  
                    "IPv6Gateway": "",  
                    "GlobalIPv6Address": "",  
                    "GlobalIPv6PrefixLen": 0,  
                    "DNSNames": null  
                }  
            }  
        },  
        "ImageManifestDescriptor": {  
            "mediaType": "application/vnd.oci.image.manifest.v1+json",  
            "size": 2222,  
            "annotations": {  
                "com.docker.official-images.bashbrew.arch": "arm64v8",  
                "org.opencontainers.image.base.name": "debian:trixie-slim",  
                "org.opencontainers.image.created": "2026-03-24T22:11:13Z",  
                "org.opencontainers.image.url": "https://hub.docker.com/_/nginx",  
                "org.opencontainers.image.version": "1.29.7"  
            },  
            "platform": {  
                "architecture": "arm",  
                "os": "linux",  
                "variant": "v8"  
            }  
        }  
    }  
]  
```


<br>

### 🟡 2-3. curl은 서버에 HTTP 요청을 보내서 응답이 오는지 확인하기  
> curl http://localhost:8080  
curl은 서버에 HTTP 요청을 보내서 응답이 오는지 확인하는 도구  
- 리눅스가 아니므로 mac에서 접속할 때에는 curl 172.17.0.2가 아닌 curl http://localhost:8080 형태로 8080으로 접속을하여 80으로 들어간다.  

```bash
curl http://localhost:8080  

# 정상적으로 응답이 오고 있음을 확인할 수 있다!!!  
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }  
body { width: 35em; margin: 0 auto;  
font-family: Tahoma, Verdana, Arial, sans-serif; }  
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, nginx is successfully installed and working.  
Further configuration is required for the web server, reverse proxy,  
API gateway, load balancer, content cache, or other features.</p>  

<p>For online documentation and support please refer to  
<a href="https://nginx.org/">nginx.org</a>.<br/>
To engage with the community please visit  
<a href="https://community.nginx.org/">community.nginx.org</a>.<br/>
For enterprise grade support, professional services, additional  
security features and capabilities please refer to  
<a href="https://f5.com/nginx">f5.com/nginx</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```
- 설명  
    - curl이 서버에 HTTP 요청(request) 을 보냄  
    - nginx가 HTTP 응답(response) 을 돌려줌  
    - 그 응답 안에 들어 있는 HTML 내용이 바로 위의 내용이다.  



<br>

### 🟡 2-4. nginx를 하나 더 실행해보자!!!  
> docker run -d --name webserver2 -p 9090:80 nginx:latest  
- webserver2 라는 이름으로 새롭게 지어줌.  
- mac port는 9090이라는 다른 port를 지정  

```bash
docker run -d --name webserver2 -p 9090:80 nginx:latest  
    # 실행  
    f42f42b0b5555de44e2c9e84cdc549512a8ad7d0f999e5005419fad31d6ae234  

docker ps  
    # webserver2라는 NAMES로 실행된 것을 확인할 수 있다.  
    # port 또한 9090->80 으로 mapping이 된 것을 확인.  
    CONTAINER ID   IMAGE          COMMAND                  CREATED          STATUS          PORTS                                     NAMES  
    f42f42b0b555   nginx:latest   "/docker-entrypoint.…"   5 seconds ago    Up 4 seconds    0.0.0.0:9090->80/tcp, [::]:9090->80/tcp   webserver2  
    704889035d1a   nginx:latest   "/docker-entrypoint.…"   10 minutes ago   Up 10 minutes   0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   webserver  
```


<br>

### 🟡 2-5. nginx webserver2에 대한 자세히보기  
> docker inspect webserver2  
```bash
<생략>
 "IPAddress": "172.17.0.3",  
<생략>
```
- 결과 확인  
    - webserver는 "172.17.0.2"  
    - webserver2는 "172.17.0.3"  



<br>

### 🟡 2-6. curl로도 응답이 오는지 확인해보기  
> curl http://localhost:8080  
```bash
curl http://localhost:9090  

# 9090 port 또한 응답이 잘 오는 것을 확인할 수 있다.  
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }  
body { width: 35em; margin: 0 auto;  
font-family: Tahoma, Verdana, Arial, sans-serif; }  
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, nginx is successfully installed and working.  
Further configuration is required for the web server, reverse proxy,  
API gateway, load balancer, content cache, or other features.</p>  

<p>For online documentation and support please refer to  
<a href="https://nginx.org/">nginx.org</a>.<br/>
To engage with the community please visit  
<a href="https://community.nginx.org/">community.nginx.org</a>.<br/>
For enterprise grade support, professional services, additional  
security features and capabilities please refer to  
<a href="https://f5.com/nginx">f5.com/nginx</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>

```


<br>

### ✅ 2-7. 배운 내용!  
- 하나의 컴퓨터에서도 여러 개의 컨테이너를 동시에 실행할 수 있다.  
- 각 컨테이너는 서로 독립된 실행 환경이므로, 같은 nginx 이미지를 사용하더라도 각각 별도로 동작한다.  
- 이때 호스트 포트를 다르게 매핑하면 외부에서 각 컨테이너에 개별적으로 접속할 수 있다.  
- 또한 각 docker 컨테이너는 내부적으로 고유한 IP 주소를 할당받아 네트워크상에서도 서로 구분된다.  
- 즉, Docker는 하나의 컴퓨터 안에서 여러 서비스를 독립적으로 분리하여 동시에 실행할 수 있게 해주는 도구라는 개념을 완벽히 이해할 수 있었다.  


<br>

### 🟡 2-8. webserver, webserver2 중지 및 삭제, 강제 삭제하기  

#### 2-8-1. webserver 중지  
```bash
docker stop webserver  
    webserver  
```


#### 2-8-2. webserver, webserver2 삭제  
```bash
docker rm webserver  
    webserver  

docker rm webserver2  
    # 실행중으로 삭제할 수 없음.  
    Error response from daemon: cannot remove container "webserver2": container is running: stop the container before removing or force remove  
```

#### 2-8-3. webserver2 강제 삭제  
> 실행중에도 강제 삭제하기  
```bash
docker rm -f webserver2  
    webserver2  
```

#### 2-8-4. docker container 전체 확인  
```bash
docker ps -a  
    # 모두 삭제하여 존재하지 않음 확인  
    CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES  
```
