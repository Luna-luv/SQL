# 1. 데이터탐색 : 조건과 추출 (SELECT, WHERE)
**1-1.**
- 기본형식 : SELECT col FROM Table
- * : 모든 컬럼을 출력하겠다 -> * EXCEPT(제외 col) 도 가능
- JOIN : 서로 다른 테이블에 있는 데이터 연결 가능

**1-2. 문법 핵심 정리**
- FROM : 데이터의 table 명시, AS 이용한 별칭 설정 가능
- WHERE : FROM 에 명시된 Table 데이터 필터링
- SELECT : Table에 저장된 col 선택(여러개 선택 가능)
- SQL 작동 순서 : FROM -> WHERE -> SELECT
    -> 쿼리 작성시 SELECT - FROM - WHERE 순으로 작성할 것임(WHERE->FROM 불가능)

# 2. 데이터 탐색 : 요약(집계, 그룹화); COUNT, DISTINCT, GROUP BY
**2-1. GROUP BY**
- 값은 값끼리 모아서 그룹화(그룹화와 동시에 집계도 가능; 집계 함수 ex. COUNT, MAX, MIN)
- 집계할 컬럼을 SELECT 에 명시하고 그 컬럼을 꼭 GROUP BY에 작성

**2-2. DISTINCT** 
- 고유값을 알고 싶은 경우 사용
- 중복 제거

**2-3. HAVING**
- 위치 : GROUP BY 이후
- WHERE와의 차이점 : WHERE(Table에 조건 설정) / HAVING(GROUP BY 후 조건 설정)

**2-4. ORDER BY : 정렬렬**
- OSC(오름차순-default), DESC(내림차순)
- 위치 : 보통 쿼리의 맨 마지막.

**2-5. LIMIT : 출력 개수 제한**
- Row 수 제한
- 위치 : 쿼리의 제일 마지막

**알쓸신잡(소감)**
- 궁금증 : SQL 의 함수는 왜 대문자로 쓰는가? 소문자는 작동을 안하나?
    -> 답 : 아니다. 소문자도 정상작동한다. 관습이 대문자로 쓰는 것이고, 구조가 한 눈에 보인다는 장점 때문에 보통 대문자로 쓴다. 함수는 대문자, 소문자 섞어 써도 동일하게 정상 작동하지만, "" 안 문자는 대소문자를 구분하기 때문에 이를 주의해야한다.
