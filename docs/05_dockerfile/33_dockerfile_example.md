# 🟩 33. Dockerfile 작성 예시

## 📋 Contents  
1. 왜 예시를 많이 봐야 하는가  
2. 정적 웹 서버용 nginx Dockerfile 예시  
3. Python 백엔드용 Dockerfile 예시  
4. Node.js 멀티 스테이지 Dockerfile 예시  
5. 현업에서 자주 보는 포인트 정리  



<br><br>

## 🟢 1. 왜 예시를 많이 봐야 하는가  
- Dockerfile은 문법만 외운다고 바로 잘 써지지 않는다.  
- 실제로는 프로젝트 종류에 따라 구조가 달라지기 때문에, **여러 형태의 예시를 보는 것**이 중요하다.  
- 아래 예시는 실무에서 자주 볼 수 있는 형태를 단순화해서 정리한 것이다.  



<br><br>

## 🟢 2. 정적 웹 서버용 nginx Dockerfile 예시  

<br>

### 🟡 가장 기본적인 실무형 예시  
```Dockerfile
# nginx 공식 이미지를 베이스로 사용
FROM nginx:1.29-alpine

# 이미지에 대한 설명 정보
LABEL maintainer="ooo"
LABEL description="Static web site served by nginx"

# 컨테이너 안에서 작업할 기본 경로
WORKDIR /usr/share/nginx/html

# 로컬의 정적 파일을 nginx 기본 웹 경로로 복사
COPY ./dist/ /usr/share/nginx/html/

# nginx가 사용할 포트를 문서처럼 표시
EXPOSE 80

# 컨테이너가 시작될 때 nginx를 포그라운드로 실행
CMD ["nginx", "-g", "daemon off;"]
```


<br>

### 🟡 한 줄씩 해석  
- `FROM nginx:1.29-alpine`  
  가볍고 빠른 alpine 기반 nginx 이미지를 사용한다.  
- `LABEL`  
  누가 만들었고 어떤 용도인지 메타데이터를 남긴다.  
- `WORKDIR /usr/share/nginx/html`  
  이후 작업 기준 경로를 nginx 기본 html 위치로 맞춘다.  
- `COPY ./dist/ /usr/share/nginx/html/`  
  프론트엔드 빌드 결과물(`dist`)을 웹 루트에 넣는다.  
- `EXPOSE 80`  
  컨테이너가 80 포트를 사용함을 표시한다.  
- `CMD ["nginx", "-g", "daemon off;"]`  
  nginx를 컨테이너 메인 프로세스로 실행한다.  


<br>

### 🟡 언제 이런 Dockerfile을 쓰는가  
```bash
React / Vue / Svelte 빌드 결과물
정적 회사 소개 페이지
이미 빌드된 프론트엔드 결과물 배포
```



<br><br>

## 🟢 3. Python 백엔드용 Dockerfile 예시  

<br>

### 🟡 FastAPI / Flask 계열에서 자주 보는 형태  
```Dockerfile
# Python 런타임 베이스 이미지
FROM python:3.12-slim

# 파이썬이 pyc 파일을 만들지 않도록 설정
ENV PYTHONDONTWRITEBYTECODE=1

# 로그가 바로 보이도록 버퍼링 비활성화
ENV PYTHONUNBUFFERED=1

# 컨테이너 내부 작업 디렉토리
WORKDIR /app

# requirements.txt 먼저 복사
# 의존성 설치 레이어 캐시를 최대한 재사용하기 위한 패턴
COPY requirements.txt .

# 패키지 설치
RUN pip install --no-cache-dir -r requirements.txt

# 나머지 애플리케이션 코드 복사
COPY . .

# 앱이 사용할 포트 표시
EXPOSE 8000

# uvicorn으로 FastAPI 앱 실행
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
```


<br>

### 🟡 한 줄씩 해석  
- `FROM python:3.12-slim`  
  Python 실행 환경을 제공한다.  
- `ENV PYTHONDONTWRITEBYTECODE=1`  
  불필요한 `.pyc` 파일 생성을 줄인다.  
- `ENV PYTHONUNBUFFERED=1`  
  로그가 바로 출력되게 해서 `docker logs` 확인이 쉬워진다.  
