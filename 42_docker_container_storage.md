# 🟩 42. container writable layer 와 데이터 보존  

## 📋 Contents  
1. container writable layer 를 이해해야 하는 이유  
2. 왜 외부 저장이 필요한가  
3. host 와 container 데이터 연결 방식  



<br><br>

## 🟢 1. container writable layer 를 이해해야 하는 이유  
- Docker image는 읽기 전용(read-only) 이며 직접 수정되지 않는다.  
- container를 실행하면 image 위에 **쓰기 가능한 writable layer** 가 하나 추가된다.  
- container 안에서 수정하거나 새로 만든 내용은 이 writable layer에 기록된다.  
- 그런데 container를 삭제하면 이 writable layer도 함께 사라질 수 있다.  


## 🟢 2. 왜 외부 저장이 필요한가  
- container 안에서만 데이터를 저장하면 container 삭제 시 함께 없어질 수 있다.  
- 그래서 중요한 데이터는 container 바깥에 따로 저장해야 한다.  
- 이때 사용하는 대표적인 방식이 **volume** 과 **bind mount** 이다.  

### 🟡 우리는 컨테이너 데이터를 영구 보존 할 수 있다.  
- 컨테이너 데이터 영구 보존  
    - container의 writable layer가 아니라 외부 저장소(volume, bind mount)에 데이터를 저장  
    - container가 삭제되어도 데이터는 유지 가능  
    - 이후 새로운 container를 실행해도 같은 데이터를 다시 연결해서 사용할 수 있다  
        
- 호스트의 디렉터리와 컨테이너 간 데이터 공유  
    - -v <호스트 경로>:<컨테이너 마운드 경로>  
    - -v <호스트 경로>:<컨테이너 마운드 경로>:<읽기 모드>  


<br><br>

## 🟢 3. host 와 container 데이터 연결 방식  
- **bind mount**  
  host의 실제 폴더를 container에 직접 연결하는 방식  
- **volume**  
  Docker가 관리하는 별도 저장공간을 container에 연결하는 방식  
- 둘 다 container 바깥에 데이터를 두기 때문에 영속성 확보에 사용한다.  


