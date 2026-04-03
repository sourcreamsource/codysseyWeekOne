# 🟩 32. dockerfile practice

## 📋 Contents  
1. practice 목표  
2. 실습 준비  
   2-1. 작업 폴더 만들기  
   2-2. HTML 파일 만들기  
   2-3. HTML 내용 확인하기  
3. Dockerfile 작성  
   3-1. Dockerfile 만들기  
   3-2. Dockerfile 해석  
4. 이미지 빌드  
   4-1. build 실행  
   4-2. build 결과에서 볼 것  
5. 컨테이너 실행  
   5-1. run 실행  
   5-2. 실행 상태 확인  
6. 결과 검증  
   6-1. curl로 확인  
   6-2. 이 실습에서 실제로 연습한 것  
7. 최종 정리  



<br><br>

## 🟢 1. practice 목표  
- 이번 실습의 목표는 **내가 직접 만든 `index.html` 파일을 nginx 이미지에 넣어서 새로운 이미지를 만들고**, 그 이미지로 컨테이너를 실행해 보는 것이다.  
- 즉, 그냥 `nginx` 이미지를 내려받아 쓰는 것이 아니라, **내가 만든 Dockerfile로 내 이미지를 만드는 과정**을 연습하는 practice다.  

<br>

### 🟡 이번 practice 에서 익히는 것  
```bash
1. 작업 폴더 준비
2. HTML 파일 작성
3. Dockerfile 작성
4. docker build
5. docker run
6. docker ps
7. curl 검증
```



<br><br>

## 🟢 2. 실습 준비  

<br>

### 🟡 2-1. 작업 폴더 만들기  
```bash
cd ~  # 나의 홈디렉토리로 가기  
mkdir prac_docker  # docker 실습할 폴더 하나 만들기  
cd prac_docker  
mkdir learn_dockerfile # dockerfile 실습 폴더 만들기  
cd learn_dockerfile  
mkdir my-nginx-app  
cd my-nginx-app  
```

- Dockerfile 실습은 보통 별도 폴더에서 진행한다.  
- 그래야 `index.html`, `Dockerfile` 같은 파일을 한 폴더 안에서 관리하기 쉽다.  


<br>

### 🟡 2-2. HTML 파일 만들기  
```bash
cat > index.html <<'EOF'  
<!DOCTYPE html>
<html>
<head>
    <title>Dockerfile Test</title>
</head>
<body>
    <h1>Hello Dockerfile</h1>
    <p>I'm a codyssey coder </p>
</body>
</html>


EOF  
```

#### 2-2-1. `<<'EOF'`  
Heredoc 시작 선언이다.  

#### 2-2-2. `'EOF'` 의 의미  
EOF에 따옴표를 붙이면 `$변수` 같은 것도 그대로 문자로 저장한다.  


<br>

### 🟡 2-3. HTML 내용 확인하기  
```bash
➜  my-nginx-app cat index.html            
<!DOCTYPE html>
<html>
<head>
    <title>Dockerfile Test</title>
</head>
<body>
    <h1>Hello Dockerfile</h1>
    <p>I'm a codyssey coder </p>
</body>
</html>
```

- 실습에서는 파일을 만든 뒤 바로 확인하는 습관이 중요하다.  
- 이후 `COPY` 할 대상 파일이 맞는지 여기서 먼저 검증한다.  



<br><br>

## 🟢 3. Dockerfile 작성  

<br>

### 🟡 3-1. Dockerfile 만들기  
```bash
cat > Dockerfile <<'EOF'  
FROM nginx:latest  
COPY index.html /usr/share/nginx/html/index.html  

EOF   # 마무리  
```

```bash
# ------------- 실행 기록 ------------------  
➜  my-nginx-app cat > Dockerfile <<'EOF'  
FROM nginx:latest  
COPY index.html /usr/share/nginx/html/index.html  
heredoc> EOF  
➜  my-nginx-app ls  
Dockerfile index.html  
```


<br>

### 🟡 3-2. Dockerfile 해석  
```Dockerfile
FROM nginx:latest
COPY index.html /usr/share/nginx/html/index.html
```

- `FROM nginx:latest`  
  nginx 기본 이미지를 베이스로 사용한다.  
- `COPY index.html /usr/share/nginx/html/index.html`  
  내가 만든 `index.html` 을 nginx 기본 웹 페이지 위치에 복사한다.  

- 즉 이 Dockerfile은 **"기본 nginx 위에 내 html 파일을 덮어쓴 이미지"** 를 만드는 설정이다.  



<br><br>

## 🟢 4. 이미지 빌드  

<br>

### 🟡 4-1. build 실행  
> docker build -t my-nginx:1.0 .

