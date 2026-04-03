# 🟩 6. git 명령 정리  

## 📋 Contents  
1. Git 이 무엇인가  
2. Git 과 GitHub 차이  
3. 🔥 Git 기본 명령 표  
4. Git 초기 설정 명령 표  
5. 자주 쓰는 Git 작업 흐름  
6. 예시 터미널 실습 로그  



<br><br>

## 🟢 1. Git 이 무엇인가  
- Git은 파일 변경 이력을 관리하는 버전 관리 도구이다.  
- 내가 어떤 파일을 수정했고, 언제 저장했고, 어떤 기록을 남겼는지 관리할 수 있다.  
- 과제에서는 Git 설정과 GitHub 연동이 요구되므로 기본 명령을 이해해야 한다.  


<br><br>

## 🟢 2. Git 과 GitHub 차이  
```bash
Git = 내 컴퓨터에서 버전 관리  
GitHub = 원격 저장소에 올려 공유하고 협업  
```



<br><br>

## 🟢 3. Git 기본 명령 표  

### 🟡 기본  
| 명령어 | 의미 | 언제 쓰는가 | 예시 |  
| :--- | :--- | :--- | :--- |
| `git init` | 현재 폴더를 Git 저장소로 시작 | 프로젝트 처음 시작할 때 | `git init` |  
| `git status` | 현재 변경 상태 확인 | 가장 자주 상태 볼 때 | `git status` |  
| `git add 파일명` | 특정 파일을 스테이징 | 일부 파일만 커밋할 때 | `git add README.md` |  
| `git add .` | 현재 변경 전체를 스테이징 | 전체 변경을 한 번에 올릴 때 | `git add .` |  
| `git commit -m "메시지"` | 변경 사항을 기록 | 저장 이력을 남길 때 | `git commit -m "add docker notes"` |  
| `git log --oneline` | 커밋 기록을 짧게 확인 | 기록 흐름 확인할 때 | `git log --oneline` |  
| `git diff` | 변경 내용 비교 | 아직 add 전 수정 확인할 때 | `git diff` |  
| `git diff --staged` | add 후 커밋 전 비교 | 커밋 직전 최종 확인 | `git diff --staged` |  

### 🟡 브랜치 관리  
| 명령어 | 의미 | 언제 쓰는가 | 예시 |  
| :--- | :--- | :--- | :--- |
| `git branch` | 브랜치 목록 확인 | 현재 브랜치 확인할 때 | `git branch` |  
| `git branch 이름` | 새 브랜치 생성 | 작업 분리할 때 | `git branch feature-readme` |  
| `git checkout 이름` | 브랜치 이동 | 다른 브랜치로 갈 때 | `git checkout main` |  
| `git checkout -b 이름` | 브랜치 생성 + 이동 | 새 작업 시작할 때 | `git checkout -b feature-git` |  
| `git checkout -b 새브랜치 기준브랜치` | 특정 브랜치를 기준으로 새 브랜치 생성 + 이동 | main 같은 기준 브랜치에서 작업을 시작할 때 | `git checkout -b feature-login main` |  
| `git switch 이름` | 브랜치 이동 | checkout 대신 더 명확하게 이동 | `git switch main` |  
| `git switch -c 이름` | 브랜치 생성 + 이동 | 새 브랜치 바로 만들 때 | `git switch -c study-git` |  
| `git switch -c 새브랜치 기준브랜치` | 특정 브랜치를 기준으로 새 브랜치 생성 + 이동 | 원하는 기준점에서 새 작업을 시작할 때 | `git switch -c feature-login main` |  

### 🟡 원격 (github)  
| 명령어 | 의미 | 언제 쓰는가 | 예시 |  
| :--- | :--- | :--- | :--- |
| `git remote -v` | 원격 저장소 확인 | GitHub 연결 확인할 때 | `git remote -v` |  
| `git push origin main` | 원격 저장소로 올리기 | GitHub 제출 시 | `git push origin main` |  
| `git pull origin main` | 원격 저장소 내용 가져오기 | 최신 내용 동기화할 때 | `git pull origin main` |  
| `git clone 저장소주소` | 저장소 복제 | 기존 저장소를 받아올 때 | `git clone https://...` |  


