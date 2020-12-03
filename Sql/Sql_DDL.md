# Sql_DDL

### 5.1.1 테이블 생성

[기본 구문]

```sql
CREATE TABLEtable_name
(column_namedatatype[DEFAULTexpr] [ column_constraint ] [, ... ]
[, table_constraint , …] ) ;
```

```sql
column_constraint
[CONSTRAINTconstraint_name ]constraint_type
table_constraint
[CONSTRAINTconstraint_name ]constraint_type (column_name , …)
```

[구문 설명]

- **table_name,column_name**
  - 테이블이름지정, 컬럼이름지정
- **datatype**
  - 컬럼의데이터타입, 크기지정
- **DEFAULTexpr**
  - 해당 컬럼에 적용될 자동 기본 값
- **CONSTRAINTS**
  - COLUMN_CONSTRAINT : 컬럼 레벨에서의 제약 조건
  - TABLE_CONSTRAINT : 테이블 레벨에서의 제약 조건

#### Naming Rule

- 테이블 및 컬럼 이름
  - 문자로 시작, 30자 이하
  - 영문 대/소문자(A~Z,a~z), 숫자(0~9), 특수문자(_,$,#), 한글만 포함가능

- 중복되는 이름은 사용할 수 없음
- 예약 키워드(CREATE, TABLE, COLUMN 등)는 사용할 수 없음

##### 테이블 생성 예

```sql
CREATE TABLE TEST
(ID NUMBER(5),
NAME CHAR(10),
ADDRESS VARCHAR2(50)
);
```

- 테이블이름: TEST
- 컬럼정의: 이름-ID, 타입-NUMBER
- 컬럼정의: 이름-NAME, 타입-CHAR(10)
- 컬럼정의: 이름-ADDRESS, 타입-VARCHAR2(50)

##### 생성 오류 예

```sql
CREATE TABLE THISISCREATETABLESAMPLEOVER30CHARACTERS
(COL1 CHAR(10));
```

- ORA-00972 : 식별자의 길이가 너무 깁니다

```sql
CREATE TABLE SAMPLETABLE(THISISSAMPLECOLUMNOVER30CHARACTERCHAR(10));
```

- ORA-00972 : 식별자의 길이가 너무 깁니다

```sql
CREATE TABLE SAMPLETABLE(COL-1CHAR(5));
```

- ORA-00902 : 데이터유형이 부적합합니다

```sql
CREATE TABLE TEST
(COL1 CHAR(10));
```

- ORA-00955 : 이미 사용된 객체명입니다

##### 실습

테이블이름: ORDERS

|  컬럼이름  | ORDERNO  |  CUSTNO  | ORDERDATE | SHIPDATE | SHIPADDRESS  | QUANTITY |
| :--------: | :------: | :------: | :-------: | :------: | :----------: | :------: |
|    설명    | 주문번호 | 고객번호 | 주문일자  | 배송일자 |   배송주소   | 주문수량 |
| 데이터타입 | CHAR(4)  | CHAR(4)  |   DATE    |   DATE   | VARCHAR2(40) |  NUMBER  |
|  DEFAULT   |    -     |    -     |  SYSDATE  |    -     |      -       |    -     |

[생성 구문]

```sql
CREATE TABLE ORDERS
(ORDERNO CHAR(4),
CUSTNO CHAR(4),
ORDERDATE DATE DEFAULT SYSDATE,
SHIPDATE DATE,
SHIPADDRESS VARCHAR2(40),
QUANTITY NUMBER);
```

### 5.1.2제약조건 Constraints

- 데이터 무결성
  - 데이터 베이스에 저장되어 있는 데이터가 손상되거나 원래의 의미를 잃지 않고 유지하는 상태

- 데이터 무결성 제약조건
  - 데이터 무결성을 보장하기 위해 오라클에서 지원하는 방법
    - 예)유효하지 않은 데이터 입력 방지, 유효한 범위에서만 데이터 변경/삭제 작업 허용

|              제약조건               | 설명                                                         |   설정레벨   |
| :---------------------------------: | ------------------------------------------------------------ | :----------: |
|              NOT NULL               | 해당 컬럼에 NULL을 포함되지 않도록 함                        |     컬럼     |
|               UNIQUE                | 해당 컬럼 또는 컬럼 조합 값이 유일하도록 함                  | 컬럼, 테이블 |
|             PRIMARY KEY             | 각 행을 유일하게 식별할 수 있도록 함                         | 컬럼, 테이블 |
| REFERENCES<br/>TABLE (column_name ) | 해당 컬럼이 참조하고 있는 테이블 <sup>부모테이블</sup>의 특정컬럼 값들과 일치하거나 또는 NULL이 되도록 보장함 | 컬럼, 테이블 |
|                CHECK                | 해당 컬럼에 특정 조건을 항상 만족시키도록 함                 | 컬럼, 테이블 |

- 이름으로관리
  - 문자로 시작,길이는 30자까지 가능
  - 이름을 따로 지정하지 않으면 자동 생성 (SYS_Cxxxxxxx 형식)

- 생성 시기
  - 테이블 생성과 동시
  - 테이블을 생성한 후
- 컬럼 레벨 또는 테이블 레벨에서 정의할 수 있다.
  - NOT NULL은 '컬럼레벨' 에서만 가능
  - 컬럼 여러 개를 조합하는 경우에는 '테이블 레벨 '에서만 가능

##### NOT NULL 사용 예

```sql
CREATE TABLE TABLE_NOTNULL
(ID CHAR(3) NOT NULL,
SNAME VARCHAR2(20));
```

| NAME  | Type         | Nullabel |
| ----- | ------------ | -------- |
| ID    | CHAR(3)      |          |
| SNAME | VARCHAR2(20) | Y        |

```sql
INSERT INTO TABLE_NOTNULL
VALUES ('100','ORACLE');
```

