# SQL_ADVANCED 2주차 정규 과제 

## Week 2 :집합 연산자 & 그룹 함수

📌**SQL_ADVANCED 정규과제**는 매주 정해진 주제에 따라 **MySQL 공식 문서 또는 한글 블로그 자료를 참고해 개념을 정리한 후, 프로그래머스/ Solvesql / LeetCode에서 SQL 3문제**와 **추가 확인문제**를 직접 풀어보며 학습하는 과제입니다. 

이번 주는 아래의 **SQL_ADVANCED_2nd_TIL**에 나열된 주제를 중심으로 개념을 학습하고, 주차별 **학습 목표**에 맞게 정리해주세요. 정리한 내용은 GitHub에 업로드한 후, **스프레드시트의 'SQL' 시트에 링크를 제출**해주세요. 



**👀 (수행 인증샷은 필수입니다.)** 

> 프로그래머스 문제를 풀고 '정답입니다' 문구를 캡쳐해서 올려주시면 됩니다. 



## SQL_ADVANCED_2nd

**1. 집합 연산자**

### 15.2.18. UNION Clause

### 15.2.14. Set Operations with UNION, INTERSECT

- UNION, UNION ALL 중심으로 개념을 정리하고, INTERSECT, EXCEPT는 구문이 어떤 기능을 하는지 간단히만 알아봅니다. EXCEPT와 INTERSECT는 대부분 MySQL 버전에서 공식 지원되지 않기 때문에, **이번주 학습은 `UNION, UNION ALL` 만 집중적으로 정리해주세요.**

**2. 그룹 함수 (집계 함수)**

### 14.19.1. Aggregate Function Descriptions



## 🏁 주차별 학습 (Study Schedule)

| 주차  | 공부 범위               | 완료 여부 |
| ----- | ----------------------- | --------- |
| 1주차 | 서브쿼리 & CTE          | ✅         |
| 2주차 | 집합 연산자 & 그룹 함수 | ✅         |
| 3주차 | 윈도우 함수             | 🍽️         |
| 4주차 | Top N 쿼리              | 🍽️         |
| 5주차 | 계층형 질의와 셀프 조인 | 🍽️         |
| 6주차 | PIVOT / UNPIVOT         | 🍽️         |
| 7주차 | 정규 표현식             | 🍽️         |



### 공식 문서 활용 팁

>  **MySQL 공식 문서는 영어로 제공되지만, 크롬 브라우저에서 공식 문서를 열고 이 페이지 번역하기에서 한국어를 선택하면 번역된 버전으로 확인할 수 있습니다. 다만, 번역본은 문맥이 어색한 부분이 종종 있으니 영어 원문과 한국어 번역본을 왔다 갔다 하며 확인하거나, 교육팀장의 정리 예시를 참고하셔도 괜찮습니다.**



# 1️⃣ 학습 내용 

> 아래의 링크를 통해 *MySQL 공식문서*로 이동하실 수 있습니다.
>
> - 15.2.18. UNION Clause : MySQL 공식문서 
>
> https://dev.mysql.com/doc/refman/8.0/en/union.html
>
> - 15.2.14. Set Operations with UNION, INTERSECT : MySQL 공식문서
>
> https://dev.mysql.com/doc/refman/8.0/en/set-operations.html
>
> (한국어 버전) https://dart-b-official.github.io/posts/mysql-UNION/
>
> - 14.19.1. Aggregate Function Descriptions : MySQL 공식문서
>
> https://dev.mysql.com/doc/refman/8.0/en/aggregate-functions.html
>
> (한국어 버전) https://dart-b-official.github.io/posts/mysql-aggregate_function/



<!-- 여기까진 그대로 둬 주세요-->

# 2️⃣ 학습 내용 정리하기

## 1. 집합 연산자

