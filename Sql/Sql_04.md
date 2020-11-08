# Sql_04

### 3.2.1 ORDER BY

- SELECT 구문 실행 결과를 특정 컬럼 값 기준으로 정렬할 때 사용

```sql
SELECT ...
FROM ...
WHERE ...
ORDER BY 기준1 [ ASC | DESC] [, 기준2 [ASC | DESC], … ];
```

**[구문 설명]**

- 항상 SELECT 구문의 맨 마지막에 위치
- 지정한 컬럼 값을 기준으로 정렬
- 정렬 조건
  - ASC : '오름차순'(기본 값)
  - DESC : '내림차순'
- 여러 개의 정렬 기준을 사용하면 기술된 순서대로 적용
- 기준 별로 정렬 조건 구분 적용 가능
- 컬럼 이름, 컬럼 별칭, 컬럼 기술 순서로 표현 가능

#### ORDER BY - 사용 예

```sql
SELECT 	EMP_NAME, SALARY
FROM 	EMPLOYEE
WHERE 	DEPT_ID = '50'
OR 		DEPT_ID IS NULL
ORDER BY SALARY DESC;
```

| EMP_NAME | SALARY  |
| -------- | ------- |
| 펭하     | 3400000 |
| 펭빠     | 2600000 |

```sql
SELECT 	EMP_NAME, HIRE_DATE, DEPT_ID
FROM 	EMPLOYEE
WHERE 	HIRE_DATE > TO_DATE('20030101','YYYYMMDD')
ORDER BY DEPT_ID DESC, HIRE_DATE, EMP_NAME;
```

| EMP_NAME | HIRE_DATE | DEPT_ID |
| -------- | --------- | ------- |
| 펭빠     | 03/09/17  |         |
| 펭하     | 04/09/30  | 90      |

- 컬럼 별칭을 사용한 경우 별칭 사용 가능

```sql
SELECT 	EMP_NAME AS 이름,
		HIRE_DATE AS 입사일,
		DEPT_ID AS 부서코드
FROM 	EMPLOYEE
WHERE 	HIRE_DATE > TO_DATE('20030101','YYYYMMDD')
ORDER BY 부서코드 DESC, 입사일, 이름;
```

| 이름 | 입사일   | 부서코드 |
| ---- | -------- | -------- |
| 펭빠 | 03/09/17 |          |
| 펭하 | 04/09/30 | 90       |

- SELECT 절에 기술된 순서로 표시 가능

```sql
SELECT 	EMP_NAME AS 이름,
		HIRE_DATE AS 입사일,
		DEPT_ID AS 부서코드
FROM 	EMPLOYEE
WHERE 	HIRE_DATE > TO_DATE('20030101','YYYYMMDD')
ORDER BY 3 DESC, 2, 1;
```

| 이름 | 입사일   | 부서코드 |
| ---- | -------- | -------- |
| 펭빠 | 03/09/17 |          |
| 펭하 | 04/09/30 | 90       |

### 3.2.2 GROUP BY - 하위 데이터 그룹 개념

| EMP_NAME | SALARY  | DEPT_ID |
| -------- | ------- | ------- |
| 펭하     | 3400000 | 10      |
| 펭빠     | 3000000 | 10      |
| 펭수     | 3500000 | 20      |
| 펭펭     | 3200000 | 20      |

| DEPT_ID | SUM(SALARY) |
| ------- | ----------- |
| 10      | 640000      |
| 20      | 670000      |

- 각 부서가 하나의 데이터 그룹
- 부서 수 만큼 데이터 그룹 생성
- 그룹 별(부서 별)로 그룹함수 적용

```sql
SELECT 	...
FROM 	...
WHERE 	...
GROUP BY column_name | expr
ORDER BY 기준1 [ ASC | DESC] [, 기준2 [ASC | DESC], … ];
```

**[구문 설명]**

- GROUP BY 절에 기술한 컬럼이나 표현식을 기준으로 데이터 그룹 생성
- 각 그룹 별로 SELECT 절에 기술한 그룹 함수가 적용
- SELECT 절에 기술한 컬럼 중, 그룹 함수에 사용되지 않은 컬럼은 GROUP BY 절에 반드시 기술되어야 함

- 제약 사항
  - WHERE 절에는 그룹 함수를 사용할 수 없음
  - GROUP BY 절에는 컬럼 이름만 사용 가능(별칭, 순서 사용 불가)

```sql
SELECT 	DEPT_ID AS 부서,
		ROUND(AVG(SALARY),-4) AS 평균급여
FROM 	EMPLOYEE
GROUP BY DEPT_ID
ORDER BY 1;
```

| 부서 | 평균급여 |
| ---- | -------- |
| 10   | 2520000  |
| 20   | 2500000  |