| ID   | SNAME  |
| ---- | ------ |
| 100  | ORACLE |

```sql
INSERT INTO TABLE_NOTNULL
VALUES (NULL,'ORACLE');
```

- ORA-01400: NULL을 ("VCC"."TABLE_NOTNULL"."ID") 안에 삽입할 수 없습니다.

- 'ID' 컬럼에 NULL을 입력하려고 했기 때문에 발생

- 에러 메시지 표시 형식
  - "VCC"."TABLE_NOTNULL"."ID"- > 계정.테이블.컬럼

```sql
CREATE TABLE TABLE_NOTNULL2
(ID CHAR(3),
SNAME VARCHAR2(20),
CONSTRAINT TN2_ID_NNNOT NULL (ID));
```

- 'NOT NULL' 제약조건은 컬럼 레벨에서만 정의 가능

- ORA-00904 : 부적합한 식별자

#### UNIQUE : 단일 컬럼 생성 예

```sql
CREATE TABLE TABLE_UNIQUE
(ID CHAR(3) UNIQUE,
SNAME VARCHAR2(20));
```

```sql
INSERT INTO TABLE_UNIQUE
VALUES ('100','ORACLE');
```

| ID   | SNAME  |
| ---- | ------ |
| 100  | ORACLE |

```sql
INSERT INTO TABLE_UNIQUE
VALUES ('100','ORACLE');
```

- ORA-00001 : 무결정 제약 조건(VCC.SYS_C0011181)에 위배됩니다.

- 'ID' 컬럼에 중복 값을 입력하려고 했기 때문에 발생
- 제약조건 이름을 지정하지 않았으므로 임의 이름 "SYS_C0011181"이 할당됨

#### UNIQUE : 조합 컬럼 생성 예

```sql
CREATE TABLE TABLE_UNIQUE2
(ID CHAR(3),
SNAME VARCHAR2(20),
SCODE CHAR(2),
CONSTRAINTTN2_ID_UNUNIQUE (ID,SNAME));
```

- 컬럼 조합 결과를 유일하게 하려는 목적이므로 테이블 레벨에서 생성해야 함

```sql
INSERT INTO TABLE_UNIQUE2
VALUES ('100', 'ORACLE', '01');
```

| ID   | SNAME  | SCODE |
| ---- | ------ | ----- |
| 100  | ORACLE | 01    |

```sql
INSERT INTO TABLE_UNIQUE2
VALUES ('200', 'ORACLE', '01');
```

| ID   | SNAME  | SCODE |
| ---- | ------ | ----- |
| 100  | ORACLE | 01    |
| 200  | ORACLE | 02    |

```sql
INSERT INTO TABLE_UNIQUE2
VALUES ('200','ORACLE','02');
```

- ORA-00001 : 무결성 제약 조건(VCC.TN2_10_UN)에 위배됩니다.

##### UNIQUE:생성예

```sql
CREATE TABLE TABLE_UNIQUE3
(ID CHAR(3) UNIQUE,
SNAME VARCHAR2(20) UNIQUE,
SCODE	CHAR(2));
```

```sql
INSERT INTO TABLE_UNIQUE3
VALUES ('100','ORACLE','01');
```

| ID   | SNAME  | SCODE |
| ---- | ------ | ----- |
| 100  | ORACLE | 01    |

```sql
INSERT INTO TABLE_UNIQUE3
VALUES ('200','ORACLE','01');
```

- ORA-00001 "" 무결성 제약 조건(VV.SYS_C0011184)에 위배됩니다.
- 'ID' 컬럼과 'SNAME' 컬럼에 각각 설정되었기 때문에, 중복된 'SNAME' 컬럼 값이 입력 될 수 없음
  - 두 컬럼의 조합 결과를 유일하게 하려면 '테이블 레벨' 에서 생성

```sql
CREATE TABLE TABLE_UNIQUE4
(ID CHAR(3) CONSTRAINT TN4_ID_UNUNIQUE ,
SNAME VARCHAR2(20) CONSTRAINT TN4_ID_UNUNIQUE ,
SCODE CHAR(2));
```

- ORA-02264 : 기존의 제약에 사용된 이름입니다.

- 제약 조건 이름을 동일하게 해서 'ID' 컬럼과 'SNAME' 컬럼 조합 결과를 유일하게 하려고 했음
  - 두 컬럼의 조합 결과를 유일하게 하려면 '테이블 레벨' 에서 생성

- UNIQUE 제약 조건이 생성된 컬럼에는 NULL 포함 가능

**[단일 컬럼 경우 예]**

```sql
CREATE TABLE TABLE_UNIQUE5
(ID NUMBER UNIQUE,
NAME VARCHAR2(10));
```

- 'ID' 컬럼에 NULL 입력 가능

| ID   | NAME   |
| ---- | ------ |
|      | ORACLE |
|      | SQL    |
|      | JAVA   |

**[컬럼 조합 경우 예]**

```sql
CREATE TABLE TABLE_UNIQUE6
(ID NUMBER,
NAME VARCHAR2(10),
UNIQUE (ID, NAME));
```

- 'ID, NAME' 컬럼 조합으로 NULL 가능

| ID   | NAME   |
| ---- | ------ |
|      | ORACLE |
| 100  | SQL    |
|      |        |
| 200  | JAVA   |
|      | ORACLE |

- ORA-00001 : 무결정 제약 조건(VSS.SYS_C0011187)에 위배됩니다.

- 'ID, NAME' 컬럼 조합 중복

#### PRIMARY KEY

- UNIQUE + NOT NULL 의미
- 테이블 당 1개만 생성 가능

```sql
CREATE TABLE TABLE_PK
(ID CHAR(3) PRIMARY KEY,
 SNAME VARCHAR2(20));
```

