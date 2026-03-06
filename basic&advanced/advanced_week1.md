# SQL_ADVANCED 1주차 정규 과제 

## Week 1 : 서브쿼리 & CTE

📌**SQL_ADVANCED 정규과제**는 매주 정해진 주제에 따라 **MySQL 공식 문서 또는 한글 블로그 자료를 참고해 개념을 정리한 후, 프로그래머스 SQL 3문제**와 **추가 확인문제**를 직접 풀어보며 학습하는 과제입니다. 

이번 주는 아래의 **SQL_ADVANCED_0th_TIL**에 나열된 주제를 중심으로 개념을 학습하고, 주차별 **학습 목표**에 맞게 정리해주세요. 정리한 내용은 GitHub에 업로드한 후, **스프레드시트의 'SQL' 시트에 링크를 제출**해주세요. 



**👀 (수행 인증샷은 필수입니다.)** 

> 프로그래머스 문제를 풀고 '정답입니다' 문구를 캡쳐해서 올려주시면 됩니다. 



## SQL_ADVANCED_1st_TIL 

### 15.2.15. SubQueries

#### 특히 15.2.15.1 ~ 15.2.15.7 (Scalar, EXISTS, Correlated, Derived 등) 

### 15.2.20 WITH (Common Table Expressions)

- `WITH RECURSIVE`에 대한 내용은 추후에 공부합니다. 해당 링크에서 `WITH`에 해당하는 부분만 정리해보세요. 




## 🏁 주차별 학습 (Study Schedule)

| 주차  | 공부 범위               | 완료 여부 |
| ----- | ----------------------- | --------- |
| 1주차 | 서브쿼리 & CTE          | ✅         |
| 2주차 | 집합 연산자 & 그룹 함수 | 🍽️         |
| 3주차 | 윈도우 함수             | 🍽️         |
| 4주차 | Top N 쿼리              | 🍽️         |
| 5주차 | 계층형 질의와 셀프 조인 | 🍽️         |
| 6주차 | PIVOT / UNPIVOT         | 🍽️         |
| 7주차 | 정규 표현식             | 🍽️         |

<br>


### 공식 문서 활용 팁

>  **MySQL 공식 문서는 영어로 제공되지만, 크롬 브라우저에서 공식 문서를 열고 이 페이지 번역하기에서 한국어를 선택하면 번역된 버전으로 확인할 수 있습니다. 다만, 번역본은 문맥이 어색한 부분이 종종 있으니 영어 원문과 한국어 번역본을 왔다 갔다 하며 확인하거나, 교육팀장의 정리 예시를 참고하셔도 괜찮습니다.**



# 1️⃣ 학습 내용 

> 아래의 링크를 통해 *MySQL 공식문서*로 이동하실 수 있습니다.
>
> - SubQueries : MySQL 공식문서 
>
> https://dev.mysql.com/doc/refman/8.0/en/subqueries.html
>
> (한국어 버전)
> https://dart-b-official.github.io/posts/mysql-subqueries/


> - CTE(공통 테이블 표현식) : MySQL 공식문서
>
> https://dev.mysql.com/doc/refman/8.0/en/with.html
>
> (한국어 버전)
> https://dart-b-official.github.io/posts/mysql-cte/

<br>
<br>
<!-- 여기까진 그대로 둬 주세요-->





# 2️⃣ 학습 내용 정리하기

---

 # 1. 서브쿼리

~~~
✅ 학습 목표 :
* SubQueries에 대한 문법을 이해하고 활용할 수 있다.  
~~~

### 1. 서브쿼리
#### 서브쿼리의 주요 장점
1) 쿼리를 구조적으로 분리하여 각 부분을 쉽게 파악할 수 있음
2) 복잡한 JOIN 이나 UNION을 대체할 수 있는 방법 제공
3) 많은 사람들이 복잡한 조인보다 서브쿼리를 더 읽기 쉽다고 느낌

