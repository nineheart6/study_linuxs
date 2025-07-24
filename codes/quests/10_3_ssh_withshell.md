# **Shell 조건문 및 텍스트 처리 실습 문제**

## **실습 환경 설정**

### **1\. 실습 디렉토리 생성 및 이동**

mkdir \~/shell\_practice

cd \~/shell\_practice

### **2\. 실습용 데이터 파일 생성**

#### **students.txt 파일 생성**

cat \> students.txt \<\< EOF

김철수:수학:85:영어:92:과학:78

이영희:수학:95:영어:88:과학:91

박민수:수학:76:영어:79:과학:82

최지원:수학:88:영어:95:과학:89

정우진:수학:92:영어:76:과학:94

김지현:수학:83:영어:91:과학:87

이준호:수학:79:영어:84:과학:76

박서연:수학:97:영어:93:과학:96

한도윤:수학:81:영어:77:과학:83

송민재:수학:86:영어:89:과학:91

EOF

#### **server\_logs.txt 파일 생성**

cat \> server\_logs.txt \<\< EOF

2024-01-15 10:30:15 INFO User login successful: user001

2024-01-15 10:31:22 ERROR Database connection failed

2024-01-15 10:32:05 INFO User login successful: user002

2024-01-15 10:33:18 WARNING Memory usage high: 85%

2024-01-15 10:34:25 ERROR Authentication failed: user003

2024-01-15 10:35:10 INFO User logout: user001

2024-01-15 10:36:33 ERROR Network timeout occurred

2024-01-15 10:37:45 INFO System backup started

2024-01-15 10:38:52 WARNING Disk space low: 90%

2024-01-15 10:39:15 ERROR Database connection failed

2024-01-15 10:40:28 INFO User login successful: user004

2024-01-15 10:41:35 ERROR Authentication failed: user005

EOF

#### **sales\_data.txt 파일 생성**

cat \> sales\_data.txt \<\< EOF

2024-01,서울,노트북,1500000

2024-01,부산,스마트폰,800000

2024-01,대구,태블릿,600000

2024-01,서울,스마트폰,850000

2024-02,부산,노트북,1450000

2024-02,서울,태블릿,620000

2024-02,대구,스마트폰,780000

2024-02,서울,노트북,1520000

2024-03,부산,태블릿,590000

2024-03,대구,노트북,1480000

2024-03,서울,스마트폰,820000

2024-03,부산,스마트폰,790000

EOF

#### **words.txt 파일 생성**

cat \> words.txt \<\< EOF

apple

banana

Apple

cherry

BANANA

date

Cherry

elderberry

Apple

fig

banana

CHERRY

EOF

---

## **실습 문제**

### **문제 1: 학생 성적 분석기** 

**파일:** `grade_analyzer.sh`

**요구사항:**

* `students.txt` 파일을 분석하여 다음 기능을 수행하는 스크립트 작성  
* 사용자로부터 과목명을 입력받아 해당 과목의 통계 정보 출력  
* 조건문을 사용하여 입력 검증 및 등급 분류 수행

**구현해야 할 기능:**

1. 사용자가 입력한 과목이 유효한지 검사 (수학, 영어, 과학)  
2. 해당 과목의 모든 점수를 추출하여 정렬된 목록 출력  
3. 최고점, 최저점, 평균점수 계산  
4. 90점 이상(A), 80점 이상(B), 70점 이상(C), 그 외(D) 등급별 학생 수 출력  
5. 평균이 85점 이상이면 "우수", 75점 이상이면 "양호", 그 외는 "보통" 출력

**힌트:**

* `cut`, `grep`, `sort`, `wc` 명령어 활용  
* 파이프라인으로 명령어 연결  
* 조건문으로 점수 범위 검사

```bash
#!/bin/bash

read -p "subject: " V_SUBJECT
if [ "$V_SUBJECT" = "수학" ]; then
        cut -d":" -f3 students.txt | sort -nr > temp_math.txt
        cat temp_math.txt
        echo "high" && cat temp_math.txt | head -1
        echo "low" && cat temp_math.txt | tail -1
        #average
        average=$(cat temp_math.txt | awk '{sum+=$1} END {print sum/NR}')
        echo "average $average"
        echo "A" && cat temp_math.txt | grep "^9" | wc -l
        echo "B" && cat temp_math.txt | grep "^8" | wc -l
        echo "C" && cat temp_math.txt | grep "^7" | wc -l
        int_average=$(echo "$average" | cut -d'.' -f1)
        if [ "$int_average" -ge 85 ]; then
                echo "우수"
        elif [ "$int_average" -ge 75 ]; then
                echo "양호"
        else
                echo "보통"
        fi
elif [ "$V_SUBJECT" = "영어" ]; then
        echo "test"
elif [ "$V_SUBJECT" = "과학" ]; then
        echo "test"
else
        echo "ERROR"
fi

```