```sql
INSERT INTO TABLE_PK
VALUES ('100','ORACLE');
```

| ID   | SNAME  |
| ---- | ------ |
| 100  | ORACLE |

```sql
INSERT INTO TABLE_PK
VALUES ('100','IBM');
```

- ORA-00001 : 무결정 제약 조건(VCC.SYS_C004108)에 위배됩니다.

```sql
INSERT INTO TABLE_PK
VALUES (NULL, 'SUN');
```

- ORA-01400 : NULL을 ("VCC"."TABLE_PK"."ID")안에 삽입할 수 없습니다.

##### PRIMARY KEY : 조합 컬럼 생성 예

```sql
CREATE TABLE TABLE_PK2
(ID CHAR(3),
SNAME VARCHAR2(20),
SCODE CHAR(2),
CONSTRAINT TP2_PKPRIMARY KEY (ID,SNAME));
```

| ID   | SNAME  | SCODE |
| ---- | ------ | ----- |
| 100  | ORACLE | 01    |
| 200  | ORACLE | 01    |

```sql
INSERT INTO TABLE_PK2
VALUES ('100','ORACLE','02');
```

- 'ID, SAME' 컬럼 조합 결과가 중복

- ORA-00001 : 무결정 제약 조건(VCC.TP2_PK)에 위배됩니다.

```sql
INSERT INTO TABLE_PK2
VALUES (NULL,'ORACLE','01');
```

- 조합되는 개별 컬럼에 NULL은 허용되지 않음

- ORA-01400 : NULL을("VCC"."TABLE_PK2"."ID")안에 삽입할 수 없습니다.

```sql
CREATE TABLE TABLE_PK3
(ID CHAR(3) PRIMARY KEY,
SNAME VARCHAR2(20) PRIMARY KEY,
SCODE CHAR(2));
```

- 'PRIMARY KEY' 키워드는 한번만 사용 가능

- ORA-00260 : 테이블에는 기본 키를 1개만 포함시킬 수 있습니다.

```sql
CREATE TABLE TABLE_PK3
(ID CHAR(3) CONSTRAINT PK1PRIMARY KEY,
SNAME VARCHAR2(20) CONSTRAINT PK1PRIMARY KEY,
SCODE CHAR(2));
```

- 동일한 제약조건 이름을 지정 -> 컬럼 조합 결과를 대상으로 하는 제약조건 생성 의미가 아님

- ORA-02260 : 테이블에는 기본 키를 1개만 포함시킬 수 있습니다.

#### FOREIGN KEY

- 참조 테이블의 컬럼 값과 일치하거나 NULL 상태를 유지하도록 한다.

[EMPLOYEE]

| EMP_ID | EMP_NAME | DEPT_ID |
| ------ | -------- | :------ |
| 100    | 펭펭     | 90      |
| 101    | 펭하     | 60      |
| 102    | 펭빠     | 50      |
| 179    | 펭쑤     |         |

[DEPARTMENT]

| DEPT_ID | DEPT_NAME   |
| ------- | ----------- |
| 90      | 해외영업3팀 |
| 60      | 기술지원팀  |
| 50      | 해외영업1팀 |

- DEPT_ID 컬럼 -> FOREIGN KEY 컬럼
- DEPARTMENT 테이블의 DEPT_ID 컬럼에 존재하지 않는 값이 포함되면 데이터 무결성에 문제가 있음

#### FOREIGN KEY : 컬럼 레벨에서 생성

```sql
CREATE TABLE TABLE_FK
(ID CHAR(3),
SNAME VARCHAR2(20),
LID CHAR(2) REFERENCES LOCATION ( LOCATION_ID ) );
```

- 참조 테이블만 기술하고 참조컬럼을 생략하면 해당 테이블의 PRIMARY KEY 컬럼을 참조하게 됨

- LOCATION : 참조 테이블
- LOCATION_ID : 참조 컬럼

```sql
INSERT INTO TABLE_FK
VALUES ('200','ORACLE','C1');
```

- ORA-02291 : 무결성 제약조건 (VCC.SYS_C0011189)이 위배되었습니다-부모 키가 없습니다.

[LOCATION 테이블]

| LOCATION_ID | COUNTRY_ID | LOC_DESCRIBE |
| ----------- | ---------- | ------------ |
| A1          | KO         | 아시아지역1  |
| U1          | US         | 미주지역     |
| OT          | ID         | 기타지역     |

- 참조 테이블 LOCATION에는 LOCATION_ID = 'C1' 인 값이 없음

#### FOREIGN KEY : 테이블 레벨에서 생성

- 테이블 레벨에서 생성하는 구문은 "FOREIGN KEY" 키워드가 추가됨

```sql
CREATE TABLE TABLE_FK2
(ID CHAR(3),
 SNAME VARCHAR2(20),
 LID CHAR(2),
 [CONSTRAINT FK1] FOREIGN KEY( LID ) REFERENCES LOCATION ( LOCATION_ID ) );
```

- FOREIGN KEY : 추가 키워드
- LID : 설정 컬럼
- LOCATION : 참조 테이블
- LOCATION_ID : 참조 컬럼

##### FOREIGNKEY : 생성예

- 참조 테이블의 PRIMARY KEY / UNIQUE 제약 조건이 설정된 컬럼만 참조 가능

```sql
CREATE TABLE TABLE_NOPK
(ID CHAR(3),
SNAME VARCHAR2(20));
```

- TABLE_NOPK 테이블에는 PRIMARY KEY나 UNIQUE 제약조건이 없음

```sql
CREATE TABLE TABLE_FK3
(ID CHAR(3) REFERENCES TABLE_NOPK,
 SNAME VARCHAR2(20));
```

- ORA-00268 : 참조 테이블에 기본 키가 없습니다.
- 참조 컬럼 이름을 생략했으므로 해당 테이블의 PRIMARY KEY 컬럼을 찾겠다는 의미
  - PRIMARY KEY가 존재하지 않으므로 생성 불가

