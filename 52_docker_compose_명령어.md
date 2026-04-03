# 🟩 52. docker compose 명령어

## 📋 Contents  
1. 가장 기본 명령  
2. up / down  
3. ps / logs  
4. build / restart  
5. 가장 많이 쓰는 흐름  



<br><br>

## 🟢 1. 가장 기본 명령  

| 명령어 | 의미 | 언제 쓰는가 |
| :--- | :--- | :--- |
| `docker compose up` | 서비스 실행 | compose 파일대로 시작할 때 |
| `docker compose up -d` | 백그라운드 실행 | 터미널을 점유하지 않게 실행할 때 |
| `docker compose down` | 서비스 종료 및 정리 | 실행 종료할 때 |
| `docker compose ps` | 실행 상태 확인 | 어떤 서비스가 떠 있는지 볼 때 |
| `docker compose logs` | 로그 확인 | 서비스 로그를 볼 때 |
| `docker compose build` | 이미지 빌드 | Dockerfile 기반 서비스 빌드할 때 |
| `docker compose restart` | 재시작 | 설정 변경 후 다시 올릴 때 |



<br><br>

## 🟢 2. up / down  

<br>

### 🟡 자주 쓰는 형태  
```bash
docker compose up -d
docker compose down
```

- `up -d` 는 서비스를 백그라운드로 실행한다.  
- `down` 은 compose로 띄운 서비스들을 정리하면서 종료한다.  



<br><br>

## 🟢 3. ps / logs  

<br>

### 🟡 상태와 로그 확인  
```bash
docker compose ps
docker compose logs
docker compose logs -f
```

- `ps` 는 실행 중인 서비스 상태를 본다.  
- `logs` 는 서비스 로그를 본다.  
- `logs -f` 는 실시간으로 계속 본다.  



<br><br>

## 🟢 4. build / restart  

<br>

### 🟡 빌드 관련  
```bash
docker compose build
docker compose up --build
docker compose restart
```

- `build` 는 이미지 먼저 빌드  
- `up --build` 는 실행 전에 다시 빌드  
- `restart` 는 컨테이너 재시작  



<br><br>

## 🟢 5. 가장 많이 쓰는 흐름  

<br>

### 🟡 기본 흐름  
```bash
docker compose up -d
docker compose ps
docker compose logs
docker compose down
```

- 이 4개만 익혀도 기본 Compose 운영 흐름을 잡을 수 있다.  
