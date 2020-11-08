# Sql_05

### 4.1.3 주요 단일 행 함수 – 기타 함수 NVL

- NULL을 지정한 값으로 변환하는 함수

| 입력 타입 | 구문                    | 반환 타입 |
| --------- | ----------------------- | --------- |
| ANY       | **NVL**( expr1, expr2 ) | ANY       |

**[파라미터]**

| 파라미터 | 설명                                                         |
| :------: | :----------------------------------------------------------- |
|  expr1   | • NULL을 포함하는 컬럼<br/>• NULL이 없는 경우는 expr1을 반환 |
|  expr2   | • expr1이 NULL인 경우 변환할 값<br/>• expr1의 데이터 타입과 동일한 타입으로 지정 |

#### 기타 함수 NVL 사용 예

```sql
SELECT 	EMP_NAME, SALARY, NVL(BONUS_PCT,0)
FROM 	EMPLOYEE
WHERE 	SALARY > 3500000;
```

| EMP_NAME | SALARY  | NVL(BONUS_PCT,0) |
| -------- | ------- | ---------------- |
| 펭하     | 9000000 | 0.2              |
| 펭펭     | 5500000 | 0                |

```sql
SELECT 	EMP_NAME,
		(SALARY*12)+((SALARY*12)*BONUS_PCT)
FROM 	EMPLOYEE
WHERE 	SALARY > 3500000;
```

| EMP_NAME | (SALARY*12)+((SALARY*12)*BONUS_PCT) |
| -------- | ----------------------------------- |
| 펭하     | 129600000                           |
| 펭펭     | 0                                   |

- NULL에 대한 전체 계산 결과는 NULL임

```sql
SELECT 	EMP_NAME,
		(SALARY*12)+
		( (SALARY*12)*NVL(BONUS_PCT,O) )
FROM 	EMPLOYEE
WHERE 	SALARY > 3500000;
```

| EMP_NAME | (SALARY\*12)+ ( (SALARY\*12)\*NVL(BONUS_PCT,O) ) |
| -------- | ------------------------------------------------ |
| 펭하     | 1296000000                                       |
| 펭펭     | 660000000                                        |
| 펭빠     | 432000000                                        |

### 기타 함수 DECODE

- SELECT 구문으로 IF-ELSE 논리를 제한적으로 구현한 오라클 DBMS 전용 함수

| 입력 타입 | 구문                                                         | 반환 타입 |
| --------- | ------------------------------------------------------------ | --------- |
| ANY       | **DECODE**(expr, search1, result1 [ ,searchN, resultN, ...] [, default ]) | ANY       |

**[파라미터]**

| 파라미터 | 설명                                                         |
| :------: | ------------------------------------------------------------ |
|   expr   | 대상 컬럼 또는 문자(열)                                      |
|  search  | expr과 비교하려는 값                                         |
|  result  | IF expr = search 인 경우의 반환 값                           |
| default  | • expr과 search가 일치하지 않는 경우의 기본 반환 값<br/>• default를 지정하지 않고 expr과 search가 일치하지 않으면 NULL 반환 |

#### 기타 함수 DECODE 사용 예

```sql
SELECT 	EMP_NAME,
		DECODE(SUBSTR(EMP_NO,8,1),'1', '남', '2', '여', '3', '남', '4', '여') AS 성별
FROM 	EMPLOYEE
WHERE 	DEPT_ID = '50';
```

**default 값을 지정하여 코드를 단순화**

```sql
SELECT 	EMP_NAME,
		DECODE(SUBSTR(EMP_NO,8,1),'1', '남', '3', '남', '여') AS 성별
FROM 	EMPLOYEE
WHERE 	DEPT_ID = '50';
```

| EMP_NAME | 성별 |
| -------- | ---- |
| 펭하     | 여   |
| 펭펭     | 남   |

```sql
SELECT 	EMP_ID, EMP_NAME,
		DECODE(MGR_ID, NULL, '없음', MGR_ID) AS 관리자
FROM 	EMPLOYEE
WHERE 	JOB_ID = 'J4';
```

- DECODE 함수는 2개의 NULL을 동일하게 취급함

```sql
SELECT 	EMP_ID, EMP_NAME,
		NVL(MGR_ID, '없음') AS 관리자
FROM 	EMPLOYEE
WHERE 	JOB_ID = 'J4';
```

- NULL을 처리하는 경우에는 NVL 함수와 동일한 결과를 나타냄

| EMP_ID | EMP_NAME | 관리자 |
| ------ | -------- | ------ |
| 103    | 펭하     | 104    |
| 207    | 펭펭     | 없음   |

```sql
SELECT 	EMP_NAME,
		JOB_ID,
		SALARY,
		DECODE(JOB_ID,
		'J7', SALARY*1.1,
		'J6', SALARY*1.15,
		'J5', SALARY*1.2,
		SALARY*1.05) AS 인상급여
FROM 	EMPLOYEE;
```

| EMP_NAME | JOB_ID | SALARY  | 인상급여 |
| -------- | ------ | ------- | -------- |
| 펭하     | J7     | 9000000 | 9450000  |
| 펭빠     | J6     | 5500000 | 5775000  |
| 펭펭     | J5     | 3600000 | 3780000  |
| 펭수     | J3     | 2600000 | 2730000  |

### CASE

- DECODE 함수와 유사한 ANSI 표준 구문

| 입력타입 | 구문                                                         | 반환타입 |
| -------- | ------------------------------------------------------------ | -------- |
| ANY      | • **CASE**  expr **WHEN ** search1 **THEN** result1 [**WHEN**\..**THEN**\..]\[**ELSE** default] **END**<br/>• **CASE**  **WHEN ** condition1 **THEN** result1 [**WHEN**\..**THEN**\..]\[**ELSE** default] **END** | ANY      |

**[파라미터]**

| 파라미터  | 설명                                                         |
| :-------: | ------------------------------------------------------------ |
|   expr    | 대상 컬럼 또는 문자(열)                                      |
|  search   | expr과 비교하려는 값                                         |
| condition | 비교 조건                                                    |
|  result   | • IF expr = search 인 경우의 반환 값<br/>• 비교 조건을 만족시키는 경우의 반환 값 |
|  default  | • expr과 search가 일치하지 않는 경우의 기본 반환 값<br/>• 비교 조건을 만족시키지 않는 경우의 기본 반환 값<br/>• default를 지정하지 않으면 일치하지 않거나 조건을 만족시키지 않는 경우 NULL 반환 |

8p