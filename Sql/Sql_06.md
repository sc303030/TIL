# Sql_06

### 4.2.4 JOIN - 개념

- 서로 연관되고 다른 테이블에 존재하는 컬럼들을 한번에 조회하기 위해 사용하는 대표적인 기법

#### 오라클 전용 구문

```sql
SELECT 	EMP_NAME, DEPT_NAME
FROM 	EMPLOYEE E,
		DEPARTMENT D
WHERE 	E.DEPT_ID = D.DEPT_ID;
```

- FROM 절에 조회 대상 테이블을 쉼표로 구분하여 기술
- WHERE 절에 테이블 사이의 관계를 표시하는 조건 기술
- 동일한 이름의 컬럼이 여러 테이블에 존재하는 경우
  - SELECT 절/WHERE 절에 컬럼 이름을 기술할 때 어떤테이블에 포함된 컬럼인지 구분해서 표시
- 테이블 이름도 별칭을 사용할 수 있다.

[EMPLOYEE]

| EMP_NAME | DEPT_NAME |
| -------- | --------- |
| 펭펭     | 90        |
| 펭하     | 50        |

[DEPARTMENT]

| DEPT_ID | DEPT_NAME |
| ------- | --------- |
| 50      | 펭귄      |

[조회된 테이블]

| EMP_NAME | DEPT_NAME |
| -------- | --------- |
| 펭하     | 펭귄      |

```sql
SELECT 	EMP_NAME, DEPT_NAME
FROM 	EMPLOYEE, DEPARTMENT
WHERE 	DEPT_ID = DEPT_ID;
```

```sql
SELECT 	EMP_NAME, DEPT_ID,
		DEPT_NAME
FROM 	EMPLOYEE E,
		DEPARTMENT D
WHERE 	E.DEPT_ID = D.DEPT_ID;
```

- `ERROR` : ORA-00918 : 열의 정의가 애매합니다.

- 양쪽 테이블에 동일한 이름의 컬럼이 모두 존재하는 경우
  - WHERE 절: 테이블 구분 필요(테이블 별칭 사용 가능)
  - SELECT 절: 양쪽 테이블의 컬럼 값은 동일하지만, 문법 상 어떤 테이블의 컬럼 값을 표시할 것인지 구분 필요

#### ANSI 표준 구문

- JOIN 유형을 세분화
- WHERE 절에서 JOIN 조건을 별도로 분리하고 'JOIN' 키워드를 명시적으로 사용

```sql
SELECT 	...
FROM 	table1
		{[INNER] JOIN table2 ON (condition1 [AND condition2 …]) |
		 [INNER] JOIN table2 USING (column1 [, …]) |
		 NATURAL [INNER] JOIN table2 |
		 LEFT|RIGHT|FULL [OUTER] JOIN table2 ON (condition1 [AND condition2 …]) |
		 LEFT|RIGHT|FULL [OUTER] JOIN table2 USING (column1 [, …]) |
		 CROSS JOIN table2 }
WHERE 	 ...
GROUP BY column_name | expr
HAVING 	 condition
ORDER BY 기준1 [ ASC | DESC] [, 기준2 [ASC | DESC], … ];
```

#### ANSI 표준 구문 - 1) JOIN USING

- 조인 조건으로 사용하는 컬럼 이름이 동일한 경우 사용

```sql
SELECT 	EMP_NAME, DEPT_NAME
FROM 	EMPLOYEE
JOIN 	DEPARTMENT USING (DEPT_ID)
WHERE 	JOB_ID = 'J6';
```

- 조인 조건에 사용되는 컬럼은 SELECT 절이나 JOIN 절에서 테이블 구분이 필요 없음

- 조인 조건으로 컬럼 여러 개를 사용하려면 쉼표로 구분
- 테이블 별칭은 사용할 수 없음

| EMP_NAME | DEPT_NAME |
| -------- | --------- |
| 펭하     | 펭귄      |
| 범이     | 물범      |

```sql
SELECT 	EMP_NAME, LOC_ID
FROM 	EMPLOYEE2
JOIN 	DEPARTMENT USING (DEPT_ID, LOC_ID);
```

