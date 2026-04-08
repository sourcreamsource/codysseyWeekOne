# 🟩 55. docker compose 멀티 컨테이너 네트워크  

## 📋 Contents  
1. 이 예제가 무엇을 보여주는가  
2. compose.yaml 한 줄씩 읽기  
3. 왜 `web` 이름으로 접근할 수 있는가  
4. 직접 실행해서 확인하는 순서  
5. 이 예제에서 꼭 기억할 핵심  



<br><br>

## 🟢 1. 이 예제가 무엇을 보여주는가  
- 이 예제는 Docker Compose로 컨테이너 2개를 함께 실행하는 가장 단순한 멀티 컨테이너 실습이다.  
- 핵심은 `web` 과 `helper` 가 같은 Compose 네트워크에 들어간다는 점이다.  
- 그래서 `helper` 컨테이너 안에서 `web` 이라는 서비스 이름만으로 웹 서버에 접근할 수 있다.  


### 🟡 실습 파일 위치  
/codysseyWeekOne/playground/docker-practice/compose-practice/03_multi_network  
- `compose.yaml` : 멀티 컨테이너 실행 설정 파일  
- `html/index.html` : nginx가 보여줄 간단한 페이지  



<br><br>

## 🟢 2. compose.yaml 한 줄씩 읽기  

### 🟡 compose.yaml 전체  
```yaml
services:  
  web:  
    image: nginx:latest  
    container_name: proof-multi-web  
    ports:  
      - "8082:80"  
    volumes:  
      - ./html/index.html:/usr/share/nginx/html/index.html:ro  

  helper:  
    image: busybox:1.36  
    container_name: proof-multi-helper  
    command: ["sh", "-c", "tail -f /dev/null"]  
    depends_on:  
      - web  
```


<br>

### 🟡 `services:`  
```yaml
services:  
```
- Compose에서 실행할 컨테이너 목록이 시작된다는 뜻이다.  
- 여기 아래에 `web`, `helper` 두 서비스가 들어 있다.  


<br>

### 🟡 `web` 서비스  
```yaml
  web:  
    image: nginx:latest  
    container_name: proof-multi-web  
    ports:  
      - "8082:80"  
    volumes:  
      - ./html/index.html:/usr/share/nginx/html/index.html:ro  
```
- `web` 은 서비스 이름이다.  
- `image: nginx:latest` 는 nginx 이미지를 사용한다는 뜻이다.  
- `container_name: proof-multi-web` 는 실제 컨테이너 이름을 고정한다.  
- `ports: "8082:80"` 는 내 컴퓨터의 `8082` 포트를 컨테이너 내부 `80` 포트에 연결한다.  
- `volumes` 는 현재 폴더의 `index.html` 파일을 nginx 기본 페이지 위치에 연결한다.  
- 뒤의 `:ro` 는 읽기 전용이다. 즉, 컨테이너가 이 파일을 수정하지 못하게 한다.  


<br>

### 🟡 `helper` 서비스  
- 단순 multi service Test를 위한 존재 (AI 도움을 받음)  

```yaml
  helper:  
    image: busybox:1.36  
    container_name: proof-multi-helper  
    command: ["sh", "-c", "tail -f /dev/null"]  
    depends_on:  
      - web  
```

- `helper` 도 서비스 이름이다.  
- `busybox` 는 매우 가볍고 단순한 리눅스 도구 이미지다.  
- `command: ["sh", "-c", "tail -f /dev/null"]` 는 컨테이너가 바로 종료되지 않도록 계속 살아 있게 만드는 명령이다.  
- `depends_on` 은 `web` 서비스를 먼저 띄우고 그 다음 `helper` 를 띄우도록 순서를 잡는다.  
- 다만 이것은 "실행 순서" 중심이지, `web` 이 완전히 준비 완료될 때까지 기다린다는 뜻은 아니다.  






<br><br>

## 🟢 3. 왜 `web` 이름으로 접근할 수 있는가? = 서비스 디스커버리  

<br>