```sql
CREATE TABLE TABLE_FK3
(ID CHAR(3) REFERENCES TABLE_NOPK(ID),
SNAME VARCHAR2(20));
```

- 참조 컬럼 ID에는 PRIMARY KEY나 UNIQUE 제약조건이 없음

- ORA-00270 : 이 열목록에 대해 일치하는 고유 또는 기본 키가 없습니다.

#### FOREIGN KEY : DELETION OPTION

- FOREIGN KEY 제약조건을 생성할 때, 참조 컬럼 값이 삭제되는 경우 FOREIGN KEY 컬럼 값을 어떻게 처리할 지 지정 가능

[구문]

```sql
[ CONSTRAINT constraint_name] constraint_typeON DELETE SET NULL
또는
[ CONSTRAINT constraint_name] constraint_typeON DELETE CASCADE
```

[구문 설명]

- **ON DELETE SET NULL**
  - 참조 컬럼 값이 삭제될 때, FOREIGN KEY 컬럼 값을 NULL로 변경하는 OPTION
- **ON DELETE CASCADE**
  - 참조 컬럼 값이 삭제될 때, FOREIGN KEY 컬럼 값도 함께 삭제(행 삭제 의미)하는 OPTION

#### FOREIGN KEY : 조합 컬럼 생성 예

```sql
CREATE TABLE TABLE_FK4
(ID CHAR(3),
SNAME VARCHAR2(20),
SCODE CHAR(2),
CONSTRAINT TF4_FKFOREIGN KEY ( ID, SNAME ) REFERENCES TABLE_PK2 );
```

- 조합 컬럼을 대상으로 FOREIGN KEY를 생성하려면 테이블 레벨에서 생성

```sql
INSERT INTO TABLE_FK4
VALUES ('200','IBM','03');
```

- ORA-02291 :  무결성 제약조건(VCC.TF4_FK)이 위베되었습니다-부모 키가 없습니다.

| ID   | SNAME  | SCODE |
| ---- | ------ | ----- |
| 100  | ORACLE | 01    |
| 200  | ORACLE | 01    |

- 참조 테이블에 ('200', 'IBM') 조합 값이 없으므로 입력 불가

- TABLE_PK2 테이블
  - PRIMARY KEY -> (ID, SNAME)

##### FOREIGN KEY : 생성 예

```sql
CREATE TABLE TABLE_FK5
(ID CHAR(3) REFERENCESTABLE_PK2,
SNAME VARCHAR2(20) REFERENCESTABLE_PK2,
SCODE CHAR(2));
```

- TABLE_PK2 테이블의 PRIMARY KEY은 (ID, SNAME) 컬럼조합이므로 단일 컬럼은 FOREIGN KEY 제약조건을생성할 수 없음

- ORA-00256 : 참조하고 있는 열의 숫자는 참조된 열의 수와 일치해야 합니다.

### 제약조건-CHECK

- 각 컬럼 값이 만족해야 하는 조건을 지정

```sql
CREATE TABLE TABLE_CHECK
(EMP_ID CHAR(3) PRIMARY KEY,
SALARY NUMBER CHECK ( SALARY > 0 ),
MARRIAGE CHAR(1),
CONSTRAINT CHK_MRGCHECK ( MARRIAGE IN ( 'Y','N' ) ) );
```

- SALARY 컬럼에는 0보다 큰 값만 포함될 수 있음
- MARRIAGE 컬럼에는'Y'/'N'만 포함될 수 있음

```sql
INSERT INTO TABLE_CHECKVALUES ('100', -100, 'Y');
```

- ORA-02290 : 체크 제약조건(VCC.SYS_C0011191)이 위배되었습니다.
- SALARY=-100이므로 CHECK 제약조건에 위배

```sql
INSERT INTO TABLE_CHECK
VALUES ('100', 500, '?');
```

- ORA-02290 : 체크 제약조건(VCC.CHK_MGR)이 위배되었습니다.
- MARRIAGE='?'이므로 CHECK 제약조건에 위배

#### CHECK : 사용예

```sql
CREATE TABLE TABLE_CHECK2
(ID CHAR(3) PRIMARY KEY,
HIREDATE DATE CHECK( HIREDATE < SYSDATE ) );
```

- ORA-02436 : CHECK 제약에 날짜 또는 시스템 변수가 잘못 지정되었습니다.
- 변하는 값은 조건으로 사용할 수 없음

```sql
CREATE TABLE TABLE_CHECK3
(EID CHAR(3) PRIMARY KEY,
ENAME VARCHAR2(10) NOT NULL,
SALARY NUMBER ,
MARRIAGE CHAR(1),
CHECK ( SALARY > 0 AND SALARY < 1000000 ));
```

- CHECK 조건을 여러 개 사용할 수 있음

##### SAMPLE SCRIPT

```sql
CREATE TABLE CONSTRAINT_EMP
(EID CHAR(3) CONSTRAINT PKEID PRIMARY KEY,
ENAME VARCHAR2(20) CONSTRAINT NENAME NOT NULL,
ENO CHAR(14) CONSTRAINT NENO NOT NULL CONSTRAINT UENO UNIQUE,
EMAIL VARCHAR2(25) CONSTRAINT UEMAIL UNIQUE,
PHONE VARCHAR2(12),
HIRE_DATE DATE DEFAULT SYSDATE,
JID CHAR(2) CONSTRAINT FKJID REFERENCES JOB ON DELETE SET NULL,
SALARY NUMBER,
BONUS_PCT NUMBER,
MARRIAGE CHAR(1) DEFAULT 'N' CONSTRAINT CHK CHECK(MARRIAGE IN ('Y','N')),
MID CHAR(3) CONSTRAINT FKMID REFERENCES CONSTRAINT_EMP ON DELETE SET NULL,
DID CHAR(2),
CONSTRAINT FKDID FOREIGN KEY(DID) REFERENCES DEPARTMENT ON DELETE CASCADE
);
```