- EMPLOYEE2 테이블은 EMPLOYEE 테이블에 LOC_ID 컬럼이 추가된 구조

| EMP_NAME | LOC_ID |
| -------- | ------ |
| 펭하     | A1     |
| 범이     | A1     |

#### ANSI 표준 구문 - 2) JOIN ON

- 조인 조건으로 사용하는 컬럼 이름이 서로 다른 경우 사용

```sql
SELECT 	DEPT_NAME,
		LOC_DESCRIBE
FROM 	DEPARTMENT
JOIN 	LOCATION ON (LOC_ID = LOCATION_ID);
```

| DEPT_NAME  | LOC_DESCRIBE |
| ---------- | ------------ |
| 회계팀     | 아시아지역1  |
| 기술지원팀 | 미주지역     |

#### OUTER JOIN

- 조건을 만족시키지 못하는 행까지 Result Set에 포함시키는 조인 유형

| EMP_NAME | DEPT_ID |
| -------- | ------- |
| 펭펭     | 90      |
| 펭하     |         |
| 펭빠     | 50      |
| 펭수     |         |

| DEPT_ID | DEPT_NAME |
| ------- | --------- |
| 90      | 회계팀    |
| 50      | 마케팅팀  |

| EMP_NAME | DEPT_NAME |
| -------- | --------- |
| 펭펭     | 회계팀    |
| 펭빠     | 마케팅팀  |

| EMP_NAME | DEPT_NAME |
| -------- | --------- |
| 펭펭     | 회계팀    |
| 펭빠     | 마케팅팀  |
| 펭하     |           |
| 펭수     |           |

#### OUTER JOIN - 2) ANSI 표준 구문

- 전체 행을 모두 포함시켜야 하는 테이블 기준
- LEFT, RIGHT 키워드 사용
  - LEFT : 기준 테이블이 JOIN 키워드보다 먼저 기술된 경우
  - RIGHT : 기준 테이블이 JOIN 키워드보다 나중에 기술된 경우

```sql
SELECT 	EMP_NAME, DEPT_NAME
FROM 	EMPLOYEE
LEFT 	JOIN DEPARTMENT USING (DEPT_ID)
ORDER BY 1;
```

```sql
SELECT 	EMP_NAME, DEPT_NAME
FROM 	DEPARTMENT
RIGHT 	JOIN EMPLOYEE USING (DEPT_ID)
ORDER 	BY 1;
```

| EMP_NAME | DEPT_NAME |
| -------- | --------- |
| 펭펭     | 회계팀    |
| 펭빠     | 마케팅팀  |
| 펭하     |           |
| 펭수     |           |

```sql
SELECT 	EMP_NAME, DEPT_NAME
FROM 	DEPARTMENT
LEFT 	JOIN EMPLOYEE USING (DEPT_ID);
```

```sql
SELECT 	EMP_NAME, DEPT_NAME
FROM 	EMPLOYEE
RIGHT 	JOIN DEPARTMENT USING (DEPT_ID);
```

| EMP_NAME | DEPT_NAME |
| -------- | --------- |
| 펭펭     | 회계팀    |
| 펭빠     | 마케팅팀  |
|          | 마케팅팀  |

#### FULL OUTER JOIN

- 양쪽 테이블을 동시에 OUTER JOIN하는 ANSI 표준 구문
- 오라클 전용 구문은 지원되지 않음

```sql
SELECT 	EMP_NAME, DEPT_NAME
FROM 	EMPLOYEE
FULL 	JOIN DEPARTMENT USING (DEPT_ID);
```

- 이 구문은 정상적으로 작동

| EMP_NAME | DEPT_NAME |
| -------- | --------- |
| 펭펭     | 회계팀    |
| 펭빠     | 마케팅팀  |
| 펭하     |           |
| 펭수     |           |
|          | 마케팅팀  |

```sql
SELECT 	EMP_NAME, DEPT_NAME
FROM 	EMPLOYEE E, DEPARTMENT D
WHERE 	E.DEPT_ID(+)= D.DEPT_ID(+);
```

- 이 구문은 에러 발생
- `ERROR` : ORA-01468 : outer-join 된 테이블은 1개만 지정할 수 있습니다.

