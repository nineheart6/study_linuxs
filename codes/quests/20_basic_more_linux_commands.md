### 연습문제 1: 기본 파일 시스템 탐색
#### 1-1. 현재 위치 확인 및 이동
1. 현재 작업 디렉터리의 절대 경로를 출력하시오.
```shell
[seungjae@localhost ~]$ pwd
/home/seungjae
```
2. 홈 디렉터리로 이동하시오.
루트 디렉터리(/)로 이동하시오.
```shell
[seungjae@localhost ~]$ cd /home
[seungjae@localhost home]$ cd /
```
3. 다시 홈 디렉터리로 돌아가시오.
```shell
[seungjae@localhost ~]$ cd /home
```

#### 1-2. 디렉터리 내용 확인
1. 현재 디렉터리의 파일과 폴더 목록을 출력하시오.
```shell
[seungjae@localhost home]$ ls
seungjae
```
2. 숨김 파일을 포함한 모든 파일의 상세 정보를 출력하시오.
```shell
[seungjae@localhost home]$ ls -a
.  ..  seungjae
```
3. /etc 디렉터리의 내용을 확인하시오.dir
### 연습문제 2: 디렉터리 및 파일 생성
#### 2-1. 디렉터리 구조 생성
다음과 같은 디렉터리 구조를 생성하시오:
```shell
practice/
├── documents/
│   ├── reports/
│   └── notes/
├── images/
└── backup/
```
```shell
[seungjae@localhost ~]$ mkdir practice
[seungjae@localhost ~]$ pwd
/home/seungjae
[seungjae@localhost ~]$ cd /home/seungjae/practice
[seungjae@localhost practice]$ mkdir documents/reports documents/notes images backup
mkdir: cannot create directory ‘documents/reports’: No such file or directory
mkdir: cannot create directory ‘documents/notes’: No such file or directory
[seungjae@localhost practice]$ mkdir documents documents/reports documents/notes images backup
mkdir: cannot create directory ‘images’: File exists
mkdir: cannot create directory ‘backup’: File exists
[seungjae@localhost practice]$ tree
.
├── backup
├── documents
│   ├── notes
│   └── reports
└── images

5 directories, 0 files
```
#### 2-2. 파일 생성 및 내용 작성
1. practice/documents/ 디렉터리에 readme.txt 파일을 생성하고 "Hello Linux!"라는 내용을 작성하시오.
```shell
[seungjae@localhost practice]$ cd /home/seungjae/practice/documents
[seungjae@localhost documents]$ touch readme.txt
```
2. practice/notes/ 디렉터리에 memo.txt 파일을 생성하고 "Linux 명령어 연습 중"이라는 내용을 작성하시오.
```shell
[seungjae@localhost documents]$ cd /home/seungjae/practice/documents/notes
[seungjae@localhost notes]$ touch memo.txt
```
### 연습문제 3: 파일 내용 확인 및 조작
#### 3-1. 파일 내용 출력
1. 앞서 생성한 readme.txt 파일의 내용을 출력하시오.
```shell
[seungjae@localhost notes]$ cd /home/seungjae/practice/documents
[seungjae@localhost documents]$ cat readme.txt 
```
2. memo.txt 파일의 내용을 출력하시오.
```shell
[seungjae@localhost documents]$ cd /home/seungjae/practice/documents/notes
[seungjae@localhost notes]$ cat memo.txt
```
#### 3-2. 파일 복사
1. readme.txt 파일을 backup/ 디렉터리에 복사하시오.
```shell
[seungjae@localhost documents]$ cp readme.txt /home/seungjae/practice/backup/
[seungjae@localhost documents]$ cd /home/seungjae/practice/
```
2. documents/ 디렉터리 전체를 backup/ 디렉터리에 복사하시오.
```shell
[seungjae@localhost practice]$ cp documents /home/seungjae/practice/backup/
cp: -r not specified; omitting directory 'documents'
[seungjae@localhost practice]$ cp -r  documents /home/seungjae/practice/backup/
```
### 연습문제 4: 파일 이동 및 이름 변경
#### 4-1. 파일 이동
1. memo.txt 파일을 documents/ 디렉터리로 이동하시오.
```shell
[seungjae@localhost backup]$ cd /home/seungjae/practice/documents/notes/
[seungjae@localhost notes]$ mv memo.txt /home/seungjae/practice/documents
```
2. images/ 디렉터리를 practice/media/로 이름을 변경하시오.
```shell
[seungjae@localhost notes]$ cd /home/seungjae/practice
[seungjae@localhost practice]$ mv images/ media/
```
#### 4-2. 파일 이름 변경
1. readme.txt를 introduction.txt로 이름을 변경하시오.
```shell
[seungjae@localhost notes]$ cd /home/seungjae/practice/documents
[seungjae@localhost documents]$ mv readme.txt introduction.txt
```
2. memo.txt를 study_notes.txt로 이름을 변경하시오.
```shell
[seungjae@localhost documents]$ mv memo.txt study_notes.txt
```
### 연습문제 5: 종합 실습
#### 5-1. 프로젝트 디렉터리 생성
다음 요구사항에 따라 프로젝트 디렉터리를 생성하시오:
1. my_project/라는 최상위 디렉터리 생성
```shell
[seungjae@localhost ~]$ mkdir my_project
[seungjae@localhost ~]$ cd /home/seungjae/my_project/
```
2. 하위에 src/ docs/ tests/ config/ 디렉터리 생성
```shell
[seungjae@localhost my_project]$ mkdir src/ docs/ tests/ config/
```
3. src/ 디렉터리에 main.py 파일 생성 (내용: "# Main Python File")
```shell
[seungjae@localhost my_project]$ cd /home/seungjae/my_project/src
[seungjae@localhost src]$ touch main.py
```
4. docs/ 디렉터리에 README.md 파일 생성 (내용: "# My Project Documentation")
```shell
[seungjae@localhost src]$ touch /home/seungjae/my_project/docs/README.md
```
5. config/ 디렉터리에 settings.conf 파일 생성 (내용: "# Configuration File")
```shell
[seungjae@localhost docs]$ touch /home/seungjae/my_project/docs/settings.conf
```
#### 5-2. 백업 및 정리
1. 전체 my_project/ 디렉터리를 my_project_backup/으로 복사하시오.
```shell
[seungjae@localhost ~]$ mkdir my_project_backup
[seungjae@localhost ~]$ cp my_project my_project_backup
cp: -r not specified; omitting directory 'my_project'
[seungjae@localhost ~]$ cp -r my_project my_project_backup
```
2. my_project/src/main.py 파일을 my_project/src/app.py로 이름을 변경하시오.
```shell
[seungjae@localhost ~]$ mv my_project/src/main.py my_project/src/app.py
```

3. my_project/docs/README.md 파일을 my_project/ 디렉터리로 이동하시오.
```shell
[seungjae@localhost ~]$ mv my_project/docs/README.md my_project/
```
