# **Linux 권한 관리 실습 문제**

## **실습 환경 설정**

**먼저 다음 명령어들을 실행하여 실습 환경을 구성하세요:**

- \# 실습 디렉터리 생성(/root 사용)  
- mkdir permission\_practice  
- cd permission\_practice  
-   
- \# 사용자 및 그룹 생성 (관리자 권한 필요)  
- sudo useradd \-m \-s /bin/bash alice  
- sudo useradd \-m \-s /bin/bash bob  
- sudo useradd \-m \-s /bin/bash charlie  
- sudo useradd \-m \-s /bin/bash diana  
- sudo useradd \-m \-s /bin/bash eve  
-   
- \# 그룹 생성  
- sudo groupadd developers  
- sudo groupadd managers  
-   
- \# 사용자를 그룹에 추가  
- sudo usermod \-a \-G developers alice  
- sudo usermod \-a \-G developers bob  
- sudo usermod \-a \-G developers charlie  
- sudo usermod \-a \-G managers diana  
- sudo usermod \-a \-G managers alice  
- \# eve는 기타 사용자로 어떤 그룹에도 속하지 않음  
-   
- \# 복잡한 디렉터리 구조 생성  
- mkdir \-p {company/{departments/{dev,hr,finance,marketing},projects/{project\_a,project\_b,project\_c}},shared/{documents,resources,tools},private/{alice,bob,charlie,diana,eve},backup/{daily,weekly,monthly},logs/{2023/{01..12},2024/{01..12}}}  
-   
- \# 다양한 파일 생성  
- touch company/departments/dev/{main.py,test.py,config.py,README.md}  
- touch company/departments/hr/{employees.xlsx,contracts.pdf,policies.txt}  
- touch company/departments/finance/{budget.xlsx,reports.pdf,invoices.csv}  
- touch company/projects/project\_a/{spec.doc,code.zip,data.json}  
- touch company/projects/project\_b/{requirements.txt,source.tar.gz,notes.md}  
- touch shared/documents/{manual.pdf,guidelines.txt,templates.docx}  
- touch shared/resources/{images.zip,fonts.tar,icons.png}  
- touch private/alice/{personal.txt,settings.conf,backup.tar}  
- touch private/bob/{notes.md,config.json,archive.zip}  
- touch backup/daily/{backup\_$(date \+%Y%m%d).tar.gz,log\_$(date \+%Y%m%d).txt}  
- touch logs/2024/06/{access.log,error.log,debug.log,system.log}  
-   
- \# 실행 가능한 스크립트 파일 생성  
- echo '\#\!/bin/bash' \> shared/tools/deploy.sh  
- echo 'echo "Deployment script"' \>\> shared/tools/deploy.sh  
- echo '\#\!/bin/bash' \> shared/tools/backup.sh  
- echo 'echo "Backup script"' \>\> shared/tools/backup.sh  
- echo '\#\!/bin/bash' \> company/departments/dev/build.sh  
- echo 'echo "Build script"' \>\> company/departments/dev/build.sh  
-   
- \# 설정 파일 생성  
- echo "database\_host=localhost" \> company/departments/dev/database.conf  
- echo "api\_key=secret123" \> company/departments/dev/api.conf  
- echo "salary\_data=confidential" \> company/departments/hr/salaries.txt  
-   
- echo "실습 환경이 구성되었습니다\!"  
- tree permission\_practice  
    
  ---

  ## **1\. 기본 권한 설정**

  ### **1-1. 개발팀 파일 권한 설정**

개발팀(developers 그룹) 관련 파일들의 권한을 다음과 같이 설정하세요:

* `company/departments/dev/` 디렉터리: 개발팀만 접근 가능, 소유자와 그룹은 읽기/쓰기/실행 가능  
* `company/departments/dev/main.py`: 개발팀은 읽기/쓰기, 기타는 읽기만 가능  
* `company/departments/dev/build.sh`: 개발팀만 실행 가능

