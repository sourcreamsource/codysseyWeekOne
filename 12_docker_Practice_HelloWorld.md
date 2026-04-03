# 🟩 12. Practice - `docker hello-world`  

- 아래 과정을 한번에 실행하여 최종 컨테이너 시작까지 되는 과정을 run 명령어 하나로 할 수 있습니다.  
    1. docker image를 pull 받는다.  (docker pull ~)  
    2. pull받은 hello-world 이미지로 container를 만든다.  (docker create ~)  
    3. hello-world container를 실행한다.  (docker start ~)  

> docker run hello-world  

```bash
docker run hello-world  

Unable to find image 'hello-world:latest' locally  
latest: Pulling from library/hello-world  
58dee6a49ef1: Pull complete  
c3bdf82c34d1: Download complete  
Digest: sha256:452a468a4  
Status: Downloaded newer image for hello-world:latest  

# 실제 내용이 출력되는 것을 확인할 수 있습니다.  
Hello from Docker!  
This message shows that your installation appears to be working correctly.  

To generate this message, Docker took the following steps:  
 1. The Docker client contacted the Docker daemon.  
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.  
    (arm64v8)  
 3. The Docker daemon created a new container from that image which runs the  
    executable that produces the output you are currently reading.  
 4. The Docker daemon streamed that output to the Docker client, which sent it  
    to your terminal.  

To try something more ambitious, you can run an Ubuntu container with:  
 $ docker run -it ubuntu bash  

Share images, automate workflows, and more with a free Docker ID:  
 https://hub.docker.com/  

For more examples and ideas, visit:  
 https://docs.docker.com/get-started/  

```



