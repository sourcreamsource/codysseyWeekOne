# 🟩 10. docker setting  

## 📋 Contents  
1. Docker가 무엇인가  
2. Docker 설치하기  
3. Docker Engine Start  
4. Docker 작동 확인  


<br><br>


## 🟢 Docker가 무엇인가?  
- Docker는 애플리케이션을 **컨테이너(container)** 라는 독립된 실행 환경에서 실행할 수 있게 해주는 도구이다.  
- 프로그램을 실행하는 데 필요한 환경을 하나로 묶어서, 다른 컴퓨터에서도 비슷한 방식으로 실행할 수 있게 도와준다.  
- 그래서 "내 컴퓨터에서는 되는데 다른 컴퓨터에서는 안 된다" 같은 문제를 줄일 수 있다.  
- Docker에서는 **이미지(image)** 로 실행 환경을 만들고, 그 이미지로 **컨테이너(container)** 를 실행한다.  
- 이번 과제에서는 Docker를 이용해 웹 서버를 실행하고, 포트 연결과 데이터 저장 방식도 함께 확인하게 된다.  



<br><br>

## 🟢 Docker 설치하기  
- OrbStack을 사용할 경우  
- mac package manager Homebrew를 통해서 설치 가능  
    - brew install orbstack  

<br><br>

## 🟢 Docker Engine Start  
- GUI를 사용하지 않고, CLI만을 통해 접근 및 제어할 것입니다.  

#### orbstack 일 경우.  
```bash 
docker desktop start  
docker desktop stop  
docker desktop status  
```

#### Docker Desktop 일 경우.  
```bash
orb start  
orb stop  
orb status  
``` 

#### colima 일 경우.  
```bash
colima list  
colima start  
colima stop  
colima status  
colima restart  
```

<br><br>

## 🟢 Docker 작동 확인  
#### docker version check    
```bash
docker version  
# Version:          29.3.1  
```

#### 전체 설치 상태, 컨테이너/이미지 수, 엔진 정보 등 확인  
```bash
docker system info  
# Client:  
    # Version:    29.3.1  
    # Context:    desktop-linux  
    # Debug Mode: false  
    # Plugins:  
    # ~~~~~~  
```
