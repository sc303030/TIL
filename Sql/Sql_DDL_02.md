### 5.1.6 테이블 수정 - 컬럼 삭제

- 컬럼 하나를 삭제 가능
- 컬럼 여러 개를 한번에 삭제 가능(구문이 달라짐)
- 주의 사항
  - 삭제 대상 컬럼에 데이터가 포함되어 있어도 삭제됨
  - 삭제 작업 후에는 테이블에 반드시 컬럼이 하나 이상 남아 있어야 함
    - 모든 컬럼을 삭제할 수 없음
  - 삭제된 컬럼은 복구할 수 없음

```sql
ALTER TABLE EMP4
DROP COLUMN EMP_ID;
또는
ALTER TABLE EMP4
DROP(EMP_ID);
```

- 단일 컬럼의 삭제 구문은 'COLUMN' 키워드나 ( ) 사용 가능

```sql
ALTER TABLE EMP5
DROP (EMP_ID, EMP_NAME);
```

- 여러 컬럼의 삭제 구문은 ( ) 사용

##### "CASCADE CONSTRAINTS" OPTION

- 삭제되는 컬럼을 참조하고 있는 다른 컬럼에 설정된 제약조건까지 함께 삭제

```sql
CREATE TABLE TB1
(PK NUMBER PRIMARY KEY,
FK NUMBER REFERENCES TB1,
COL1 NUMBER,
CHECK ( PK > 0 AND COL1 > 0 ));
```

```sql
ALTER TABLE TB1
DROP (PK) CASCADE CONSTRAINTS;
ALTER TABLE TB1
DROP (COL1)CASCADE CONSTRAINTS;
```

```sql
ALTER TABLE TB1 DROP (PK);
```

- FK 컬럼에 PK 컬럼을 참조하는 REFREENCES 제약조건이 설정되어 있으므로 삭제 불가
- ERROR : ORA-12992 : 부모 키 열을 삭제할 수 없습니다.

```sql
ALTER TABLE TB1 DROP (COL1);
```

- COL1컬럼에 CHECK 제약조건이 설정되어 있으므로 삭제 불가
- ERROR : ORA-12991 : 열이 다중-열 제약 조건에 참조되었습니다.

#### 제약조건 삭제 예

[CONSTRAINT_EMP 테이블 제약조건 현황]

| 이름   | 유형 | 컬럼  | 참조     | 삭제규칙 | 내용    |
| ------ | ---- | ----- | -------- | -------- | ------- |
| NENAME | C    | ENAME |          |          | \<LONG> |
| NENO   | C    | ENO   |          |          | \<LONG> |
| PKEID  | P    | EID   |          |          | \<LONG> |
| FKJID  | R    | JID   | PK_JOBID | SET NULL | \<LONG> |

[CHECK, REFERENCES 유형 제약조건 삭제 예]

```sql
-CHECK 제약조건
ALTER TABLE CONSTRAINT_EMP
DROP CONSTRAINT CHK;
-REFERENCE제약조건
ALTER TABLE CONSTRAINT_EMP
DROP CONSTRAINT FKJID
DROP CONSTRAINT FKMID
DROP CONSTRAINT FKDID;
```

- CHECK. REFERENCES 유형 제약조건 삭제
  - 'DROP CONSTRAINT' 키워드와 제약조건 이름을 기술하여 삭제 가능

[PRIMARY KEY 유형 제약조건 삭제 예]

```sql
ALTER TABLE CONSTRAINT_EMP
DROP CONSTRAINT PK_EMPID [CASCADE];
또는
ALTER TABLE CONSTRAINT_EMP
DROP PRIMARY KEY [CASCADE];
```

- PRIMARY KEY 제약조건 삭제
  - DROP CONSTRAINT 키워드와 제약조건 이름을 기술하여 삭제 가능
  - DROP PRIMARY KEY 구문으로도 삭제 가능

[UNIQUE 유형 제약조건 삭제 예]

```sql
ALTER TABLE CONSTRAINT_EMP
DROP CONSTRAINT UENO [CASCADE]
DROP CONSTRAINT UEMAIL [CASCADE];
또는
ALTER TABLE CONSTRAINT_EMP
DROP UNIQUE (ENO) [CASCADE]
DROP UNIQUE (EMAIL) [CASCADE];
```

- UNIQUE 제약조건 삭제
  - DROP CONSTRAINT 키워드와 제약조건 이름을 기술하여 삭제 가능
  - DROP UNIQUE (컬럼이름) 구문으로도 삭제 가능

