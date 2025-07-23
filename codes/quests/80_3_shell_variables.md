# **🧪 Shell Script 실습 문제 세트: "변수 중심 텍스트 분석"**

* 📁 디렉토리 및 파일 구조 생성 스크립트

mkdir \-p \~/shell\_practice/env

cd \~/shell\_practice/env

\# 샘플 파일 1: article.txt (단어 빈도수/정렬/고유단어 실습용)

cat \> article.txt \<\<EOF

Linux is an open-source operating system.

Linux is popular for servers and embedded systems.

Many developers use Linux for programming and automation.

EOF

\# 샘플 파일 2: logfile.txt (grep 실습용)

cat \> logfile.txt \<\<EOF

\[2025-07-23 14:00\] INFO Start processing

\[2025-07-23 14:01\] ERROR Failed to load file

\[2025-07-23 14:02\] INFO Retrying

\[2025-07-23 14:03\] ERROR Timeout

\[2025-07-23 14:04\] ERROR Disk full

EOF

\# 샘플 파일 3, 4: file1.txt, file2.txt (tail, diff 실습용)

cat \> file1.txt \<\<EOF

Line 1

Line 2

Last line A

EOF

cat \> file2.txt \<\<EOF

Line 1

Line 2

Last line B

EOF

\# 샘플 파일 5: people.txt (이메일 처리용)

cat \> people.txt \<\<EOF

Alice \<alice@gmail.com\>

Bob \<bob@naver.com\>

Charlie \<charlie@gmail.com\>

Diana \<diana@daum.net\>

Eve \<eve@naver.com\>

Frank \<frank@daum.net\>

Grace \<grace@gmail.com\>

Hank \<hank@naver.com\>

EOF

---

### **✅ \[문제 1\] 파일 내 단어 수 세기**

\# 문제 설명

사용자로부터 파일명을 입력받고, 해당 파일의 단어 수를 계산해서 출력하는 스크립트를 작성하세요.

\# 요구사항

\- read 명령어로 파일명 입력

\- 변수에 파일명 저장

\- wc 명령어 사용

🔧 예시 실행:

bash wordcount.sh

Enter filename: sample.txt

Word count in sample.txt: 123
```bash
[seungjae@localhost env]$ nano wordcount.sh

read -p "Enter filename: " V_FILENAME 
echo "Word count in $V_FILENAME: " && wc -c "$V_FILENAME"

[seungjae@localhost env]$ source wordcount.sh 
Enter filename: sample.sh
Word count in sample.sh: 
959 sample.sh

```
---

### **✅ \[문제 2\] 특정 단어 검색 및 빈도수 세기**

\# 문제 설명

스크립트 실행 시 단어(keyword)와 파일명을 인자로 받아 해당 단어의 등장 횟수를 출력하세요.

\# 요구사항

\- $1: 검색할 단어

\- $2: 파일명

\- grep, wc, 변수 사용

🔧 예시 실행:

bash count\_keyword.sh error logfile.txt

The word 'error' appeared 5 times.
```bash

[seungjae@localhost env]$ nano count_keyword.sh

V_KEYWORD="$1"
V_FILENAME="$2"

grep -i "$1" "$2" | wc -l

[seungjae@localhost env]$ source count_keyword.sh error logfile.txt 
3

```
---

### **✅ \[문제 3\] 고유 단어 목록 만들기**

\# 문제 설명

파일에서 고유한 단어만 추출하고, 정렬하여 새로운 파일로 저장하세요.

\# 요구사항

\- read 로 입력 받을 파일명

\- cut, tr, sort, uniq 조합

\- 변수 활용 및 리다이렉션 사용

🔧 예시 실행:

bash unique\_words.sh

Enter input file: article.txt

Unique words saved to: article\_unique.txt
```bash

[seungjae@localhost env]$ nano unique_words.sh

read -p "Enter input file: " V_FILE_NAME
cat article.txt | tr " " "\n" | sort | uniq > "unique.txt"
echo "Unique words saved to: unique.txt"

[seungjae@localhost env]$ source unique_words.sh 
Enter input file: article.txt
Unique words saved to: unique.txt
[seungjae@localhost env]$ cat unique.txt 
an
and
automation.
developers
embedded
for
is
Linux
Many
open-source
operating
popular
programming
servers
system.
systems.
use

```
---

### **✅ \[문제 4\] 두 파일의 마지막 줄 비교**

\# 문제 설명

두 개의 텍스트 파일을 인자로 받아 각각의 마지막 줄을 비교하고, 같으면 "Same", 다르면 "Different" 출력

\# 요구사항

\- 인자 $1, $2 활용

\- tail \-n 1, diff 사용

\- 임시 변수에 각 줄 저장

🔧 예시 실행:

bash compare\_lastline.sh file1.txt file2.txt

Result: Different
```bash
[seungjae@localhost env]$ nano compare_lastline.sh

V_FILEA="$1"
V_FILEB="$2"
V_LASTA=$(tail -n 1 "$1")
V_LASTB=$(tail -n 1 "$2")

if [ "$V_LASTA" = "$V_LASTB" ]; then
        echo "Same"
else
        echo "Differnt"
fi

[seungjae@localhost env]$ source compare_lastline.sh file1.txt file2.txt 
Differnt

```
---

### **✅ \[문제 5\] 이메일 리스트 정제 및 카운트**

\# 문제 설명

이메일이 섞인 텍스트 파일에서 이메일 주소만 추출하고 도메인별 개수를 출력하는 스크립트 작성

\# 요구사항

\- read로 파일명 받기

\- grep/awk, cut, sort, uniq \-c 활용

\- 결과를 정렬된 상태로 출력

🔧 예시 실행:

bash email\_domains.sh

Enter file name: people.txt

Output:

5 gmail.com

3 naver.com

2 daum.net
```bash

[seungjae@localhost env]$ nano email_domains.sh

read -p "Enter file name: " V_FILE   
cat "$V_FILE" | tr -d "<>" | cut -d"@" -f2 | sort | uniq -c | sort

[seungjae@localhost env]$ source email_domains.sh 
Enter file name: people.txt
      2 daum.net
      3 gmail.com
      3 naver.com
```
---

### **✅ \[문제 6\] 다단계 파이프라인 정제**

\# 문제 설명

사용자에게 파일명을 받아, 모든 단어를 소문자로 변환한 후, 단어 빈도수를 내림차순으로 정렬해 출력

\# 요구사항

\- read 사용

\- 변수에 파일명 저장

\- tr, sort, uniq \-c, sort \-nr 조합

\- 파이프라인 필수

🔧 예시 실행:

bash word\_freq\_sort.sh

Enter file to process: document.txt

Output:

45 the  

30 and  

20 python  
```bash
[seungjae@localhost env]$ nano word_freq_sort.sh

read -p "Enter file to process: " V_FILE
cat "$V_FILE" | tr "A-Z" "a-z" | tr " " "\n" | sort | uniq -c | sort -nr

[seungjae@localhost env]$ source word_freq_sort.sh 
Enter file to process: article.txt
      3 linux
      2 is
      2 for
      2 and
      1 use
      1 systems.
      1 system.
      1 servers
      1 programming
      1 popular
      1 operating
      1 open-source
      1 many
      1 embedded
      1 developers
      1 automation.
      1 an

```
...