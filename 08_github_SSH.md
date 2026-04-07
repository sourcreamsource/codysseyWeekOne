# 🟩 GitHub SSH 설정 방법  

## 🟢 1. 로컬에서 SSH 키 생성  

| 부분                            | 의미                  |  
| ----------------------------- | ------------------- |
| `ssh-keygen`                  | SSH 키를 생성하는 프로그램    |  
| `-t ed25519`                  | 키 타입을 `ed25519`로 지정 |  
| `-C "your_email@example.com"` | 키에 붙일 설명(comment)   |  
| `-f .ssh/id_ed25519_test`   | 저장할 파일 경로/이름 지정     |  

터미널에서 아래 명령어를 실행합니다.  

```bash
ssh-keygen -t ed25519 -C "test@test.com" -f .ssh/id_ed25519_test  
```
- SSH 파일 설정을 위한 질문들  
    - 1. 이메일은 GitHub에 등록한 이메일을 사용하는 것이 좋습니다.  
    - 2. 저장 경로는 기본값(`~/.ssh/id_ed25519_test`)을 그대로 사용해도 됩니다.  
    - 3. 비밀번호(passphrase)를 설정합니다.  
    - 4. 생성 완료!  

<br>

### 🟡 실제 실행 내용  
```bash
❯ cat ~/.ssh/id_ed25519_test.pub  

ssh-ed25519 AAWEWC3NzaC1lZDI1TTE5EEAA8ef7we89fewdqw89T6yN4zK8aBcD2wEfG5hJ7mNoPqRs test@test.com  
```


<br><br>

## 🟢 2. 공개 키 확인 및 복사  

생성한 공개 키를 확인합니다.  

```bash
cat ~/.ssh/id_ed25519_test.pub  
```

출력된 내용을 전체 복사합니다.  

<br>

### 🟡 실제 실행 내용  
```bash
❯ cat ~/.ssh/id_ed25519_test.pub  

ssh-ed25519 AAWEWC3NzaC1lZDI1TTE5EEAA8ef7we89fewdqw89T6yN4zK8aBcD2wEfG5hJ7mNoPqRs test@test.com  
```


<br><br>

## 🟢 3. GitHub에 SSH 키 등록  

GitHub에서 아래 순서로 이동합니다.  

`Settings` → `SSH and GPG keys` → `New SSH key`  

- `Title`: 현재 사용하는 컴퓨터 이름 등으로 입력  
- `Key`: 🔥 `복사한 공개 키` 붙여넣기  

저장하면 GitHub에 SSH 키 등록이 완료됩니다.  



<br>

### 🟡 실제 실행 내용  
```bash
Title: macbook-air  
Key: ssh-ed25519 AAWEWC3NzaC1lZDI1TTE5EEAA8ef7we89fewdqw89T6yN4zK8aBcD2wEfG5hJ7mNoPqRs test@test.com  
```
→ Add SSH key 클릭  

→ GitHub에 새 SSH 키가 등록됨  


<br><br>

## 🟢 4. 연결 테스트  

아래 명령어로 정상 연결 여부를 확인합니다.  

```bash
ssh -T git@github.com  
```
- 처음 접속 시 확인 메시지(해시값 확인)가 나오면 `yes`를 입력합니다.  
- 비밀번호를 설정했다면 비밀번호를 입력해야합니다.  
- 정상적으로 연결되면 GitHub 사용자 이름과 함께 인증 성공 메시지가 표시됩니다!  

<br>

### 🟡 실제 실행 내용  
```bash
❯ ssh -T git@github.com  

Enter passphrase for key '/Users/username/.ssh/id_rsa':  
Hi username! You've successfully authenticated, but GitHub does not provide shell access.  
```
