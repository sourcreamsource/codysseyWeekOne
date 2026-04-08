# 🟩 57. compose 환경 변수 자세히 보기  

## 📋 Contents  
1. 이 예제가 무엇을 배우게 하는가  
2. `.env` 파일 읽기  
3. .env 환경 병수가 `compose.yaml` 에서 쓰이는 방법  
4. 실행 흐름과 결과 확인  
5. 왜 이것이 설정과 코드의 분리인가  



<br><br><br><br>

## 🟢 1. 이 예제가 무엇을 배우게 하는가  
- 이 예제의 핵심은 포트 번호와 실행 모드를 코드에 직접 적지 않고, `.env` 파일로 따로 빼는 것이다.  
- `compose.yaml` 과 `nginx 설정`만으로 환경 변수 변경을 확인하는 연습이다.  
- Docker Compose에서는 이런 방식으로 설정과 실행을 분리하는 경우가 많다.  


<br>

### 🟡 실습 파일 위치  
```bash
# /codysseyWeekOne/playground/docker-practice/compose-practice/04_env  
ls -la  
```
- `.env` : 환경 변수 값을 적는 파일  
- `compose.yaml` : 컨테이너 실행 설정 파일  
- `templates/default.conf.template` : nginx 포트를 환경 변수로 받는 설정 템플릿  
- `templates/index.html.template` : 실행 모드와 포트를 화면에 보여주는 HTML 템플릿  



<br><br><br><br>

## 🟢 2. `.env` 파일 읽기  

<br>

### 🟡 `.env` 예시  
```bash
HOST_PORT=8090  
APP_PORT=80  
APP_MODE=study  
```

- `HOST_PORT=8090`  
내 컴퓨터에서 접속할 포트 번호이다. 즉 브라우저나 `curl` 에서 `localhost:8090` 으로 접속하게 된다.  

- `APP_PORT=80`  
컨테이너 안에서 nginx가 실제로 열 포트 번호이다.  

- `APP_MODE=study`  
애플리케이션이 어떤 모드로 실행되는지 나타내는 값이다. 여기서는 연습용 문자열이다.  


<br>

### 🟡 왜 `.env` 로 따로 빼는가  
```bash
값을 바꿀 때 compose.yaml 안의 숫자나 HTML 내용을 직접 수정하지 않기 위해서이다.  
```

- 포트 번호를 바꾸고 싶을 수 있다.  
- 실행 모드를 `study` 에서 `debug` 로 바꾸고 싶을 수 있다.  
- 이런 값을 파일 안에 하드코딩하면, 설정을 바꿀 때마다 설정 파일과 화면 내용을 같이 수정해야 한다.  
- `.env` 를 사용하면 설정만 바꾸고 나머지 파일은 그대로 둘 수 있다.  



<br><br><br><br>

## 🟢 3. .env 환경 병수가 `compose.yaml` 에서 쓰이는 방법  

<br>

### 🟡 `compose.yaml` 전체  
```yaml
services:  
  web:  
    image: nginx:1.27-alpine  
    container_name: compose-env-web  
    🔥 environment:  
      🔥 APP_PORT: ${APP_PORT}  
      🔥 APP_MODE: ${APP_MODE}  
    🔥 ports:  
      🔥 - "${HOST_PORT}:${APP_PORT}"  
    volumes:  
      - ./templates/default.conf.template:/templates/default.conf.template:ro  
      - ./templates/index.html.template:/templates/index.html.template:ro  
    command:  
      - /bin/sh  
      - -c  
      - envsubst '$$APP_PORT' < /templates/default.conf.template > /etc/nginx/conf.d/default.conf && envsubst '$$APP_MODE $$APP_PORT' < /templates/index.html.template > /usr/share/nginx/html/index.html && nginx -g 'daemon off;'  
```


<br>

