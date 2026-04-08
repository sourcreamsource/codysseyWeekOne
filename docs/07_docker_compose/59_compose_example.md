# 🟩 55. 마지막으로 docker compose 사용 예시들 모음  

## 📋 Contents  
1. 가장 기본적인 nginx 예시  
2. bind mount 포함 예시  
3. Dockerfile build 포함 예시  
4. web + db 멀티 컨테이너 예시  
5. 읽을 때 먼저 볼 포인트  



<br><br>

## 🟢 1. 가장 기본적인 nginx 예시  

<br>

### 🟡 1-1. 단일 서비스 예시  
```yaml
services:  
  web:  
    image: nginx:latest  
    container_name: compose-web  
    ports:  
      - "8080:80"  
```

- `web` 이라는 서비스 하나만 실행  
- nginx 이미지 사용  
- 호스트 `8080` 과 컨테이너 `80` 연결  



<br><br>

## 🟢 2. bind mount 포함 예시  

<br>

### 🟡 2-1. html 폴더 연결 예시  
```yaml
services:  
  web:  
    image: nginx:latest  
    container_name: compose-web  
    ports:  
      - "8080:80"  
    volumes:  
      - ./html:/usr/share/nginx/html  
```

- 현재 폴더의 `html` 을 nginx 웹 폴더에 연결  
- host 파일을 수정하면 바로 반영 확인 가능  
- 개발 중 정적 파일 확인용으로 자주 쓴다  



<br><br>

## 🟢 3. Dockerfile build 포함 예시  

<br>

### 🟡 3-1. Dockerfile로 직접 이미지 만들기  
```yaml
services:  
  web:  
    build:  
      context: .  
      dockerfile: Dockerfile  
    container_name: my-nginx-app  
    ports:  
      - "8080:80"  
```

- 현재 폴더의 Dockerfile로 이미지를 직접 빌드  
- `docker run` 전에 따로 이미지 이름을 관리하지 않아도 Compose가 처리 가능  
- Dockerfile 실습과 Compose를 연결할 때 자주 쓰는 형태  



<br><br>

## 🟢 4. web + db 멀티 컨테이너 예시  

<br>

### 🟡 4-1. 웹 + 데이터베이스 예시  
```yaml
services:  
  web:  
    image: nginx:latest  
    container_name: compose-web  
    ports:  
      - "8080:80"  
    depends_on:  
      - db  

  db:  
    image: mysql:8.0  
    container_name: compose-db  
    environment:  
      MYSQL_ROOT_PASSWORD: 1234  
      MYSQL_DATABASE: appdb  
    volumes:  
      - db_data:/var/lib/mysql  

volumes:  
  db_data:  
```

- `web` 과 `db` 두 서비스를 함께 실행  
- `depends_on` 으로 실행 순서를 잡음  
- `db_data` volume 으로 DB 데이터를 영속 저장  
- 실제 서비스에서는 비밀번호를 문서에 그대로 쓰기보다 환경 변수 파일 등을 사용하는 것이 더 안전하다  



<br><br>

## 🟢 5. 읽을 때 먼저 볼 포인트  

<br>

### 🟡 5-1. compose 파일 읽는 순서  
| 먼저 볼 것 | 왜 중요한가 |  
| :--- | :--- |
| `services` | 어떤 컨테이너들이 실행되는지 알 수 있다 |  
| `image` / `build` | 이미지를 가져오는지, 직접 만드는지 알 수 있다 |  
| `ports` | 외부 접속 포트를 알 수 있다 |  
| `volumes` | 데이터가 어디에 저장되는지 알 수 있다 |  
| `environment` | 어떤 환경 변수로 동작하는지 알 수 있다 |  
| `depends_on` | 서비스 간 관계를 알 수 있다 |  


<br>

### 🟡 5-2. 한 줄 정리  
```bash
compose.yaml 은 여러 docker run 명령을 구조적으로 정리한 실행 설정 파일이다.  
```