#### Non Equijoin

```sql
SELECT 	EMP_NAME, SALARY, SLEVEL
FROM 	EMPLOYEE
JOIN 	SAL_GRADE ON (SALARY BETWEEN LOWEST AND HIGHEST)
ORDER BY 3;
```

[SAL_GRADE]

| SLEVEL | LOWEST  | HIGHEST |
| ------ | ------- | ------- |
| A      | 3000000 | 9000000 |
| B      | 2500000 | 2999999 |
| C      | 2000000 | 2490000 |
| D      | 1500000 | 1999999 |
| E      | 1000000 | 1499999 |

- 컬럼 값이 같은 경우가 아닌 범위에 속하는지의 여부를 확인하는 의미

| EMP_NAME | SALARY  | SLEVEL |
| -------- | ------- | ------ |
| 펭펭     | 5500000 | A      |
| 펭하     | 2500000 | B      |
| 펭빠     | 260000  | B      |

#### SELF JOIN

- 한 테이블을 두 번 조인하는 유형
- 테이블 별칭을 사용해야 함

```sql
SELECT 	E.EMP_NAME AS 직원,
		M.EMP_NAME AS 관리자
FROM 	EMPLOYEE E
JOIN 	EMPLOYEE M ON (E.MGR_ID = M.EMP_ID)
ORDER BY 1;
```

| 직원 | 관리자 |
| ---- | ------ |
| 펭펭 | 펭수   |
| 펭빠 | 펭수   |

#### N개 테이블 JOIN

- N개의 테이블을 조인하려면 최소 N-1개의 조인 조건(또는 N-1개의 JOIN 키워드)이 필요

| EMP_NAME | JOB_TITLE | DEPT_NAME   |
| -------- | --------- | ----------- |
| 펭수     | 대표이사  | 남극영업3팀 |

[JOB]

| JOB_ID | JOB_TITLE |
| ------ | --------- |
| J1     | 대표이사  |

[EMPLOYEE]

| EMP_NAME | JOB_ID | DEPT_ID |
| -------- | ------ | ------- |
| 펭수     | J1     | 90      |

[DEPARTMENT]]

| DEPT_ID | DEPT_NAME   |
| ------- | ----------- |
| 90      | 남극영업3팀 |

- 3개 테이블을 조인하려면 2개의 조건이 필요하다

```sql
SELECT 	EMP_NAME,
		LOC_DESCRIBE,
		DEPT_NAME
FROM 	EMPLOYEE
JOIN 	LOCATION ON (LOCATION_ID = LOC_ID)
JOIN 	DEPARTMENT USING (DEPT_ID);
```

- 구문 순서대로 EMPLOYEE와 LOCATION이 먼저 조인 되어야 함
- 두 테이블은 직접적인 연관 관계가 없으므로 오류 발생
- `ERROR` : ORA-00904 : 'LOC_ID' : 부적합한 식별자

```sql
SELECT EMP_NAME,
LOC_DESCRIBE,
DEPT_NAME
FROM EMPLOYEE
JOIN DEPARTMENT USING (DEPT_ID)
JOIN LOCATION ON (LOCATION_ID = LOC_ID);
```

- DEPARTMENT 테이블이 EMPLOYEE와 LOCATION을 연결시킬 수 있도록 구문을 수정해야 함

**결과(일부)**

| EMP_NAME | LOC_DESCRIBE | DEPT_NAME   |
| -------- | ------------ | ----------- |
| 펭펭     | 아시아지역   | 남극영업3팀 |

```sql
SELECT 	EMP_NAME,
		DEPT_NAME
FROM 	EMPLOYEE
JOIN 	JOB USING (JOB_ID)
JOIN 	DEPARTMENT USING (DEPT_ID)
JOIN 	LOCATION ON (LOC_ID = LOCATION_ID)
WHERE 	JOB_TITLE='대리'
AND 	LOC_DESCRIBE LIKE '아시아%';
```

- 컬럼 조회 목적이 아닌 WHERE 조건을 위해 조인에 포함되어야 하는 테이블이 필요

- JOB 테이블은 직급 조건을 위해, LOCATION 테이블은 지역 조건을 위해 조인되었음

