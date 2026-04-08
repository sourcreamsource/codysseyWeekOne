# 🟩 4. rwx 권한변경 실습  

## 📋 Contents  
1. rwx가 무엇인가  
2. rwx 권한 한 번에 이해하기  
3. 파일과 디렉토리에서의 차이  
4. 실제 실습  



<br><br>

## 🟢 rwx 권한 한 번에 이해하기  


### 🟡 권한 비트 (`r`ead / `w`rite / e`x`ecute)  
- 기호는 이렇고  
- 보통 숫자로 사용합니다.  

| 기호 | 숫자 | 파일 | 디렉토리 |   
| :--- | :---: | :--- | :--- |
| `r`ead | `4` | 내용 읽기 | 목록 보기 (`ls`) |  
| `w`rite | `2` | 내용 수정 | 파일 생성 · 삭제 · 이름 변경 |  
| e`x`ecute | `1` | 파일 실행 | 디렉토리 진입 (`cd`) |  

<br>

### 🟡 권한을 부여하는 대상 (`u`ser / `g`roup / `o`ther)  
| 기호 | 대상 | 설명 |  
| :--- | :--- | :--- |
| `u` | user | 파일 소유자 |  
| `g` | group | 소유 그룹 소속 사용자 |  
| `o` | other | 그 외 모든 사용자 |  



<br>

### 🟡 하나의 권한을 숫자로 이렇게 계산  
* 숫자를 통해 표현 및 사용한다고 이해하면 됩니다.  

| 숫자 | 기호 | 계산 | 파일 | 디렉토리 |  
| :---: | :--- | :--- | :--- | :--- |
| `7` | `rwx` | 4+2+1 | 읽기 · 쓰기 · 실행 | 목록 · 수정 · 진입 |  
| `6` | `rw-` | 4+2 | 읽기 · 쓰기 | 목록 · 수정 |  
| `5` | `r-x` | 4+1 | 읽기 · 실행 | 목록 · 진입 |  
| `4` | `r--` | 4 | 읽기만 | 목록만 |  
| `0` | `---` | 0 | 권한 없음 | 권한 없음 |  

<br>

### 🟡 대표적인 `권한 패턴` (3자리 = u g o 순서)<br>  
- 숫자로 이런 형태로 쓰입니다!  
- 권한은 보통 `u / g / o` 세 묶음으로 본다.  
- 디렉토리 또는 파일에 각각 한번에 `소유자(user)`, `그룹(group)`, `이외(other)`를 수정 및 부여합니다!  

| 숫자 | 기호 | u (소유자) | g (그룹) | o (others) | 주요 용도 |  
| :---: | :--- | :---: | :---: | :---: | :--- |
| `755` | `rwxr-xr-x` | `rwx` | `r-x` | `r-x` | 공개 디렉토리, 실행 파일 |  
| `644` | `rw-r--r--` | `rw-` | `r--` | `r--` | 일반 텍스트 파일 |  
| `700` | `rwx------` | `rwx` | `---` | `---` | 비공개 스크립트 · 디렉토리 |  
| `600` | `rw-------` | `rw-` | `---` | `---` | SSH 키, 민감한 설정 파일 |  
| `777` | `rwxrwxrwx` | `rwx` | `rwx` | `rwx` | 전체 공개 (보안 위험 ⚠️) |  

- 예를 들어 `rwxr-xr--` 는 `user / group / other` 순서이다.  

<br>

### 🟡 자주 쓰는 명령어  
| 목적 | 명령어 예시 |  
| :--- | :--- |
| 권한 변경 (change mode) | `chmod 755 파일명` |  
| 소유자 변경 (change owner) | `chown user:group 파일명` |  
| 권한 확인 | `ls -l 파일명`, `ls -ld 폴더명` |  

  



<br><br>

## 🟢 파일과 디렉토리에서 어떻게 쓰는가  

<br>

### 🟡 파일에서 자주 쓰는 권한  
```bash
chmod 644 note.txt  
chmod 755 run.sh  
```

- 일반 텍스트 파일은 보통 `644`  
- 실행해야 하는 스크립트 파일은 보통 `755`  


<br>

### 🟡 디렉토리에서 자주 쓰는 권한  
```bash
chmod 755 mydir  
chmod 700 private_dir  
```

- 일반 작업 폴더는 보통 `755`  
- 혼자만 접근해야 하는 폴더는 `700`  



<br><br>

## 🟢 실제 실습  

<br>

### 🟡 파일 권한 바꾸기  
```bash
touch note.txt  
ls -l note.txt  
chmod 444 note.txt  
ls -l note.txt  

# note.txt 만들기  
➜  ws_authorization git:(main) ✗ touch note.txt  

# note.txt 의 현재 권한 확인 = 644  
➜  ws_authorization git:(main) ✗ ls -l note.txt  
-rw-r--r--@ 1 ooo  staff  0 Apr  3 11:08 note.txt  

# 권한을 444로 변경해보기  
➜  ws_authorization git:(main) ✗ chmod 444 note.txt  

# 444로 변경된 것을 확인  
➜  ws_authorization git:(main) ✗ ls -l note.txt  
-r--r--r--@ 1 ooo  staff  0 Apr  3 11:08 note.txt  
```


<br>

### 🟡 디렉토리 권한 바꾸기  
```bash
mkdir mydir  
ls -ld mydir  
chmod 644 mydir  
ls -ld mydir  

# mydir이라는 폴더 만들기  
➜  ws_authorization git:(main) ✗ mkdir mydir  

# mydir 폴더에 대한 권한을 확인한다.  
➜  ws_authorization git:(main) ✗ ls -ld mydir  
drwxr-xr-x@ 2 ooo  staff  64 Apr  3 11:11 mydir  

# mydir 폴더 권한을 644로 변경  
➜  ws_authorization git:(main) ✗ chmod 644 mydir  

# mydir 폴더 권한이 644로 바뀐 것을 확인  
➜  ws_authorization git:(main) ✗ ls -ld mydir  
drw-r--r--@ 2 ooo  staff  64 Apr  3 11:11 mydir  
```


<br>

### 🟡 핵심 요약  
```bash
파일은 644, 실행 파일은 755, 개인 폴더는 700 먼저 기억하면 된다.  
```