- `WORKDIR /app`  
  앱 코드를 둘 기본 위치를 지정한다.  
- `COPY requirements.txt .`  
  의존성 설치에 필요한 파일만 먼저 복사한다.  
- `RUN pip install --no-cache-dir -r requirements.txt`  
  파이썬 패키지를 설치한다.  
- `COPY . .`  
  실제 프로젝트 코드를 이미지 안으로 복사한다.  
- `EXPOSE 8000`  
  백엔드 앱 포트를 표시한다.  
- `CMD [...]`  
  컨테이너 시작 시 FastAPI 서버를 실행한다.  


<br>

### 🟡 왜 `requirements.txt` 를 먼저 복사하는가  
```bash
의존성 파일이 안 바뀌면
pip install 레이어를 캐시 재사용할 수 있기 때문이다.
```

- 실무에서 Dockerfile은 **빌드 속도**도 중요하다.  
- 코드가 자주 바뀌더라도 패키지 설치를 매번 다시 하지 않게 만드는 패턴이다.  



<br><br>

## 🟢 4. Node.js 멀티 스테이지 Dockerfile 예시  

<br>

### 🟡 프론트엔드 / Node 앱에서 자주 보는 실무형 예시  
```Dockerfile
# ---------- 1단계: build stage ----------
# 앱을 빌드하는 전용 단계
FROM node:22-alpine AS builder

# 작업 디렉토리 설정
WORKDIR /app

# package 파일 먼저 복사
COPY package.json package-lock.json ./

# 의존성 설치
RUN npm ci

# 전체 소스 복사
COPY . .

# 프로덕션 빌드 실행
RUN npm run build


# ---------- 2단계: runtime stage ----------
# 실제 서비스용 이미지는 더 가볍게 구성
FROM nginx:1.29-alpine

# 빌드 결과물만 nginx 웹 경로에 복사
COPY --from=builder /app/dist /usr/share/nginx/html

# 사용할 포트 표시
EXPOSE 80

# nginx 실행
CMD ["nginx", "-g", "daemon off;"]
```


<br>

### 🟡 왜 멀티 스테이지를 쓰는가  
- 빌드에 필요한 Node.js 와 패키지들은 최종 배포 이미지에 꼭 남길 필요가 없다.  
- 최종 이미지는 nginx 와 빌드 결과물만 있으면 되므로 더 작고 안전하게 만들 수 있다.  


<br>

### 🟡 핵심 해석  
- 첫 번째 stage `builder`  
  앱을 빌드하는 용도  
- 두 번째 stage `nginx`  
  빌드된 결과물만 복사해서 실제 배포용으로 사용  
- `COPY --from=builder ...`  
  앞 stage에서 만든 결과물만 가져오는 멀티 스테이지 핵심 문법  



<br><br>

## 🟢 5. 현업에서 자주 보는 포인트 정리  

<br>

### 🟡 Dockerfile 읽을 때 먼저 볼 것  
| 확인 포인트 | 왜 중요한가 |
| :--- | :--- |
| `FROM` | 어떤 베이스 이미지에서 시작하는지 알 수 있다 |
| `COPY` / `ADD` | 어떤 파일이 이미지 안으로 들어가는지 알 수 있다 |
| `RUN` | 설치되는 패키지와 빌드 과정이 보인다 |
| `WORKDIR` | 기준 작업 경로를 알 수 있다 |
| `EXPOSE` | 앱이 어떤 포트를 쓰는지 알 수 있다 |
| `CMD` / `ENTRYPOINT` | 컨테이너가 시작될 때 무엇을 실행하는지 알 수 있다 |


<br>

### 🟡 초보자가 특히 기억할 것  
```bash
1. FROM = 시작점
2. COPY = 파일 넣기
3. RUN = 설치 / 빌드
4. WORKDIR = 작업 위치
5. CMD = 실행 명령
```

- 이 5개를 먼저 이해하면 대부분의 Dockerfile을 읽기 시작할 수 있다.  
- 그 다음에 `ENV`, `EXPOSE`, `ENTRYPOINT`, `ARG`, 멀티 스테이지를 확장해서 보면 된다.  