<br><br>

## 🟢 4. Git 설정 명령 표  

| 명령어 | 의미 | 예시 |  
| :--- | :--- | :--- |
| `git config --global user.name "이름"` | 사용자 이름 설정 | `git config --global user.name "sourcreamsource"` |  
| `git config --global user.email "메일"` | 사용자 이메일 설정 | `git config --global user.email "sourcreamsource@example.com"` |  
| `git config --global init.defaultBranch main` | 기본 브랜치를 main으로 설정 | `git config --global init.defaultBranch main` |  
| `git config --list` | 현재 Git 설정 전체 확인 | `git config --list` |  



<br><br>

## 🟢 5. 자주 쓰는 Git 작업 흐름  

### 🟡 가장 기본 흐름  
```bash
git init  
git status  
git add .  
git commit -m "first commit"  
git branch  
git remote -v  # 원격 레포지토리 확인  
git push origin main  
```

- `init` 으로 시작  
- `status` 로 상태 확인  
- `add` 로 올릴 파일 선택  
- `commit` 으로 기록  
- `push` 로 GitHub에 업로드  



<br><br>

## 🟢 6. 예시 터미널 실습 로그  

<br>

### 🟡 Git 설정 예시 로그  
```bash
➜  codysseyWeekOne git config --global user.name "sourcreamsource"  
➜  codysseyWeekOne git config --global user.email "sourcreamsource@example.com"  
➜  codysseyWeekOne git config --global init.defaultBranch main  
➜  codysseyWeekOne git config --list  
user.name=sourcreamsource  
user.email=sourcreamsource@example.com  
init.defaultbranch=main  
core.editor=vim  
```


<br>

### 🟡 저장소 시작 예시 로그  
```bash
➜  codysseyWeekOne git init  
Initialized empty Git repository in /Users/sourcreamsource/container/codysseyWeekOne/.git/  

➜  codysseyWeekOne git status  
On branch main  

No commits yet  

Untracked files:  
  (use "git add <file>..." to include in what will be committed)  
        README.md  
        04_rwx_권한변경실습.md  
        06_git.md  

nothing added to commit but untracked files present  
```


<br>

### 🟡 add / commit 예시 로그  
```bash
➜  codysseyWeekOne git add README.md 04_rwx_권한변경실습.md 06_git.md  

➜  codysseyWeekOne git status  
On branch main  

No commits yet  

Changes to be committed:  
  (use "git rm --cached <file>..." to unstage)  
        new file:   README.md  
        new file:   04_rwx_권한변경실습.md  
        new file:   06_git.md  

➜  codysseyWeekOne git commit -m "docs: add git and permission study notes"  
[main (root-commit) a1b2c3d] docs: add git and permission study notes  
 3 files changed, 120 insertions(+)  
 create mode 100644 README.md  
 create mode 100644 04_rwx_권한변경실습.md  
 create mode 100644 06_git.md  
```


<br>

### 🟡 log / branch / remote 예시 로그  
```bash
➜  codysseyWeekOne git log --oneline  
a1b2c3d docs: add git and permission study notes  

➜  codysseyWeekOne git branch  
* main  

➜  codysseyWeekOne git switch -c feature-login main  
Switched to a new branch 'feature-login'  

➜  codysseyWeekOne git remote -v  
origin  https://github.com/sourcreamsource/codysseyWeekOne.git (fetch)  
origin  https://github.com/sourcreamsource/codysseyWeekOne.git (push)  
```


<br>

### 🟡 push 예시 로그  
```bash
➜  codysseyWeekOne git push origin main  
Enumerating objects: 8, done.  
Counting objects: 100% (8/8), done.  
Writing objects: 100% (8/8), 3.12 KiB | 3.12 MiB/s, done.  
Total 8 (delta 0), reused 0 (delta 0), pack-reused 0  
To https://github.com/sourcreamsource/codysseyWeekOne.git  
 * [new branch]      main -> main  
```



