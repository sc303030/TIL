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

### 2.2.5 SELECT 사용 - 리터럴 Literal

- 임의로 지정한 문자열
- SELECT 절에 사용하면 테이블에 존재하는 데이터처럼 사용가능

```sql
SELECT	EMP_ID,
		EMP_NAME,
		'재직' AS 근무여부
FROM	EMPLOYEE;
```

- 문자(또는 날짜) 리터럴은 ' ' 기호를 사용해서 표현
- 리터럴은 Result Set의 모든 행에 반복적으로 표시됨

### 2.2.6 SELECT 사용 - DISTINCT

- 컬럼에 포함된 중복 값을 한번씩만 표시하고자 할 때 사용

```sql
SELECT	* | { [DISTINCT] {{column_name | expr } {[AS] [alias]}, ... }}
FROM	table_name
WHERE	search_condition [ {AND | OR }, search_condition...];
```

**[구문 설명]**

- SELECT 절에 1회만 기술
- 컬럼 별로 적용 불가능
- 여러  개 컬럼을 조회하는 경우에는 조회 대상 컬럼들의 조합 결과를 기준으로 중복 여부 판단

### 2.2.6 SELECT 사용 예 6 - DISTINCT

```sql
SELECT	DISTINCT DEPT_ID
FROM	EMPLOYEE;
```

```sql
SELECT	JOB_ID, DEPT_ID
FROM	EMPLOYEE;
```

### 2.2.7 SELECT 사용 예 - WHERE

**DEPT_ID 값이 '90'인 행만 조회**

```sql
SELECT	EMP_NAME AS 이름,
		DEPT_ID AS 부서
FROM	EMPLOYEE
WHERE	DEPT_ID = '90';
```

**급여가 4000000 보다 많은 직원 이름과 급여 조회**

```sql
SELECT	EMP_NAME AS 이름,
		SALARY AS 급여
FROM	EMPLOYEE
WHERE	SALARY > 4000000;
```

**부서 코드가 '90'이고 급여를 2000000보다 많이 받는 부서원 이름과 부서 코드, 급여 조회**

```sql
SELECT	EMP_NAME AS 이름,
		DEPT_ID AS 부서,
		SALARY AS 급여
FROM	EMPLOYEE
WHERE	DEPT_ID = '90'
AND		SALARY > 2000000;
```

**'90' 부서나 '20' 부서에 소속된 부서원 이름, 부서 코드, 급여 조회**

```sql
SELECT	EMP_NAME AS 이름,
		DEPT_ID AS 부서,
		SALARY AS 급여
FROM	EMPLOYEE
WHERE	DEPT_ID = '90'
OR		DEPT_ID = '20'
```

### 2.3.1 연결 연산자(ConcatenationOperator)

- 연결 연산자 '||'를 사용하여 여러 컬럼을 하나의 컬럼인 것처럼 연결하거나, 컬럼과 리터럴을 연결할 수 있음

**컬럼과 컬럼을 연결한 경우**

```sql
SELECT	EMP_ID||EMP_NAME||SALARY
FROM	EMPLOYEE;
```

**컬럼과 리터럴을 연결한 경우**

```sql
SELECT	EMP_NAME||'의 월급은'||SALARY||'원 입니다.'
FROM	EMPLOYEE;
```

### 2.2.2 논리 연산자(LogicalOperator)

- 여러 개의 제한 조건 결과를 하나의 논리 결과(TRUE, FALSE, NULL)로 만든다.

| Operator |                           의미                           |
| :------: | :------------------------------------------------------: |
|   AND    |     여러 조건이 동시에 TRUE일 경우에만 TRUE 값 반환      |
|    OR    | 여러 조건들 중에 어느 하나의 조건만TRUE이면 TRUE 값 반환 |
|   NOT    |         조건에 대한 반대 값으로 반환 (NULL 예외)         |

**[AND 연산 결과]**

|       | TRUE | FALSE | NULL |
| :---: | :--: | :---: | :--: |
| TRUE  |  T   |   F   |  N   |
| FALSE |  F   |   F   |  F   |
| NULL  |  N   |   F   |  N   |

**[OR 연산 결과]**

|       | TRUE | FALSE | NULL |
| :---: | :--: | :---: | :--: |
| TRUE  |  T   |   T   |  T   |
| FALSE |  T   |   F   |  N   |
| NULL  |  T   |   N   |  N   |

### 2.3.3 비교 연산자 (ComparisonOperator)

- 표현식 사이의 관계를 비교하기 위해 사용
- 비교 결과는 논리 결과 중의 하나(TRUE, FALSE, NULL)가 됨
- 비교하는 두 컬럼 값/ 표현식은 서로 동일한 데이터 타입이어야 함

**[주요 비교 연산자]**

|       Operator        |                의미                 |
| :-------------------: | :---------------------------------: |
|           =           |                같다                 |
|        \> , \<        |             크다, 작다              |
|       \>= , <=        |      크거나 같다/ 작거나 같다       |
|      <>, !=, ^=       |              같지 않다              |
|      BETWEEN AND      |     특정 범위에 포함되는지 비교     |
|    LIKE/ NOT LIKE     |          문자 패턴을 비교           |
| IS NULL / IS NOT NULL |           NULL 여부 비교            |
|          IN           | 비교 값 목록에 포함되는지 여부 비교 |

#### BETWEEN AND

-  비교하려는 값이 지정한 (상한 값과 하한 값의 경계 포함)에 포함되면 TRUE를 반환하는 연산자

