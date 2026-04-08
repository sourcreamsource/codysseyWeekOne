# 🟩 45. docker bind mount  

## 📋 Contents  
1. bind mount 개념  
   1-1. bind mount 사용하는 이유  
2. 실행 방법  
   2-1. host 폴더를 nginx container 에 연결하기  
   2-2. 변경 사항 확인하기  
3. volume 과 차이  
   3-1. 간단 비교  



<br><br>

## 🟢 1. docker bind mount 개념  
- bind mount는 host의 실제 폴더를 container 내부 경로에 직접 연결하는 방식이다.  
- host 파일을 수정하면 container 안에서도 바로 반영되는 것이 가장 큰 특징이다.  
- 과제에서는 "호스트 변경 전/후 비교"를 보여주기 좋기 때문에 사용한다.  

<br>

### 🟡 1-1. bind mount 를 사용하는 이유  
```bash
host 폴더와 container 폴더를 바로 연결해서  
변경 사항을 즉시 반영하기 위해 사용한다.  
```



<br><br>

## 🟢 2. docker bind mount 실행 방법  

<br>

### 🟡 2-1. host 폴더를 nginx container 에 연결하기  
```bash
mkdir -p ~/bindmount-test  
echo "<h1>bind mount test</h1>" > ~/bindmount-test/index.html  

docker run -d --name bind-nginx \  
  -p 8080:80 \  
  -v ~/bindmount-test:/usr/share/nginx/html \  
  nginx  
```

- `~/bindmount-test` 는 host의 실제 폴더이다.  
- `/usr/share/nginx/html` 는 nginx가 웹 페이지를 읽는 container 내부 경로이다.  
- 이 둘을 연결하면 host의 `index.html` 내용이 바로 웹 페이지로 보인다.  


<br>

### 🟡 2-2. 변경 사항 확인하기  
```bash
echo "<h1>changed by bind mount</h1>" > ~/bindmount-test/index.html  
```

- 파일을 다시 수정한 뒤 `http://localhost:8080` 에 접속하면 바뀐 내용이 반영된다.  
- 이것이 bind mount의 핵심 확인 방법이다.  



<br><br>

## 🟢 3. volume 과 차이  

<br>

### 🟡 3-1. 간단 비교  
```bash
bind mount = host 폴더 직접 연결  
volume     = Docker가 관리하는 저장 공간  
```

- bind mount는 개발 중 빠른 수정 확인에 편하다.  
- volume은 데이터 영속성을 설명할 때 더 적합하다.  
