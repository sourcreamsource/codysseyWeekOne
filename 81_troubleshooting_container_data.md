# 🟩 81. Troubleshooting - 컨테이너 삭제 후 데이터 사라짐

## 📋 Contents  
1. 문제 상황  
2. 원인 가설  
3. 확인 방법  
4. 해결  
5. 배운 점  



<br><br>

## 🟢 1. 문제 상황  
- 컨테이너 안에서 파일을 수정한 뒤 컨테이너를 삭제하고 다시 실행했더니, 수정한 내용이 사라졌다.  



<br><br>

## 🟢 2. 원인 가설  
- 수정 내용이 volume이나 bind mount가 아니라, 컨테이너의 writable layer에만 저장되었기 때문이라고 판단했다.  



<br><br>

## 🟢 3. 확인 방법  
- 처음에는 일반 `docker run` 으로만 컨테이너를 실행했다.  
- `docker inspect` 나 실행 명령을 다시 보면 volume / bind mount 연결이 없었다.  
- 즉 데이터가 컨테이너 바깥 저장소로 빠져 있지 않았다.  



<br><br>

## 🟢 4. 해결  
- 영속성이 필요한 데이터는 volume 또는 bind mount를 사용해서 컨테이너 외부에 저장하도록 바꿨다.  

```bash
docker volume create my-nginx-volume
docker run -d --name my-nginx -p 8080:80 -v my-nginx-volume:/usr/share/nginx/html nginx:latest
```

- 이후 파일을 수정하고 컨테이너를 삭제한 뒤 다시 실행해도 데이터가 유지되는 것을 확인했다.  



<br><br>

## 🟢 5. 배운 점  
- 컨테이너는 쉽게 생성하고 삭제하는 실행 단위이지, 영구 저장소가 아니다.  
- 중요한 데이터는 container writable layer가 아니라 volume 또는 bind mount에 저장해야 한다.  
- 운영 환경에서는 volume, 개발 중 즉시 반영 확인은 bind mount가 더 적합하다는 점을 이해했다.  
