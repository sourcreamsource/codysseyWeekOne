# 🟩 Dockerfile 명령어 정리  

## 📋 Contents  
1. Dockerfile 작성에 사용하는 핵심 명령들  
   1-1. `FROM`  
   1-2. `COPY`  
   1-3. `ADD`  
   1-4. `RUN`  
   1-5. `WORKDIR`  
   1-6. `CMD`  
   1-7. `ENTRYPOINT`  
   1-8. `EXPOSE`  
   1-9. `ENV`  
   1-10. `ARG`  
   1-11. `USER`  
   1-12. `VOLUME`  
   1-13. `LABEL`  
   1-14. `.dockerignore`  
2. dockerfile을 사용하는 명령어 정리  
   2-1. 이미지 빌드  
   2-2. 이미지 목록을 확인하여 잘 빌드 되었는지 보기  
   2-3. 컨테이너 실행  
   2-4. 컨테이너 실행 여부 확인  
   2-5. 접속 확인  
3. Dockerfile 작성 전에 꼭 알고 갈 것  
   3-1. build 와 run 은 다르다  
   3-2. build context  
   3-3. `COPY` / `.dockerignore` 관계  
   3-4. `EXPOSE` 와 `-p` 차이  
   3-5. `CMD` 와 `ENTRYPOINT` 차이  


<br><br>

## 🟢 1. Dockerfile 작성에 사용하는 핵심 명령들  

<br>

| 명령 | 역할 | 언제 쓰는가 | 예시 |  
| :--- | :--- | :--- | :--- |
| `FROM` | 베이스 이미지 지정 | 이미지 시작점 정할 때 | `FROM nginx:latest` |  
| `COPY` | 파일 복사 | 내 파일을 이미지 안에 넣을 때 | `COPY index.html /usr/share/nginx/html/index.html` |  
| `ADD` | 파일 추가 | 압축 해제 등 추가 기능이 필요할 때 | `ADD app.tar.gz /app` |  
| `RUN` | 빌드 중 명령 실행 | 패키지 설치, 폴더 생성, 권한 변경 시 | `RUN apt-get update && apt-get install -y curl` |  
| `WORKDIR` | 작업 기본 경로 지정 | 이후 명령의 기준 폴더 정할 때 | `WORKDIR /app` |  
| `CMD` | 기본 실행 명령 | 컨테이너 시작 시 기본 명령 지정 | `CMD ["python", "app.py"]` |  
| `ENTRYPOINT` | 고정 실행 명령 | 항상 같은 메인 명령으로 실행할 때 | `ENTRYPOINT ["python"]` |  
| `EXPOSE` | 사용할 포트 표시 | 컨테이너 포트를 문서처럼 남길 때 | `EXPOSE 80` |  
| `ENV` | 환경 변수 설정 | 앱 모드, 경로, 설정값 지정할 때 | `ENV APP_ENV=production` |  
| `ARG` | 빌드용 변수 | 빌드 시점 값만 받고 싶을 때 | `ARG APP_VERSION=1.0` |  
| `USER` | 실행 사용자 지정 | root 대신 일반 사용자로 실행할 때 | `USER nginx` |  
| `VOLUME` | 데이터 저장 경로 표시 | 영속 데이터 경로를 지정할 때 | `VOLUME ["/data"]` |  
| `LABEL` | 메타데이터 기록 | 작성자, 버전, 설명을 남길 때 | `LABEL maintainer="ooo"` |  
| `.dockerignore` | 불필요한 파일 제외 | build context를 줄이고 싶을 때 | `.git`, `node_modules`, `.env` |  




<br><br>

## 🟢 2. dockerfile을 사용하는 명령어 정리  

<br>

| 명령 | 역할 | 예시 |  
| :--- | :--- | :--- |
| `docker build` | Dockerfile로 이미지 만들기 | `docker build -t my-nginx:1.0 .` |  
| `docker images` | 이미지 목록 확인 | `docker images` |  
| `docker run` | 이미지로 컨테이너 실행 | `docker run -d --name my-nginx -p 8080:80 my-nginx:1.0` |  
| `docker ps` | 실행 중인 컨테이너 확인 | `docker ps` |  
| `curl` | 웹 응답 확인 | `curl http://localhost:8080` |  



<br><br>


## 🟢 3. Dockerfile 작성 전에 꼭 알고 갈 것  


| 구분 | 꼭 알아야 할 점 | 짧은 설명 |  
| :--- | :--- | :--- |
| `build 와 run 은 다르다` | `build` 는 이미지 생성, `run` 은 컨테이너 실행 | Dockerfile은 `build` 할 때 사용하고, 서비스는 `run` 으로 띄운다. |  
| `build context` | 마지막 `.` 이 재료 범위다 | `docker build -t my-nginx .` 에서 `.` 폴더 안 파일만 `COPY` 가능하다. |  
| `COPY / .dockerignore 관계` | 복사 가능한 파일과 제외 파일을 같이 생각해야 한다 | `.dockerignore` 에 넣은 파일은 build context에서 빠져서 `COPY` 대상에서도 제외된다. |  
| `EXPOSE 와 -p 차이` | `EXPOSE` 만으로는 외부 접속이 안 된다 | `EXPOSE 80` 은 문서 표시이고, 실제 연결은 `docker run -p 8080:80` 으로 한다. |  
| `CMD 와 ENTRYPOINT 차이` | 둘 다 실행 명령이지만 역할이 다르다 | `ENTRYPOINT` 는 고정 실행, `CMD` 는 기본 인자 또는 기본 명령으로 자주 쓴다. |  
