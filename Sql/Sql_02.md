# Sql_02

### 2.1.1 데이터 조회 범위 및 결과

- 데이터 조회 범위
  - 특정 컬럼 조회
  - 특정 행 조회
  - 특정 행/컬럼 조회
  - 여러 테이블의 특정 행/컬럼 조회(조인)
- 데이터 조회 결과
  - 데이터를 조죄한 결과는  'Result Set' 이라고 함
  - SELECT 구문에 의해 반환된 행들의 집합을 의미
  - Result Set에는 0개, 1개, 여러 개 행이 포함될 수 있음
  - Result Set은 특정한 기준에 의해 정렬될 수 있음

### 2.1.2 SELECT 구문 작성 시 고려 사항 1

- 키워드, 테이블 이름, 컬럼 이름은 대/소문자를 구분하지 않는다.

```sql
SELECT DEPT_ID, DEPT_NAME
FROM DEPARTMENT;

select dept_id, dept_name
from department;
```

- 나누어 쓰기/ 들여쓰기를 하면 가독성이 좋아지고 편집이 쉽다.

```sql
SELECT DEPT_ID, DEPT_NAME FROM DEPARTMENT;

SELECT DEPT_ID, DEPT_NAME
FROM   DEPARTMENT;

SELECT DEPT_ID,
	   DEPT_NAME
FROM   DEPARTMENT;
```

### SELECT 구문 작성 시 고려 사항 2

- 키워드, 테이블 이름, 컬럼 이름은 약자로 줄여 쓰거나 분리할 수 없다.