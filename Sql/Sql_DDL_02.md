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

59p