| EMP_NAME | DEPT_NAME   |
| -------- | ----------- |
| 펭빠     | 해외영업2팀 |

### 4.2.5 SET Operator - 개념

- 두 개 이상의 쿼리 결과를 하나로 결합시키는 연산자
- SELECT 절에 기술하는 컬럼 개수와 데이터 타입은 모든 쿼리에서 동일해야 함

|   유형    |                             설명                             |
| :-------: | :----------------------------------------------------------: |
|   UNION   |   양쪽 쿼리 결과를 모두 포함<br/>(중복 결과는 1번만 표현)    |
| UNION ALL |    양쪽 쿼리 결과를 모두포함<br/>(중복 결과도 모두 표현)     |
| INTERSECT |         양쪽 쿼리 결과에 모두<br/>포함되는 행만 표현         |
|   MINUS   | 쿼리1 결과에만 포함되고 쿼리2<br/>결과에는 포함되지 않는 행만<br/>표현 |

#### 활용

- SET Operator와 JOIN 관계

```sql
SELECT 	EMP_ID,
		ROLE_NAME
FROM 	EMPLOYEE_ROLE
JOIN 	ROLE_HISTORY USING (EMP_ID, ROLE_NAME);
```

```sql
SELECT 	EMP_ID,
		ROLE_NAME
FROM 	EMPLOYEE_ROLE
INTERSECT
SELECT 	EMP_ID,
		ROLE_NAME
FROM 	ROLE_HISTORY;
```

| EMP_ID | ROLE_NAME |
| ------ | --------- |
| 104    | SE        |

```sql
SELECT 	EMP_NAME,
		JOB_TITLE 직급
FROM 	EMPLOYEE
JOIN 	JOB USING (JOB_ID)
WHERE 	JOB_TITLE IN ('대리', '사원')
ORDER BY 2,1;
```

```sql
SELECT 	EMP_NAME, '사원' 직급
FROM 	EMPLOYEE
JOIN 	JOB USING (JOB_ID)
WHERE 	JOB_TITLE = '사원'
UNION
SELECT 	EMP_NAME, '대리'
FROM 	EMPLOYEE
JOIN 	JOB USING (JOB_ID)
WHERE 	JOB_TITLE = '대리'
ORDER BY 2,1;
```

| EMP_NAME | 직급 |
| -------- | ---- |
| 펭하     | 대리 |
| 펭펭     | 사원 |

### 4.2.6 Subquery - 개념

- 하나의 쿼리가 다른 쿼리에 포함되는 구조
- 다른 쿼리에 포함된 내부 쿼리(서브 쿼리)는 외부 쿼리(메인 쿼리)에 사용될 값을 반환하는 역할

```
요구사항 : '나승원' 직원과 같은 부서원들을 조회하라
요구사항을 해결하기 위해서는
	1. '나승원' 직원이 속한 부서가 어떤 부서인지(부서 번호 또는 부서 이름) 찾고
	2. 부서 정보(부서 번호 또는 부서 이름)를 이용하여 해당 부서에 속한 직원들을 찾아야 함
```

[메인쿼리]

```sql
SELECT 	EMP_NAME 서브쿼리
FROM 	EMPLOYEE
WHERE 	DEPT_ID = '나승원의 소속부서 ID' ;
```

[서브쿼리]

```sql
SELECT 	DEPT_ID
FROM 	EMPLOYEE
WHERE 	EMP_NAME = '나승원';
```

#### 구문

```sql
SELECT 	...
FROM 	...
WHERE 	expr operator ( SELECT ...
FROM 	...
WHERE 	... ) ;
```

**[구문 설명]**

- 서브쿼리는 일반적인 SQL 구문과 동일(별도 형식이 존재하는 것이 아님)
- SELECT, FROM, WHERE, HAVING 절 등에서 사용 가능
- 서브쿼리는 ( )로 묶어서 표현
- 서브쿼리에는 ; 를 사용하지 않음
- 유형에 따라 연산자를 구분해서 사용

#### 유형

- 단일 행 서브쿼리
  - 단일 행 반환
  - 단일 행 비교 연산자(=, >, >=, <, <=, <> 등) 사용