#### 제약조건 삭제 예

[CONSTRAINT_EMP 테이블 제약조건 현황]

| 이름   | 유형 | 컬럼  | 참조     | 삭제규칙 | 내용    |
| ------ | ---- | ----- | -------- | -------- | ------- |
| NENAME | C    | ENAME |          |          | \<LONG> |
| NENO   | C    | ENO   |          |          | \<LONG> |
| PKEID  | P    | EID   |          |          | \<LONG> |
| FKJID  | R    | JID   | PK_JOBID | SET NULL | \<LONG> |

[NOT NULL 제약조건 삭제 예]

```sql
ALTER TABLE CONSTRAINT_EMP
DROP CONSTRAINT NENAME
DROP CONSTRAINT NENO;
또는
ALTER TABLE CONSTRAINT_EMP
MODIFY (ENAME NULL, ENO NULL);
```

- NOT NULL 제약조건 삭제
  - DROP CONSTRAINT 키워드와 제약조건 이름을 기술하여 삭제 가능
  - MODIFY (컬럼이름 NULL) 구문으로도 삭제 가능

#### 이름 변경

[샘플 데이터 생성]

```sql
CREATE TABLE TB_EXAM
(COL1 CHAR(3) PRIMARY KEY,
ENAME VARCHAR2(20)
FOREIGN KEY (COL1) REFERENCES EMPLOYEE);
```

```sql
SELECT COLUMN_NAME
FROM USER_TAB_COLS
WHERE TABLE_NAME = 'TB_EXAM';
```

[컬럼 이름 조회]

| COLUMN_NAME |
| ----------- |
| COL1        |
| ENAME       |

```sql
SELECT 	CONSTRAINT_NAME AS 이름,
		CONSTRAINT_TYPE AS 유형,
		COLUMN_NAME AS 컬럼,
		R_CONSTRAINT_NAME AS 참조,
		DELETE_RULE AS 삭제규칙
FROM 	USER_CONSTRAINTS
JOIN 	USER_CONS_COLUMNS
USING 	(CONSTRAINT_NAME, TABLE_NAME)
WHERE 	TABLE_NAME='TB_EXAM';
```

- 제약조건 현황 조회

| 이름         | 유형 | 컬럼 | 참조     | 삭제 규칙 |
| ------------ | ---- | ---- | -------- | --------- |
| SYS_C0011409 | P    | COL1 |          |           |
| SYS_C0011410 | R    | COL1 | PK_EMPID | NO ACTION |

- 컬럼 이름 조회	

```sql
ALTER TABLE TB_EXAM
RENAME COLUMN COL1 TOEMPID;
```

| COLUMN_NAME |
| ----------- |
| EMPID       |
| ENAME       |

- 제약조건 이름 변경

```sql
ALTER TABLE TB_EXAM
RENAME CONSTRAINTS SYS_C0011409 TO PK_EID;
```

| 이름         | 유형 | 컬럼  | 참조     | 삭제규칙  |
| ------------ | ---- | ----- | -------- | --------- |
| PK_EID       | P    | EMPID |          |           |
| SYS_C0011410 | R    | EMPID | PK_EMPID | NO ACTION |

- 제약조건 이름 변경

```sql
ALTER TABLE TB_EXAM
RENAME CONSTRAINTS SYS_C0011410 TO FK_EID;
```

| 이름   | 유형 | 컬럼  | 참조     | 삭제규칙  |
| ------ | ---- | ----- | -------- | --------- |
| PK_EID | P    | EMPID |          |           |
| FK_EID | R    | EMPID | PK_EMPID | NO ACTION |

- 테이블 이름 변경

```sql
ALTER TABLE TB_EXAM RENAME TO TB_SAMPLE;
또는
RENAME TB_EXAM TO TB_SAMPLE;
```

### 5.1.6 테이블 삭제

```sql
DROP TABLE table_name [CASCADE CONSTRAINTS];
```

[구문 설명]

- 포함된 데이터 및 테이블과 관련된 데이터 딕셔러니 정보까지 모두 삭제
- 삭제 작업은 복구할 수 없음
- CASCADE CONSTRAINTS
  - 삭제 대상 테이블의 PRIMARY KEY 또는 UNIQUE 제약 조건을 참조하는 다른 제약조건을 삭제하는 OPTION
  - 참조중인 제약조건이 있는 경우 OPTION 미 사용시 삭제할 수 없음

