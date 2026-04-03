# 🟩 41. container image layer 개념  

## 📋 Contents  
1. overlay라는 layer file 시스템을 사용한다  



<br><br>

## 🟢 1. overlay라는 layer file 시스템을 사용한다.  
- Docker image는 여러 개의 **읽기 전용(read-only) layer** 로 이루어져 있다.  
- container를 실행하면, image layer 맨 위에 **쓰기 가능한 writable layer** 가 하나 추가된다.  
- 파일 수정, 생성, 삭제 같은 작업은 이 writable layer에 기록된다.  
- 사용자는 image layer와 container의 writable layer가 **overlay 되어 하나의 파일 시스템처럼 합쳐진 모습**을 보게 된다.  
- 즉, image 자체를 직접 수정하는 것이 아니라, **image 위에 container layer를 덧붙여서 사용하는 구조**이다.  




