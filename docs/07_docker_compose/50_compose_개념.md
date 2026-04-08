# 🟩 50. docker compose 개념

## 📋 Contents  
1. Docker Compose가 무엇인가  
2. 왜 Docker Compose를 쓰는가  
3. Docker run 과 Docker Compose 차이  
4. 언제 특히 유용한가  
5. 한 줄 정리  



<br><br>

## 🟢 1. Docker Compose가 무엇인가  
- Docker Compose는 여러 개의 컨테이너 실행 설정을 **하나의 YAML 파일**로 관리하는 도구이다.  
- 예를 들어 웹 서버, 백엔드 서버, DB를 각각 길게 `docker run` 으로 실행하는 대신, `compose.yaml` 파일 하나로 정의할 수 있다.  
- 즉, 컨테이너 실행 명령을 문서처럼 저장해두는 방식이라고 보면 된다.  


<br><br>

## 🟢 2. 왜 Docker Compose를 쓰는가  
- `docker run` 명령은 한두 번은 괜찮지만 길어지면 관리가 어렵다.  
- 포트, 볼륨, 환경 변수, 네트워크 설정이 많아질수록 실수하기 쉽다.  
- Docker Compose를 쓰면 실행 설정을 파일로 남길 수 있어서 **재현성**이 좋아진다.  
- 팀원이 같은 프로젝트를 실행할 때도 같은 파일로 같은 환경을 띄울 수 있다.  


<br>

### 🟡 쉽게 말하면  
```bash
docker run = 한 줄 실행 명령
docker compose = 실행 설정을 파일로 저장한 방식
```



<br><br>

## 🟢 3. Docker run 과 Docker Compose 차이  

| 구분 | docker run | docker compose |
| :--- | :--- | :--- |
| 실행 방식 | 터미널 명령 한 줄 | `compose.yaml` 파일 기반 |
| 관리 대상 | 보통 컨테이너 1개씩 | 여러 서비스 함께 관리 |
| 재현성 | 명령을 따로 기억해야 함 | 파일만 있으면 다시 실행 가능 |
| 가독성 | 길어지면 복잡함 | 구조적으로 보기 쉬움 |
| 협업 | 명령 공유가 번거로움 | 파일 공유로 해결 가능 |



<br><br>

## 🟢 4. 언제 특히 유용한가  
- 웹 서버 + 백엔드 + DB 같이 여러 컨테이너를 같이 띄울 때  
- 개발 환경을 팀원과 동일하게 맞추고 싶을 때  
- 포트, 볼륨, 환경 변수 설정을 반복 실행해야 할 때  
- 과제나 실무에서 실행 설정을 문서처럼 남기고 싶을 때  



<br><br>

## 🟢 5. 한 줄 정리  

<br>

### 🟡 핵심 요약  
```bash
Docker Compose는 여러 컨테이너 실행 설정을 compose.yaml 파일 하나로 관리하는 도구이다.
```
