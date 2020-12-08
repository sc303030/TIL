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

69p