```SQL
DELETE FROM t1
WHERE s11 > ANY
  (SELECT COUNT(*) FROM t2
    WHERE NOT EXISTS
      (SELECT * FROM t3
        WHERE ROW(5*t2.s1 FROM t4 UNION SELECT 50, 77 FROM
        (SELECT * FROM t5))));
```
```
이게 뭔데
```

#### 서브쿼리 결과 형태
- Scalar : 단일 값 반환
- Column : 단일 컬럼(여러 행 가능)
- Row : 단일 행(여러 컬럼 가능)
- Table : 다수의 행과 컬럼

#### 서브쿼리에서 허용되는 요소
서브쿼리는 일반 SELECT 문에서 허용되는 대부분의 요소를 포함할 수 있음
- DISTINCT, GROUP BY, ORDER BY, LIMIT
- JOIN, 인덱스 힌트, UNION, 함수, 주석 등
- 외부구문: SELECT, INSERT, UPATE, DELETE, SET, DO

>다음의 세 쿼리는 모두 동일한 결과 반환!
```SQL
SELECT * FROM tt
  WHERE b > ANY (VALUES ROW(2), ROW(4), ROW(6));
```
```SQL
SELECT * FROM tt
  WHERE b > ANY (SELECT * FROM ts);
```
```SQL
SELECT * FROM tt
  WHERE b > ANY (TABLE ts);
```

### 2. 스칼라 서브쿼리
- 가장 단순한 형태의 서브쿼리
- 단순한 피연산자로, 단일 컬럼 값이나 리터럴이 허용되는 거의 모든 곳에서 사용 가능
- 데이터 타입, 길이, NULL 가능 여부 등의 특성을 가짐
```SQL
CREATE TABLE t1 (s1 INT, s2 CHAR(5) NOT NULL);
INSERT INTO t1 VALUES(100, 'abcde');
SELECT (SELECT s2 FROM t1);
```
➡️ 결과값 abcde 반환
- CHAR 타입, 길이 5, 테이블 생성 시점의 기본 문자셋과 콜레이션을 따름
- 결과 값은 NULL 이 될 수 있음
  - NULL 가능성 : 원래 컬럼 정의(NOT NULL)과 무관
  - ex. s2가 NOT NULL 로 정의되어도, t1이 비어있다면 결과는 NULL

#### 스칼라 서브쿼리 사용 제한
어떤 문법은 리터럴 값만 허용합니다.
- ex. LIMIT -> 정수 리터럴만 허용
  - 서브쿼리를 허용한다면, 먼저 다른 쿼리를 실행해서 나온 결과를 보고서 LIMIT 을 정해야 하는데, 옵티마이저가 실행 계획을 세우는 시점에는 그 값이 아직 존재하지 않음
- ex. LOAD DATA -> 문자열 리터럴 파일 이름만 허용
  - 파일 경로를 미리 알아야 OS 레벨에서 접근할 수 있음. 파일 경로가 실행 도중 서브쿼리로 결정된다면 파일 접근 자체를 준비할 수 없음.
```
리터럴(literal) : 소스 코드에 직접 써놓은 값 자체(고정된 상수값)
- ex. 정수 리터럴, 문자열 리터럴, 날짜 리터럴, 불리언(bool type)리터럴

리터럴이 아닌것(Expression, Variable, Subquery 등) : 실행해야 값이 나오는 것들
- ex.컬럼 참조, 연산식, 함수 호출, 서브쿼리, 변수
```
```SQL
-- 리터럴 사용 🅾️
SELECT * FROM users LIMIT 10;

-- 리터럴 아님 -> 에러 ⚠️
SELECT * FROM users LIMIT (SELECT COUNT(*) FROM orders);
```
#### 표현식(Expression)안에서 스칼라 서브쿼리 사용

```SQL
SELECT UPPER((SELECT s1 FROM t1)) FROM t2;

--MySQL 8.0.19 이후 버전
SELECT UPPER((TABLE t1)) FROM t2;
```
```
❓표현식(Expression)
- 값을 계산하거나 반환할 수 있는 모든 것
- 리터럴 + 컬럼 + 연산 + 함수+ 서브쿼리 모두 표현식의 일부
```

