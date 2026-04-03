# 🟩 53. docker compose basic practice

## 📋 Contents  
1. 실습 목표  
2. 실습 폴더 준비  
3. compose.yaml 작성  
4. 서비스 실행  
5. 접속 확인  
6. 종료 및 정리  



<br><br>

## 🟢 1. 실습 목표  
- nginx 컨테이너 하나를 Docker Compose로 실행해 본다.  
- `docker run` 없이 `compose.yaml` 파일만으로 실행 환경을 정의하는 감각을 익힌다.  



<br><br>

## 🟢 2. 실습 폴더 준비  

<br>

### 🟡 폴더 만들기  
```bash
cd ~
mkdir compose-basic
cd compose-basic
```



<br><br>

## 🟢 3. compose.yaml 작성  

<br>

### 🟡 예시 파일  
```yaml
services:
  web:
    image: nginx:latest
    container_name: compose-web
    ports:
      - "8080:80"
```

- nginx 컨테이너를 `compose-web` 이름으로 실행  
- 내 컴퓨터 `8080` 을 컨테이너 `80` 과 연결  



<br><br>

## 🟢 4. 서비스 실행  

<br>

### 🟡 실행 명령  
```bash
docker compose up -d
docker compose ps
```

- `up -d` 로 실행  
- `ps` 로 상태 확인  



<br><br>

## 🟢 5. 접속 확인  

<br>

### 🟡 브라우저 또는 curl  
```bash
curl http://localhost:8080
```

- nginx 기본 페이지가 보이면 성공이다.  
- 이 실습은 `docker run -d --name ... -p ... nginx` 를 Compose 방식으로 바꾼 것이다.  



<br><br>

## 🟢 6. 종료 및 정리  

<br>

### 🟡 종료 명령  
```bash
docker compose down
```

- Compose로 띄운 서비스를 정리하며 종료한다.  