~~~
✅ 학습 목표 :
* UNION과 UNION ALL의 차이와 사용법을 이해한다.
* 중복 제거 여부, 컬럼 정렬 조건 등을 고려하여 올바르게 집합 연산자를 사용할 수 있다. 
~~~

### 1) UNION / UNION ALL 핵심 정리

- **의미**
  - `UNION` = 결과를 **결합 + 중복 제거(DISTINCT)**  
  - `UNION ALL` = 결과를 **결합만(중복 허용)** → 보통 더 빠름
- **사용 규칙**
  - 각 블록(SELECT)의 **컬럼 개수 동일**해야 함
  - 같은 위치의 컬럼은 **타입 호환**되어야 하며, 결과 컬럼명은 **첫 번째 SELECT의 별칭/이름**을 따름
  - **정렬/제한**을 전체 결과에 적용하려면 **맨 마지막에 `ORDER BY / LIMIT`**  
    - 블록별 정렬/제한을 하려면 **각 SELECT를 괄호로 감싸기**
- **중복 판단**
  - 중복은 **전체 컬럼 튜플 기준**(NULL 포함 동일 튜플이면 1개만 남음)
- **성능 관점**
  - `UNION`은 내부적으로 **DISTINCT(정렬/해시)**가 들어가 임시 테이블이 생길 수 있음
  - **가능하면 `UNION ALL` 먼저 고려** → 의미상 중복 제거가 꼭 필요할 때만 `UNION` 사용

#### 예시
```sql
-- 1) 단순 결합 (중복 제거 필요 없음)
SELECT id, name FROM a
UNION ALL
SELECT id, name FROM b;

-- 2) 중복 제거가 필요한 경우
SELECT id, name FROM a
UNION
SELECT id, name FROM b;

-- 3) 블록별 정렬/제한 + 전체 정렬
(SELECT id, created_at FROM a ORDER BY created_at DESC LIMIT 5)
UNION ALL
(SELECT id, created_at FROM b ORDER BY created_at DESC LIMIT 5)
ORDER BY created_at DESC;

-- 4) 출처 열(tag) 추가로 후처리/분석 편의성 확보
SELECT id, name, 'A' AS src FROM a
UNION ALL
SELECT id, name, 'B' AS src FROM b;
```

#### ⚠️ 주의
- `ORDER BY`는 **전체 결과에 적용하려면 맨 마지막**에만 둡니다. 블록별 정렬/제한을 하려면 **각 SELECT를 괄호로 감싸기**가 필요합니다. 또한 최종 `ORDER BY`에서 사용할 **별칭/컬럼명은 첫 번째 SELECT**의 정의를 따름
- 특정 컬럼만 중복 제거하고 싶다면 `UNION` 대신
  ```sql
  SELECT DISTINCT col1, col2
  FROM (
    SELECT col1, col2 FROM A
    UNION ALL
    SELECT col1, col2 FROM B
  ) AS t;
  ```


## 2. 그룹함수

~~~
✅ 학습 목표 :
* COUNT, SUM, AVG, MAX, MIN 함수의 기본 사용법을 익힌다.
* GROUP BY와 HAVING 절을 적절히 활용할 수 있다.
* NULL과 집계 함수가 어떻게 상호작용하는지 이해한다. 
~~~

### 2) 그룹 함수(집계) 요약

- **핵심 개념**
  - 집계 함수는 여러 행을 하나의 값으로 요약합니다.
  - 기본 목표: `COUNT`, `SUM`, `AVG`, `MAX`, `MIN`의 동작, `GROUP BY`/`HAVING`의 역할, `NULL` 처리 이해.

#### 지원 함수와 동작 개요
- `COUNT(*)` : **NULL 포함 전체 행 수**를 셉니다.
- `COUNT(expr)` : `expr`이 **NULL이 아닌 행만** 셉니다.
- `COUNT(DISTINCT col)` / `COUNT(DISTINCT col1, col2)` : **중복 제거 후** 고유 튜플 개수.
- `SUM(expr)` : `expr`의 합(**NULL 무시**).
- `AVG(expr)` : `expr`의 평균(**NULL 무시**, 정수도 평균은 소수로 승격).
- `MAX(expr)` / `MIN(expr)` : 최댓값/최솟값(**NULL 무시**; 비교 가능한 타입 혼합 시 암묵 변환 가능).