### 3. Comparisons Using Subqueries
```SQL
-- 가장 일반적인 사용 방식
non_subquery_operand comparision_operator(subquery)

--comparison_operator example
=, >, <, >=, <=, <>, !=, <=>
```
- 과거에는 서브쿼리를 비교 연산자의 오른쪽에서만 사용할 수 있었고, 지금도 일부 오래된 DBMS 는 이 규칙을 강제하기도 함

>하단의 쿼리는 모두 JOIN으로는 표현할 수 없는 쿼리이다.
```SQL
SELECT * FROM t1
  WHERE column1 = (SELECT MAX(column2) FROM t2);
```
- t2 테이블의 column2의 최댓값과 같은 값을 가지는 t1의 모든 행을 찾음

```SQL
SELECT * FROM t1 AS t
  WHERE 2 = (SELECT COUNT(*) FROM t1 WHERE t1.id=t.id);
```
- t1테이블에서 특정값이 2번 등장하는 경우를 찾음

#### ⚠️주의사항
- 서브쿼리를 스칼라 값과 비교하려면, 해당 서브쿼리는 반드시 단일 값을 반환해야함
  - 여러 행이 반환되면 안됨
- 서브쿼리를 Row Constructor 와 비교하려면, 해당 서브쿼리는 Row Subquery 여야 하며, 반환되는 값의 개수가 Row Constructor 와 동일해야함
  - Row Subquery : 여러 컬럼을 반환하는 서브 쿼리

### 4. ANY, IN, SOME, ALL
#### ANY
```SQL
SELECT s1 FROM t1 WHERE s1 > ANY (SELECT s1 FROM t2);
```
- 조건 : 비교 연산자 뒤에 와야함
- 서브쿼리에서 반환된 값들 중 하나라도 조건을 만족하면 TRUE 반환
- TRUE가 아닌 경우 (t1에 10이라는 행 존재)
  - t2가 (20, 10)이라면 10 미만인 행이 없으니 FALSE
  - t2가 비어있으면 FALSE
  - t2가 NULL값으로 채워져있으면 결과는 NULL

#### IN
```SQL
SELECT s1 FROM t1 WHERE s1 = ANY (SELECT s1 FROM t2);
SELECT s1 FROM t1 WHERE s1 IN (SELECT s1 FROM t2);
```
➡️ 두 쿼리의 결과값은 동일함
- IN은 =ANY의 별칭
- ⚠️주의사항 : IN과 =ANY가 항상 동의어는 아님
  - IN은 **표현식 리스트**도 받을 수 있음
  - =ANY는 **서브쿼리**만 사용 가능
  - NOT IN 도 <>ANY와 항상 동의어는 아님

#### SOME
```SQL
SELECT s1 FROM t1 WHERE s1 <> ANY (SELECT s1 FROM t2);
SELECT s1 FROM t1 WHERE s1 <> SOME (SELECT s1 FROM t2);
```
➡️ 두 쿼리의 결과값은 동일함
- SOME 은 ANY의 또 다른 별칭
- ⚠️주의사항 : 잘 사용되진 않지만, 의미를 더 명확하게 전달할 때 유용
  - <>ANY의 의미
    - 🅾️ a와 같지 않은 b가 하나라도 존재한다 (하나라도 다르면 참)
    - ❌ 모든 b가 a와 같지 않다 (= <>ALL)
  ➡️ *하나라도 다르면 참* 이라는 말을 더 직관적으로 받아들일 수 있게 <>SOME 을 사용하는 것

#### ALL
```SQL
SELECT s1 FROM t1 WHERE s1 > ALL (SELECT s1 FROM t2);
```
- 조건 : 비교 연산자 뒤에 와야 함
- 서브쿼리에서 반환된 모든 값에 대해 비교 결과가 TRUE이면 TRUE 반환
- 빈 테이블과 NULL 값 처리
```SQL
-- t2가 비어있는 경우
SELECT * FROM t1 WHERE 1 > ALL (SELECT s1 FROM t2);
```
➡️ 결과는 TRUE 
  - ❓WHY? t2가 비어있으면 비교 대상이 없으므로, 조건 위반 사례도 없어서 TRUE

