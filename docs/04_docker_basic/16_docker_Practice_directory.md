# 🟩 16. nginx 컨테이너 내부 파일 다루기  

## 📋 Contents  
1. 컨테이너 안으로 들어가기  
2. 기본 이동 / 확인  
   2-1. 목록 정리  
3. 폴더 생성 / 삭제   
   3-1. 목록 정리  
4. 파일 생성 / 복사 / 이동 / 삭제  
   4-1. 목록 정리  
5. 파일 내용 확인  
   5-1. 목록 정리  
6. 파일 내용 쓰기  
   6-1. 목록 정리  
   6-2. 편집기  
7. 마무리 정리  
   7-1. Mac 터미널에서 하는 것  
   7-2. 컨테이너 안에서 하는 것  



<br><br>

## 🟢 1. 컨테이너 안으로 들어가기  
> docker exec -it web1 bash  

```bash
docker exec -it web1 bash  
```
* `docker exec`  
    * 실행 중인 컨테이너 안에서 새 명령 실행  
* `-i`  
    * **interactive**  
    * 입력 가능  
* `-t`  
    * **tty**  
    * 터미널처럼 연결  
* `bash`  
    * 셸 프로그램  



<br><br>

## 🟢 2. 기본 이동 / 확인  

### 🟡 2-1. 목록 정리  
* 현재 디렉토리 출력  
    * `pwd` = **print working directory**  

* 파일 목록 보기  
    * `ls` = **list**  
    * `ls -al`  
    * 예시 명령어:  
        ```bash
        ls  
        ls -al  
        ls /usr/share/nginx/html  
        ```

* 경로 변경  
    * `cd` = **change directory**  
    * 예시 명령어:  
        ```bash
            # 특정 디렉토리로 들어가기  
        cd /usr/share/nginx/html  

            # 상위 디렉토리로 돌아가기  
        cd ..  

            # 사용자 계정의 홈 디렉토리  
        cd ~    # pwd = /root: root라는 이름을 가진 사용자의 집(홈 디렉토리).  

            # 최상위 루트 디렉토리  
        cd /    # pwd = / : 모든 디렉토리의 시작점인 최상위 지점.  
        ```

<br><br>

## 🟢 3. 폴더 생성 / 삭제  

### 🟡 3-1. 목록 정리  
* 폴더 만들기  
    * `mkdir` = **make directory**  
    * 예시 명령어:  
        ```bash
        mkdir test  
        mkdir -p a/b/c  
            # -p 옵션 (Parents)  
            #  a/b/c  : 생성하고자 하는 디렉토리의 계층 구조를 의미함  
                # a폴더 안에 b폴더 생성하고, b폴더 안에 c폴더를 생성한다.  
        ```
    * 실제 실행 명령어  
        ```bash
        root@afd7f13b99cc:/# mkdir test  
        root@afd7f13b99cc:/# cd test/  
        root@afd7f13b99cc:/test#   
        ```
* **빈 폴더** 삭제  
    * `rmdir` = **remove directory**  
    * 예시 명령어:  
        ```bash
        rmdir test  
        ```
    * 실제 실행 명령어  
        ```bash
        root@afd7f13b99cc:/# rmdir test  
        root@afd7f13b99cc:/# ls  
            bin   dev		   docker-entrypoint.sh  home  media  opt   root  sbin	sys   tmp  var  
            boot  docker-entrypoint.d  etc			 lib   mnt    proc  run   srv  usr  
        ```

* **폴더째** 삭제  
    * `rm -r`  
    * 예시 명령어:  
        ```bash
            # r (Recursive)  
        rm -r test  
            # r (Recursive) / -f (Force)  
        rm -rf test  
        ```
    * 실제 실행 명령어  
        ```bash
        root@afd7f13b99cc:/# rm -r test  
        root@afd7f13b99cc:/# ls  
            bin   dev		   docker-entrypoint.sh  home  media  opt   root  sbin	sys  usr  
            boot  docker-entrypoint.d  etc			 lib   mnt    proc  run   srv	tmp  var  
        ```



<br><br>

## 🟢 4. 파일 생성 / 복사 / 이동 / 삭제  

