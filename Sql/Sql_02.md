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

```sql
SELECT DEPT_
	   ID, DEPT_NAME
FROM   DEPARTMENT;
>
Error ORA-00904 : DEPT_부적합한 식별자
```

```sql
SELECT DEPT_ ID, DEPT_NAME
FROM   DEPAR
TMENT;
>
Error ORA-00942 : 테이블 또는 뷰가 존재하지 않습니다
```

```sql
SEL
ECT DEPT_ ID, DEPT_NAME
FROM   DEPARTMENT;
>
Error ORA-00900 : SQL 문이 부적합합니다.
```

- 구문은 ; 이나 /로 종료된다.

```sql
SELECT DEPT_NAME
FROM   DEPARTMENT;
```

```sql
SELECT DEPT_NAME
FROM   DEPARTMENT
;
```

```sql
SELECT DEPT_NAME
FROM   DEPARTMENT
/
```

- / 기호는 반드시 새로운 줄에서 사용해야 한다.

### 2.1.3 SELECT 구문 작성 가이드

- 키워드를 포함한 모든 SQL 문장은 대문자로 작성
- 한 칸 공백 사용
  - 단어와 단어 사이
  - ',' 다음
  - 비교 연산자 ( <, >, = 등)의 앞뒤
- 줄 나눔 및 세로 열 맞춤
  - SELECT, FROM, WHERE 절은 각각 다른 줄에 왼쪽 정렬하여 작성
  - 불필요한 공백 라인은 사용하지 않는다.

### 2.2.1 기본 구문 1

```sql
SELECT * | { [DISTINCT] {{column_name | expr}  {[AS] [alias]}, ...} }
FROM   table_name
WHERE  search_condition  [  {AND | OR}, search_condition ...];
```

**[구문 설명]**

- 키워드 SELECT 다음에는 조회하려고 하는 컬럼 이름(또는 표현 식)을 기술
  - 모든 컬럼을 조회하는 경우 * 기호를 사용할 수 있음
    - 컬럼은 생성된 순서대로 표시됨
  - 여러 컬럼을 조회하는 경우 각 컬럼 이름은 쉼표로 구분 (마지막 컬럼 이름 다음에는 쉼표를 사용하지 않음)
  - 조회 결과는 기술된 컬럼 이름 순서로 표시
- 키워드 FROM 다음에는 조회 대상 컬럼이 포함된 테이블 이름을 기술
- 키워드 WHERE 다음에는 행을 선택하는 조건을 기술
  - 제한 조건을 여러 개 포함할 수 있으며, 각각의 제한 조건은 논리 연산자로 연결
  - 제한 조건을 만족시키는 행 (논리 연산 결과가 TURE 인 행)들만 Result set에 포함

### 2.2.2 SELECT 사용 예 1

* 직원들의 사번과 이름을 조회하는 SELECT 구문

```sql
SELECT	EMP_ID, 
		EMP_NAME
FROM	EMPLOYEE;
```

### SELECT  사용 예 2

- 직원들의 모든 정보를 조회하는 SELECT 구문

```sql
SELECT	EMP_ID, EMP_NAME, EMP_NO, EMAIL, PHONE, HIRE_DATE, JOB_ID, SALARY, BONUS_PCT, MARRIAGE, MGR_ID, DEPT_ID
FROM 	EMPLOYEE
또는 
SELECT	*
FROM 	EMPLOYEE;
```

### SELECT  사용 예 3

- 컬럼 값에 대해 산술 연산한 결과 조회

```sql
SELECT	EMP_NAME,
		SALARY * 12,
		( SALARY+(SALARY*BONUS_PCT))*12
FROM	EMPLOYEE;
```

| EMP_NAME | SALARY * 12 | ( SALARY+(SALARY*BONUS_PCT))\*12 |
| -------- | ----------- | -------------------------------- |
|          |             |                                  |

- 컬럼 헤더는 산술 연산 식으로 표시된다.
- 실제 컬럼 값이 변경 되는 것은 아니다.

### 2.2.4 SELECT 사용 - 컬럼 별칭(Alias)

- 컬럼 별칭을 사용하면 SELECT 절에 기술된 내용과 동일하게 표시되는 실행 결과 헤더 부분을 변경할 수 있음

| EMP_NAME | 1년 급여 | 총 소득 |
| -------- | -------- | ------- |
|          |          |         |

#### 컬럼 별핑 구문

- [AS] [alias]}

```sql
SELECT * | { [DISTINCT] {{column_name | expr}  {[AS] [alias]}, ...} }
FROM   table_name
WHERE  search_condition  [  {AND | OR}, search_condition ...];
```

**[구문 설명]**

- 별칭을 사용하려는 대상 뒤에 'AS + 원하는 별칭'을 기술(별칭은 공백으로 구분)
- AS는 생략 가능
- 따옴표를 반드시 사용해야 하는 경우가 있음
  - 영문 대/소문자를 구분해서 별핑을 표시해야 하는 경우(기본적으로 대문자로 표시)
  - 별칭에 특수 문자(공백, &, 괄도 등)가 포함된 경우
    - 별칭이 숫자로 시작하거나 숫자 자체인 경우 숫자도 특수 문자로 취급됨

### 2.2.4 SELECT 사용 예 4 - 컬럼 별칭 구문

```sql
SELECT	EMP_NAME AS 이름,
		SALARY * 12 AS "1년 급여",
		(SALARY + (SALARY * BONUS_PCT) ) * 12 AS 총소득
FROM	EMPLOYEE;
```

| 이름 | 1년급여 | 총소득 |
| ---- | ------- | ------ |
|      |         |        |

```sql
SELECT	EMP_ID AS "empid",
		EMP_NAME AS "이름",
		SALARY AS "급여(원)"
FROM 	EMPLOYEE;
```

| empid | 이름 | 급여(원) |
| ----- | ---- | -------- |
|       |      |          |

```sql
SELECT	EMP_NAME,
		SALARY AS 급여(원)
FROM	EMPLOYEE;
---------------------------
SELECT	EMP_NAME,
		SALARY AS 1달급여
FROM	EMPLOYEE;
>
Error ORA-00923 : FROM 키워드가 필요한 위치에 없습니다.
```

15p