```SQL
SELECT * FROM t1 WHERE 1 > (SELECT s1 FROM t2);
SELECT * FROM t1 WHERE 1 > ALL (SELECT MAX(s1) FROM t2);
```
➡️ **이 때는 NULL 값 반환**
  - NULL 값과 빈 테이블은 서브쿼리 작성 시 반드시 고려해야하는 엣지 케이스임

#### NOT IN과 ALL 의 관계
- NOT IN은 <>ALL 의 별칭
```SQL
SELECT s1 FROM t1 WHERE s1 <> ALL (SELECT s1 FROM t2);
SELECT s1 FROM t1 WHERE s1 NOT IN (SELECT s1 FROM t2);
```
➡️ 같은 결과 반환

- ALL과 NOT IN 구문에서 TABLE 사용 조건
  - 서브쿼리에서 참조하는 테이블은 단일 컬럼만 포함해야함
  - 서브쿼리가 컬럼 표현식에 의존하지 않아야함
```SQL
--🅾️ 
SELECT s1 FROM t1 WHERE s1 <> ALL(TABLE t2);
--❌
SELECT * FROM t1 WHERE 1 > ALL(SELECT MAX(s1) FROM t2);
```
❓WHY : MAX(s1)과 같은 컬럼 표현식에 의존하기 때문에 불가능 

### ROW 서브쿼리
```SQL
SELECT * FROM t1
  WHERE (col1, col2) = (SELECT col3, col4 FROM t2 WHERE id=10);

SELECT * FROM t1
  WHERE ROW(col1, col2) = (SELECT col3, col4 FROM t2 WHERE id = 10);
```
- t2에 id=10인 **단일 행**이 존재할 경우 ➡️ TRUE/FALSE
- 서브쿼리가 **행을 반환하지 않으면** ➡️ 결과는 NULL
- 서브쿼리가 2개 이상의 행을 반환하면 ➡️ 오류 발생
  - ❓Row 서브쿼리는 최대 1개의 행만 반환 가능

#### ROW Constructor
- Row Constructor 와 Row 서브쿼리가 반환하는 행은 동일한 개수의 값을 가져야함
  - 2개 이상의 컬럼을 반환하는 서브쿼리와 비교할 때 Row Consturctor 사용 가능
  - 단일 컬럼만 반환하는 서브쿼리는 **스칼라 값**으로 취급되므로 Row Constructor 사용 불가
```SQL
SELECT * FROM t1 WHERE ROW(1) = (SELECT column1 FROM t2);
```
➡️ 단일 컬럼 반환이므로 문법 오류

### EXISTS, NOT EXISTS
- EXISTS 서브쿼리 : 서브쿼리가 한 행이라도 반환하면 TRUE
- NOT EXISTS 서브쿼리 : 서브쿼리가 아무 행도 반환하지 않으면 TRUE
```SQL
-- 한 개 이상의 도시에서 존재하는 상점 유형 찾기
SELECT DISTINCT store_type FROM stores
  WHERE EXISTS (
    SELECT * FROM cities_stores
      WHERE cities_stores.store_type = stores.store_type
  );
```
```SQL
--어느 도시에도 존재하지 않는 상점 유형 찾기
SELECT DISTINCT store_type FROM stores
  WHERE NOT EXISTS(
    SELECT * FROM cities_stores
      WHERE cities_stores.store_type = stores.store_type
  );
```
```SQL
-- 모든 도시에 존재하는 상점 유형 찾기(이중 NOT EXISTS 구조)
SELECT DISTINCT store_type FROM stores
  WHERE NOT EXISTS(
    SELECT * FROM cities WHERE NOT EXISTS(
      SELECT * FROM cities_stores
        WHERE cities_stores.city = cities.city
          AND cities_sotres.store_type = sotres.store_type
    )
  );
```
### 상관 서브쿼리
- 서브쿼리 안에서 외부 쿼리에 있는 테이블을 참조하는 경우를 의미
```SQL
SELECT * FROM t1
  WHERE column1 = ANY(
    SELECT column1 FROM t2
    WHERE t2.column2 = t1.column2
  );
```
- 비록 t1이 서브쿼리의 FROM 절에 없지만, 외부쿼리에서 찾아 참조 가능