**[급여를 3,500,000원 보다 많이 받고 5,500,000원 보다 적게 받는 직원 이름과 급여 조회]**

```sql
SELECT	EMP_NAME,
		SALAEY
FROM	EMPLOYEE
WHERE	SALARY BETWEEN 3500000 AND 5500000;
또는
SELECT	EMP_NAME,
		SALAEY
FROM	EMPLOYEE
WHERE	SALARY >= 3500000
AND		SALARY <= 5500000;
```

#### LIKE

- 비교하려는 값이 지정한 특정 패턴을 만족시키면 TRUE를 반환하는 연산자
- 패턴 지정을 위해 와일트 카드 사용
  - % : % 부분에는 임의 문자열(0개 이상의 임의의 문자)이 있다는 의미
  - _  : _부분에는 문자 1개만 있다는 의미

**['김'씨 성을 가진 직원 이름과 급여 조회]**

```sql
SELECT	EMP_NAME,
		SALARY
FROM	EMPLOTEE
WHERE	EMP_NAME LIKE '김%'
```

**[9000번 대 4자리 국번의 전화번호를 사용하는 직원 전화번호 조회]**

```sql
SELECT	EMP_NAME,
		PHONE
FROM	EMPLOYEE
WHERE	PHONE LIKE '___9________';
```

- '_' 사이에는 공백이 없음

**[EMAILID중'_'앞자리가3자리인직원조회]**

```sql
SELECT	EMP_NAME,
		EMAIL
FROM EMPLOYEE
WHERE EMAIL LIKE'_ _ _ _%';
```

- '_' 사이에는공백없음

- 전체 데이터가 모두 조회됨
  - 와일트 카드 자체를 처리하는 방법 필요

##### EscapeOption : 와일드카드('%','_')자체를 데이터로 처리해야하는 경우에 사용

**[email id 중'_' 앞자리가 3자리인 직원 조회]**

```sql
SELECT	EMP_NAME,
		EMAIL
FROM EMPLOYEE
WHERE EMAIL LIKE'_ _ _\_%' ESCAPE '\';
```

- '_' 사이에는 공백없음

**[ESCAPEOPTION에 사용하는 문자는 임의 지정 가능]**

```sql
SELECT EMP_NAME, EMAIL
FROM EMPLOYEE
WHERE EMAIL LIKE'_ _ _#_%' ESCAPE '#';
```

- '_' 사이에는 공백없음

#### NOT LIKE

**['김'씨 성이 아닌 직원이름과 급여 조회]**

```sql
SELECT EMP_NAME,
	   SALARY
FROM EMPLOYEE
WHERE EMP_NAME NOTLIKE '김%';
또는
SELECT  EMP_NAME,
		SALARY
FROM EMPLOYEE
WHERE NOTEMP_NAME LIKE '김%';
```

#### IS NULL / IS NOT NULL

- NULL여부를비교하는연산자

**[관리자도 없고 부서 배치도 받지 않은 직원 이름 조회]**

```sql
SELECT	EMP_NAME, MGR_ID, DEPT_ID
FROM EMPLOYEE
WHERE MGR_IDIS NULL AND DEPT_IDIS NULL;
```

**[부서 배치를 받지 않았음에도 보너스를 지급받는 직원 이름 조회]**

```sql
SELECT EMP_NAME, DEPT_ID, BONUS_PCT
FROM EMPLOYEE
WHERE DEPT_IDIS NULL
AND BONUS_PCTIS NOT NULL;
```

#### IN

- 비교하려는 값 목록에 일치하는 값이 있으면 TRUE를 반환하는 연산자

**[60번 부서나 90번 부서원들의 이름, 부서 코드, 급여 조회]**

```sql
SELECT EMP_NAME, DEPT_ID, SALARY
FROM EMPLOYEE
WHERE DEPT_IDIN ( '60', '90' );
또는
SELECT EMP_NAME, DEPT_ID, SALARY
FROM EMPLOYEE
WHERE DEPT_ID= '60'OR DEPT_ID= '90';
```

### 2.3.4 연산자 우선 순위

- 여러 연산자를 함께 사용할 때 우선순위를 고려해야함

- `()` 를 사용하면 연산자 우선 순위를 조절할 수 있음

| 순위 |            연산자             |
| :--: | :---------------------------: |
|  1   |          산술연산자           |
|  2   |          연결 연산자          |
|  3   |          비교 연산자          |
|  4   | IS (NOT) NULL, LIKE, (NOT) IN |
|  5   |       (NOT) BETWEEN-AND       |
|  6   |        논리연산자-NOT         |
|  7   |        논리연산자-AND         |
|  8   |         논리연산자-OR         |

#### 연산자 우선 순위 예

**[20번 또는 90번부서원 중 급여를 3000000원보다 많이 받는 직원 이름, 급여, 부서코드 조회]**

```sql
SELECT EMP_NAME, SALARY, DEPT_ID
FROM EMPLOYEE
WHERE DEPT_ID = '20'ORDEPT_ID = '90'ANDSALARY > 3000000;
```

- 논리연산자 AND가 먼저 처리되어 "급여를3000000원보다 많이 받는 90번부서원 또는 20번부서원"을 조회하는 의미의 구문이 됨

**[20번 또는 90번부서원 중 급여를 3000000원보다 많이 받는 직원 이름, 급여, 부서코드 조회]**

```sql
SELECT EMP_NAME, SALARY, DEPT_ID
FROM EMPLOYEE
WHERE ( DEPT_ID = '20'ORDEPT_ID = '90' )
AND		SALARY > 3000000;
```

