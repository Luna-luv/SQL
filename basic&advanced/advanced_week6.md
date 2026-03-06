# SQL_ADVANCED 6주차 정규 과제

## **Week 6 : PIVOT / UNPIVOT**

📌**SQL_ADVANCED 정규과제**는 매주 정해진 주제에 따라 **MySQL 공식 문서 또는 한글 블로그 자료를 참고해 개념을 정리한 후, 프로그래머스 SQL 문제 3문제**와 **추가 확인문제**를 직접 풀어보며 학습하는 과제입니다.

이번 주는 아래의 **SQL_ADVANCED_6th_TIL**에 나열된 주제를 중심으로 개념을 학습하고, 주차별 **학습 목표**에 맞게 정리해주세요. 정리한 내용은 GitHub에 업로드한 후, **스프레드시트의 ‘SQL’ 시트에 링크를 제출**해주세요.

**(수행 인증샷은 필수입니다.)**

> 프로그래머스 문제를 풀고 ‘정답입니다’ 문구를 캡쳐해서 올려주시면 됩니다.

------

## **SQL_ADVANCED_6th_TIL**

- MySQL 공식문서에는 `PIVOT / UNPIVOT`에 대한 **전용 문법을 제공하지 않고 있습니다.** 따라서 이번 주차에서는 PIVOT / UNPIVOT에 대한 개념을 이해하고, 이를 `CASE WHEN + GROUP BY` 또는 `UNION ALL` 등의 조합으로 수동 구현할 수 있는 방법을 학습하면 됩니다. 



## **🏁 강의 수강 (Study Schedule)**

| **주차** | **공부 범위**           | **완료 여부** |
| -------- | ----------------------- | ------------- |
| 1주차    | 서브쿼리 & CTE          | ✅             |
| 2주차    | 집합 연산자 & 그룹 함수 | ✅             |
| 3주차    | 윈도우 함수             | ✅             |
| 4주차    | Top N 쿼리              | ✅             |
| 5주차    | 계층형 질의 & 셀프 조인 | ✅             |
| 6주차    | PIVOT / UNPIVOT         | ✅             |
| 7주차    | 정규 표현식             | 🍽️             |

<br>



# 1️⃣ 학습 내용

**참고할 자료를 아래에 같이 첨부합니다.**

<!-- 꼭 아래의 자료를 참고하지 않고, 개인적인 학습 방법으로 진행하셔도 좋습니다. -->

1. **PIVOT / UNPIVOT 개념 학습 Blog**

https://m.blog.naver.com/regenesis90/222205833002

https://blog.naver.com/regenesis90/222205964866

2. **MySQL 로 PIVOT 구현하기**

https://shxrecord.tistory.com/181



<br>

---

# 2️⃣ 학습 내용 정리하기

## 1. PIVOT

```
✅ 학습 목표 :
* MySQL에서 직접적인 `PIVOT` 문법은 없으므로, `CASE WHEN`, `GROUP BY`, `MAX()` 등으로 대체 구현할 수 있다.
* 데이터를 행 → 열 방향으로 전개하는 기본 로직을 이해한다.
```

- PIVOT : 데이터의 특정 행에 있는 고유 값들을 새로운 여러 개의 열로 변환하는 과정
- `GROUP BY` 와 `CASE WHEN`문을 집계함수(`SUM`, `MAX`)와 함께 구현
- 구현 로직
  - `GROUP BY` : PIVOT 이후에도 행으로 유지하고 싶은 기준 컬럼을 기준으로 그룹화
  - `CASE WHEN` : 새로운 컬럼으로 만들고 싶은 값들을 조건으로 지정
    - 조건에 맞으면 실제 값 반환, 아니면 NULL or 0
    - 각 그룹 내 해당 조건에 맞는 값이 하나만 존재하므로 `MAX`나 `SUM` 어떤 것을 사용해도 동일 결과


## 2. UNPIVOT