#### NULL과의 상호작용 (중요)
- `SUM/AVG/MAX/MIN/COUNT(expr)`는 **NULL을 제외**하고 계산됩니다.
- `COUNT(*)`만 **NULL 포함** 전 행을 카운트합니다.
- NULL을 0처럼 다루고 싶으면 `COALESCE`로 명시적으로 변환하세요.

~~~sql
-- NULL 제외 동작 확인
SELECT
  SUM(amount)         AS sum_amount,   -- NULL 제외
  AVG(amount)         AS avg_amount,   -- NULL 제외
  COUNT(amount)       AS cnt_amount,   -- NULL 제외
  COUNT(*)            AS cnt_rows      -- NULL 포함 전체 행
FROM payments;

-- NULL을 0으로 간주
SELECT SUM(COALESCE(amount, 0)) AS total_amount
FROM payments;
~~~

#### GROUP BY와 HAVING
- `GROUP BY` : 그룹 키별로 집계. **집계되지 않은 모든 컬럼은 그룹 키에 포함**(ONLY_FULL_GROUP_BY 기준).
- `HAVING` : **그룹핑 이후** 조건(집계 함수 사용 가능).  
- `WHERE` : **그룹핑 이전** 조건(집계 함수 사용 불가).

~~~sql
-- 사전 필터(WHERE) 후 그룹 집계, 사후 필터(HAVING)
SELECT user_id, SUM(amount) AS total
FROM payments
WHERE status = 'PAID'                -- 그룹핑 이전 필터
GROUP BY user_id
HAVING SUM(amount) >= 100;           -- 그룹핑 이후 필터

-- GROUP BY 없이 전체 집계 조건(단일 그룹에 HAVING)
SELECT SUM(amount) AS total_amount
FROM payments
HAVING SUM(amount) > 1000;
~~~

#### DISTINCT와 집계
- 중복 제거가 필요하면 `DISTINCT`를 집계 인자에 적용합니다.
- 다중 컬럼 DISTINCT 가능(`COUNT(DISTINCT col1, col2)`).

~~~sql
-- 일별 고유 사용자 수(DAU)
SELECT event_date, COUNT(DISTINCT user_id) AS dau
FROM events
GROUP BY event_date;

-- 고유 (user_id, device) 튜플 수
SELECT COUNT(DISTINCT user_id, device) AS uniq_pairs
FROM sessions;
~~~

#### 타입/정밀도 주의
- `AVG(INT)`는 **소수 반환**(암묵 승격). 필요 시 `CAST/ROUND`로 스케일 고정.
- 합계가 커지면 정밀도 손실 가능 → `DECIMAL` 캐스팅 고려.

~~~sql
-- 평균을 소수 2자리로 고정
SELECT CAST(AVG(score) AS DECIMAL(10,2)) AS avg_score
FROM exams
WHERE subject = 'math';

-- 합계 정밀도 관리
SELECT CAST(SUM(amount) AS DECIMAL(18,2)) AS total_amount
FROM payments;
~~~

#### 성능 팁
- **선제 WHERE 필터링**으로 대상 행을 줄여 집계 비용 절감.
- 그룹 키/정렬 키에 **적절한 인덱스**가 있으면 임시 테이블/파일 정렬을 줄일 수 있음.
- 필요 이상으로 `DISTINCT`/`HAVING`을 사용하지 말고, 가능하면 `WHERE`로 먼저 제한.