### 🟡 4-1. 목록 정리  
* 빈 파일 만들기  
    * `touch`  
    * 예시 명령어:  
        ```bash
        touch test.html  
        touch test2.txt  
        ```
    * 실제 실행 명령어  
        ```bash
        root@afd7f13b99cc:/# mkdir test  
        root@afd7f13b99cc:/# cd test/  
        root@afd7f13b99cc:/test# touch test.html  
        root@afd7f13b99cc:/test# touch test2.txt  
        root@afd7f13b99cc:/test# ls  
        test.html  test2.txt  

        root@afd7f13b99cc:/test# rmdir test  
            # 해당 test폴더가 빈 폴더가 아니라서 삭제 불가  
        rmdir: failed to remove 'test': Directory not empty  
        ```

* 파일 복사  
    * `cp` = **copy**  
    * 예시 명령어:  
        ```bash
        cp index.html index.backup.html  
        cp test.html /tmp/test_cp.html  
        ```
    * 실제 실행 명령어  
        ```bash        
        root@afd7f13b99cc:/test# cp test.html /tmp/test_cp.html  
        root@afd7f13b99cc:/test# ls  
        test.html  test2.txt  
        root@afd7f13b99cc:/test# cd /  
        root@afd7f13b99cc:/# cd tmp  
        root@afd7f13b99cc:/tmp# ls  
        test_cp.html  
        ```

* 파일 이동 / 이름 변경  
    * `mv` = **move**  
    * 예시 명령어:  
        ```bash
        mv index.backup.html backup.html  
        mv test_cp.html /test/test_cp.html  
        ```
    * 실제 실행 명령어  
        ```bash
        root@afd7f13b99cc:/tmp# mv test_cp.html /test/test_cp.html  
        root@afd7f13b99cc:/tmp# ls  
        root@afd7f13b99cc:/tmp# cd /  
        root@afd7f13b99cc:/# cd test/  
        root@afd7f13b99cc:/test# ls  
        test.html  test2.txt  test_cp.html  
        ```

* 파일 삭제  
    * `rm` = **remove**  
    * 예시 명령어:  
        ```bash
        rm test_cp.html  
        rm -f test_cp.html  
        ```
    * 실제 실행 명령어  
        ```bash
        root@afd7f13b99cc:/test# rm -f test_cp.html  
        root@afd7f13b99cc:/test# ls  
        test.html  test2.txt  
        ```


<br><br>

## 🟢 5. 파일 내용 확인  

### 🟡 5-1. 목록 정리  

* 파일 내용 전체 보기  
    * `cat` = **concatenate**  
    * 예시 명령어:  
        ```bash
        cat index.html  
            # 특정 경로까지 들어가서 그 파일을 읽는다.  
        cat /etc/os-release  
        ```
    * 실제 실행 명령어  
        ```bash
        oot@afd7f13b99cc:/test# cd /  
        root@afd7f13b99cc:/# cd usr  
        
        root@afd7f13b99cc:/usr# ls  
        bin  games  include  lib  libexec  local  sbin	share  src  
        
        root@afd7f13b99cc:/usr# cd share/  
        
        root@afd7f13b99cc:/usr/share# ls  
        X11		 bug		  dict	    fontconfig	java	     locale	 nginx	      polkit-1	  zoneinfo  
        apt		 ca-certificates  doc	    fonts	keyrings     man	 pam	      tabset	  zsh  
        base-files	 common-licenses  doc-base  gcc		libc-bin     maven-repo  pam-configs  terminfo  
        base-passwd	 debconf	  dpkg	    gdb		libgcrypt20  menu	 perl5	      util-linux  
        bash-completion  debianutils	  fish	    info	lintian      misc	 pixmaps      xml  
        
        root@afd7f13b99cc:/usr/share# cd nginx/  
        root@afd7f13b99cc:/usr/share/nginx# cd html/  
        root@afd7f13b99cc:/usr/share/nginx/html# ls  
        50x.html  index.html  
        
        root@afd7f13b99cc:/usr/share/nginx/html# cp index.html /test/index_cp.html  
        
        root@afd7f13b99cc:/usr/share/nginx/html# cd /  
        
        root@afd7f13b99cc:/# cd test  
        
        root@afd7f13b99cc:/test# ls  
        index_cp.html  test.html  test2.txt  
        
        root@afd7f13b99cc:/test# cat index_cp.html  
        <!DOCTYPE html>
        <html>
        <head>
        <title>Welcome to nginx!</title>
        <style>
        html { color-scheme: light dark; }  
        body { width: 35em; margin: 0 auto;  
        font-family: Tahoma, Verdana, Arial, sans-serif; }  
        </style>
        </head>
        <body>
        <h1>Welcome to nginx!</h1>
        <p>If you see this page, nginx is successfully installed and working.  
        Further configuration is required for the web server, reverse proxy,  
        API gateway, load balancer, content cache, or other features.</p>  

        <p>For online documentation and support please refer to  
        <a href="https://nginx.org/">nginx.org</a>.<br/>
        To engage with the community please visit  
        <a href="https://community.nginx.org/">community.nginx.org</a>.<br/>
        For enterprise grade support, professional services, additional  
        security features and capabilities please refer to  
        <a href="https://f5.com/nginx">f5.com/nginx</a>.</p>

        <p><em>Thank you for using nginx.</em></p>
        </body>
        </html>
        ```