```sql
CREATE TABLE DEPT
(	DID 	CHAR(2) PRIMARY KEY,
	DNAME 	VARCHAR2(10));
CREATE TABLE EMP^
(	EID 	CHAR(3) PRIMARY KEY,
	ENAME 	VARCHAR2(10),
	DID  	CHAR(2) REFERENCES DEPT);
```

- 샘플 데이터 생성

```sql
DROP TABLE DEPT CASCADE CINSTRAINTS;
```

```sql
DROP TABLE DEPT;
```

- ERROR  : ORA-02449 : 외래 키에 의해 참조되는 고유/기본 키가 테이블에 있습니다.
- EMP6 테이블의 DID 컬럼이 DEPT 테이블의 DID 컬럼을 참조하고 있으므로 삭제 불가

### 5.2.1 뷰 - 개요

- 다른 테이블이나 뷰에 포함된 데이터의 맞춤표현<sup>Tailored Presentation</sup>
- 'STORED QUERY' 또는 'VIRTUAL TABLE'로 간주되며 데이터베이스 객체

#### 개념

- 하나 또는 하나 이상의 테이블/뷰에 포함된 데이터 부분 집합을 나타내는 논리적인 객체 -> 선택적인 정보만 제공 가능
- 자체적으로 데이터를 포함하지 않는다.
- 베이스 테이블<sup>Base Table</sup> : 뷰를 통해 보여지는 데이터를 포함하고 있는 실제 테이블

[베이스 테이블1 (일부)]

| EMP_ID | EMP_NO         | EMP_NAME | SALARY   |
| ------ | -------------- | -------- | -------- |
| 100    | 123456-7892345 | 펭펭     | 90000000 |
| 101    | 123444-7846451 | 펭하     | 8000000  |

[뷰1 (일부)]

| EMP_ID | EMP_NAME |
| ------ | -------- |
| 100    | 펭펭     |
| 101    | 펭하     |

[베이스 테이블2 (일부)]

| EMP_ID | EMP_NAME | DEPT_ID |
| ------ | -------- | ------- |
| 100    | 펭펭     |         |
| 101    | 펭하     | 60      |

[베이스 테이블3 (일부)]

| DEPT_ID | DEPT_NAME   |
| :------ | ----------- |
| 60      | 기술지원팀  |
| 50      | 해외영업1팀 |

[뷰2 (일부)]

| EMP_NAME | DEPT_NAME   |
| -------- | ----------- |
| 펭하     | 해외영업3팀 |
| 펭펭     | 기술지원팀  |

### 5.2.2 뷰 - 사용 목적 및 장점

- Restricted data access
  - 뷰에 접근하는 사용자는 미리 정의된 결과만을 볼 수 있음 -> 데이터 접근을 제한함으로써 중요한 데이터를 보호할 수 있음
- Hide data complexity
  - 여러 테이블을 조인하는 등 복잡한 SQL 구문을 사용해야 하는 경우 자세한 SQl 구문의 내용을 숨길 수 있음
- Simplify statement for the user
  - 복잡한 SQL 구문을 모르는 사용자라도 SELECT 구문만으로 원하는  결과를 조회할 수 있음
- Present the data in a different perspective
  - 뷰에 포함되는 컬럼은 참조 대상 테이블에 영향을 주지 않고 다른 이름으로 참조 가능
- Isolate applications from changes in definitions of base tables
  - 베이스 테이블에 포함된 여러 개 컬럼 중 일부만 사용하도록 뷰를 생성한 경우, 뷰가 참조하지 않는 나머지 컬럼이 변경되어도 뷰를 사용하는 다른 프로그램들은 영향을 받지 않음
- Sava complex queries
  - 복잡한 SQL 문을 뷰 형태로 저장하여 반복적으로 사용 가능

### 5.2.3 뷰 - 생성 구문

```sql
CREATE [OR REPLACE] [FORCE | NOFORCE] VIEW view_name [(alias [, alias ...])]
AS Subquery
[WITH CHECK OPTION [ CONSTRAINT constraint_name]]
[WITH READ ONLY [ CONSTRAINT constraint_name]];
```

[구문 설명]

- CREATE OR REPLACE
  - 지정한 이름의 뷰가 없으면 새로 생성, 동일 이름이 존재하면 수정<sup>Overwrite</sup>
- FORCE | NOFORCE
  - NOFORCE : 베이스 테이블이 존재하는 경우에만 뷰 생성 가능
  - FORCE : 베이스 테이블이 존재하지 않아도 뷰 생성 가능
