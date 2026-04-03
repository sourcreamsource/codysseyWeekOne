# 🟩 15. Practice - execute in the nginx container  

## 📋 Contents  
1. nginx 컨테이너 먼저 실행하기  
   1-1. nginx 실행  
   1-2. 왜 이렇게 실행하나?  
2. 실행 중인 컨테이너 확인하기  
   2-1. 실행 확인  
3. 컨테이너 안으로 들어가기  
   3-1. 컨테이너 안으로 들어가기  
   3-2. 참고  
      3-2-1. `run -it`  
      3-2-2. `exec -it`  
   3-3. 쉽게 말하면  
4. `sh`, `bash`, `zsh` 는 뭐냐  
   4-1. 전부 셸(shell)이다  
   4-2. 차이점  
5. 컨테이너 안에서 해볼 것  
   5-1. 현재 위치 확인  
   5-2. 파일 목록 확인  
   5-3. 운영체제 정보 확인  
6. 나가기  
   6-1. 셸 종료  



<br><br>

## 🟢 1. nginx 컨테이너 먼저 실행하기  

### 🟡 1-1. nginx 실행  
> docker run -d --name web1 -p 5678:80 nginx  

```bash
➜  ~ docker run -d --name web1 -p 5678:80 nginx  
    afd7f13b99cc85b420854bc3fc033578774cdfe4fdb566c16c6e5314744129d2  
```
* `docker run`  
  * 새 컨테이너를 만들고 바로 시작한다.  
* `-d`  
  * **detached mode**  
  * 백그라운드 실행  
  * 터미널을 점유하지 않고 뒤에서 계속 실행된다.  
* `--name web1`  
  * 컨테이너 이름을 `web1` 로 지정한다.  
* `-p 5678:80`  
  * Mac의 `5678` 포트를 컨테이너의 `80` 포트와 연결한다.  
* `nginx:latest`  
  * nginx 최신 버전 이미지를 사용한다.  

<br>

### 🟡 1-2. 왜 이렇게 실행하나?  
* nginx는 웹서버다.  
* 웹서버를 켜 두고,  
* 나중에 그 안으로 들어가서 내부를 확인하려는 것이다.  

즉 순서는 이렇다.  

```text
    1) 컨테이너 실행  
    2) 실행 중인지 확인  
    3) 컨테이너 안으로 들어감  
    4) 내부 파일과 OS 확인  
```

<br><br>

## 🟢 2. 실행 중인 컨테이너 확인하기  

### 🟡 2-1. 실행 확인  

> docker ps  

```bash
docker ps  
```

* 현재 **실행 중인 컨테이너 목록**을 보여준다.  
* 여기서 `web1` 이 보여야 정상이다.  
* `PORTS` 에 `5678->80` 이 보이면 포트 매핑도 정상이다.  




<br><br>

## 🟢 3. 컨테이너 안으로 들어가기  

### 🟡 3-1. 컨테이너 안으로 들어가기  

> docker exec -it web1 bash  

* `docker exec`  
  * **이미 실행 중인 컨테이너 안에서 새 명령을 실행**한다.  
* `-i`  
    * **interactive**  
    * 키보드 입력을 받을 수 있게 한다.  
* `-t`  
    * **tty** (tele type writer)  
    * 대화할 수 있게 해주는 입출력 장치 / 즉, 터미널 화면처럼 연결해준다.  
* `web1`  
    * 들어갈 대상 컨테이너 이름  
* `sh`  
    * 컨테이너 안에서 실행할 셸 프로그램  

```bash
docker exec -it web1 bash  
# # 만 뜬다.  접속 완료!  
```

### 3-2. 참고  
#### 3-2-1. `run -it`  

* 새 컨테이너를 만들면서 바로 대화형 진입  

#### 3-2-2. `exec -it`  

* 이미 실행 중인 컨테이너 안에 들어감  





<br>

### 🟡 3-3. 쉽게 말하면  
* `docker run` = 새 집 만들고 켜기  
* `docker exec` = 이미 켜진 집 안에 들어가서 뭔가 하기  




<br><br>

## 🟢 4. `sh`, `bash`, `zsh` 는 뭐냐  

### 🟡 4-1. 전부 셸(shell)이다  
`sh`, `bash`, `zsh` 셸은 **명령어를 입력받아 실행하는 프로그램**이다.  

<br>

### 🟡 4-2. 차이점  
* `sh`  
  * 가장 기본적인 셸  
  * 최소 기능  
  * docker exec -it 컨테이너이름 sh  
* `bash`  
  * `sh` 보다 기능이 더 많다  
  * 리눅스에서 매우 흔함  
  * * docker exec -it 컨테이너이름 bash  
* `zsh`  
  * Mac에서 자주 쓰는 셸  
  * 네가 현재 Mac 터미널에서 쓰는 셸일 가능성이 높음  
  * docker exec -it 컨테이너이름 zsh  




<br><br>

## 🟢 5. 컨테이너 안에서 해볼 것  

### 🟡 5-1. 현재 위치 확인  
```bash
pwd  
```
* 지금 내가 어느 디렉터리에 있는지 보여준다.  


### 🟡 5-2. 파일 목록 확인  

```bash
ls  
```
* 현재 위치에 어떤 파일/폴더가 있는지 보여준다.  


### 🟡 5-3. 운영체제 정보 확인  

```bash
cat /etc/os-release  

    # 정보가 출력됨  
    PRETTY_NAME="Debian GNU/Linux 13 (trixie)"  
    NAME="Debian GNU/Linux"  
    VERSION_ID="13"  
    VERSION="13 (trixie)"  
    VERSION_CODENAME=trixie  
    DEBIAN_VERSION_FULL=13.4  
    ID=debian  
    HOME_URL="https://www.debian.org/"  
    SUPPORT_URL="https://www.debian.org/support"  
    BUG_REPORT_URL="https://bugs.debian.org/"  
```



<br><br>

## 🟢 6. 나가기  

### 🟡 6-1. 셸 종료  

```bash
exit  
```

* 컨테이너 자체가 꺼지는 것이 아니다.  
* 내가 들어가 있던 셸만 종료된다.  