```bash
docker build -t my-nginx:1.0 .  

➜  my-nginx-app docker build -t my-nginx:1.0 .  
[+] Building 0.3s (7/7) FINISHED                                                                   docker:desktop-linux  
 => [internal] load build definition from Dockerfile                                                               0.0s  
 => => transferring dockerfile: 108B                                                                               0.0s  
 => [internal] load metadata for docker.io/library/nginx:latest                                                    0.0s  
 => [internal] load .dockerignore                                                                                  0.0s  
 => => transferring context: 2B                                                                                    0.0s  
 => [internal] load build context                                                                                  0.0s  
 => => transferring context: 198B                                                                                  0.0s  
 => [1/2] FROM docker.io/library/nginx:latest@sha256:7150b3a39203cb5bee612ff4a9d18774f8c7caf6399d6e8985e97e28eb75  0.1s  
 => => resolve docker.io/library/nginx:latest@sha256:7150b3a39203cb5bee612ff4a9d18774f8c7caf6399d6e8985e97e28eb75  0.0s  
 => [2/2] COPY index.html /usr/share/nginx/html/index.html                                                         0.0s  
 => exporting to image                                                                                             0.1s  
 => => exporting layers                                                                                            0.0s  
 => => exporting manifest sha256:a945b239461014939cad38b0eb8e451789cb8859157e310b5f5f4b207989e862                  0.0s  
 => => exporting config sha256:47f6f6b37386151903b1390cb851353d40ab63888c903e3f075925851b7cc312                    0.0s  
 => => exporting attestation manifest sha256:1fb6f6ed20ec63254b0e2bd55fd62edda45805455dfeb72c0768a0813072046e      0.0s  
 => => exporting manifest list sha256:2e94b21de0fcda83deb25c65acc196a9c214d3dac286fb2704b5017867ac9966             0.0s  
 => => naming to docker.io/library/my-nginx:1.0                                                                    0.0s  
 => => unpacking to docker.io/library/my-nginx:1.0                                                                 0.0s  

View build details: docker-desktop://dashboard/build/desktop-linux/desktop-linux/we82mgewd3scgcm78dpsharad  
```


<br>

### 🟡 4-2. build 결과에서 볼 것  
- `-t`  
  이미지 이름과 태그를 붙인다.  
- 마지막의 `.`  
  현재 폴더를 build context 로 사용한다.  
- 로그 안의 `[1/2] FROM ...`, `[2/2] COPY ...`  
  Dockerfile의 각 단계가 실제로 수행되었다는 뜻이다.  

- 즉 이 단계에서는 **내 Dockerfile이 읽혔고, 내 파일이 포함된 이미지가 생성되었는지** 확인하면 된다.  



<br><br>

## 🟢 5. 컨테이너 실행  

<br>

### 🟡 5-1. run 실행  
> docker run -d --name my-nginx-container -p 8080:80 my-nginx:1.0

```bash
docker run -d --name my-nginx-container -p 8080:80 my-nginx:1.0  

# ------ 실행 기록 --------  
➜  my-nginx-app docker images   
IMAGE                ID             DISK USAGE   CONTENT SIZE   EXTRA  
hello-world:latest   452a468a4bf9       22.6kB         10.3kB        
my-nginx:1.0         2e94b21de0fc        255MB         61.3MB        
nginx:latest         7150b3a39203        258MB         64.1MB        

➜  my-nginx-app docker ps -a  
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES  

➜  my-nginx-app docker run -d --name my-nginx-container -p 8080:80 my-nginx:1.0  
47f3fec05c4408f717d5bee713684ae396a2c6f828f036151c30a9c1eb05fd4e  
```


<br>

### 🟡 5-2. 실행 상태 확인  
```bash
➜  my-nginx-app docker ps   
CONTAINER ID   IMAGE          COMMAND                  CREATED          STATUS          PORTS                                     NAMES  
47f3fec05c44   my-nginx:1.0   "/docker-entrypoint.…"   12 seconds ago   Up 12 seconds   0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   my-nginx-container  
```

- `my-nginx:1.0` 이미지로 컨테이너가 실행된 것을 확인한다.  
- `8080->80` 이 보이면 포트 매핑도 정상이다.  



<br><br>

## 🟢 6. 결과 검증  

<br>

### 🟡 6-1. curl로 확인  
> curl http://localhost:8080

```bash
curl http://localhost:8080  

# ------ 실행 기록 --------  
➜  my-nginx-app curl http://localhost:8080  
<!DOCTYPE html>
<html>
<head>
    <title>Dockerfile Test</title>
</head>
<body>
    <h1>Hello Dockerfile</h1>
    <p>I'm a codyssey coder </p>
</body>
</html>
```

- 내가 만든 html 내용이 그대로 나오면 성공이다.  
- 즉, 기본 nginx가 아니라 **내가 커스텀한 이미지가 실행되고 있다는 뜻**이다.  


<br>

### 🟡 6-2. 이 실습에서 실제로 연습한 것  
```bash
1. 파일 만들기
2. Dockerfile 작성
3. 이미지 빌드
4. 컨테이너 실행
5. 포트 연결
6. 웹 응답 검증
```

- 이 흐름이 Dockerfile practice 의 가장 기본형이다.  
- 과제 기준으로도 "Dockerfile 기반 커스텀 이미지 제작"의 최소 흐름을 직접 수행한 셈이다.  



<br><br>

## 🟢 7. 최종 정리  

<br>

### 🟡 7-1. 핵심 요약  
* 남이 만든 `nginx` 이미지를 바탕으로  
* 내 파일 `index.html` 을 덮어놓고  
* 내 이미지 `my-nginx:1.0` 을 새로 만든 것  

```bash
nginx:latest = 재료
Dockerfile   = 조리법
my-nginx:1.0 = 내가 만든 결과물
```


<br>

### 🟡 7-2. 이 practice 가 충분한 이유  
- 단순 개념 설명이 아니라, 실제로 파일을 만들고  
- Dockerfile을 작성하고  
- build 와 run 을 수행하고  
- 마지막에 `curl` 로 검증까지 했다.  

- 따라서 이 문서는 **입문 Dockerfile practice 문서로는 충분한 구성**이다.  