- ALIAS
  - 뷰에서 사용할 표현식 이름(테이블 컬럼 이름 의미)
  - 생략 : SUBQUERY에서 사용한 이름 작용
  - ALIAS 개수 : SUBQUERY에서 사용한 SELECT LIST 개수와 일치
- SUBQUERY
  - 뷰에서 표현하는 데이터를 생성하는 SELECT 구문
- 제약 조건
  - WITH CHECK OPTION : 뷰를 통해 접근 가능한 데이터에 대해서만 DML 작업 허용
  - WITH READ ONLY : 뷰를 통해 DML 작업 허용 안 함
  - 제약조건으로 간주되므로 별도 이름 지정 가능 

#### 생성 예

- 뷰를 생성할 때 사용하는 서브쿼리는 일반적인 SELECT 구문을 사용
- 생성된 뷰는 테이블처럼 취급됨

```sql
CREATE OR REPLACE VIEW V_EMP
AS 	SELECT 	EMP_NAME, DEPT_ID
	FROM	EMPLOYEE
	WHERE	DEPT_ID = '90';
```

```sql
SELECT	*
FROM	V_EMP;
```

| EMP_NAME | DEPT_ID |
| -------- | ------- |
| 한선기   | 90      |
| 강중훈   | 90      |
| 최만식   | 90      |

```sql
SELECT	COLUMN_NAME, DATA_TYPE, NULLABLE
FROM	USER_TAB_COLS
WHERE	TABLE_NAME = 'V_EMP';
```

| COLUMN_NAME | DATA_TYPE | NULLABLE |
| ----------- | --------- | -------- |
| EMP_NAME    | VARCHAR2  | N        |
| DEPT_ID     | CHAR      | Y        |

```sql
CREATE	OR	REPLACE VIEW V_EMP_DEPT_JOB
AS 		SELECT  EMP_NAME,
				DEPT_NAME,
				JOB_TITLE
		FROM	EMPLOYEE
		LEFT	JOIN DEPARTMENT USING (DEPT_ID)
		LEFT	JOIN JOB USING (JOB_ID)
		WHERE		 JOB_TITLE = '사원';
```

```sql
SELECT	*
FROM	V_EMP_DEPT_JOB;
```

| EMP_NAME | DEPT_NAME   | JOB_TITLE |
| -------- | ----------- | --------- |
| 펭펭     | 해외영업1팀 | 사원      |
| 펭하     |             | 사원      |
| 펭수     | 해외영업1팀 | 사원      |
| 펭빠     | 본사 인사팀 | 사원      |

```sql
SELECT	COLUMN_NAME, DATA_TYPE,	NULLABLE
FROM	USER_TAB_COLS
WHERE	TABLE_NAME = 'V_EMP_DEPT_JOB';
```

| COLUMN_NAME | DATA_TYPE | NULLABLE |
| ----------- | --------- | -------- |
| EMP_NAME    | VARCHAR2  | N        |
| DEPT_NAME   | VARCHAR2  | Y        |
| JOB_TITLE   | VARCHAR2  | Y        |

#### 생성 예 : ALIAS 사용

- 뷰 정의 부분에서 지정 가능
- 서브쿼리 부분에서 지정 가능

```sql
CREATE OR REPLACE VIEW V_EMP_DEPT_JOB (ENM, DNM, TITLE)
AS 	SELECT EMP_NAME, DEPT_NAME, JOB_TITLE
	FROM EMPLOYEELEFT 
	JOIN DEPARTMENT USING (DEPT_ID)
	LEFT JOIN JOB USING (JOB_ID)
	WHERE JOB_TITLE = '사원';
```

```sql
CREATE OR 	REPLACE VIEW V_EMP_DEPT_JOB
AS SELECT 	EMP_NAME AS ENM,
		  	DEPT_NAME AS DNM,
			JOB_TITLE AS TITLE
	FROM EMPLOYEE
    LEFT JOIN DEPARTMENT USING (DEPT_ID)
    LEFT JOIN JOB USING (JOB_ID)
    WHERE JOB_TITLE = '사원';
```

| COLUMN_NAME | DATA_TYPE | NULLABLE |
| ----------- | --------- | -------- |
| ENM         | VARCHAR2  | N        |
| DNM         | VARCHAR2  | Y        |
| TITLE       | VARCHAR2  | Y        |

- 뷰 컬럼이 함수나 표현식에서 파생되는 경우 반드시 사용해야 함