* 파일 앞부분 보기  
    * `head`  
    * 예시 명령어:  
        ```bash
            # 옵션 없이 처음부터 기본 10줄 출력  
        head index_cp.html  
            # 앞에서 3줄만 보기  
        head -n 3 index_cp.html  
        ```
    * 실제 실행 명령어  
        ```bash
        root@afd7f13b99cc:/test# head -n 3 index_cp.html  
        <!DOCTYPE html>
        <html>
        <head>
        ```

* 파일 뒷부분 보기  
    * `tail`  
    * 예시 명령어:  
        ```bash
            # 옵션 없이 처음부터 기본 10줄 출력  
        tail index_cp.html  
            # 뒤에서 3줄만 보기  
        tail -n 3 index_cp.html  
        ```
    * 실제 실행 명령어  
        ```bash
        root@afd7f13b99cc:/test# tail -n 3 index_cp.html  
        <p><em>Thank you for using nginx.</em></p>
        </body>
        </html>
        ```

        

<br><br>

## 🟢 6. 파일 내용 쓰기  

### 🟡 6-1. 목록 정리  
* 파일에 내용 넣기  
    * `echo`  
    * 🔥 `>` = 덮어쓰기  
    * 🔥 `>>` = 이어쓰기  
    * 예시 명령어:  
        ```bash
        echo '<h1>no data</h1>' > hello_cp.html  
        echo '<p>World</p>' >> hello_cp.html  
        ```
    * 실제 실행 명령어  
        ```bash
        root@afd7f13b99cc:/test# echo '<h1>no data</h1>' > hello_cp.html  
        root@afd7f13b99cc:/test# cat hello_cp.html  
        <h1>no data</h1>
        root@afd7f13b99cc:/test# echo '<p>World</p>' >> hello_cp.html  
        root@afd7f13b99cc:/test# cat hello_cp.html  
        <h1>no data</h1>
        <p>World</p>
        ```

### 🟡 6-2. 편집기  
* nano  
    - 쉬운 텍스트 편집기  
    - 예시 명령어:  
        ```bash
        nano index_cp.html  
        nano /usr/share/nginx/html/index.html        
        ```

* vim  
    - 강력한 텍스트 편집기  
    - 명령어:  
        ```bash
        vim index_cp.html  
        vim /usr/share/nginx/html/index.html        
        ```




<br><br><br><br>

---

## 🟢 7. 마무리 정리  
컨테이너 접속: `docker exec -it web1 bash`  
현재 위치: `pwd`  
목록 보기: `ls`, `ls -al`, `ls /usr/share/nginx/html`  
경로 이동: `cd ..`, `cd ~`, `cd /`, `cd /usr/share/nginx/html`  
폴더 생성: `mkdir`, `mkdir -p`  
빈 폴더 삭제: `rmdir`  
폴더째 삭제: `rm -r`, `rm -rf`  
빈 파일 생성: `touch`  
파일 복사: `cp`  
파일 이동/이름변경: `mv`  
파일 삭제: `rm`, `rm -f`  
파일 전체 보기: `cat`  
파일 앞부분: `head`, `head -n`  
파일 뒷부분: `tail`, `tail -n`  
덮어쓰기: `echo >`  
이어쓰기: `echo >>`  
쉬운 편집기: `nano`  
강력한 편집기: `vim`  


### 🟡 7-1. Mac 터미널에서 하는 것  

```bash
docker run -d --name web1 -p 5678:80 nginx:latest  
docker exec -it web1 bash  
docker cp ./index.html web1:/usr/share/nginx/html/index.html  
```

### 🟡 7-2. 컨테이너 안에서 하는 것  

```bash
pwd  
ls -al  
cd /usr/share/nginx/html  
cat index.html  
cp index.html backup.html  
mv backup.html old.html  
mkdir test  
rm -r test  
```



