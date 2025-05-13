# JOIN
### 정의
- 서로 다른 table 연결시 사용
- HOW? 공통으로 가지고 있는 컬럼을 기준으로 

### ✔️JOIN 종류
(1) INNER
- 두 테이블의 공통 요소만 연결
- A와 B의 교집합

(2) LEFT/RIGHT
- 왼쪽/오른쪽 테이블 기준으로 연결 
- A + (A-B) or B + (B - A) -> 즉, A or B

(3) FULL
- 양쪽 기준으로 연결 
- A와 B의 합집합

(4) CROSS
- 두 테이블의 각각의 요소 곱하기기
- 컬럼 개수 : A의 컬럽 수 * B의 컬럼 수

### 쿼리 작성 흐름

step1 : 테이블 확인

step2 : 기준 테이블 정의(기준 테이블을 왼쪽으로 두는게 좋음)

step3 : JOIN key 찾기(공통 컬럼 찾기) -> ON 정리

step 4 : 결과 예상 및 쿼리 작성/검증

### 문법 예시
(1) INNER JOIN
```SQL
SELECT A.name, B.salary
FROM Employees A
INNER JOIN Salaries B ON A.emp_id = B.emp_id;
```

(2) LEFT JOIN
```SQL
SELECT A.name, B.salary
FROM Employees A
LEFT JOIN Salaries B ON A.emp_id = B.emp_id;
```

(3) RIGHT JOIN
```SQL
SELECT E.name, D.department_name
FROM Employees E
RIGHT JOIN Departments D ON E.dept_id = D.dept_id;
```

(4) FULL JOIN
```SQL
SELECT E.name, D.department_name
FROM Employees E
FULL OUTER JOIN Departments D ON E.dept_id = D.dept_id;
```

(5) CROSS JOIN
```SQL
SELECT E.name, D.department_name
FROM Employees E
CROSS JOIN Departments D;
```

```
여태까지는 파이썬이 있는데 왜 SQL이 중요한지 100% 이해가 가진 않았는데 JOIN 은 확실히 파이썬 대비 정말 간편한 것 같음!!
```