```sql
CREATE OR REPLACE VIEW V_EMP ("Enm", "Gender", "Years")AS
SELECT 	EMP_NAME, 
		DECODE(SUBSTR(EMP_NO, 8,1),'1','남자','3','남자','여자'),				    
		ROUND(MONTHS_BETWEEN(SYSDATE, HIRE_DATE)/12, 0)
FROM EMPLOYEE;
```

- 서브쿼리 부분에서 ALIAS 지정해도 됨

| COLUMN_NAME | DATA_TYPE | NULLABLE |
| ----------- | --------- | -------- |
| Enm         | VARCHAR2  | N        |
| Gender      | VARCHAR2  | Y        |
| Years       | NUMBER    | Y        |

```sql
CREATE OR REPLACE VIEW V_EMP AS
SELECT EMP_NAME , 
		DECODE(SUBSTR(EMP_NO, 8,1),'1','남자','3','남자','여자'),
		ROUND(MONTHS_BETWEEN(SYSDATE, HIRE_DATE)/12, 0)
FROM EMPLOYEE;
```

- ERROR : ORA-00998 : 이 식은 열의 별명과 함께 지정해야 합니다.

---

- 특정 컬럼에만 선택적으로 ALIAS를 지정하는 것은 서브쿼리 부분에서만 가능

```sql
CREATE OR REPLACE VIEW V_EMP AS
SELECT 	EMP_NAME, 
		DECODE(SUBSTR(EMP_NO, 8,1),'1','남자','3','남자','여자')AS "Gender",
		ROUND(MONTHS_BETWEEN(SYSDATE, HIRE_DATE)/12,0) AS "Years"
FROM EMPLOYEE;
```

| COLUMN_NAME | DATA_TYPE | NULLABLE |
| ----------- | --------- | -------- |
| EMP_NAME    | VARCHAR2  | N        |
| Gender      | VARCHAR2  | Y        |
| Years       | NUMBER    | Y        |

```sql
CREATE OR REPLACE VIEW V_EMP ("Gender", "Years") AS
SELECT 	EMP_NAME ,
		DECODE(SUBSTR(EMP_NO, 8,1),'1','남자','3','남자','여자'),
		ROUND(MONTHS_BETWEEN(SYSDATE, HIRE_DATE)/12, 0)
FROM EMPLOYEE;
```

- ERROR : ORA-01730 : 지정한 열명의 수가 부적합합니다.
- 서브쿼리의 EMP_NAME 컬럼은 ALIAS 없이 그대로 사용하려는 의미로 생략
  - 뷰 생성부분에서는 전체 컬럼에 대해 지정해야 함

#### 생성 예 : 제약 조건

- 뷰의 원래 목적은 아니지만 뷰를 통한 DML 작업은 가능함
- DML 작업 결과는 베이스 테이블의 데이터에 적용 -> COMMIT / ROLLBACK 작업 필요
- 뷰를 통한 DML 작업은 여러 가지 제한이 있음
- 뷰 생성 시 DML 작업에 대한 제한을 설정할 수 있음
  - WITH READ ONLY : 뷰를 통한 DML 작업 불가
  - WITH CHECK OPTION : 뷰를 통해 접근 가능한 데이터에 대해서만 DML 작업 수행 가능
- WITH READ ONLY

```SQL
CREATE OR REPLACE VIEW V_EMP
AS SELECT *
	FROM EMPLOYEE
WITH READ ONLY;
```

- WITH READ ONLY를 사용한 샘플 뷰

- DML 작업에 따라 에러 유형은 다르지만 DML 작업을 허용하지 않는다.

```sql
UPDATE V_EMP
SET PHONE = NULL;
```

```sql
INSERT INTO V_EMP (EMP_ID, EMP_NAME, EMP_NO)
VALUES ('666','펭펭','666666-6666666');
```

- ERROR : ORA-01799 : 가상 열은 사용할 수 없습니다.	

```sql
DELETE FROM V_EMP;
```

- ERROR : ORA-01752 : 뷰로 부터 정확하게 하나의 키 - 보전된 테이블 없이 삭제할 수 없습니다.

---

- WITH CHECK OPTION - 조건에 따라 INSERT / UPDATE 작업 제한(DELETE는 제한 없음)

```sql
CREATE OR REPLACE VIEW V_EMP
AS 	SELECT EMP_ID, EMP_NAME, EMP_NO, MARRIAGE
	FROM EMPLOYEE
	WHERE MARRIAGE = 'N'
WITH CHECK OPTION;
```

- WITH CHECK OPTION을 사용한 샘플 뷰

