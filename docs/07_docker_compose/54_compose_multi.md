# 🟩 54. docker compose multi container practice

## 📋 Contents  
1. 실습 목표  
2. 왜 멀티 컨테이너가 필요한가  
3. compose 예시 파일  
4. 실행과 확인  
5. 배운 핵심  



<br><br>

## 🟢 1. 실습 목표  
- 웹 서비스와 DB 같은 여러 컨테이너를 함께 실행하는 감각을 익힌다.  
- `depends_on` 과 서비스 이름 기반 연결 개념을 이해한다.  



<br><br>

## 🟢 2. 왜 멀티 컨테이너가 필요한가  
- 실제 서비스는 컨테이너 1개로 끝나는 경우가 드물다.  
- 보통 웹 서버, 백엔드, DB, 캐시 서버가 나뉜다.  
- Docker Compose는 이런 여러 서비스를 한 번에 띄우기 좋다.  



<br><br>

## 🟢 3. compose 예시 파일  

<br>

### 🟡 web + db 예시  
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
```

- `web` 과 `db` 두 서비스를 함께 정의  
- `depends_on` 은 `db` 가 먼저 준비되도록 실행 순서를 잡을 때 사용  
- DB 비밀번호는 예시이며, 실제 문서나 실무에서는 더 안전하게 관리해야 한다.  



<br><br>

## 🟢 4. 실행과 확인  

<br>

### 🟡 실행 명령  
```bash
docker compose up -d
docker compose ps
docker compose logs
```

- `up -d` 로 두 서비스 실행  
- `ps` 로 두 컨테이너가 떠 있는지 확인  
- `logs` 로 에러 여부 확인  



<br><br>

## 🟢 5. 배운 핵심  

<br>

### 🟡 핵심 요약  
```bash
Docker Compose는 여러 컨테이너를 하나의 파일로 정의하고 함께 실행할 수 있게 해준다.
```

- 단일 컨테이너보다 멀티 컨테이너에서 Compose의 장점이 더 크게 보인다.  
- 그래서 Docker Compose를 이해하려면 반드시 멀티 컨테이너 예시까지 봐야 한다.  