```
✅ 학습 목표 :
* UNPIVOT 역시 직접 문법이 없으므로, `UNION ALL`과 `JOIN`, JSON 등의 방법으로 열 → 행 형태로 변환하는 과정을 익힌다.
* `UNION ALL`로 수동 구현 시의 컬럼 이름 통일과 데이터 병합 과정을 익힌다.
```
- UNPIVOT : 여러 열로 넓게 펼쳐진 데이터를 특정 행으로 변환하는 과정
- 주로 `UNION ALL`로 구현
- 구현 로직
  - 여러 `SELECT`문 작성 : 행으로 변환하고자 하는 각 컬럼에 대해 개별 `SELECT`문으로 작성
    - ⚠️ 모든 `SELECT`문의 컬럼 순서와 이름, 데이터 타입 일치 必
  - `UNION ALL`으로 결합 : 작성된 모든 `SELECT`문의 결과를 수직 결합
    - ⚠️ `UNION` : 중복을 제거하는 과정이 포함 -> 중복될 일이 없는 UNPIVOT 작업에서는 `UNION ALL`을 사용하는것이 효율적

---

# 3️⃣ 실습 문제

## Leetcode 문제 

https://leetcode.com/problems/reformat-department-table/description/

> LeetCode. Reformat Department Table
>
> 학습 포인트 : MySQL 에서는 PIVOT을 쉽게 구할 수 있는 방법이 없다. 
>
> - 수동으로 구하기 : CASE WHEN + 집계함수 / GROUP BY + 조건 분기 사용



## 문제 인증란

![alt text](image-12.png)



## 확인 문제 

### 문제 1

> **🧚지희는 매월 각 매장의 월별 매출 데이터가 담긴 테이블을 가공하려 합니다. 아래와 같은 테이블이 있다고 가정합시다.**

| **branch** | **Jan_sales** | **Feb_sales** | **Mar_sales** |
| ---------- | ------------- | ------------- | ------------- |
| A          | 100           | 120           | 130           |
| B          | 90            | 110           | 140           |

> **Q. 이 테이블을 아래와 같은 형태로 바꾸고 싶습니다. SQL에서 UNION ALL을 활용하여 UNPIVOT 구조를 수동으로 구현해보세요.**

| **branch** | **month** | **sales** |
| ---------- | --------- | --------- |
| A          | Jan       | 100       |
| A          | Feb       | 120       |
| A          | Mar       | 130       |
| B          | Jan       | 90        |
| B          | Feb       | 110       |
| B          | Mar       | 140       |

```SQL
-- 테이블 이름 : 'sales_report'
SELECT branch, 'Jan' AS month, Jan_sales AS sales
FROM sales_report
UNION ALL
SELECT branch, 'Feb' AS month, Feb_sales AS sales
FROM sales_report
UNION ALL
SELECT branch, 'Mar' AS month, Mar_sales AS sales
FROM sales_report;
```



### 문제 2

> **🧚태연이는 지점별로 월별 매출을 한 눈에 보기 위해, 아래와 같이 매출 데이터가 저장된 데이터를 가공하려고 합니다.**

| **branch** | **month** | **sales** |
| ---------- | --------- | --------- |
| A          | Jan       | 100       |
| A          | Feb       | 120       |
| A          | Mar       | 130       |
| B          | Jan       | 90        |
| B          | Feb       | 110       |
| B          | Mar       | 140       |

> **이 데이터를 아래와 같이 월별 매출 컬럼이 각각 존재하도록 PIVOT 형태로 바꾸고 싶습니다.MySQL에서는 PIVOT 문법이 없기 때문에, CASE WHEN, GROUP BY, MAX() 또는 SUM() 등을 이용해 수동으로 구현해보세요.**

- 원하는 결과 

| **branch** | **Jan_sales** | **Feb_sales** | **Mar_sales** |
| ---------- | ------------- | ------------- | ------------- |
| A          | 100           | 120           | 130           |
| B          | 90            | 110           | 140           |

~~~SQL
-- 테이블 이름 : 'sales_data'
SELECT
    branch,
    MAX(CASE WHEN month = 'Jan' THEN sales END) AS Jan_sales,
    MAX(CASE WHEN month = 'Feb' THEN sales END) AS Feb_sales,
    MAX(CASE WHEN month = 'Mar' THEN sales END) AS Mar_sales
FROM
    sales_data
GROUP BY
    branch;
~~~

### **🎉 수고하셨습니다.**