### 🟡 핵심 개념  
```bash
Compose는 같은 프로젝트의 서비스들을 기본 네트워크에 함께 연결한다.  
```

- `docker compose up -d` 를 하면 Compose가 기본 네트워크를 자동으로 만든다.  
- 같은 `compose.yaml` 안의 서비스들은 그 네트워크에 같이 들어간다.  
- 그래서 컨테이너끼리 IP 주소를 직접 몰라도 된다.  
- 서비스 이름으로 서로를 찾을 수 있다. 이것을 **`서비스 디스커버리`**라고 생각하면 된다.  


<br>

### 🟡 이 예제에 적용하면  
```bash
helper  ->  http://web  
```

- `helper` 입장에서는 `web` 이 하나의 호스트 이름처럼 동작한다.  
- 즉 `http://web` 요청은 같은 Compose 네트워크 안의 `web` 서비스로 전달된다.  
- 사람이 직접 IP를 찾고 입력하지 않아도 된다.  


<br>

### 🟡 브라우저 접속과 컨테이너 내부 접속의 차이  
```bash
http://localhost:8082  
http://web  
```

- `http://localhost:8082` 는 내 컴퓨터에서 접속하는 방식이다.  
- `http://web` 은 Compose 네트워크 안의 다른 컨테이너가 접속하는 방식이다.  
- 앞은 `ports` 설정이 필요하고, 뒤는 같은 네트워크 안이라 서비스 이름으로 통신한다.  







<br><br>

## 🟢 4. 직접 실행해서 확인하는 순서  

<br>

### 🟡 1단계. 서비스 실행  
```bash
docker compose up -d  

[+] up 16/16  
 ✔ Image nginx:latest               Pulled                                                     10.9s  
 ✔ Image busybox:1.36               Pulled                                                     8.3ss  
 ✔ Network 03_multi_network_default Created                                                    0.0s  
 ✔ Container proof-multi-web        Started                                                    0.4s  
 ✔ Container proof-multi-helper     Started                                                    0.2s  
```

- `web`, `helper` 두 컨테이너가 함께 올라간다.  


<br>

### 🟡 2단계. 상태 확인  
```bash
docker compose ps  

NAME                 IMAGE          COMMAND                  SERVICE   CREATED          STATUS          PORTS  
proof-multi-helper   busybox:1.36   "sh -c 'tail -f /dev…"   helper    22 seconds ago   Up 22 seconds  
proof-multi-web      nginx:latest   "/docker-entrypoint.…"   web       23 seconds ago   Up 22 seconds   0.0.0.0:8082->80/tcp, [::]:8082->80/tcp  
```

- 두 서비스가 모두 `Up` 상태인지 본다.  
- 하나라도 꺼져 있으면 네트워크 테스트 전에 로그부터 봐야 한다.  


<br>

### 🟡 3단계. 내 컴퓨터에서 웹 확인  
```bash
curl http://localhost:8082  

<!DOCTYPE html>
<html lang="ko">
  <head>
    <meta charset="UTF-8" />
    <title>Compose Multi Network Practice</title>
  </head>
  <body>
    <h1>03_multi_network practice</h1>
    <p>helper container can access this page by service name.</p>
  </body>
</html>
```

- `03_multi network practice` 문구가 보이면 `web` 서비스는 정상이다.  
- 이것은 호스트에서 nginx에 접속한 것이다!!!  


<br>

### 🟡 4단계. 🔥🔥 컨테이너 안에서 다른 서비스 접속  
```bash
# helper로 접속!  
docker compose exec helper sh  
/ #      접속 완료!  

# ping web          # 간단한 핑 확인  
# curl http://web   # curl이 있다면  
```

```bash
# 같은 네트워크에 있는 web에 접근하여 응답 받기  
wget -qO- http://web  

<!DOCTYPE html>
<html lang="ko">
  <head>
    <meta charset="UTF-8" />
    <title>Compose Multi Network Practice</title>
  </head>
  <body>
    <h1>03_multi_network practice</h1>
    <p>helper container can access this page by service name.</p>
  </body>
</html>
```

