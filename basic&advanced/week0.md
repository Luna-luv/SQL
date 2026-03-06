![image](https://github.com/user-attachments/assets/c4ff48ff-bd66-40fe-921c-b623531914c6)

# 1-1. BigQuery 기초 지식
- SQL : 언어, SQL 이 관리하는 저장소 : 데이터베이스(MySQL, PostgreSQL 등)
    - 데이터베이스 : Online Transaction Processing
- 테이블 구조 : 행(Row), 열(Column)
- OLAP와 DW의 등장
    - OLAP : Online Analytical Processing; 분석을 위한 기능 제공
    - DW : Data Warehouse; 저장소
- BigQuery (DW of Google Cloud)
    - 장점 : 난이도 낮음, 속도 빠름, Firebase, Google Analytics 데이터 추출 쉬움, DW를 위한 서버 필요 없음

# 1-2. BigQuery 환경 설정
- 구성 요소 : 프로젝트 > 데이터셋 > 테이블

# 2. 데이터 탐색 
- ERD(Entity Relationship Diagram)
    - 목적 : 데이터베이스 구조 한 눈에 알아보기
    - 예시 : Product(테이블)-product_id(열)
- 회사에 존재할 수 있는 데이터 예시
    - 서비스에 사용될 DB
    - 앱/웹 로그 데이터 : 과정 파악 가능
    - 공공데이터, 서드파티 데이터 ex.날씨

# 느낀점
- 왜 SQL을 써야하는가? 
    SQL은 데이터 정리에 특화되어 있어서 간결한 쿼리로 데이터 정리 가능.
    DB에서 직접 연산을 수행하므로 빠름
-> Python 과 비교해서 SQL이 가진 장점이 무엇이며, 왜 기업들이 SQL을 많이 쓰는지 궁금했는데 이번 기회에 SQL의 장점을 알게 되었습니다. 또한, Python을 더 잘하고 싶은데, SQL과 Python이 상호보완관계라는 것을 알게 되어서 한 학기동안 SQL 강의를 잘 따라가야겠다고 생각했습니다. 
  
