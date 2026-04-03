# 🟩 51. docker compose 파일 구조

## 📋 Contents  
1. compose 파일 이름  
2. 가장 기본 구조  
3. services 이해하기  
4. 자주 쓰는 항목  
5. 예시로 읽는 연습  



<br><br>

## 🟢 1. compose 파일 이름  
- 보통 `compose.yaml` 또는 `docker-compose.yml` 파일을 사용한다.  
- 최신 방식에서는 `compose.yaml` 이름을 많이 사용한다.  



<br><br>

## 🟢 2. 가장 기본 구조  

<br>

### 🟡 가장 작은 예시  
```yaml
services:
  web:
    image: nginx:latest
    ports:
      - "8080:80"
```

- `services:` 아래에 실행할 컨테이너들을 적는다.  
- 위 예시는 `web` 이라는 이름으로 nginx 컨테이너 하나를 띄우는 설정이다.  



<br><br>

## 🟢 3. services 이해하기  
- `services` 는 "실행할 컨테이너 묶음" 이라고 생각하면 쉽다.  
- 예를 들어 `web`, `api`, `db` 를 각각 하나의 서비스로 나눌 수 있다.  
- 서비스 이름은 Compose 내부에서 컨테이너 간 통신 이름처럼도 쓰인다.  


<br>

### 🟡 예시  
```yaml
services:
  web:
    image: nginx:latest

  db:
    image: mysql:8
```



<br><br>

## 🟢 4. 자주 쓰는 항목  

| 항목 | 의미 | 예시 |
| :--- | :--- | :--- |
| `image` | 사용할 이미지 | `image: nginx:latest` |
| `build` | Dockerfile로 직접 이미지 빌드 | `build: .` |
| `ports` | 호스트 포트와 컨테이너 포트 연결 | `- "8080:80"` |
| `volumes` | 데이터 연결 | `- ./app:/app` |
| `environment` | 환경 변수 설정 | `- APP_ENV=dev` |
| `container_name` | 컨테이너 이름 지정 | `container_name: my-nginx` |
| `depends_on` | 다른 서비스 먼저 실행 | `depends_on: - db` |



<br><br>

## 🟢 5. 예시로 읽는 연습  

<br>

### 🟡 조금 더 실제 같은 예시  
```yaml
services:
  web:
    image: nginx:latest
    container_name: my-web
    ports:
      - "8080:80"
    volumes:
      - ./html:/usr/share/nginx/html
```

- `web` 서비스 실행  
- nginx 이미지 사용  
- 컨테이너 이름은 `my-web`  
- 내 컴퓨터 `8080` 을 컨테이너 `80` 과 연결  
- 현재 폴더의 `html` 을 nginx 웹 폴더에 연결  

- 즉, 이 파일 하나가 `docker run -d --name ... -p ... -v ...` 역할을 대신한다.  
