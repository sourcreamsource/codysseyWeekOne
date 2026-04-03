# 🟩 44. docker volume  

## 📋 Contents  
1. docker volume 개념 이해  
   1-1. volume 이 필요한 이유  
   1-2. volume 관련 기본 명령어  
2. docker volume 실습 과정 정리 (docker volume 영속성 확인)  
   2-1. volume 생성하기  
   2-2. volume 상세 정보 확인하기  
   2-3. container 에 volume 연결하기  
   2-4. volume 데이터가 유지되는지 확인하기  
   2-5. bind mount 와 volume 차이 정리  



<br><br>

## 🟢 1. docker volume 개념 이해  
- Docker volume은 컨테이너가 삭제되어도 데이터를 유지하기 위해 사용하는 Docker 전용 저장소이다.  
- 컨테이너 내부 파일은 기본적으로 writable layer에 저장되는데, 컨테이너를 삭제하면 함께 사라질 수 있다.  
- volume은 이 문제를 해결하기 위해 컨테이너 바깥에 데이터를 안전하게 분리해서 저장한다.  
- 주로 DB 데이터, 업로드 파일, 로그, 설정 파일처럼 지속적으로 유지해야 하는 데이터를 저장할 때 사용한다.  

<br>

### 🟡 1-1. docker volume 이 필요한 이유  
- 컨테이너는 쉽게 생성하고 삭제할 수 있지만, 데이터까지 같이 지워지면 서비스 운영이 어려워진다.  
- image는 불변이고 container는 일시적이기 때문에 데이터는 외부 저장소에 분리하는 것이 안전하다.  
- 같은 volume을 다른 컨테이너에 다시 연결하면 이전 데이터를 그대로 사용할 수 있다.  
- host의 실제 저장 경로를 직접 관리하지 않아도 Docker가 volume 위치를 관리해준다.  



<br>

### 🟡 1-2. volume 관련 기본 명령어  
```bash
docker volume create my-volume  
docker volume ls  
docker volume inspect my-volume  
docker volume rm my-volume  
```




<br><br>

## 🟢 2. docker volume 실습 과정 정리 (docker volume 영속성 확인)  

<br>

### 🟡 2-1. volume 생성하기  
```bash
docker volume create my-nginx-volume  

➜  my-nginx-app git:(main) ✗ docker volume create my-nginx-volume  
    my-nginx-volume  
```
- `docker volume create my-nginx-volume` : `my-nginx-volume` 이라는 이름의 volume을 만든다.  


```bash
docker volume ls  

➜  my-nginx-app git:(main) ✗ docker volume ls          
DRIVER    VOLUME NAME  
local     my-nginx-volume  
```
- `docker volume ls` : 현재 Docker에 생성된 volume 목록을 확인한다.  


<br>

### 🟡 2-2. volume 상세 정보 확인하기  
```bash
docker volume inspect my-nginx-volume  

➜  my-nginx-app git:(main) ✗ docker volume inspect my-nginx-volume  
[  
    {  
        "CreatedAt": "2026-04-02T16:24:13Z",  
        "Driver": "local",  
        "Labels": null,  
        "Mountpoint": "~~~~/docker/volumes/my-nginx-volume/_data",  
        "Name": "my-nginx-volume",  
        "Options": null,  
        "Scope": "local"  
    }  
]  
```

- volume의 이름, 드라이버, 마운트 위치 등 상세 정보를 확인할 수 있다.  
- `Mountpoint` 항목을 통해 Docker가 실제로 데이터를 저장하는 host 경로를 볼 수 있다.  


<br>

### 🟡 2-3. 🔥 나의 nginx container 에 volume 연결하기  
```bash
# 내가 만든 my-nginx-volume 사용  
docker run -d --name my-nginx -p 8080:80 -v my-nginx-volume:/usr/share/nginx/html nginx:latest  

# docker 컨테이너 실행  
➜  my-nginx-app git:(main) ✗ docker run -d --name my-nginx -p 8080:80 -v my-nginx-volume:/usr/share/nginx/html nginx:latest  
862f61c2052ba30da26f6fce409507c7d0718e91106b8e1922616089444fb94d  

# 실행중인 docker container 확인  
➜  my-nginx-app git:(main) ✗ docker ps  
CONTAINER ID   IMAGE          COMMAND                  CREATED          STATUS          PORTS                                     NAMES  
862f61c2052b   nginx:latest   "/docker-entrypoint.…"   15 seconds ago   Up 14 seconds   0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   my-nginx  

```

- `-v my-nginx-volume:/usr/share/nginx/html`는 Docker volume을 컨테이너 내부의 `/usr/share/nginx/html` 경로에 연결한다.  

- 즉, `:/usr/share/nginx/html`는 `my-nginx`컨테이너 안에서 웹 파일을 읽는 위치  

- 이제 해당 경로에 저장되는 파일은 `컨테이너`가 아니라 `volume`에 기록된다!!!  


<br>

### 🟡 2-4. volume 데이터가 유지되는지 확인하기  
```bash
# my-nginx 해당 컨테이너 내부로 들어가기  
➜  ~ docker exec -it my-nginx /bin/bash  

# 파일 내용 수정하기  
➜  ~ echo "<h1>docker volume test</h1>" > /usr/share/nginx/html/index.html  

# 잘 수정되었는지 확인하기  
➜  ~ cat /usr/share/nginx/html/index.html  
    # 잘 수정된 것을 확인  
    <h1>docker volume test</h1>

# my-nginx 컨테이너 내부에서 빠져나가기  
exit  

# my-nginx 실행 중 강제 삭제  
docker rm -f my-nginx  
➜  ~ docker rm -f my-nginx  
my-nginx  

# 컨테이너 없음 확인  
➜  ~ docker ps -a  
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES  

# 다시 nginx2 컨테이너 생성  
➜  ~ docker run -d --name my-nginx-2 -p 9900:80 -v my-nginx-volume:/usr/share/nginx/html nginx  

# curl로 요청 및 응답을 통해 변경했던 응답을 그대로 가지고 오는지 확인  
➜  ~ curl http://localhost:9900  
    # 변경했던 내용을 가져오는 것을 확인  
    <h1>docker volume test</h1>
```

- 첫 번째 컨테이너에서 `index.html` 을 수정한다.  
- 이후 컨테이너를 삭제하고 같은 volume을 연결한 새 컨테이너를 실행한다.  
- 브라우저에서 `localhost:9900` 으로 접속했을 때 수정한 내용이 그대로 보이면 volume이 정상적으로 데이터 보존 역할을 한 것이다.  

  
<br>

### 🟡 2-5. bind mount 와 volume 차이 정리  
* -v 를 호스트(Mac) 경로를 다이렉트로 넣게 되면 `bind mount`가 됩니다.  
    * `-v ~/my_docker/html:`  
```bash
docker run -d --name bind-nginx -p 8081:80 -v ~/my_docker/html:/usr/share/nginx/html nginx  
```

- `volume` 은 Docker가 관리하는 저장소를 사용하는 방식이다.  
- `bind mount` 는 host의 특정 디렉터리를 컨테이너에 직접 연결하는 방식이다.  
- 개발 중에는 bind mount가 편리하고, 운영 환경에서는 volume이 더 안정적이고 관리하기 쉽다.  
- bind mount는 host 경로 구조에 의존하지만, volume은 Docker 내부 관리 방식이라 이식성이 더 좋다.  