### 🟡 `environment:` 부분  
```yaml
🔥 environment:  
  🔥 APP_PORT: ${APP_PORT}  
  🔥 APP_MODE: ${APP_MODE}  
```
- 왼쪽 `APP_PORT`, `APP_MODE` 는 컨테이너 내부에 전달될 환경 변수 이름이다.  
- 오른쪽 `${APP_PORT}`, `${APP_MODE}` 는 `.env` 파일에서 값을 읽어오겠다는 뜻이다.  
- 즉 `.env` 에 적어 둔 값을 nginx 설정 템플릿과 HTML 템플릿에서 사용할 수 있게 전달하는 부분이다.  


<br>

### 🟡 `ports:` 부분  
```yaml
🔥 ports:  
  🔥 - "${HOST_PORT}:${APP_PORT}"  
```
- 앞의 `HOST_PORT` 는 내 컴퓨터 쪽 포트이다.  
- 뒤의 `APP_PORT` 는 컨테이너 내부 nginx 포트이다.  
- 즉 `.env` 값에 따라 `8090:80`, 혹은 `8091:81` 같은 형태가 된다.  


<br>

### 🟡 `command:` 부분  
```yaml
command:  
  - /bin/sh  
  - -c  
  - envsubst '$$APP_PORT' < /templates/default.conf.template > /etc/nginx/conf.d/default.conf && envsubst '$$APP_MODE $$APP_PORT' < /templates/index.html.template > /usr/share/nginx/html/index.html && nginx -g 'daemon off;'  
```

- `envsubst` 는 환경 변수 값을 실제 문자열로 바꿔 넣는 도구이다.  
- 먼저 `default.conf.template` 의 `${APP_PORT}` 를 실제 포트 값으로 바꿔 nginx 설정 파일을 만든다.  
- 이 명령은 `index.html.template` 안의 `${APP_MODE}`, `${APP_PORT}` 를 실제 값으로 치환해서 `index.html` 을 만든다.  
- 그 다음 nginx를 포그라운드로 실행한다.  
- 즉 별도 Python 서버 없이도 환경 변수가 반영된 결과를 브라우저에서 확인할 수 있다.  



<br><br><br><br>

## 🟢 4. 실행 흐름과 결과 확인  

<br>

### 🟡 1단계. 현재 값으로 실행  
```bash
# /codysseyWeekOne/playground/docker-practice/compose-practice/04_env  
docker compose up -d  

[+] up 13/13  
 ✔ Image nginx:1.27-alpine   Pulled                                                             6.5s  
 ✔ Network 04_env_default    Created                                                            0.0s  
 ✔ Container compose-env-web Started  


# -------------------------  
docker compose ps  

NAME              IMAGE               COMMAND                  SERVICE   CREATED          STATUS          PORTS  
compose-env-web   nginx:1.27-alpine   "/docker-entrypoint.…"   web       24 seconds ago   Up 23 seconds   0.0.0.0:8090->80/tcp, [::]:8090->80/tcp  


# -------------------------  
curl http://localhost:8090  

<!DOCTYPE html>
<html lang="ko">
  <head>
    <meta charset="UTF-8" />
    <title>Compose Env Practice</title>
  </head>
  <body>
    <h1>compose env practice</h1>
    <p>mode=study</p>
    <p>port=80</p>
  </body>
</html>
```

- 이 상태에서는 `.env` 값이 `HOST_PORT=8090`, `APP_PORT=80`, `APP_MODE=study` 이다.  
- 따라서 `curl http://localhost:8090` 결과 HTML 안에 `mode=study`, `port=80` 이 보인다.  


<br>

### 🟡 왜 `8090` 으로 접속하는가  
```bash
HOST_PORT=8090  
APP_PORT=80  
```

- nginx는 컨테이너 안에서 `80` 포트로 동작한다.  
- 하지만 내 컴퓨터에서는 `8090` 으로 접속한다.  
- 그 연결을 Compose의 `ports` 설정이 해준다.  


<br>

