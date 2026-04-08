# 🟩 7. GitHub 연동 증거 정리  

## 📋 Contents  
1. Git 기본 설정 확인  
2. 원격 저장소 연결 확인  
3. 현재 브랜치 추적 상태 확인  



<br><br>

## 🟢 1. Git 기본 설정 확인  

<br>

### 🟡 1-1. `git config --list` 결과  
```bash
credential.helper=osxkeychain  
user.name=sourcreamsource  
user.email=rovotev@gmail.com  
core.excludesfile=/Users/ooo/.gitignore_global  
difftool.sourcetree.cmd=opendiff "$LOCAL" "$REMOTE"  
difftool.sourcetree.path=  
mergetool.sourcetree.cmd=/Users/username/Applications/Sourcetree.app/Contents/Resources/opendiff-w.sh "$LOCAL" "$REMOTE" -ancestor "$BASE" -merge "$MERGED"  
mergetool.sourcetree.trustexitcode=true  
commit.template=/Users/ooo/.stCommitMsg  
init.defaultbranch=main  
core.repositoryformatversion=0  
core.filemode=true  
core.bare=false  
core.logallrefupdates=true  
core.ignorecase=true  
core.precomposeunicode=true  
remote.origin.url=https://github.com/sourcreamsource/codysseyWeekOne.git  
remote.origin.fetch=+refs/heads/*:refs/remotes/origin/*  
branch.main.remote=origin  
branch.main.merge=refs/heads/main  
```

- `user.name` 과 `user.email` 이 설정되어 있다.  
- `init.defaultbranch=main` 으로 기본 브랜치도 `main` 으로 맞춰져 있다.  



<br><br>

## 🟢 2. 원격 저장소 연결 확인  

<br>

### 🟡 2-1. `git remote -v` 결과  
```bash
origin  https://github.com/sourcreamsource/codysseyWeekOne.git (fetch)  
origin  https://github.com/sourcreamsource/codysseyWeekOne.git (push)  
```

- 현재 로컬 저장소는 GitHub 원격 저장소 `origin` 과 연결되어 있다.  
- fetch / push 주소가 모두 같은 저장소로 설정되어 있다.  



<br><br>

## 🟢 3. 현재 브랜치 추적 상태 확인  

<br>

### 🟡 3-1. `git branch -vv` 결과  
```bash
* main 53eb9~~ [origin/main] feat: trouble shooting  
```

- 현재 작업 브랜치는 `main` 이다.  
- `origin/main` 을 추적하고 있으므로 원격 저장소와 연결된 상태이다.  


