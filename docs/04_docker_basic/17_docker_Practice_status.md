# 🟩 17. doker container 파악하기 - top, logs, diff  

## 📋 Contents  
1. 컨테이너 프로세스 목록 보기  
   1-1. `docker top`  
2. 컨테이너 로그 보기   
   2-1. `docker logs`  
3. 컨테이너 내부 파일 변경 이력 보기  
   3-1. `docker diff`  


<br><br>

## 🟢 1. 컨테이너 프로세스 목록 보기  
### 🟡 1-1. `docker top`  
* 뜻  
  * **컨테이너 안에서 현재 돌아가는 프로세스 목록 확인**  

* 기본 명령어  
  ```bash
  docker top 컨테이너_이름  
  ```

* 실행 기록  
  ```bash
  docker top web1  

    UID                 PID                 PPID                C                   STIME               TTY                 TIME                CMD  
    root                522                 499                 0                   06:29               ?                   00:00:00            nginx: master process nginx -g daemon off;  
    statd               567                 522                 0                   06:29               ?                   00:00:00            nginx: worker process  
    statd               568                 522                 0                   06:29               ?                   00:00:00            nginx: worker process  
    statd               569                 522                 0                   06:29               ?                   00:00:00            nginx: worker process  
    statd               570                 522                 0                   06:29               ?                   00:00:00            nginx: worker process  
    statd               571                 522                 0                   06:29               ?                   00:00:00            nginx: worker process  
    statd               572                 522                 0                   06:29               ?                   00:00:00            nginx: worker process  
    statd               573                 522                 0                   06:29               ?                   00:00:00            nginx: worker process  
    statd               574                 522                 0                   06:29               ?                   00:00:00            nginx: worker process  

  ```


<br><br>

## 🟢 2. 컨테이너 로그 보기  

### 🟡 2-1. `docker logs`  

* 뜻  
  * **컨테이너가 출력한 로그 보기**  

  
* 기본 명령어  
    ```bash
    docker logs 컨테이너_이름  

    docker logs web1  

    /docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration  
    /docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/  
    /docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh  
    10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf  
    10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf  
    /docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh  
    /docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh  
    /docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh  
    /docker-entrypoint.sh: Configuration complete; ready for start up  
    2026/04/02 06:23:13 [notice] 1#1: using the "epoll" event method  
    2026/04/02 06:23:13 [notice] 1#1: nginx/1.29.7  
    2026/04/02 06:23:13 [notice] 1#1: built by gcc 14.2.0 (Debian 14.2.0-19)  
    2026/04/02 06:23:13 [notice] 1#1: OS: Linux 6.12.76-linuxkit  
    2026/04/02 06:23:13 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576  
    2026/04/02 06:23:13 [notice] 1#1: start worker processes  
    2026/04/02 06:23:13 [notice] 1#1: start worker process 29  
    2026/04/02 06:23:13 [notice] 1#1: start worker process 30  
    2026/04/02 06:23:13 [notice] 1#1: start worker process 31  
    2026/04/02 06:23:13 [notice] 1#1: start worker process 32  
    2026/04/02 06:23:13 [notice] 1#1: start worker process 33  
    2026/04/02 06:23:13 [notice] 1#1: start worker process 34  
    2026/04/02 06:23:13 [notice] 1#1: start worker process 35  
    2026/04/02 06:23:13 [notice] 1#1: start worker process 36  
    ```

* 실시간 계속 보기  
    ```bash
    docker logs -f 컨테이너_이름  

    docker logs -f web1  

    /docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration  
    /docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/  
    /docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh  
    10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf  
    10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf  
    /docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh  
    /docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh  
    /docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh  
    /docker-entrypoint.sh: Configuration complete; ready for start up  
    2026/04/02 06:23:13 [notice] 1#1: using the "epoll" event method  
    2026/04/02 06:23:13 [notice] 1#1: nginx/1.29.7  
    2026/04/02 06:23:13 [notice] 1#1: built by gcc 14.2.0 (Debian 14.2.0-19)  
    2026/04/02 06:23:13 [notice] 1#1: OS: Linux 6.12.76-linuxkit  
    2026/04/02 06:23:13 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576  
    2026/04/02 06:23:13 [notice] 1#1: start worker processes  
    2026/04/02 06:23:13 [notice] 1#1: start worker process 29  
    2026/04/02 06:23:13 [notice] 1#1: start worker process 30  
    2026/04/02 06:23:13 [notice] 1#1: start worker process 31  
    2026/04/02 06:23:13 [notice] 1#1: start worker process 32  
    2026/04/02 06:23:13 [notice] 1#1: start worker process 33  
    2026/04/02 06:23:13 [notice] 1#1: start worker process 34  
    2026/04/02 06:23:13 [notice] 1#1: start worker process 35  
    2026/04/02 06:23:13 [notice] 1#1: start worker process 36  

    
    ```
* 최근 몇 줄만 보기  
    ```bash
    docker logs --tail 10 컨테이너_이름  

    docker logs --tail 10 web1  

    2026/04/02 06:23:13 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576  
    2026/04/02 06:23:13 [notice] 1#1: start worker processes  
    2026/04/02 06:23:13 [notice] 1#1: start worker process 29  
    2026/04/02 06:23:13 [notice] 1#1: start worker process 30  
    2026/04/02 06:23:13 [notice] 1#1: start worker process 31  
    2026/04/02 06:23:13 [notice] 1#1: start worker process 32  
    2026/04/02 06:23:13 [notice] 1#1: start worker process 33  
    2026/04/02 06:23:13 [notice] 1#1: start worker process 34  
    2026/04/02 06:23:13 [notice] 1#1: start worker process 35  
    2026/04/02 06:23:13 [notice] 1#1: start worker process 36  
    ```

* 최근 시점 이후만 보기  
    ```bash
    docker logs --since 10m 컨테이너_이름  

    docker logs --since 10m web1  
    ```


<br><br>

## 🟢 3. 컨테이너 내부 파일 변경 이력 보기  

### 🟡 3-1. `docker diff`  
* 뜻  
    * **컨테이너 파일시스템에서 변경된 파일/폴더 확인**  

* 기본 명령어  
    ```bash
    docker diff 컨테이너_이름  
    ```

* 실행 기록  
    ```bash
    docker diff web1  

    C /etc  
    C /etc/nginx  
    C /etc/nginx/conf.d  
    C /etc/nginx/conf.d/default.conf  
    C /root  
    A /root/.bash_history  
    C /var  
    C /var/cache  
    C /var/cache/nginx  
    A /var/cache/nginx/client_temp  
    A /var/cache/nginx/fastcgi_temp  
    A /var/cache/nginx/proxy_temp  
    A /var/cache/nginx/scgi_temp  
    A /var/cache/nginx/uwsgi_temp  
    A /test  
    A /test/hello_cp.html  
    A /test/index_cp.html  
    A /test/test.html  
    A /test/test2.txt  
    C /run  
    A /run/nginx.pid  
    ```

* 출력 기호 뜻  
    ```text
    A = Added     새로 생성됨  
    C = Changed   변경됨  
    D = Deleted   삭제됨  
    ```