| EMP_ID | EMP_NAME | EMP_NO         | MARRIAGE |
| ------ | -------- | -------------- | -------- |
| 124    | 펭펭     | 641231-2269080 | N        |
| 149    | 펭하     | 640524-2148639 | N        |
| 205    | 펭빠     | 790833-2105839 | N        |

```sql
INSERT INTO V_EMP (EMP_ID, EMP_NAME, EMP_NO, MARRIAGE)
VALUES ('666','펭펭','666666-6666666', 'Y');
```

```sql
UPDATE V_EMP
SET MARRIAGE = 'Y';
```

- ERROR : ORA-01405 : 뷰의 WITH CHECK OPTION 의 조건에 위배 됩니다.

```sql
UPDATE V_EMP
SET EMP_ID = '000'
WHERE EMP_ID = '124';
```

| EMP_ID | EMP_NAME | EMP_NO         | MARRIAGE |
| ------ | -------- | -------------- | -------- |
| 124    | 펭펭     | 641231-2269080 | N        |
| 149    | 펭하     | 640524-2148639 | N        |
| 205    | 펭빠     | 790833-2105839 | N        |

- 뷰를 생성할 때 사용한 WHERE 조건에 적용되지 않는 범위에서는 허용됨

### 5.2.4 뷰 - 내용 확인

- 뷰 생성 시 사용한 서브쿼리 자체가 데이터 딕셔너리에 저장됨

```sql
CREATE OR REPLACE VIEW V_EMP
AS 	SELECT EMP_ID, EMP_NAME, EMP_NO, MARRIAGE
	FROM EMPLOYEE
	WHERE MARRIAGE = 'N'
	WITH CHECK OPTION;
```

```sql
SELECT 	VIEW_NAME, TEXT
FROM 	USER_VIEWS
WHERE 	VIEW_NAME = 'V_EMP';
```

- USER_VIEW
  - 뷰 정보를 관리하는 데이터 딕셔너리

| VIEW_NAME | TEXT    |
| --------- | ------- |
| V_EMP     | \<Long> |

### 5.2.5 뷰 - 데이터 조회 절차

1. 뷰를 사용한 SQL 구문 해석
2. 데이터 딕셔너리 "USER_VIEWS"에서 뷰 정의 검색
3. SQL 구문을 실행한 계정이 관련된 베이스 테이블에 접근하여 SELECT 할 수 있는 권한이 있는지 확인
4. 뷰 대신 베이스 테이블을 기반으로 하는 동등한 작업으로 변환
5. 베이스 테이블을 대상으로 데이터 조회

### 5.2.6 뷰 - 사용

```SQL
CREATE OR REPLACE VIEW V_EMP_INFO
AS 	SELECT EMP_NAME, DEPT_NAME, JOB_TITLE
	FROM EMPLOYEE
	LEFT JOIN DEPARTMENT USING (DEPT_ID)
	LEFT JOIN JOB USING (JOB_ID);
```

- V_EMP_INFO 데이터(일부)

| EMP_NAME | DEPT_NAME   | JOB_TITLE |
| -------- | ----------- | --------- |
| 펭펭     | 해외영업1팀 | 사원      |
| 펭하     | 기술지원팀  | 부사장    |

```sql
SELECT EMP_NAME
FROM V_EMP_INFO
WHERE DEPT_NAME = '해외영업1팀'
AND JOB_TITLE = '사원';
```

| EMP_NAME |
| -------- |
| 펭펭     |

```sql
CREATE OR REPLACE VIEW V_DEPT_SAL ("Did", "Dnm", "Davg")
AS	SELECT 	NVL(DEPT_ID,'N/A'),
			NVL(DEPT_NAME,'N/A'),
			ROUND(AVG(SALARY),-3)
FROM DEPARTMENT
RIGHT JOIN EMPLOYEE USING (DEPT_ID)
GROUP BY DEPT_ID, DEPT_NAME;
```

- V_DEPT_SAL 데이터

| Did  | Dnm         | Davg    |
| ---- | ----------- | ------- |
| N/A  | N/A         | 1900000 |
| 60   | 기술지원팀  | 3300000 |
| 90   | 해외영업3팀 | 6033000 |

- \" "를 사용하여 alias를 지정한 경우에는 \" "까지 기술해야 함

```sql
SELECT "Dnm", "Davg"
FROM V_DEPT_SAL
WHERE "Davg" > 3000000;
```

| Dnm         | Davg    |
| ----------- | ------- |
| 기술지원팀  | 3300000 |
| 해외영업3팀 | 6033000 |

```sql
SELECT Dnm, Davg
FROM V_DEPT_SAL
WHERE Davg > 3000000;
```