```bash
# docker compose exec helper wget -qO- http://web  
```
- 이 명령은 `helper` 컨테이너 내부에서 실행된다.  
- `wget -qO-` 는 결과 화면만 조용히 출력한다.  
- 여기서도 `03_multi network practice` 문구가 보이면 `helper -> web` 통신이 성공한 것이다.  


<br>

### 🟡 5단계. 로그 확인  
```bash
docker compose logs  

# 로그 기록  
proof-multi-web  | /docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration  
proof-multi-web  | /docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/  
proof-multi-web  | /docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh  
proof-multi-web  | 10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf  
proof-multi-web  | 10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf  
proof-multi-web  | /docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh  
proof-multi-web  | /docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh  
proof-multi-web  | /docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh  
proof-multi-web  | /docker-entrypoint.sh: Configuration complete; ready for start up  
proof-multi-web  | 2026/04/08 08:34:39 [notice] 1#1: using the "epoll" event method  
proof-multi-web  | 2026/04/08 08:34:39 [notice] 1#1: nginx/1.29.8  
proof-multi-web  | 2026/04/08 08:34:39 [notice] 1#1: built by gcc 14.2.0 (Debian 14.2.0-19)  
proof-multi-web  | 2026/04/08 08:34:39 [notice] 1#1: OS: Linux 6.12.76-linuxkit  
proof-multi-web  | 2026/04/08 08:34:39 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576  
proof-multi-web  | 2026/04/08 08:34:39 [notice] 1#1: start worker processes  
proof-multi-web  | 2026/04/08 08:34:39 [notice] 1#1: start worker process 29  
proof-multi-web  | 2026/04/08 08:34:39 [notice] 1#1: start worker process 30  
proof-multi-web  | 2026/04/08 08:34:39 [notice] 1#1: start worker process 31  
proof-multi-web  | 2026/04/08 08:34:39 [notice] 1#1: start worker process 32  
proof-multi-web  | 2026/04/08 08:34:39 [notice] 1#1: start worker process 33  
proof-multi-web  | 2026/04/08 08:34:39 [notice] 1#1: start worker process 34  
proof-multi-web  | 2026/04/08 08:34:39 [notice] 1#1: start worker process 35  
proof-multi-web  | 2026/04/08 08:34:39 [notice] 1#1: start worker process 36  
proof-multi-web  | 192.168.65.1 - - [08/Apr/2026:08:37:24 +0000] "GET / HTTP/1.1" 200 263 "-" "curl/8.7.1" "-"  
proof-multi-web  | 172.18.0.3 - - [08/Apr/2026:08:39:22 +0000] "GET / HTTP/1.1" 200 263 "-" "Wget" "-"  
proof-multi-web  | 172.18.0.3 - - [08/Apr/2026:08:41:38 +0000] "GET / HTTP/1.1" 200 263 "-" "Wget" "-"  
```

- `web` 의 nginx 로그를 보고 요청이 들어왔는지 확인할 수 있다.  
- 필요하면 서비스별로 `docker compose logs web` 처럼 볼 수도 있다.  


<br>

### 🟡 6단계. 종료  
```bash
docker compose down  
```

- 실습이 끝나면 네트워크와 컨테이너를 함께 정리한다.  







<br><br>

## 🟢 5. 이 예제에서 꼭 기억할 핵심  

<br>

### 🟡 핵심 요약  
```bash
같은 compose.yaml 안의 서비스들은 기본적으로 같은 네트워크에 들어가며, 서비스 이름으로 서로 통신할 수 있다.  
```

- `web` 은 웹 서버 역할이다.  
- `helper` 는 다른 컨테이너에서 `web` 에 접속해 보는 확인용 보조 서비스다.  
- `localhost:8082` 는 호스트에서 접근하는 방법이다.  
- `http://web` 은 컨테이너끼리 접근하는 방법이다.  
- 이 차이를 이해하면 Docker Compose 멀티 컨테이너 구조를 훨씬 쉽게 볼 수 있다.  