**명령어를 작성하세요:**

- \# 1-1 답안 작성란  
```shell
[root@localhost departments]# chown -R alice:developers dev/
[root@localhost departments]# chmod 770 dev/
[root@localhost departments]# ls -ld dev
total 0
drwxrwx---. 2 alice developers 123 Jul 21 16:51 dev

[root@localhost dev]# chmod 664 main.py 
[root@localhost dev]# ls -l main.py 
-rw-rw-r--. 1 alice developers 0 Jul 21 16:51 main.py

[root@localhost dev]# chmod 050 build.sh 
[root@localhost dev]# ls -l build.sh 
----r-x---. 1 alice developers 32 Jul 21 16:51 build.sh

[root@localhost dev]# cd ..
[root@localhost departments]# su bob
[bob@localhost departments]$ cd dev
[bob@localhost dev]$ cat main.py 
[bob@localhost dev]$ ./build.sh 
Build script
[bob@localhost dev]$ exit
exit
[root@localhost departments]# su diana
[diana@localhost departments]$ cd dev
bash: cd: dev: Permission denied
[diana@localhost departments]$ exit
exit

```
- 


  ### **1-2. 개인 디렉터리 보안 설정**

각 사용자의 개인 디렉터리와 파일을 다음과 같이 설정하세요:

* `private/alice/` 디렉터리: alice만 접근 가능  
* `private/alice/personal.txt`: alice만 읽기/쓰기 가능  
* `private/bob/config.json`: bob만 읽기/쓰기 가능

**명령어를 작성하세요:**

- \# 1-2 답안 작성란  
-   
- 
```shell
[root@localhost departments]# cd ../../private/
[root@localhost private]# chown -R alice:developers alice
[root@localhost private]# ls -ld alice
drwxr-xr-x. 2 alice developers 65 Jul 21 16:51 alice
[root@localhost private]# chmod 700 alice/
[root@localhost private]# ls -ld alice
drwx------. 2 alice developers 65 Jul 21 16:51 alice
[root@localhost private]# cd alice/
[root@localhost alice]# chmod 700 personal.txt 
[root@localhost alice]# ls -l personal.txt 
-rwx------. 1 alice developers 0 Jul 21 16:51 personal.txt

[root@localhost private]# chown -R bob:developers bob/
[root@localhost private]# cd bob
[root@localhost bob]# chmod 600 config.json 
[root@localhost bob]# ls -l
total 0
-rw-r--r--. 1 bob developers 0 Jul 21 16:51 archive.zip
-rw-------. 1 bob developers 0 Jul 21 16:51 config.json
-rw-r--r--. 1 bob developers 0 Jul 21 16:51 notes.md

[root@localhost private]# su alice
[alice@localhost private]$ cat bob/config.json 
cat: bob/config.json: Permission denied
[alice@localhost private]$ cat ./alice/personal.txt 
[alice@localhost private]$ exit
exit
[root@localhost private]# su bob
[bob@localhost private]$ cd alice/
bash: cd: alice/: Permission denied
[bob@localhost private]$ cat ./bob/config.json 
[bob@localhost private]$ exit
exit
```  
    
  ---

  ## **2\. 그룹 기반 권한 관리**

  ### **2-1. 공유 리소스 접근 권한**

`shared/` 디렉터리의 권한을 다음과 같이 설정하세요:

* `shared/documents/`: developers와 managers 그룹 모두 읽기 가능, 소유자만 쓰기 가능  
* `shared/resources/`: developers 그룹만 접근 가능  
* `shared/tools/`: 모든 사용자가 읽기 가능, developers 그룹만 실행 가능

**명령어를 작성하세요:**