### 🟡 2단계. 값 변경 후 다시 실행  
```bash
# 환경변수 변경  
sed -i '' 's/HOST_PORT=8090/HOST_PORT=8091/' .env  
sed -i '' 's/APP_PORT=80/APP_PORT=81/' .env  
sed -i '' 's/APP_MODE=study/APP_MODE=debug/' .env  

# docker compose down으로 기존 docker compose 제거  
docker compose down  
[+] down 2/2  
 ✔ Container compose-env-web Removed                                                            0.2s  
 ✔ Network 04_env_default    Removed  
 
# 새로운 환경변수를 적용하여 잘 실행되는지 실행해보기  
docker compose up -d  
[+] up 2/2  
 ✔ Network 04_env_default    Created                                                            0.0s  
 ✔ Container compose-env-web Started  

# comopse servies 정상 실행 여부 확인  
docker compose ps  
NAME              IMAGE               COMMAND                  SERVICE   CREATED          STATUS          PORTS  
compose-env-web   nginx:1.27-alpine   "/docker-entrypoint.…"   web       17 seconds ago   Up 16 seconds   0.0.0.0:8091->81/tcp, [::]:8091->81/tcp  

# 해당 포트로 확인해보기  
curl http://localhost:8091  

<!DOCTYPE html>
<html lang="ko">
  <head>
    <meta charset="UTF-8" />
    <title>Compose Env Practice</title>
  </head>
  <body>
    <h1>compose env practice</h1>
    <p>mode=debug</p>
    <p>port=81</p>
  </body>
</html>
```

- `.env` 값을 바꾸면 Compose가 새 값으로 컨테이너 설정을 다시 적용한다.  
- 이제 내 컴퓨터에서는 `8091` 로 접속해야 한다.  
- 결과에는 `mode=debug`, `port=81` 이 표시된다.  


<br>

### 🟡 이때 실제로 바뀐 것  
```bash
바뀐 것은 설정값이고, compose 템플릿 파일은 그대로이다.  
```

- 포트가 `8090 -> 8091` 로 바뀌었다.  
- 내부 앱 포트가 `80 -> 81` 로 바뀌었다.  
- 실행 모드가 `study -> debug` 로 바뀌었다.  
- 하지만 `compose.yaml` 과 템플릿 파일 내용은 수정하지 않았다는 사실이다!!!  
  - 환경변수 변경만으로 설정값을 바꿀 수 있다는 사실을 알 수 있다.  





<br><br><br><br>

## 🟢 5. 왜 이것이 설정과 코드의 분리인가  

<br>

### 🟡 설정을 직접 박아두는 방식의 문제  
```bash
포트와 모드를 파일 안에 직접 적어두면, 설정 변경 때마다 템플릿과 실행 설정을 함께 수정해야 한다.  
```

- 예를 들어 포트 충돌이 나면 파일 속 숫자를 다시 적어야 한다.  
- 개발용, 테스트용, 배포용 모드가 다르면 파일 안의 값을 계속 바꿔야 한다.  
- 이런 방식은 실수하기 쉽고 관리도 어렵다.  


<br>

### 🟡 환경 변수 방식의 장점  
```bash
코드는 그대로 두고, 실행 설정만 바꿀 수 있다.  
```

- 개발 환경에서는 `APP_MODE=study`  
- 디버깅 환경에서는 `APP_MODE=debug`  
- 포트가 겹치면 `HOST_PORT` 만 바꾸면 된다.  
- 여러 사람이 같은 Compose 예제를 쓰더라도 각자 다른 설정으로 실행할 수 있다.  


<br>

### 🟡 핵심 요약  
```bash
Compose의 환경 변수는 "프로그램 코드"와 "실행 설정"을 분리하기 위한 도구이다.  
```

- `.env` 는 값을 보관하는 곳이다.  
- `compose.yaml` 은 그 값을 읽어 컨테이너 실행 설정에 반영한다.  
- nginx 템플릿과 HTML 템플릿은 전달받은 값을 화면에 보여준다.  
- 이 흐름을 이해하면 Docker Compose에서 환경 변수를 왜 많이 쓰는지 쉽게 이해할 수 있다.  
