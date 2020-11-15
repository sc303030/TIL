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

29p