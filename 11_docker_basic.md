# 🟩 11. docker basic  

## 📋 Contents  
1. Docker CLI Commands Flow  
   1-1. docker cli image 관리  
   1-2. docker cli container 관리  
   1-3. foreground execute  


## 🟢 1. Docker CLI Commands Flow  

### 🟡 1-1. [docker cli] Management the **`IMAGE`**  
- Docker Image Search  
    - docker search <image 명칭>  

- Docker community image repository pull  
    - docker pull <image 명칭>  
        ```bash
            docker pull hello-world  
                Using default tag: latest  
                latest: Pulling from library/hello-world  
                58dee6a49ef1: Pull complete  
                c3bdf82c34d1: Download complete  
                Digest: sha256:4 ~ ~ ~  
                Status: Downloaded newer image for hello-world:latest  
                docker.io/library/hello-world:latest  

            docker pull nginx  
                Using default tag: latest  
                latest: Pulling from library/nginx  
                5716fd893132: Pull complete  
                f4badedbec24: Pull complete  
                0d9786b44f2a: Pull complete  
                e14d31524fc8: Pull complete  
                881ff00f5a79: Pull complete  
                0a43e039475a: Pull complete  
                b1a5e9a47bf9: Pull complete  
                068f82ab28b2: Download complete  
                6be5707a6ba5: Download complete  
                Digest: sha256:715 ~ ~ ~  
                Status: Downloaded newer image for nginx:latest  
                docker.io/library/nginx:latest  
        ```


- Image list  
    - docker images  
    - docker image ls  (ls = list)  
        ```bash
            docker images  
                IMAGE                ID       DISK USAGE   CONTENT SIZE   EXTRA  
                hello-world:latest   452a468   22.6kB      10.3kB        
                nginx:latest         7150b3a   258MB       64.1MB      
        ```

- Remove the image  
    - docker rmi <image 명칭>  
        ```bash
            docker rmi hello-world  
                Untagged: hello-world:latest  
                Deleted: sha256:452a468a4bf9  
            
            docker images         
                IMAGE          ID         DISK USAGE   CONTENT SIZE   EXTRA  
                nginx:latest   7150b3a     258MB         64.1MB   
            # hello-world는 지워지고 nginx만 남은 것을 확인.  
        ```

- Create container  
    - docker create <image 명칭>  
        ```bash
            docker create nginx  
                a9f2f17e5bfe25d45a8d01f3895~~~~~~~~~~  
            # 컨데이너가 만들어진 뒤, 컨데이너 ID 값이 출력됨.  
        ```
    - 🔥 docker create --name <정해줄 이름> <image 명칭>  
        - docker create --help 를 통해 options를 확인할 수 있으며 그 중에 container 이름을 정해줄 수 있는 --name에 대해서 살펴보겠습니다.  
            - 이렇게 --name을 주어 create하게 되면 container 관리가 편해진다.  
        ```bash
            docker pull hello-world  
                Using default tag: latest  
                latest: Pulling from library/hello-world  
                58dee6a49ef1: Pull complete  
                c3bdf82c34d1: Download complete  
                Digest: sha256:4 ~ ~ ~  
                Status: Downloaded newer image for hello-world:latest  
                docker.io/library/hello-world:latest  

            docker create --name basic hello-world  
                9502cf9fe0e8470ad3fbcb84~~~~~~~~~~  
            # 컨데이너가 만들어진 뒤, 컨데이너 ID 값이 출력됨.  

            docker ps -a    # (컨테이너 전체를 확인하는 명령어로 믿에서 배울 예정.)  
                # NAMES 가 basic인 것을 확인  
                CONTAINER ID   IMAGE         COMMAND    CREATED          STATUS    PORTS     NAMES  
                9502cf9fe0e8   hello-world   "/hello"   12 seconds ago   Created             basic  
        ```

### 🟡 1-2. [docker cli] Management the **`CONTAINER`**  
- Watch the **Runing Container List**  
    - (참고: docker container ls)  
    - docker ps  (ps = process status)  
      
- Watch the **All Container List**  
    - (참고: docker container ls -a)  
    - docker ps -a   (-all)  
        ```bash
            # 생성된 모든 컨테이너 확인  
            docker ps -a  
                CONTAINER ID   IMAGE     COMMAND                  CREATED              STATUS    PORTS     NAMES  
                a9f2f17e5bfe   nginx     "/docker-entrypoint.…"   About a minute ago   Created             priceless_black  
        ```

- Control the container  
    - docker start <CONTRAINER ID or NAMES>  :  해당 컨테이너 시작  
        ```bash
            # 특정 CONTRAINER ID or NAMES를 가진 컨테이너 시작  
            docker start a9f2f17e5bfe  
                a9f2f17e5bfe  

            # 현재 실행중인 컨테이너 확인  
            docker ps  
            # nginx 컨테이너 실행준인 것을 확인  
                CONTAINER ID   IMAGE     COMMAND                  CREATED        STATUS          PORTS     NAMES  
                a9f2f17e5bfe   nginx     "/docker-entrypoint.…"   11 hours ago   Up 13 seconds   80/tcp    priceless_black  
        ``` 
        
    - docker stop <CONTRAINER ID or NAMES>  :  해당 컨테이너 중지  
        ```bash
            # 특정 CONTRAINER ID or NAMES를 가진 컨테이너 중지  
            docker stop a9f2f17e5bfe  
                a9f2f17e5bfe  
        
            docker ps          
            # 실행중인 컨데이너 없음 확인     
                CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES  
        ```
    - docker kill <CONTRAINER ID or NAMES>  :  해당 컨테이너 강제종료  

  
- Remove the container  
    - docker rm <CONTRAINER ID or NAMES>  :  해당 컨테이너 삭제  
        (컨테이너 ID 다 타이핑 안해도 됩니다. 중복만 없으면 그대로 삭제됩니다.)  
        ```bash
            docker run hellow-world  

                Unable to find image 'hello-world:latest' locally  
                latest: Pulling from library/hello-world  
                58dee6a49ef1: Pull complete  
                c3bdf82c34d1: Download complete  
                Digest: sha256:452a468a4  
                Status: Downloaded newer image for hello-world:latest  
        
           
        ```
    - docker rm -f <CONTRAINER ID or NAMES>  :  해당 컨테이너 강제 삭제  
        (실행된 상태이 컨테이너도 바로 삭제해버림)  
        ```


### 🟡 1-3. [docker cli] Foreground Execute  
- docker run <image 명칭>  
    - 설명: image download + container create + container start를 한번에 해준다.  