- ERROR : ORA-00904 : 'DAVG' : 부적합한 식별자

### 5.2.7 뷰 - 수정

- 뷰 수정 의미 -> 별도 구문 없음
  - 뷰를 삭제하고 새로 생성
  - 기존 내용을 덮어써서 수정

```sql
CREATE OR REPLACE	VIEW V_EMP
AS 	SELECT EMP_NAME, JOB_ID
	FROM EMPLOYEE
	WHERE SALARY > 3000000;
```

```sql
CREATE OR REPLACEVIEW V_EMP
AS 	SELECT EMP_NAME, JOB_ID
	FROM EMPLOYEE
	WHERE SALARY > 4000000;
```

- CREATE IR REPLACE 구문을 사용했으므로 기존에 존재하는 V_EMO 이름을 그대로 사용하고, 내용만 수정되었음

```SQL
CREATE VIEW V_EMP
AS SELECT EMP_NAME, JOB_ID
FROM EMPLOYEE
WHERE SALARY > 4000000;
```

- CREATE구문을 사용했으므로 이미 사용중인 V_EMP 이름이 중복되어 에러 발생함
- ERROR - ORA-00955 : 기존의 객체가 이름을 사용하고 있습니다.

### 5.2.8 뷰 - 삭제

- 데이터 딕셔너리에 저장된 서브쿼리를 삭제하는 의미

```sql
DROP VIEW view_name;
```

### 5.2.9 뷰 - 인라인 뷰<sup>Inline View</sup> 개념

- 별칭을 사용하는 서브쿼리 -> 일반적으로 FROM 절에서 사용

```sql
CREATE OR REPLACE VIEW V_DEPT_SALAVG("Did", "Davg")
AS SELECT 	NVL(DEPT_ID, 'N/A'),
			ROUND(AVG(SALARY),-3)
FROM 		EMPLOYEE
GROUP BY 	DEPT_ID;
SELECT		 EMP_NAME, SALARY
FROM		EMPLOYEE
JOIN		V_DEPT_SALAVGON ( NVL(DEPT_ID, 'N/A') = "Did" )
WHERE		SALARY > "Davg"
ORDER BY 2 DESC;
```

```sql
SELECT EMP_NAME, SALARY
FROM 	(SELECT NVL(DEPT_ID,'N/A') AS "Did",
				ROUND(AVG(SALARY),-3) AS "Davg"
		  FROM EMPLOYEE
		  GROUP BY DEPT_ID) INLV
JOIN 	EMPLOYEE ON ( NVL(DEPT_ID, 'N/A') = INLV."Did" )
WHERE 	SALARY > INLV."Davg"
ORDER BY 2 DESC;
```

| Did  | Davg    |
| ---- | ------- |
| N/A  | 1900000 |
| 50   | 2300000 |
| 20   | 2500000 |
| 90   | 6033000 |

| EMP_NAME | SALARY  |
| -------- | ------- |
| 펭펭     | 9000000 |
| 펭하     | 3500000 |
| 펭빠     | 3410000 |

- FROM 절이 수행되면서 별칭 INLV로 지정된 뷰가 생성되고 사용됨
- 별도로 생성하는 경우와 동일한 효과

#### 뷰 - 인라인 뷰 활용 : Top N 분석 개념

- Top N 분석 : 조건에 맞는 최상위(또는 최하위) 레코드 n개를 식별해야 하는 경우에 사용
  - 최상위 소득자 3명
  - 최근 6개월 동안 가장 많이 팔린 제품 3가지
  - 실적이 가장 좋은 영업 사원 5명
- 오라클 환경에서 Top N 분석 원리
  - 원하는 순서대로 정렬
  - ROWNUM 이라는 가상 컬럼을 이용하여 정렬 순서대로 순번 부여
  - 부여된 순번을 이용하여 필요한 수 만큼 식별

- ROWNUM 개념
  - SQL 구문 수행 후, Result Set 각 행에 1부터 시작하는 일련의 숫자를 자동으로 할당한 가상 컬럼

```sql
SELECT ROWNUM, EMP_NAME, SALARY
FROM (	SELECT NVL(DEPT_ID,'N/A') AS "Did",
		ROUND(AVG(SALARY),-3) AS "Davg"
		FROM EMPLOYEE
		GROUP BY DEPT_ID) INLV
JOIN EMPLOYEE ON ( NVL(DEPT_ID, 'N/A') = INLV."Did")
WHERE SALARY > INLV."Davg";
```

