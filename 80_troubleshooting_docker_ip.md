# 🟩 80. Troubleshooting - 컨테이너 IP로 직접 접속 실패  

## 📋 Contents  
1. 문제 상황  
2. 원인 가설  
3. 확인 방법  
4. 해결  
5. 배운 점  



<br><br>

## 🟢 1. 문제 상황  
- nginx 컨테이너를 실행한 뒤 `docker inspect` 로 확인한 컨테이너 IP(`http://localhost:8080`)에 `curl` 로 접속했는데 응답이 오지 않았다.  

<br>

### 🟡 1-1. 예시 상황  
```bash
curl http://localhost:8080  

curl: (28) Failed to connect to http://localhost:8080 port 80  
```



<br><br>

## 🟢 2. 원인 가설  
- macOS + Docker Desktop / OrbStack 환경에서는 Docker가 내부 Linux VM 안에서 동작하므로, 리눅스 호스트처럼 바로 붙는 방식과 다를 수도 있을 것이다.  



<br><br>

## 🟢 3. 확인 방법  
- `docker inspect webserver` 로 컨테이너 내부 IP를 확인했다.  
- `docker ps` 를 확인했을 때 포트 매핑(`PORTS`)이 없었다.  
- 즉, 외부에서 접근 가능한 통로를 따로 열지 않은 상태였다.  



<br><br>

## 🟢 4. 해결  
- 포트 매핑 옵션을 추가해서 다시 실행했다.  

```bash
docker run -d --name webserver -p 8080:80 nginx:latest  
curl http://localhost:8080  
```

- 이후 `localhost:8080` 으로 정상 응답을 확인했다.  



<br><br>

## 🟢 5. 배운 점  
- 컨테이너 IP를 직접 아는 것과, 호스트에서 접속 가능한 것은 다르다.  
- 외부 접속이 필요하면 `-p 호스트포트:컨테이너포트` 로 포트 매핑을 해야 한다.  
- Docker 네트워크 문제를 볼 때는 `docker inspect` 와 `docker ps` 를 함께 확인하는 습관이 중요하다.  