### 5.1.3 서브쿼리를 활용한 테이블 생성 구문

- "AS SUBQUERY" 옵션을 사용하면 테이블 생성과 행 삽입을 동시에 할 수 있음

[구문]

```sql
CREATE TABLE table_name [(column_name[DEFAULT expr] [, ... ] ) ]
AS SUBQUERY;
```

[구문 특징]

- **특징**
  - 테이블을 생성하고, 서브쿼리 실행결 과가 자동으로 입력됨

- **컬럼정의**
  - 데이터 타입 정의 불가 : 컬럼 이름 및 DEFAULT 값만 정의 가능
  - 컬럼 이름 생략 가능 : 서브쿼리에서 사용한 컬럼 이름이 적용
  - 제약조건 : 서브쿼리에서 사용한 대상 컬럼들의 NOT NULL 조건은 자동 반영됨
  - 생성 시점에 컬럼 레벨에서 제약조건 생성 가능 -> REFERENCES 제약조건 불가

```sql
CREATE TABLE TABLE_SUBQUERY1
AS SELECT EMP_ID, EMP_NAME, SALARY, DEPT_NAME, JOB_TITLE
FROM EMPLOYEE
LEFT JOIN DEPARTMENT USING (DEPT_ID)
LEFT JOIN JOB USING (JOB_ID);
```

| Name      | Type         | Nullable |
| --------- | ------------ | -------- |
| EMP_ID    | CHAR(3)      | Y        |
| EMP_NAME  | VARCHAR2(20) |          |
| SALARY    | NUMBER       | Y        |
| DEPT_NAME | VARCHAR2(30) | Y        |
| JOB_TITLE | VARCHAR2(35) | Y        |

- EMP_ID 컬럼은 원래 PRIMARY KEY 제약조건이 생성되어 있었으나 해당 제약조건은 자동으로 적용되지 않았으므로 NULL이 허용되는 상태임
- EMP_NAME 컬럼은 설정된 NOT NULL 제약조건이 자동으로 적용된 상태임
-  컬럼 이름을 별도로 지정하지 않았으므로 서브쿼리에서 사용한 컬럼 이름이 적용됨

```sql
CREATE TABLE TABLE_SUBQUERY2 ( EID, ENAME, SALARY, DNAME, JTITLE)
AS SELECT EMP_ID, EMP_NAME, SALARY, DEPT_NAME, JOB_TITLE
FROM EMPLOYEE
LEFT JOIN DEPARTMENT USING (DEPT_ID)
LEFT JOIN JOB USING (JOB_ID);
```

[생성 결과]

| Name   | Type         | Nullable |
| ------ | ------------ | -------- |
| EID    | CHAR(3)      | Y        |
| ENAME  | VARCHAR2(20) |          |
| SALARY | NUMBER       | Y        |
| DNAME  | VARCHAR2(30) | Y        |
| JTITLE | VARCHAR2(35) | Y        |

- 지정한 컬럼 이름으로 생성됨

```sql
CREATE TABLE TABLE_SUBQUERY3
( EID PRIMARY KEY,
ENAME,
SALARY CHECK (SALARY > 2000000),
DNAME,
JTITLE NOT NULL)
AS SELECT EMP_ID, EMP_NAME, SALARY, DEPT_NAME, JOB_TITLE
FROM EMPLOYEE
LEFT JOIN DEPARTMENT USING (DEPT_ID)
LEFT JOIN JOB USING (JOB_ID);
```

- ORA-02446 : CREATE TABLE ... AS SELECT 실패 - 제약 위반 점검

- SALARY >2000000 조건때문에 서브쿼리 실행 결과 중 2000000 보다 작은 SALARY값을 입력할 수 없음

- JTITLE컬럼의 NOT NULL 조건때문에 서브쿼리 실행 결과 중 JOB_TITLE이 NULL인값을 입력할 수 없음

```sql
CREATE TABLE TABLE_SUBQUERY3
    ( EID PRIMARY KEY, 
     ENAME, 
     SALARY CHECK (SALARY > 2000000), 
     DNAME, 
     JTITLE NOT NULL)
     AS SELECT EMP_ID, EMP_NAME, SALARY, DEPT_NAME, JOB_TITLE
     FROM EMPLOYEELEFT JOIN DEPARTMENT USING (DEPT_ID)
     LEFT JOIN JOB USING (JOB_ID)
     WHERE SALARY > 2000000;
```

