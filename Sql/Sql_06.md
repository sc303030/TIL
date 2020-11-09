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

13p