| ROWNUM | EMP_NAME | SALARY  |
| ------ | -------- | ------- |
| 1      | 펭펭     | 9000000 |
| 2      | 펭하     | 3500000 |
| 3      | 펭빠     | 3410000 |

- ROWNUM 특징
  - WHERE 절이 실행되면서 순차적으로 할당됨
  - 할당된 후에는 변경되지 않음

```sql
SELECT ROWNUM, EMP_NAME, SALARY 
FROM (SELECT NVL(DEPT_ID,'N/A') AS "Did", 
      		 ROUND(AVG(SALARY),-3) AS "Davg"
      FROM EMPLOYEE 
      GROUP BY DEPT_ID) INLV 
JOIN EMPLOYEE ON ( NVL(DEPT_ID, 'N/A') = INLV."Did")
WHERE SALARY > INLV."Davg"
ORDER BY 3 DESC;
```

| ROWNUM | EMP_NAME | SALARY  |
| ------ | -------- | ------- |
| 1      | 펭펭     | 9000000 |
| 2      | 펭하     | 3500000 |
| 3      | 펭빠     | 3410000 |

- WHERE 절이 수행되면서 조건을 만족시키는 행에 ROWNUM을 할당한 결과로 1차 Result set을 생성
- 1차 Result set에 대해 정렬을 수행하므로 정렬 순서대로 ROWNUM이 할당될 수 없음

#### 뷰 - 인라인 뷰 활용 : Top N 분석

- ROWNUM 사용
  - ROWNUM 값으로 특정 행을 선택할 수 없음
  - 단, Result set의 1<sup>st</sup> 행(ROWNUM = 1)은 선택 가능

```sql
SELECT ROWNUM, EMP_NAME, SALARY
FROM (SELECT 	NVL(DEPT_ID,'N/A') AS "Did",
				ROUND(AVG(SALARY),-3) AS "Davg"
	  FROM EMPLOYEE
	  GROUP BY DEPT_ID) INLV
JOIN EMPLOYEE ON ( NVL(DEPT_ID, 'N/A') = INLV."Did")
WHERE SALARY > INLV."Davg"
AND ROWNUM = 3;
```

| ROWNUM | EMP_NAME | SALARY |
| ------ | -------- | ------ |
|        |          |        |

- WHERE 절이 모두 수행되어야 ROWNUM이 할당됨
- 특정 ROWNUM 값이 할당되기 이전이므로 실행 되지만 원하는 결과를 만들 수 없음

```sql
SELECT ROWNUM, EMP_NAME, SALARY 
FROM (SELECT NVL(DEPT_ID,'N/A') AS "Did", 
      		 ROUND(AVG(SALARY),-3) AS "Davg"
      FROM EMPLOYEE GROUP BY DEPT_ID) INLV 
JOIN EMPLOYEE ON ( NVL(DEPT_ID, 'N/A') = INLV."Did")
WHERE SALARY > INLV."Davg"
AND ROWNUM = 1;
```

| ROWNUM | EMP_NAME | SALARY  |
| ------ | -------- | ------- |
| 1      | 펭펭     | 9000000 |

- ROWNUM 사용
  - ROWNUM 값을 이용하여 일정 범위에 해당하는 행만 선택할 수 있음
  - N 순위보다 같거나 작은 범위만 식별 가능 : 예) 상위 5건 -> [ROWNUM <=5]

```sql
SELECT ROWNUM, EMP_NAME, SALARY 
FROM (SELECT 	NVL(DEPT_ID,'N/A') AS "Did", 
      			ROUND(AVG(SALARY),-3) AS "Davg"
      FROM EMPLOYEE 
      GROUP BY DEPT_ID) INLV 
JOIN EMPLOYEE ON ( NVL(DEPT_ID, 'N/A') = INLV."Did")
WHERE SALARY > INLV."Davg"
AND ROWNUM <= 5;
```

| ROWNUM | EMP_NAME | SALARY  |
| ------ | -------- | ------- |
| 1      | 펭펭     | 9000000 |
| 2      | 펭하     | 3500000 |
| 3      | 펭빠     | 3800000 |

- 지정한 범위에 포함되는 행 선택 가능
- 원하는 순서대로 정렬된 결과는 아님

[ 원하는 순서대로 정렬된 결과 ]

| ROWNUM | EMP_NAME | SALARY  |
| ------ | -------- | ------- |
| 1      | 펭펭     | 9000000 |
| 3      | 펭빠     | 3800000 |
| 2      | 펭하     | 3500000 |

 90P