- ORA-01400 : NULL을 ('VCC'.'TABLE_SUBSQUERYS"."JTITLE")안에 삽입할 수 업습니다.

- WHERE 조건에 SALARY > 2000000조건을 추가하여 해결

- JTITLE 컬럼의 NOT NULL 조건때문에 서브쿼리 실행 결과 중 JOB_TITLE이 NULL인 값을 입력할 수 없음

### 5.1.3 서브쿼리를 활용한 테이블 생성 구문

``` sql
CREATE TABLE TABLE_SUBQUERY3
( EID PRIMARY KEY,
	ENAME,
	SALARY CHECK (SALARY > 2000000),
	DNAME,
	JTITLE DEFAULT 'N/A'NOT NULL)
AS SELECT EMP_ID, EMP_NAME, SALARY, DEPT_NAME, JOB_TITLE
	FROM EMPLOYEE
	LEFT JOIN DEPARTMENT USING (DEPT_ID)
	LEFT JOIN JOB USING (JOB_ID)
	WHERE SALARY > 2000000;
```

| Name   | Type         | Nullable | Default |
| ------ | ------------ | -------- | ------- |
| EID    | CHAR(3)      |          |         |
| ENAME  | VARCHAR2(20) |          |         |
| SALARY | NUMBER       | Y        |         |
| DNAME  | VARCHAR2(30) | Y        |         |
| JTITLE | VARCHAR2(35) |          | 'N/A'   |

- EID컬럼에 PRIMARY KEY 제약 조건 적용됨

- JTITLE 컬럼의 DEFAULT 설정으로 해결됨

```sql
CREATE TABLE TABLE_SUBQUERY4
( EID PRIMARY KEY,
	ENAME ,
	SALARY CHECK (SALARY > 2000000),
	DID REFERENCES DEPARTMENT,
	JTITLE DEFAULT 'N/A' NOT NULL)
AS SELECT EMP_ID, EMP_NAME, SALARY, DEPT_ID, JOB_TITLE
	FROM EMPLOYEE
	LEFT JOIN JOB USING (JOB_ID)
	WHERE SALARY > 2000000;
```

- Error : ORA-02440 : 참조 제약과 함게 create as select는 허용되지 않습니다.
  - REFERENCES 제약 조건 사용 불가

### 5.1.4 테이블 구조 확인 - SQL*Plus

- SQL> DESC[RIBE]  _table_name_

| 이름      | NULL     | 유형         |
| --------- | -------- | ------------ |
| EMP_ID    | NOT NULL | CHAR(3)      |
| EMAIL     |          | VARCHAR2(25) |
| HIRE DATE |          | DATE         |

- 컬럼이름, NULL포함여부, 데이터타입 확인 가능

[활용 가능한 기타 SQL 구문]

```sql
SQL> COL COLUMN_NAME FOR A15
SQL> COL DATA_TYPE FOR A15
SQL> COL DATA_DEFAULT A15
SQL> SELECT COLUMN_NAME,
	2 DATA_TYPE,
	3 DATA_DEFAULT,
	4 NULLABLE 
	5 FROM USER_TAB_COLS 
	6 WHERE TABLE_NAME='EMPLOYEE';
```

| COLUMN_NAME | DATA_TYPE | DATA_DEFAULT | NULLABLE |
| ----------- | --------- | ------------ | -------- |
| EMP_ID      | CHAR      |              | N        |
| HIRE_DATE   | DATE      | SYSDATE      | Y        |
| MARRIAGE    | CHAR      | 'N'          | Y        |

- COL 컬럼이름 FOR A15
  - 지정한 컬럼을 문자 15자리까지만 표시하는 명령

- 원하는 테이블 이름으로 조회 가능

- SQL 윈도우에서 코드 작성 시, 테이블 이름 부분에 커서 위치 -> 오른쪽 마우스 클릭 -> DESCRIBE 선택

- 화면 왼쪽 Browser창 -> 테이블 이름 선택 -> 오른쪽 마우스 클릭 -> DESCRIBE 선택

| Name      | Type         | Nullabel | Default | Comments |
| --------- | ------------ | -------- | ------- | -------- |
| EMP_ID    | CHAR(3)      |          |         | 사번     |
| EMP_NAME  | VARCHAR2(20) |          |         | 이름     |
| HIRE_DATE | DATE         | Y        |         | 전화번호 |

- 컬럼이름, NULL포함여부, 데이터 타입 이외에 DEFAULT 값, 컬럼 설명 까지 확인 가능

### 5.1.5 데이터 딕셔너리 <sup>DataDictionary</sup>

- 사용자 테이블
  - 일반 사용자가 생성하고 유지/관리하는 테이블
  - 사용자 데이터를 포함
- 데이터 딕셔너리
  - 오라클 DBMS가 내부적으로 생성하고 유지/관리하는 테이블
  - 데이터베이스 정보(사용자 이름, 사용자 권한, 객체 정보, 제약 조건 등)를 포함
  - USER_XXXXX 형식의 데이터 딕셔너리에 접근 가능

#### 테이블이름조회

```sql
SELECT 	TABLE_NAME
FROM 	USER_TABLES;
```

- 테이블 정보 관리

```sql
SELECT 	TABLE_NAME
FROM 	USER_CATALOG;
```

- 테이블, 뷰, 시퀀스 정보 관리

[결과(일부)]

| TABLE_NAME    |
| ------------- |
| TABLE_NOTNULL |
| TABLE_UNIQUE  |
| TABLE_UNIQUE2 |
| TABLE_UNIQUE3 |

```sql
SELECT 	OBJECT_NAME
FROM 	USER_OBJECTS
WHERE 	OBJECT_TYPE = 'TABLE';
```

- 테이블, 뷰, 시퀀스, 인덱스등 사용자 객체의 모든 정보 관리

[결과(일부)]

| OBJECT_NAME   |
| ------------- |
| TABLE_NOTNULL |
| TABLE_UNIQUE  |
| TABLE_UNIQUE2 |

#### 테이블 및 객체 정보 조회

```sql
SELECT  OBJECT_TYPE AS 유형,
		COUNT(*) AS 개수
FROM USER_OBJECTS
GROUP BY OBJECT_TYPE;
```

- 사용자 객체별 개수 현황 확인

[결과(일부)]

| 유형      | 개수 |
| --------- | ---- |
| PROCEDURE | 8    |
| TABLE     | 40   |
| INDEX     | 23   |
| FUNCTION  | 5    |

```sql
SELECT 	OBJECT_NAME AS 이름,
		OBJECT_TYPE AS 유형,
		CREATEDAS 생성일,
		LAST_DDL_TIMEAS 최종수정일
FROM USER_OBJECTS
WHERE OBJECT_TYPE = 'TABLE';
```

- 테이블별 생성일, 수정일 현황 확인

[결과(일부)]

| 이름          | 유형  | 생성일   | 최종수정일 |
| ------------- | ----- | -------- | ---------- |
| TABLE_NOTNULL | TABLE | 09/12/11 | 09/12/11   |
| TABLE_UNIQUE  | TABLE | 09/12/11 | 09/12/11   |
| TABLE_UNIQUE2 | TABLE | 09/12/11 | 09/12/11   |

#### 제약조건

- USER_CONSTRAINTS, <sup>주</sup>USER_CONS_COLUMNS를 이용

[USER_CONSTRAINTS 구조(일부)]

| Name            |
| --------------- |
| OWNER           |
| CONSTRAINT_NAME |
| TABLE_NAME      |

[주요 항목 요약]

|       항목        | 설명                                                         |
| :---------------: | ------------------------------------------------------------ |
|  CONSTRAINT_TYPE  | • P : PRIMARYKEY<br/>• U : UNIQUE<br/>• R : REFERENCES<br/>• C : CHECK, NOT NULL |
| SEARCH_CONDITION  | CHECK 제약조건 내용                                          |
| R_CONSTRAINT_NAME | 참조 테이블의 PRIMARY KEY 이름                               |
|    DELETE_RULE    | 참조 테이블의 PRIMARY KEY 컬럼이 삭제될 때 적용되는 규칙<br/>"NoAction", "SETNULL", "CASCADE" 로 표시 |

- <sup>주</sup>USER_CONS_COLUMNS에서는 컬럼 이름을 확인할 수 있음

#### 제약 조건 확인

```sql
SELECT 	CONSTRAINT_NAMEAS 이름,
        CONSTRAINT_TYPEAS 유형,
        R_CONSTRAINT_NAMEAS 참조,
        DELETE_RULEAS 삭제규칙,
        SEARCH_CONDITIONAS 내용
FROM 	USER_CONSTRAINTS
WHERE 	TABLE_NAME='CONSTRAINT_EMP';
```

| 이름   | 유형 | 참조 | 삭제규칙 | 내용    |
| ------ | ---- | ---- | -------- | ------- |
| NENAME | C    |      |          | \<Long> |
| NENO   | C    |      |          | \<Long> |
| CHK    | C    |      |          | \<Long> |
| PKEID  | P    |      |          | \<Long> |

- 'NOT NULL' 제약 조건은 CHECK 제약조건 유형으로 관리됨

```sql
SELECT 	CONSTRAINT_NAME AS 이름,
		CONSTRAINT_TYPE AS 유형,
		COLUMN_NAME AS 컬럼,
		R_CONSTRAINT_NAME AS 참조,
		DELETE_RULE AS 삭제규칙,
		SEARCH_CONDITION AS 내용
FROM 	USER_CONSTRAINTS
JOIN 	USER_CONS_COLUMNSUSING (CONSTRAINT_NAME, TABLE_NAME)
WHERE 	TABLE_NAME='CONSTRAINT_EMP';
```

| 이름   | 유형 | 컬럼     | 참조     | 삭제규칙 | 내용    |
| ------ | ---- | -------- | -------- | -------- | ------- |
| NENAME | C    | ENAME    |          |          | \<Long> |
| NENO   | C    | ENO      |          |          | \<Long> |
| CHK    | C    | MARRIAGE |          |          | \<Long> |
| PKEID  | P    | EID      |          |          | \<Long> |
| FKJID  | R    | JID      | PK_JOBID | SET NULL | \<Long> |
| FKMID  | R    | MID      | PKEID    | SET NULL | \<Long> |

### 5.1.6 테이블 수정 - 범위

- **컬럼 관련**
  - 컬럼 추가/삭제
  - 데이터 타입 변경, DEFAULT 변경
  - 컬럼 이름 변경
- **제약조건 관련**
  - 제약조건 추가/ 삭제
  - 제약조건 이름 변경
- **테이블 관련**
  - 테이블 이름 변경

#### 구문

[구문]

```sql
ALTER TABLE table_name
ADD (column_name datatype [DEFAUL Texpr] [, …] ) |
{ADD [CONSTRAINT constraint_name] constraint_type(column_name) } ... |
MODIFY (column_name datatype [DEFAULTexpr] [, …] ) |
DROP{ [COLUMN column_name] | (column_name, …) } [CASCADE CONSTRAINTS] |
DROP PRIMARY KEY [CASCADE] | 
	{ UNIQUE (column_name, …) [CASCADE] }... |
	CONSTRAINT constraint_name[CASCADE];
```

[이름 변경 구문]

```sql
ALTER TABLE old_table_name RENAME TO new_table_name ;
RENAME old_table_name TO new_table_name;
ALTER TABLE table_name RENAME COLUMN old_column_name TO new_column_name ;
ALTER TABLE table_name RENAME CONSTRAINT old_const_name TO new_const_name ;
```

#### 컬럼 추가

- 추가되는 컬럼은 테이블의 맨 마지막에 위치하며, 생성위치를 변경할 수 없음

```sql
ALTER TABLE DEPARTMENT
ADD( MGR_ID CHAR(3) );
```

| Name      | Type         | Nullable |
| --------- | ------------ | -------- |
| DEPT_ID   | CHAR(2)      |          |
| DEPT_NAME | VARCHAR2(30) | Y        |
| DEPT_NAME | CHAR(2)      |          |
| MGR_ID    | CHAR(3)      | Y        |

| DEPT_ID | DEPT_NAME | DEPT_NAME | MGR_ID |
| ------- | --------- | --------- | ------ |
| 20      | 회계팀    | A1        |        |

- DEFAULT 값이 없으면 추가되는 컬럼은 NULL이 적용됨

```sql
ALTER TABLE DEPARTMENT
ADD( MGR_ID CHAR(3) DEFAULT '101' );
```

| Name      | Type         | Nullable | Default |
| --------- | ------------ | -------- | ------- |
| DEPT_ID   | CHAR(2)      |          |         |
| DEPT_NAME | VARCHAR2(30) | Y        |         |
| DEPT_NAME | CHAR(2)      |          |         |
| MGR_ID    | CHAR(3)      | Y        | '101'   |

| DEPT_ID | DEPT_NAME | DEPT_NAME | MGR_ID |
| ------- | --------- | --------- | ------ |
| 20      | 회계팀    | A1        | 101    |

- DEFAULT 값을 설정하면 추가되는 컬럼에 DEFAULT 값이 적용됨

#### 제약조건 추가

- 'NOTNULL' 이외의 제약조건은 ADD 구문 사용(테이블 레벨에서의 정의 구문과 유사)

- 'NOTNULL' 제약조건은 MODIFY 구문 사용

```sql
CREATE TABLE EMP3 AS SELECT * FROM EMPLOYEE;
ALTER TABLE EMP3
ADD PRIMARY KEY (EMP_ID)
ADD UNIQUE (EMP_NO)
MODIFY HIRE_DATE NOT NULL;
```

1. 샘플 테이블 EMP3  생성
2. EMP_ID  컬럼에 PRIAMRY KEY 제약조건 추가
3. EMP_NO 컬럼에 UNIQUE 제약조건 추가
4. HIRE_DATE 컬럼에 NOT NULL 제약조건 추가

[제약조건 추가 결과 확인]

| 이름         | 유형 | 컬럼     | 참조 | 삭제규칙 | 내용    |
| ------------ | ---- | -------- | ---- | -------- | ------- |
| SYS_C0011246 | C    | EMP_NAME |      |          | \<Long> |

#### 컬럼 수정

- **컬럼 데이터 타입 관련**
  - 대상 컬럼이 비어있는 경우에만 타입 변경 가능
  - 단, 컬럼 크기를 변경하지 않거나 증가시키는 경우 CHAR⇔VARCHAR2 변환 가능

- **컬럼 크기 관련**
  - 대상 컬럼이 비어있는 경우 또는 테이블 전체에 데이터가 없는 경우 크기 감소 가능
  - 포함된 데이터에 영향을 미치지않는 범위에서는 크기 감소 가능
- **DEFAULT 관련**
  - DEFAULT 값이 변경되면 변경 이후부터 적용

#### 컬럼 수정 예

```sql
CREATE TABLE EMP4
AS SELECT EMP_ID, EMP_NAME, HIRE_DATE
FROM EMPLOYEE;
```

| Name      | Type         |
| --------- | ------------ |
| EMP_ID    | CHAR(3)      |
| EMP_NAME  | VARCHAR2(20) |
| HIRE_DATE | DATE         |

```sql
ALTER TABLE EMP4
MODIFY (EMP_ID VARCHAR2(5),
EMP_NAME CHAR(20));
```

| Name      | Type        |
| --------- | ----------- |
| EMP_ID    | VARCHAR2(5) |
| EMP_NAME  | CHAR(20)    |
| HIRE_DATE | DATE        |

- EMP_ID 컬럼 : 크기를 증가시켰으므로 VARCHAR2 타입 변환 가능

- EMP_NAME 컬럼 : 크기 변경없으므로 CHAR 타입 변환 가능

```sql
ALTER TABLE EMP4
MODIFY (HIRE_DATE CHAR(8));
```

- DATE 타입을 CHAR 타입으로 변환하려면 데이터가 없어야 함
- ORA-01439 : 데이터 유형을 변경할 열은 비어 있어야 합니다.

```sql
ALTER TABLE EMP4
MODIFY (EMP_NAME CHAR(15));
```

- CHAR(20)에서 크기를 감소시키는 것은 불가

- ORA-01441 : 일부 값이 너무 커서 열 길이를 줄일 수 없음

#### 컬럼 수정 예

```sql
CREATE TABLE EMP5
	(EMP_ID CHAR(3),
	EMP_NAME VARCHAR2(20),
	ADDR1 VARCHAR2(20) DEFAULT'서울',
	ADDR2 VARCHAR2(100));
INSERT INTO EMP5
VALUES ('A10','임태희', DEFAULT, '청담동');
INSERT INTO EMP5
VALUES ('B10', '이병언', DEFAULT, '분당정자동');
SELECT * FROM EMP5;
```

| Name     | Type          | Nullable | Default |
| -------- | ------------- | -------- | ------- |
| EMP_ID   | CHAR(3)       | Y        |         |
| EMP_NAME | VARCHAR2(20)  | Y        |         |
| ADDR1    | VARCHAR2(20)  | Y        | '서울'  |
| ADDR2    | VARCHAR2(100) | Y        |         |

| EMP_ID | EMP_NAME | ADDR1 | ADDR2       |
| ------ | -------- | ----- | ----------- |
| A10    | 임태희   | 서울  | 청담동      |
| B10    | 이병언   | 서울  | 분당 정자동 |

```sql
ALTER TABLEEMP5
MODIFY (ADDR1 DEFAULT '경기');
INSERT INTO EMP5
VALUES ('C10', '임승우', DEFAULT, '분당효자촌');
SELECT * FROM EMP5;
```

- DEFAULT 값은 변경 이후부터 적용

| Name     | Type          | Nullable | Default |
| -------- | ------------- | -------- | ------- |
| EMP_ID   | CHAR(3)       | Y        |         |
| EMP_NAME | VARCHAR2(20)  | Y        |         |
| ADDR1    | VARCHAR2(20)  | Y        | '경기'  |
| ADDR2    | VARCHAR2(100) | Y        |         |

| EMP_ID | EMP_NAME | ADDR1 | ADDR2       |
| ------ | -------- | ----- | ----------- |
| A10    | 임태희   | 서울  | 청담동      |
| B10    | 이병언   | 서울  | 분당 정자동 |
| C10    | 임승우   | 경기  | 분당 효자촌 |