- \# 2-1 답안 작성란  
-   
- 
```shell
[root@localhost permission_practice]# cd shared/
[root@localhost shared]# chown alice:developers documents/
[root@localhost shared]# chmod 540 documents/
[root@localhost shared]# ls -ld documents/
dr-xr-----. 2 alice developers 68 Jul 21 16:51 documents/

[root@localhost shared]# su diana
[diana@localhost shared]$ ls -ld documents/
dr-xr-----. 2 alice developers 68 Jul 21 16:51 documents/
[diana@localhost shared]$ exit
exit

[root@localhost shared]# chown bob:developers resources/
[root@localhost shared]# chmod 770 resources/
[root@localhost shared]# chown bob:developers tools/
[root@localhost shared]# chmod 454 tools/
[root@localhost shared]# ls -l
total 0
dr-xr-----. 2 alice developers 68 Jul 21 16:51 documents
drwxrwx---. 2 bob   developers 58 Jul 21 16:51 resources
dr--r-xr--. 2 bob   developers 40 Jul 21 16:51 tools
[root@localhost shared]# su diana
[diana@localhost shared]$ ls -l
total 0
dr-xr-----. 2 alice developers 68 Jul 21 16:51 documents
drwxrwx---. 2 bob   developers 58 Jul 21 16:51 resources
dr--r-xr--. 2 bob   developers 40 Jul 21 16:51 tools
[diana@localhost shared]$ ls resources/
ls: cannot open directory 'resources/': Permission denied
[diana@localhost shared]$ ls tools/
ls: cannot access 'tools/deploy.sh': Permission denied
ls: cannot access 'tools/backup.sh': Permission denied
backup.sh  deploy.sh
[diana@localhost shared]$ exit
exit
[root@localhost shared]# su alice
[alice@localhost shared]$ ls resources/
fonts.tar  icons.png  images.zip
[alice@localhost shared]$ ls tools/
backup.sh  deploy.sh

```

  ### **2-2. 프로젝트별 협업 권한**

프로젝트 디렉터리의 권한을 다음과 같이 설정하세요:

* `company/projects/project_a/`: developers 그룹 구성원들이 협업할 수 있도록 설정  
* `company/projects/project_b/`: alice와 bob만 접근 가능하도록 설정

**명령어를 작성하세요:**

- \# 2-2 답안 작성란  
-   
-   
```shell
[root@localhost permission_practice]# cd company/projects/
[root@localhost projects]# chown bob:developers project_a/
[root@localhost projects]# chmod 770 project_a/

[root@localhost projects]# groupadd alice_bob
[root@localhost projects]# usermod \-a \-G alice_bob alice
[root@localhost projects]# usermod \-a \-G alice_bob bob
[root@localhost projects]# chown alice:alice_bob project_b/
[root@localhost projects]# chmod 770 project_b/

[root@localhost projects]# su charlie
[charlie@localhost projects]$ cd project_b/
bash: cd: project_b/: Permission denied
[charlie@localhost projects]$ cd project_a/

[root@localhost projects]# su alice
[alice@localhost projects]$ cd project_a/
[alice@localhost project_a]$ cd ..
[alice@localhost projects]$ cd project_b/

```
  ---

  ## **3\. 고급 권한 설정**

  ### **3-2. 숫자 표기법으로 복합 권한 설정**

다음 파일들의 권한을 숫자 표기법으로 설정하세요:

* `company/departments/finance/budget.xlsx`: 소유자(rw-), 그룹(r--), 기타(---)  
* `shared/documents/manual.pdf`: 소유자(rw-), 그룹(r--), 기타(r--)  
* `logs/2024/06/system.log`: 소유자(rw-), 그룹(r--), 기타(---)

**명령어를 작성하세요:**

- \# 3-2 답안 작성란  
-   
-   
```shell

```    
  ---

  ## **4\. 소유권 및 그룹 관리**

  ### **4-1. 소유권 변경**

다음과 같이 파일과 디렉터리의 소유권을 변경하세요:

* `company/departments/dev/` 디렉터리와 모든 하위 파일: alice 소유, developers 그룹  
* `company/departments/hr/` 디렉터리와 모든 하위 파일: diana 소유, managers 그룹  
* `shared/tools/` 디렉터리와 모든 하위 파일: root 소유, developers 그룹