#### 자주 틀리는 포인트
- `WHERE`에 집계 함수를 쓰면 오류. **집계 기반 조건은 `HAVING`**에.
- `ONLY_FULL_GROUP_BY` 활성화 시, **집계되지 않은 컬럼은 모두 `GROUP BY`에 포함**(또는 함수적 종속성 충족).
- `COUNT(col)`은 NULL을 세지 않습니다. **전체 행 수**가 필요하면 `COUNT(*)`.
- 평균/합계 반환 스케일이 기대와 다를 수 있음 → **명시적 `CAST/ROUND`**로 제어.

##### 작은 종합 예시
~~~sql
-- 상태별 결제 요약: 건수/고객수/합계/평균(소수 2자리), 상위 그룹만 필터
SELECT
  status,
  COUNT(*)                         AS cnt_rows,          -- 전체 행 수
  COUNT(DISTINCT user_id)          AS uniq_users,        -- 고유 고객 수
  CAST(SUM(COALESCE(amount,0)) AS DECIMAL(18,2)) AS total_amount,
  CAST(AVG(amount) AS DECIMAL(10,2))              AS avg_amount
FROM payments
WHERE created_at >= CURRENT_DATE - INTERVAL 30 DAY
GROUP BY status
HAVING SUM(COALESCE(amount,0)) >= 1000
ORDER BY total_amount DESC;
~~~


<br><br>

---

# 3️⃣ 실습 문제

https://leetcode.com/problems/customers-who-never-order/

> LeetCode 183. Customers Who never Order
>
> 학습 포인트 : 주문 내역이 없는 고객을 찾기 위한 패턴 익히기  

https://leetcode.com/problems/department-highest-salary/description/

> LeetCode 184. Department Highest Salary
>
> 학습 포인트 : 부서별 최고 연봉자 추출을 위한 **그룹별 정렬 / 필터링** 방식 이해하기

---

## 문제 인증란

<!-- 이 주석을 지우고 여기에 문제 푼 인증사진을 올려주세요. -->



---

# 확인문제

## 문제 1

> **🧚동혁이는 SQL 문제를 풀면서 `UNION과 UNION ALL`의 차이를 명확히 이해하지 못해 중복된 값이 생기거나 누락되는 문제를 계속 겪고 있습니다.** 아래는 동혁이가 작성한 쿼리입니다.

~~~sql
SELECT name FROM member
UNION
SELECT name FROM blackList;
~~~

> **그런데 예상과 달리 blacklist에만 있는 이름이 결과에 안 나오거나, 중복된 이름이 사라져서 헷갈리고 있습니다. UNION과 UNION ALL의 차이를 설명하고, 중복 포함 여부에 따라 어떤 경우에 어떤 쿼리를 써야 하는지 예시와 함께 설명해주세요**

<br>

- **UNION**: 두 결과를 합친 뒤 **중복을 제거**(유니크한 이름 목록이 필요할 때)
- **UNION ALL**: 두 결과를 그대로 이어붙여 중복 유지(실제 건수/중복까지 보고 싶을 때)
```sql
SELECT name FROM member
UNION ALL
SELECT name FROM blacklist;
```


## 참고자료
그룹 함수가 많아서 중요하게 많이 쓰이는 함수들을 정리해놓은 참고자료를 첨부합니다. 아래 블로그를 통해서 더욱 쉽게 공부해보시고 문제를 풀어보세요.
1. [SQL 10] 그룹 함수, GROUP BY 절, HAVING 절
https://keep-cool.tistory.com/37

또한, MySQL 문서 이외에 Oracle 함수에서 사용하는 그룹함수에 대한 소개도 같이 첨부합니다. SQLD 시험 준비하시는 분이 있다면 이 자격증 시험에서는 Oracle 언어를 기반으로 문제가 출제하오니 아래 블로그도 같이 공부해보세요. (선택사항입니다.)
2. 그룹 함수 (Group FUNCTION)
https://dkkim2318.tistory.com/48

<br>

### 🎉 수고하셨습니다.