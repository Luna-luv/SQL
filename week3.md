![alt text](image.png)

## 에러 디버깅

| 오류 메시지 | 의미 / 원인 | 해결 방법 |
|-------------|--------------|-----------|
| `Number of arguments does not match for aggregate function COUNT` | COUNT 함수에는 컬럼 하나만 들어갈 수 있음 | `COUNT(*)`, `COUNT(column_name)` 형식으로 수정 |
| `SELECT list expression references column type1 a which is neither grouped nor aggregated` | GROUP BY 없이 집계되지 않은 컬럼 사용 | 해당 컬럼을 `GROUP BY` 절에 포함시키거나 집계 함수로 감싸기 |
| `Syntax error: Expected end of input but got keyword SELECT` | 쿼리 여러 개 작성 시 쿼리 구분 안 됨 | 쿼리마다 `;` 붙이기 |
| `Syntax error: Expected end of input but got keyword WHERE` | LIMIT이 WHERE 앞에 옴 | `LIMIT`은 쿼리의 **맨 마지막**에 위치 |
| `Syntax error: Expected ")" but got end of script` | 여는 괄호 닫지 않음 | 괄호 닫아주기 `)` |

---

## 데이터 타입 변환

| 함수 | 설명 | 예시 |
|------|------|------|
| `CAST()` | 데이터 타입을 다른 타입으로 변환 | `CAST(1 AS STRING)` → `"1"` |
| `SAFE_CAST()` | 변환 불가능할 경우 오류 대신 NULL 반환 | `SAFE_CAST("abc" AS INT64)` → `NULL` |
| `SAFE_DIVIDE(x, y)` | 0으로 나눌 경우 NULL 반환 | `SAFE_DIVIDE(10, 0)` → `NULL` |

---

## 문자열 함수

| 함수 | 설명 | 예시 | 결과 |
|------|------|------|-------|
| `CONCAT()` | 문자열 붙이기 | `CONCAT("안녕", "하세요", "!")` | `"안녕하세요!"` |
| `SPLIT()` | 구분자로 문자열 나누기 → 배열 반환 | `SPLIT("가, 나, 다", ", ")` | `["가", "나", "다"]` |
| `REPLACE()` | 특정 단어를 다른 단어로 수정 | `REPLACE("안녕하세요", "안녕", "인사")` | `"인사하세요"` |
| `TRIM()` | 앞뒤에서 특정 문자 제거 | `TRIM("안녕하세요", "하세요")` | `"안녕"` |
| `UPPER()` | 소문자 → 대문자 | `UPPER("abc")` | `"ABC"` |

---

## 날짜/시간 함수

| 함수 | 설명 | 예시 | 결과 |
|------|------|------|-------|
| `DATE()` | 날짜 생성 (YYYY, MM, DD) | `DATE(2025, 4, 8)` | `2025-04-08` |
| `DATETIME()` | 날짜+시간 생성 | `DATETIME(2025, 4, 8, 15, 30, 0)` | `2025-04-08T15:30:00` |
| `TIMESTAMP()` | 타임스탬프 생성 | `TIMESTAMP("2025-04-08 15:30:00")` | `2025-04-08 15:30:00 UTC` |

> UTF + 9 = Asia, Seoul
---

## 참고

- `FROM` 없이도 실행 가능한 함수 존재 (`CONCAT`, `UPPER`, `CURRENT_DATE` 등)
- BigQuery 공식 문서에서 함수 상세 설명 및 포맷 확인 가능