---

### **문제 2: 서버 로그 모니터링 도구**

**파일:** `log_monitor.sh`

**요구사항:**

* `server_logs.txt` 파일을 분석하여 로그 수준별 통계 및 문제 상황 감지  
* 조건문을 사용하여 경고 수준 결정

**구현해야 할 기능:**

1. 전체 로그 라인 수 출력  
2. ERROR, WARNING, INFO 각각의 개수 계산 및 출력  
3. ERROR 로그만 별도 파일(`errors.log`)로 저장  
4. 가장 많이 발생한 ERROR 유형 찾기 (중복 제거 후 개수 확인)  
5. ERROR 비율이 30% 이상이면 "위험", 20% 이상이면 "주의", 그 외는 "정상" 출력  
6. 마지막 5개 로그 항목을 시간 역순으로 출력

**힌트:**

* `grep`, `wc`, `uniq`, `sort`, `tail` 명령어 활용  
* 리다이렉션으로 파일 저장  
* 수치 계산을 위한 조건문 사용

---

### **문제 3: 판매 데이터 분석 시스템**

**파일:** `sales_analyzer.sh`

**요구사항:**

* `sales_data.txt` 파일을 분석하여 매출 데이터 통계 생성  
* 사용자 입력에 따른 다양한 분석 결과 제공

**구현해야 할 기능:**

1. 사용자로부터 분석 타입 입력받기 (월별/지역별/제품별)  
2. 입력값이 유효하지 않으면 사용법 출력 후 종료  
3. 선택한 분석 타입에 따라 다음 수행:  
   * **월별**: 각 월의 총 매출액을 높은 순으로 정렬하여 출력  
   * **지역별**: 각 지역의 총 매출액과 평균 매출액 출력  
   * **제품별**: 각 제품의 판매 횟수와 총 매출액 출력  
4. 전체 매출액이 1000만원 이상이면 "목표 달성", 800만원 이상이면 "양호", 그 외는 "노력 필요" 출력

**힌트:**

* `cut`, `sort`, `uniq`, `grep` 명령어 조합  
* 필드 분리를 위한 `-d` 옵션 활용  
* 조건문으로 분석 타입 분기 처리

---

### **문제 4: 단어 빈도 분석기**

**파일:** `word_frequency.sh`

**요구사항:**

* `words.txt` 파일의 단어들을 분석하여 빈도수 계산  
* 대소문자 처리 옵션 제공

**구현해야 할 기능:**

1. 사용자로부터 대소문자 구분 여부 입력받기 (y/n)  
2. 입력값 검증 (y 또는 n이 아니면 재입력 요구)  
3. 대소문자 구분하지 않는 경우:  
   * 모든 단어를 소문자로 변환  
   * 중복 제거 후 빈도수와 함께 출력  
   * 빈도수 기준 내림차순 정렬  
4. 대소문자 구분하는 경우:  
   * 원본 그대로 중복 제거 후 빈도수 계산  
5. 총 고유 단어 개수 출력  
6. 가장 빈도가 높은 단어가 3회 이상 나타나면 "높은 중복도", 2회면 "보통 중복도", 1회면 "낮은 중복도" 출력

**힌트:**

* `tr`, `sort`, `uniq`, `wc` 명령어 활용  
* 대소문자 변환을 위한 `tr A-Z a-z`  
* 조건문으로 사용자 선택 처리

---

## **제출 방법**

1. 각 문제별로 스크립트 파일 작성  
2. 스크립트 실행 권한 부여: `chmod +x *.sh`  
3. 각 스크립트 실행하여 정상 동작 확인  
4. 실행 결과 스크린샷과 함께 스크립트 파일 제출

## **평가 기준**

* **정확성 (40%)**: 요구사항 충족도 및 결과의 정확성  
* **코드 품질 (30%)**: 조건문 사용, 에러 처리, 가독성  
* **명령어 활용 (20%)**: 텍스트 처리 명령어와 파이프라인 적절한 사용  
* **창의성 (10%)**: 추가 기능 구현 및 사용자 편의성

## **실습 시 주의사항**

1. 스크립트 첫 줄에 `#!/bin/bash` 추가  
2. 변수명은 의미있게 작성  
3. 사용자 입력값 검증 필수  
4. 에러 상황에 대한 적절한 메시지 출력  
5. 코드에 주석 추가하여 가독성 향상