**명령어를 작성하세요:**

- \# 4-1 답안 작성란  
-   
- 
```shell

```

  ### **4-2. 그룹 전용 변경**

다음 디렉터리들의 그룹만 변경하세요:

* `company/projects/`: managers 그룹으로 변경  
* `backup/daily/`: developers 그룹으로 변경

**명령어를 작성하세요:**

- \# 4-2 답안 작성란  
-   
-   
```shell

```    
  ---  
  ---

  ## **6\. umask 및 기본 권한 관리**

  ### **6-1. umask 설정 및 테스트**

현재 시스템의 umask 값을 확인하고 다음과 같이 변경한 후 테스트하세요:

* umask 값을 027로 설정  
* 새 파일과 디렉터리를 생성하여 기본 권한 확인  
* 원래 umask 값으로 복원

**명령어를 작성하세요:**

- \# 6-1 답안 작성란  
```shell

```

  ### **6-2. 사용자별 umask 커스터마이징**

각 사용자별로 다른 umask 값을 설정하세요:

* alice: umask 022 (일반적인 개발자 설정)  
* diana: umask 077 (보안 강화 설정)  
* eve: umask 002 (그룹 협업 친화적 설정)

**명령어를 작성하세요:**

- \# 6-2 답안 작성란  
```shell

```   
  ---

  ## **8\. 실행 권한 및 스크립트 관리**

  ### **8-1. 스크립트 실행 환경 설정**

다음 스크립트 파일들의 실행 권한을 적절히 설정하세요:

* `shared/tools/deploy.sh`: developers 그룹만 실행 가능  
* `shared/tools/backup.sh`: alice와 diana만 실행 가능 (ACL 사용)  
* `company/departments/dev/build.sh`: 소유자만 실행 가능

**명령어를 작성하세요:**

- \# 8-1 답안 작성란  
```shell

```

  ### **8-2. 시스템 스크립트 보안 설정**

시스템 관리용 스크립트를 다음과 같이 설정하세요:

* root 소유의 시스템 관리 스크립트 생성 (system\_check.sh)  
* 일반 사용자가 sudo 없이 실행할 수 있도록 SetUID 설정  
* 실행 로그를 남기도록 권한 설정

**명령어를 작성하세요:**

- \# 8-2 답안 작성란  
```shell

```    
  ---

  ## **9\. 디렉터리별 접근 제어**

  ### **9-1. 계층적 접근 제어**

다음과 같은 계층적 접근 권한을 설정하세요:

* `company/` : 모든 직원이 읽기 가능  
* `company/departments/` : 각 부서원만 해당 부서 디렉터리 접근 가능  
* `company/departments/finance/` : managers 그룹만 접근 가능  
* `company/projects/` : 프로젝트 참여자만 해당 프로젝트 접근 가능

**명령어를 작성하세요:**

- \# 9-1 답안 작성란  
```shell

```
  ### **9-2. 임시 작업 공간 설정**

임시 작업을 위한 공간을 다음과 같이 설정하세요:

* `temp/` 디렉터리 생성 (모든 사용자가 파일 생성 가능)  
* Sticky Bit 설정으로 자신의 파일만 삭제 가능  
* 1주일 후 자동 삭제되도록 권한 설정 (cron 작업용)

**명령어를 작성하세요:**

- \# 9-2 답안 작성란  
```shell

```  
  ---

  ## **10\. 백업 및 아카이브 권한 관리**

  ### **10-1. 백업 파일 보안**

백업 관련 파일들의 보안을 다음과 같이 설정하세요:

* `backup/daily/` : developers 그룹이 백업 생성 가능, 읽기 전용  
* `backup/weekly/` : managers만 접근 가능  
* `backup/monthly/` : root만 접근 가능  
* 모든 백업 파일은 생성 후 읽기 전용으로 자동 변경

**명령어를 작성하세요:**

- \# 10-1 답안 작성란  
```shell

```    
  ---

  