- 다중 행 서브쿼리
  - 여러 행 반환
  - 다중 행 비교 연산자(IN, ANY, ALL 등) 사용

#### 단일 행 서브쿼리

```sql
SELECT 	EMP_NAME,
		JOB_ID,
		SALARY
FROM 	EMPLOYEE
WHERE 	JOB_ID = (SELECT 	JOB_ID
				   FROM  	EMPLOYEE
				   WHERE 	EMP_NAME = '나승원')
AND 	SALARY > (SELECT 	SALARY
				  FROM 		EMPLOYEE
				  WHERE 	EMP_NAME = '나승원') ;
```

```sql
SELECT 	EMP_NAME,
		JOB_ID,
		SALARY
FROM 	EMPLOYEE
WHERE 	JOB_ID = 'J5'
AND 	SALARY > 2300000;
```

| EMP_NAME | JOB_ID | SALARY  |
| -------- | ------ | ------- |
| 펭펭     | J5     | 2600000 |

```sql
SELECT 	EMP_NAME,
		JOB_ID,
		SALARY
FROM 	EMPLOYEE
WHERE 	SALARY = (SELECT MIN(SALARY)
				  FROM 	 EMPLOYEE) ;
```

```sql
SELECT 	EMP_NAME,
		JOB_ID,
		SALARY
FROM 	EMPLOYEE
WHERE 	SALARY = 1500000;
```

| EMP_NAME | JOB_ID | SALARY  |
| -------- | ------ | ------- |
| 펭펭     | J7     | 1500000 |
| 물범     | J7     | 1500000 |

```sql
SELECT 		DEPT_NAME,
			SUM(SALARY)
FROM 		EMPLOYEE
LEFT JOIN 	DEPARTMENT USING (DEPT_ID)
GROUP BY 	DEPT_ID, DEPT_NAME
HAVING 		SUM(SALARY) = (SELECT 	 MAX(SUM(SALARY))
					  		FROM 	 EMPLOYEE
					  		GROUP BY DEPT_ID);
```

```sql
SELECT 		DEPT_NAME,
			SUM(SALARY)
FROM 		EMPLOYEE
LEFT JOIN 	DEPARTMENT USING (DEPT_ID)
GROUP BY  	DEPT_ID, DEPT_NAME
HAVING 	  	SUM(SALARY) = 18100000;
```

| DEPT_NAME   | SUM(SALARY) |
| ----------- | ----------- |
| 해외영업3팀 | 18100000    |

```sql
SELECT 	EMP_ID,
		EMP_NAME
FROM 	EMPLOYEE
WHERE 	SALARY = (SELECT MIN(SALARY)
				  FROM 	 EMPLOYEE
				  GROUP BY DEPT_ID);
```

- `ERROR` : ORA-01427 : 단일 행 하위 질의에 2개 이상의 행이 리턴되었습니다.

| MIN(SALARY) |      |
| ----------- | ---- |
| 15000000    |      |
| 20000000    |      |
| 25000000    |      |

#### 다중 행 서브쿼리 1) IN, NOT IN 연산자

```sql
SELECT 	EMP_ID,
		EMP_NAME,
		'관리자' AS 구분
FROM 	EMPLOYEE
WHERE 	EMP_ID IN (SELECT MGR_ID FROM EMPLOYEE)
UNION
SELECT 	EMP_ID,
		EMP_NAME,
		'직원'
FROM 	EMPLOYEE
WHERE 	EMP_ID NOT IN (SELECT MGR_ID FROM EMPLOYEE
					   WHERE 	MGR_ID IS NOT NULL)
ORDER BY 3, 1;
```

- NOT IN 연산자와 다중 행 서브쿼리를 함께 사용하는 경우, 서브쿼리 결과에 NULL이 포함되면 전체 결과가 NULL이 됨
  - 서브쿼리 결과에서 NULL인 경우를 제외시켜야 함

| EMP_ID | EMP_NAME | 구분   |
| ------ | -------- | ------ |
| 100    | 펭펭     | 관리자 |
| 104    | 펭하     | 관리자 |
| 201    | 범이     | 직원   |



