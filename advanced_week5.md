# SQL_ADVANCED 5주차 정규 과제 

## Week 5 : 계층형 질의 & 셀프 조인

📌**SQL_ADVANCED 정규과제**는 매주 정해진 주제에 따라 **MySQL 공식 문서 또는 한글 블로그 자료를 참고해 개념을 정리한 후, 프로그래머스 SQL 문제 3문제**와 **추가 확인문제**를 직접 풀어보며 학습하는 과제입니다. 

이번 주는 아래의 **SQL_ADVANCED_5th_TIL**에 나열된 주제를 중심으로 개념을 학습하고, 주차별 **학습 목표**에 맞게 정리해주세요. 정리한 내용은 GitHub에 업로드한 후, **스프레드시트의 'SQL' 시트에 링크를 제출**해주세요. 



**(수행 인증샷은 필수입니다.)** 

> 프로그래머스 문제를 풀고 '정답입니다' 문구를 캡쳐해서 올려주시면 됩니다. 



## SQL_ADVANCED_5th

### 15.2.20 WITH (Common Table Expressions)

- **재귀 CTE를 통한 계층형 구조 탐색 방법을 중심으로 학습해주세요.**

> Self Join은 따로 MySQL 공식문서가 없습니다. 다른 블로그나 유튜브 영상을 참고하여 스스로 학습하고, 넣어주세요. 



## 🏁 강의 수강 (Study Schedule)

| 주차  | 공부 범위               | 완료 여부 |
| ----- | ----------------------- | --------- |
| 1주차 | 서브쿼리 & CTE          | ✅         |
| 2주차 | 집합 연산자 & 그룹 함수 | ✅         |
| 3주차 | 윈도우 함수             | ✅         |
| 4주차 | Top N 쿼리              | ✅         |
| 5주차 | 계층형 질의와 셀프 조인 | ✅         |
| 6주차 | PIVOT / UNPIVOT         | 🍽️         |
| 7주차 | 정규 표현식             | 🍽️         |



### 공식 문서 활용 팁

>  **MySQL 공식 문서는 영어로 제공되지만, 크롬 브라우저에서 공식 문서를 열고 이 페이지 번역하기에서 한국어를 선택하면 번역된 버전으로 확인할 수 있습니다. 다만, 번역본은 문맥이 어색한 부분이 종종 있으니 영어 원문과 한국어 번역본을 왔다 갔다 하며 확인하거나, 교육팀장의 정리 예시를 참고하셔도 괜찮습니다.**



# 1️⃣ 학습 내용

> 아래의 링크를 통해 *MySQL 공식문서*로 이동하실 수 있습니다.
>
> - 15.2.20 WITH (Common Table Expressions) : MySQL 공식문서 
>
> https://dev.mysql.com/doc/refman/8.0/en/with.html
>
> (한국어 버전) https://dart-b-official.github.io/posts/mysql-RecursiveWith/



<br>

---

# 2️⃣ 학습 내용 정리하기

## 1. 계층형 질의 (WITH RECURSIVE)

~~~
✅ 학습 목표 :
* 'WITH RECURSIVE' 문법을 활용해 계층형 구조를 탐색할 수 있다.
~~~

1) 개요 & 기본형

- 재귀 CTE는 자기 자신을 참조하는 CTE로, 계층/그래프/연속값 생성 같은 문제를 다룸

- 기본 구조(시드 + 재귀 부분):
```sql
WITH RECURSIVE cte (n) AS (
  SELECT 1             -- 비재귀(시드)
  UNION ALL
  SELECT n + 1 FROM cte -- 재귀
  WHERE n < 5
)
SELECT * FROM cte;
```
- WITH RECURSIVE를 빠뜨리면 Table 'cte_name' doesn’t exist 오류 발생

2) 구성 규칙

- CTE는 두 부분으로 이뤄짐:
  - (a) 비재귀(시드) SELECT: 초기 행 생성, CTE 미참조
  - (b) 재귀 SELECT: FROM에서 CTE를 1회 참조해 다음 단계 생성

- UNION ALL 또는 UNION DISTINCT로 연결
  - 중복 제거가 필요하면 DISTINCT

3) 타입 추론 & 문자열 폭

- 열 타입은 시드 SELECT만으로 결정되고, 모든 열은 NULL 허용으로 간주.
- 문자열 폭은 시드 쪽 정의를 따름 → 재귀 단계에서 더 긴 문자열이 생기면 잘림/에러 가능.

➡️ 시드에서 CAST(... AS CHAR(n))로 충분히 넓혀두기.

4) 이름 기반 참조
- 재귀부는 열 위치가 바뀌어도 “열 이름”으로 참조 가능.

5) 재귀 SELECT 문법 제약
- 집계 함수, 윈도 함수, GROUP BY, ORDER BY, DISTINCT 사용 불가.
  - (MySQL 8.0.19+) 재귀부에서 LIMIT [OFFSET] 허용.
- CTE는 FROM에서 단 한 번만 참조, 서브쿼리 내부에서 참조 금지.
- JOIN 가능하나 LEFT JOIN의 오른쪽에 CTE 배치 불가.

6) 반복(Iteration) 동작
- 각 재귀 반복은 직전 반복에서 생성된 행만을 입력으로 사용.
- 재귀부에 여러 쿼리 블록이 있으면 실행 순서는 미정.