#### 스코프 규칙
- 안쪽에서 바깥쪽 순서로 평가
```SQL
SELECT column1 FROM t1 AS x
  WHERE x.column1 = (
    SELECT column1 FROM t2 AS x
      WHERE x.column1 = (
        SELECT column1 FROM t3
          WHERE x.column2 = t3.column1
      )
  );


#### 옵티마이저 최적화
- subquery_to_derived 플래그가 켜져 있으면, 상관 스칼라 서브쿼리 -> 파생 테이블 변환 가능

##### 변환 조건




# 2. CTE

~~~
✅ 학습 목표 :
* CTE에 대한 문법을 이해하고 활용할 수 있다. 
~~~

<!-- 새롭게 배운 내용을 자유롭게 정리해주세요.-->
```

```

```

```
<br>

<br>

---

# 3️⃣ 실습 문제

**두 문제 중에서 한 문제는 SubQuery와 CTE를 사용한 방법을 각각 활용해서 2개의 답변을 제시해주세요**

## 프로그래머스 문제 

https://school.programmers.co.kr/learn/courses/30/lessons/131123

> 즐겨찾기가 가장 많은 식당 정보 출력하기 (GROUP BY, SubQuery) : Lev 3

https://school.programmers.co.kr/learn/courses/30/lessons/131115

> 가격이 제일 비싼 식품의 정보 출력하기 (SUM, MAX, MIN, SubQuery) : Lev 2



---

## 문제 인증란

<!-- 이 주석을 지우고 여기에 문제 푼 인증사진을 올려주세요. -->



---


## 문제 1

> **🧚예린이는 최근 여러 주문 데이터를 분석하는 업무를 맡게 되었습니다. 특정 고객의 주문 이력을 분석하기 위해, 다음과 같이 최근 30일간 주문만 필터링한 CTE를 사용해 쿼리를 작성했습니다.**

~~~sql
WITH RecentOrders AS (
  SELECT *
  FROM Orders
  WHERE order_date >= DATE_SUB(CURDATE(), INTERVAL 30 DAY)
)
SELECT customer_id, COUNT(*) AS recent_order_count
FROM RecentOrders
GROUP BY customer_id;
~~~

> **그런데 예린이는 "이 쿼리를 WITH 없이, 서브쿼리 방식으로 바꿔서 실행해보라" 는 피드백을 받았고, 서브쿼리로 작성해보려 했지만 익숙하지 않아 SQL_ADVANCED를 듣는 학회원분들에게 도움을 요청하고 있습니다. 예린이의 쿼리를 WITH 없이 서브쿼리로 변환해보세요. 그리고 두 방식의 차이점을 설명해보고, 각각의 장단점을 정리해보세요**

```SQL
SELECT  customer_id,
        COUNT(*) AS recent_order_count
FROM (
  SELECT customer_id
  FROM Orders
  WHERE order_date >= DATE_SUB(CURDATE(), INTERVAL 30 DAY)
) AS RecentOrders
GROUP BY customer_id;
```
```

```

## 참고자료

서브쿼리를 사용하는 이유가 너무 어려우신 분들을 위해 참고자료를 첨부합니다. 아래 블로그를 통해서 더욱 쉽게 공부해보시고 문제를 풀어보세요.

1. [SQL] 서브쿼리는 언제 쓰는걸까? 
   https://project-notwork.tistory.com/38

2. [SQLD] 서브 쿼리 (SubQeury) 개념 및 종류
   https://bommbom.tistory.com/entry/%EC%84%9C%EB%B8%8C-%EC%BF%BC%EB%A6%ACSub-Query-%EA%B0%9C%EB%85%90-%EB%B0%8F-%EC%A2%85%EB%A5%98


### 🎉 수고하셨습니다.