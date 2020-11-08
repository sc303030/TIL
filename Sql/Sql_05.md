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

##### 기타 함수 NVL 사용 예

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

#### 기타 함수 DECODE

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

##### 기타 함수 DECODE 사용 예

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

#### CASE

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

##### CASE 사용 예

- DECODE 함수와 유사한 ANSI 표준 구문

| 입력 타입 | 구문                                                         | 반환타입 |
| --------- | ------------------------------------------------------------ | -------- |
| ANY       | **CASE** expr **WHEN** search1 **THEN** result1 [**WHEN**..**THEN**..]\[**ELSE** default] **END** | ANY      |
| ANY       | **CASE** **WHEN** condition1 **THEN** result1 [**WHEN**..**THEN**..]\[**ELSE** default] **END** | ANY      |

**[파라미터]**

| 파라미터  | 설명                                                         |
| :-------: | ------------------------------------------------------------ |
|   expr    | 대상 컬럼 또는 문자(열)                                      |
|  search   | expr과 비교하려는 값                                         |
| condition | 비교 조건                                                    |
|  result   | • IF expr = search 인 경우의 반환 값<br/>• 비교 조건을 만족시키는 경우의 반환 값 |
|  default  | • expr과 search가 일치하지 않는 경우의 기본 반환 값<br/>• 비교 조건을 만족시키지 않는 경우의 기본 반환 값<br/>• default를 지정하지 않으면 일치하지 않거나 조건을 만족시키지 않는 경우 NULL 반환 |

```sql
SELECT 	EMP_NAME,
		JOB_ID,
		SALARY,
		CASE JOB_ID
			WHEN 'J7' THEN TO_CHAR(SALARY*1.1)
			WHEN 'J6' THEN TO_CHAR(SALARY*1.15)
			WHEN 'J5' THEN TO_CHAR(SALARY*1.2)
			ELSE TO_CHAR(SALARY*1.05) END AS 인상급여
FROM 	EMPLOYEE;
```

| EMP_NAME | JOB_ID | SALARY  | 인상급여 |
| -------- | ------ | ------- | -------- |
| 펭하     | J7     | 1500000 | 1650000  |
| 펭빠     | J6     | 2420000 | 2783000  |
| 펭펭     | J5     | 2600000 | 3120000  |
| 펭수     | J4     | 2500000 | 2625000  |

```sql
SELECT 	EMP_ID,
		EMP_NAME,
		SALARY,
		CASE WHEN SALARY <= 3000000 THEN '초급'
			 WHEN SALARY <= 4000000 THEN '중급'
			 ELSE '고급' END AS 구분
FROM EMPLOYEE;
```

| EMP_ID | EMP_NAME | SALARY  | 구분 |
| ------ | -------- | ------- | ---- |
| 100    | 펭펭     | 9000000 | 고급 |
| 144    | 펭하     | 3090000 | 초급 |
| 205    | 펭빠     | 3500000 | 중급 |

### 4.1.4 주요 단일 행 함수 – 함수 중첩

- 중첩 사용 가능
- 가장 안쪽의 함수부터 바깥 쪽 방향으로 차례대로 실행
- 먼저 실행된 함수의 반환 값이 바깥 쪽 함수의 입력 값이 됨 -> 반환되는 함수 결과 데이터 타입에 주의

| 구문                       |      |
| -------------------------- | ---- |
| 함수3( 함수2( 함수1( ) ) ) |      |

**[실행순서]**

```sql
함수1( ) -> 결과1
		   함수2( )-> 결과2
					  함수3( )->결과3
```

- 직원 별 Email ID 조회

```sql
SELECT 	EMP_NAME,
		EMAIL,
		SUBSTR(EMAIL, 1, 
               INSTR(EMAIL, '@')-1) AS ID
FROM 	EMPLOYEE;
```

| EMP_NAME | EMAIL          | ID     |
| -------- | -------------- | ------ |
| 펭하     | aa_aaa@acc.com | aa_aaa |

### 4.1.5 주요 그룹 함수

- 그룹 함수는 NULL을 계산하지 않음

| 함수  | 설명                                                         |
| :---: | ------------------------------------------------------------ |
|  SUM  | 총합 계산                                                    |
|  AVG  | • 평균 계산<br/>• NULL을 제외하므로 결과가 달라질 수 있음    |
|  MIN  | • 최소 값 반환<br/>• DATE 타입: 가장 오래 전 날짜를 의미<br/>• CHARACTER 타입: 해당 character set의 내부 값이 가장 작은 문자를 의미 |
|  MAX  | • 최대 값 반환<br/>• DATE 타입: 가장 최근 날짜를 의미<br/>• CHARACTER 타입: 해당 character set의 내부 값이 가장 큰 문자를 의미 |
| COUNT | Result Set 전체 행 수 반환                                   |