7) 성능/안전 장치
- EXPLAIN의 Extra=Recursive로 표시. 비용은 반복 1회 기준.
- 결과가 커지면 임시 테이블이 디스크로 전환 → 메모리 한도 조정 고려.
- 무한 재귀 방지:
  - cte_max_recursion_depth (레벨 제한)
  - max_execution_time (실행 시간 제한)
  - 옵티마이저 힌트 MAX_EXECUTION_TIME

## 2. 셀프 조인

~~~
✅ 학습 목표 :
* 같은 테이블 내에서 상호 관계를 탐색할 수 있는 셀프 조인의 구조를 이해하고 사용할 수 있다. 
~~~

1) 개념
- **셀프 조인(Self Join)**은 하나의 테이블을 두 번 이상 별칭으로 참조해, 동일 테이블 내의 관계(예: 직원–관리자, 카테고리–상위카테고리)를 탐색하는 방식.
- 전형적 패턴:
```
SELECT e.id, e.name, m.name AS manager_name
FROM Employees e
LEFT JOIN Employees m
  ON e.manager_id = m.id;
```
- 상하관계가 얕은 1~2단계에서 필요한 정보만 가져올 때 간단/직관적.
- 여러 단계(가변 깊이)의 계층 전체를 탐색해야 한다면 셀프 조인만으로는 한계 
  - → `WITH RECURSIVE` 적합

2) 셀프 조인 vs 재귀 CTE
- 셀프 조인: 고정 깊이의 단순 관계 확인에 적합(가독성 좋고 빠름).
- 재귀 CTE: 가변 깊이/트리 전체 경로/레벨(depth) 계산/경로 문자열(path) 생성 등 계층 전개에 적합.

<br>

<br>

---

# 3️⃣ 실습 문제

## 문제 

- https://leetcode.com/problems/employees-earning-more-than-their-managers/ 

> LeetCode 181. Employees  Earning More Than Their Managers
>
> 학습 포인트 : 동일 테이블을 두 번 조인 (왜 동일 테이블을 JOIN 해야하는 문제일까)

- https://leetcode.com/problems/tree-node/description/

> LeetCode 608. Tree Node 
>
> 학습 포인트 : id, parent_id 기반의 트리 구조에서 **부모 ~ 자식 관계 재귀 탐색**
>
> Hint : (문제 해석) 
>
> - 어떤 노드가 Root Node 이려면, 부모노드가 존재하지 않아야 한다. 
> - 어떤 노드가 Inner Node 이려면, 나를 부모로 가지는 노드가 하나 이상 존재하여야 한다.
>   - 그 외네는 모두 Leaf Node 이다. --> (CASE 문을 사용하는 것을 추천드립니다.)

- https://school.programmers.co.kr/learn/courses/30/lessons/144856

> 프로그래머스 : 저자 별 카테고리 별 매출액 집계하기 
>
> 학습 포인트 : 카테고리와 서브카테고리 계층 구조를 분석하는 로직, SELF JOIN / CTE를 다 활용할 수 있다.
>
> - 위에 2가지의 문제를 풀어보고 난 이후, 더 편리한 방법으로 문제를 풀어보세요.

---

## 문제 인증란
![alt text](image-9.png)



---

# 확인문제

## 문제 1

> **🧚윤서는 어떤 기업의 조직 구조를 분석하는 SQL 쿼리를 작성하고 있습니다. 각 직원은 상위 관리자 ID(manager_id)를 가지며, 조직도는 같은 Employees 테이블 내에서 계층적으로 연결됩니다. 윤서는 최상위 관리자부터 각 사원까지의 계층 깊이(depth)를 계산하고자 다음과 같은 SELF JOIN 기반 쿼리를 시도했습니다.** 

~~~sql
SELECT e1.id, e1.name, e2.name AS manager_name
FROM Employees e1
LEFT JOIN Employees e2 ON e1.manager_id = e2.id;
~~~

> **쿼리를 잘 작성했다고 생각을 했지만, 막상 실행을 해보니 1단계 매니저까지만 추적할 수 있어 계층 구조의 전체를  표현하는데 한계가 존재했습니다. 이에 여러분에게 다음과 같은 미션을 요청합니다. WITH RECURSIVE를 활용하여  최상위 관리자부터 시작해 각 직원까지의 조직 구조 계층 깊이(depth)를 구하고, 결과를 depth가 높은 순으로 정렬하는 쿼리를 작성하세요.**



~~~sql
WITH RECURSIVE org AS (
  SELECT
    e.id,
    e.name,
    e.manager_id,
    0 AS depth,
    CAST(e.id AS CHAR(200)) AS path   
  FROM Employees e
  WHERE e.manager_id IS NULL

  UNION ALL

  SELECT
    c.id,
    c.name,
    c.manager_id,
    p.depth + 1 AS depth,
    CONCAT(p.path, '>', c.id) AS path
  FROM Employees c
  JOIN org p
    ON c.manager_id = p.id
)
SELECT id, name, depth
FROM org
ORDER BY depth DESC, id;
~~~



---

### 참고자료

<!--셀프조인에 대해 학습하시기에 도움이 되도록 참고할말한 잘 설명된 블로그들을 같이 첨부하겠습니다. -->

https://step-by-step-digital.tistory.com/101



<br>

### 🎉 수고하셨습니다.