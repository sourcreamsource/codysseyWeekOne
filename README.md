# 🟩 1주차: AI/SW 개발 워크스테이션 구축  

## 📜 프로젝트 개요  
- 이번 과제의 목표는 터미널, Docker, Git/GitHub를 직접 다루면서 **재현 가능한 개발 환경**을 만드는 것이다.  
- README는 단순 소개 문서가 아니라, **평가자가 이 저장소만 보고 수행 내용을 확인할 수 있게 하는 제출 문서** 역할을 한다.  
- 따라서 각 항목은 `docs` 폴더 아래의 관련 마크다운 파일로 바로 이동할 수 있도록 링크로 정리했다.  



<br><br>

## 🖥️ 실행 환경  
- OS: macOS Tahoe 26.4  
- Shell: zsh  
- Docker: 29.3.1  
- Git: 2.51.0  



<br><br>

## ✅ 필수 수행 체크리스트  
- [x] 요구사항 분석  
- [x] 권한 변경 실습  
- [x] Git 명령 정리  
- [x] Docker 설치/점검 정리  
- [x] hello-world 실행 정리  
- [x] nginx 실행 및 포트매핑 정리  
- [x] Dockerfile 개념 및 실습 정리  
- [x] container layer / writable layer 정리  
- [x] volume / bind mount 정리  
- [x] Docker Compose 학습 문서 작성  
- [x] GitHub 연동 증거 정리  
- [x] 트러블슈팅 2건 이상 문서 작성  



<br><br>

## 📋 Docs Contents  

### 🟡 1. 요구사항 / 분석  
- [01_codyssey_요구사항.md](./docs/01_requirements/01_codyssey_요구사항.md)  
- [02_codyssey_요구사항분석.md](./docs/01_requirements/02_codyssey_요구사항분석.md)  

### 🟡 2. Linux 권한 변경 실습  
- [04_rwx_권한변경실습.md](./docs/02_permissions/04_rwx_권한변경실습.md)  


### 🟡 3. Git과 Github  

- [06_git.md](./docs/03_git_github/06_git.md)  

- [07_github_연동증거.md](./docs/03_git_github/07_github_연동증거.md)  

- [08_github_SSH.md](./docs/03_git_github/08_github_SSH.md)  



### 🟡 4. Docker 기초  
- [10_docker_setting.md](./docs/04_docker_basic/10_docker_setting.md)  
- [11_docker_basic.md](./docs/04_docker_basic/11_docker_basic.md)  
- [12_docker_Practice_HelloWorld.md](./docs/04_docker_basic/12_docker_Practice_HelloWorld.md)  
- [13_why_use_nginx.md](./docs/04_docker_basic/13_why_use_nginx.md)  
- [14_docker_Practice_nginx.md](./docs/04_docker_basic/14_docker_Practice_nginx.md)  
- [15_docker_Practice_execute.md](./docs/04_docker_basic/15_docker_Practice_execute.md)  
- [16_docker_Practice_directory.md](./docs/04_docker_basic/16_docker_Practice_directory.md)  
- [17_docker_Practice_status.md](./docs/04_docker_basic/17_docker_Practice_status.md)  

### 🟡 5. Dockerfile  
- [31_dockerfile_arrange.md](./docs/05_dockerfile/31_dockerfile_arrange.md)  
- [32_dockerfile_practice.md](./docs/05_dockerfile/32_dockerfile_practice.md)  
- [33_dockerfile_example.md](./docs/05_dockerfile/33_dockerfile_example.md)  

### 🟡 6. Docker 데이터 관리  
- [40_docker_data_control.md](./docs/06_docker_data/40_docker_data_control.md)  
- [41_docker_container_layer.md](./docs/06_docker_data/41_docker_container_layer.md)  
- [42_docker_container_storage.md](./docs/06_docker_data/42_docker_container_storage.md)  
- [44_docker_volume.md](./docs/06_docker_data/44_docker_volume.md)  
- [45_docker_bindmount.md](./docs/06_docker_data/45_docker_bindmount.md)  

### 🟡 7. Docker Compose  
- [50_compose_개념.md](./docs/07_docker_compose/50_compose_개념.md)  
- [51_compose_구조.md](./docs/07_docker_compose/51_compose_구조.md)  
- [52_compose_명령어.md](./docs/07_docker_compose/52_compose_명령어.md)  
- [53_compose_practice_basic.md](./docs/07_docker_compose/53_compose_practice_basic.md)  
- [54_compose_practice_multi.md](./docs/07_docker_compose/54_compose_practice_multi.md)  
- [55_compose_example.md](./docs/07_docker_compose/55_compose_example.md)  
- [56_compose_practice_requirements.md](./docs/07_docker_compose/56_compose_practice_requirements.md)  
- [57_compose_env_detail.md](./docs/07_docker_compose/57_compose_env_detail.md)  

### 🟡 8. 포트 확인  
- [60_port_search.md](./docs/08_port/60_port_search.md)  

### 🟡 9. 트러블슈팅  
- [80_troubleshooting_docker_ip.md](./docs/09_troubleshooting/80_troubleshooting_docker_ip.md)  
- [81_troubleshooting_container_data.md](./docs/09_troubleshooting/81_troubleshooting_container_data.md)  


<br><br>

## 🧠 이번 과제를 통해 설명할 수 있어야 하는 것  
- 절대 경로와 상대 경로 차이  
- 파일 권한 `r / w / x` 와 `755`, `644` 의미  
- Docker image 와 container 차이  
- Dockerfile로 커스텀 이미지 만드는 흐름  
- 포트 매핑이 필요한 이유  
- volume 과 bind mount 차이  
- Git 과 GitHub 역할 차이  
- Docker Compose가 필요한 이유  