```sql
SELECT DECODE(SUBSTR(EMP_NO,8,1),'1', '남', '3', '남', '여') AS 성별,
		ROUND(AVG(SALARY),-4) AS 평균급여
FROM 	EMPLOYEE
GROUP BY DECODE(SUBSTR(EMP_NO,8,1),'1', '남', '3', '남', '여')
ORDER BY 2;
```

| 성별 | 평균급여 |
| ---- | -------- |
| 여   | 2260000  |
| 남   | 3360000  |

#### GROUP BY 사용 예

```sql
SELECT 	DEPT_ID, COUNT(*)
FROM 	EMPLOYEE;
```

- 'ORA-00937' 에러가 발생하면 GROUP BY 가 누락되었는지 확인

- 그룹 함수 COUNT는 유일한 반환 값만 생성할 수 있는데, SELECT 절에서는 여러 개의 DEPT_ID 값을 함께
  표시하도록 작성하였음
  - 그룹 함수가 동작해야 하는 그룹을 찾지 못한 경우

**[수정 구문]**

```sql
SELECT 	DEPT_ID,
		COUNT(*)
FROM 	EMPLOYEE
GROUP BY DEPT_ID
ORDER BY 1;
```

| DEPT_ID | COUNT(*) |
| ------- | -------- |
| 10      | 3        |
| 20      | 3        |

```sql
SELECT EMP_NAME, DEPT_ID, COUNT(*)
FROM EMPLOYEE
GROUP BY DEPT_ID;
```

- 'ORA-00979' 에러가 발생하면 GROUP BY 절에 누락된 select list가 있는지 확인

**[수정 구문]**

```sql
SELECT 	EMP_NAME, DEPT_ID, COUNT(*)
FROM 	EMPLOYEE
GROUP BY EMP_NAME, DEPT_ID;
```

| EMP_NAME | DEPT_ID | COUNT(*) |
| -------- | ------- | -------- |
| 펭하     | 60      | 1        |
| 펭수     | 50      | 1        |

- 컬럼 별칭이나 컬럼 기술 순서는 사용할 수 없음

```sql
SELECT 	DEPT_ID AS 부서,
		SUM(SALARY)
FROM 	EMPLOYEE
GROUP BY 부서;
```

- `ERROR` ORA-00904 : '부서': 부적합한 식별자

```sql
SELECT 	DEPT_ID AS 부서,
		SUM(SALARY)
FROM 	EMPLOYEE
GROUP BY 1;
```

- `ERROR` ORA-00979 : GROUP BY 표현식이 아닙니다.

```sql
SELECT 	MAX(SUM(SALARY))
FROM 	EMPLOYEE
GROUP BY DEPT_ID;
```

| MAX(SUM(SALARY)) |      |
| ---------------- | ---- |
| 18100000         |      |

| DEPT_ID | SUM(SALARY) |
| ------- | ----------- |
|         | 3800000     |
| 50      | 13800000    |
| 90      | 18100000    |

- 그룹 함수는 2번까지 중첩 사용 가능

```sql
SELECT 	DEPT_ID,
		MAX(SUM(SALARY))
FROM 	EMPLOYEE
GROUP BY DEPT_ID;
```

- `ERROR` ORA-00937 : 단일 그룹의 그룹 함수가 아닙니다.

### 3.2.3 HAVING

- GROUP BY에 의해 그룹화 된 데이터에 대한 그룹 함수 실행 결과를 제한하기 위해 사용
  (WHERE는 테이블에 포함된 원본 데이터를 제한하기 위해 사용)

```sql
SELECT 	...
FROM 	...
WHERE 	...
GROUP BY column_name | expr
HAVING condition
ORDER BY 기준1 [ ASC | DESC] [, 기준2 [ASC | DESC], … ];
```

```sql
SELECT 	DEPT_ID, SUM(SALARY)
FROM 	EMPLOYEE
GROUP BY DEPT_ID
HAVING SUM(SALARY) > 9000000;
```

| DEPT_ID | SUM(SALARY) |
| ------- | ----------- |
| 50      | 13800000    |
| 90      | 18100000    |
| 60      | 9900000     |

- 부서 별 급여 총합을 계산한 결과 중 9000000 이상인 경우만 선택

```sql
SELECT 	DEPT_ID, SUM(SALARY)
FROM 	EMPLOYEE
WHERE 	SUM(SALARY) > 9000000
GROUP BY DEPT_ID;
```

- `ERROR` ORA-00934 : 그룹 함수는 허가되지 않습니다.

- WHERE 절에는 그룹 함수를 사용할 수 없음
  - WHERE 절이 수행되어야 그룹 함수가 실행 될 대상 그룹이 결정