#### SUM

- 입력 값의 총합을 계산하여 반환하는 함수

| 입력 타입 | 구문                           | 반환 타입 |
| --------- | ------------------------------ | --------- |
| NUMBER    | **SUM**( **[DISTINCT]** expr ) | NUMBER    |

| DEPT_ID | SALARY  |
| ------- | ------- |
| 50      | 1500000 |
| 50      | 2100000 |
| 50      | 2300000 |
| 50      | 2100000 |
| 50      | 2300000 |

```sql
SELECT 	SUM(SALARY), SUM(DISTINCT SALARY)
FROM 	EMPLOYEE
WHERE 	DEPT_ID = '50'
OR 		DEPT_ID IS NULL;
```

| SUM(SALARY) | SUM(DISTINCT SALARY) |
| ----------- | -------------------- |
| 10300000    | 5900000              |

- **SUM(SALARY)** : 전체 8건의 합
- **SUM(DISTINCT SALARY)** : 중복 값 제외한 3건의 합

#### AVG

- 입력 값의 평균을 계산하여 반환하는 함수

| 입력 타입 | 구문                           | 반환 타입 |
| --------- | ------------------------------ | --------- |
| NUMBER    | **AVG**( **[DISTINCT]** expr ) | NUMBER    |

| DEPT_ID | BONUS_PCT |
| ------- | --------- |
| 50      | 0.2       |
| 90      |           |
| 50      | 0.1       |
| 50      | 0.3       |
| 90      | 0.1       |

```sql
SELECT 	AVG(BONUS_PCT) AS 기본평균,
		AVG(DISTINCT BONUS_PCT) AS 중복제거평균,
		AVG(NVL(BONUS_PCT,0)) AS NULL포함평균
FROM 	EMPLOYEE
WHERE 	DEPT_ID IN ('50', '90')
OR 		DEPT_ID IS NULL;
```

| 기본평균 | 중복제거평균 | NULL포함평균     |
| -------- | ------------ | ---------------- |
| 0.175    | 0.2          | 0.06363636363636 |

- **기본평균** : BONUS_PCT 값이 있는 4건의 평균
- **중복제거평균** : 중복 값을 제외한 3건의 평균
- **NULL포함평균** : 전체 5건의 평균

#### MIN/MAX

- 최소 값/최대 값을 반환하는 함수

| 입력 타입 | 구문                             | 반환 타입 |
| --------- | -------------------------------- | --------- |
| ANY       | **MIN**( expr ), **MAX**( expr ) | ANY       |

| JOB_ID | HIRE_DATE | SALARY  |
| ------ | --------- | ------- |
| J1     | 90/04/01  | 9000000 |
| J2     | 04/04/30  | 5500000 |
| J2     | 95/12/30  | 3600000 |

```sql
SELECT 	MAX(JOB_ID), MIN(JOB_ID),
		MAX(HIRE_DATE), MIN(HIRE_DATE),
		MAX(SALARY), MIN(SALARY)
FROM 	EMPLOYEE
WHERE 	DEPT_ID IN ('50','90');
```

| MAX(JOB_ID) | MIN(JOB_ID) | MAX(HIRE_DATE) | MIN(HIRE_DATE) | MAX(SALARY) | MIN(SALARY) |
| ----------- | ----------- | -------------- | -------------- | ----------- | ----------- |
| J2          | J1          | 04/04/30       | 90/04/01       | 9000000     | 3600000     |

#### COUNT

- Result Set의 행 수를 반환하는 함수

| 입력 타입 | 구문                                   | 반환 타입 |
| --------- | -------------------------------------- | --------- |
| ANY       | **COUNT**( \* \| **[DISTINCT]** expr ) | NUMBER    |

**[파라미터]**

|   파라미터    | 설명                                                   |
| :-----------: | ------------------------------------------------------ |
|       *       | Result Set의 전체 행 수 반환                           |
| DISTINCT expr | expr에 포함된 값 중 NULL과 중복 값을 제외한 행 수 반환 |
|     expr      | expr에 포함된 값 중 NULL을 제외한 행 수 반환           |

##### COUNT 사용 예

| JOB_ID |      |
| ------ | ---- |
| J7     |      |
| J6     |      |
| J7     |      |
|        |      |
| J4     |      |
| J5     |      |

```sql
SELECT 	COUNT(*),
		COUNT(JOB_ID),
		COUNT(DISTINCT JOB_ID)
FROM 	EMPLOYEE
WHERE 	DEPT_ID = '50'
OR 		DEPT_ID IS NULL;
```

| COUNT(*) | COUNT(JOB_ID) | COUNT(DISTINCT JOB_ID) |
| -------- | ------------- | ---------------------- |
| 6        | 5             | 4                      |

