# Sql_03

### 3.1.1 함수 개념

- 하나의 큰 프로그램에서 반복적으로 사용되는 부분들을 분리핚 작은 서브 프로그램

- 호출하고, 실행 결과를 리턴 하는 방식으로 사용

### 3.1.2 함수 유형

- 반환 결과에 따라 단일 행 함수와 그룹 함수로 구분

#### 단일행 함수

- 각 행마다 반복적으로 적용되어 입력 받은 행개수만큼 결과를 반환
- Input N개 -> `단일 행 함수` -> Output N개

#### 그룹 함수

- 그룹(특정한 행들의 집합)별로 적용되고 그룹당 유일한 결과를 반환

- Input N개 -> `그룹 함수` -> Output 1개

#### 함수 유형 예

**[직원 테이블]**

| 사번 | 성별 |            근무지             | 나이 |
| :--: | :--: | :---------------------------: | :--: |
| 001  |  M   |   마포구 상암동 상암IT센터    |  29  |
| 002  |  M   |    중구 회현동 프라임 타워    |  30  |
| 003  |  F   |   영등포구 문래동 강서빌딩    |  25  |
| 004  |  M   | 영등포구 여의도동 LG 트윈타워 |  27  |
| 005  |  F   |     강남구 역삼동 GS타워      |  24  |

- 단일 행 함수 사용 

| 사번 |  근무지  |
| :--: | :------: |
| 001  |  마포구  |
| 002  |   중구   |
| 003  | 영등포구 |
| 004  | 영등포구 |
| 005  |  강남구  |

- 그룹 함수 사용

| 성별 | 평균나이 |
| :--: | :------: |
|  M   |   28.6   |
|  F   |   24.5   |

### 3.1.3 주요 단일 행 함수

|     구분      | 입력 값 타입 | 리턴 값 타입 |                종류                 |
| :-----------: | :----------: | :----------: | :---------------------------------: |
| 문자(열) 함수 |  CHARACTER   |  CHARACTER   | LPAD/RPAD, LTRIM/RTRIM/TRIM, SUBSTR |
| 문자(열) 함수 |  CHARACTER   |    NUMBER    |        INSTR, LENGTH/LENGTHB        |
|   숫자 함수   |    NUMBER    |    NUMBER    |            ROUND, TRUNC             |
|   날짜 함수   |     DATE     |     DATE     |         ADD_MONTHS, SYSDATE         |
|   날짜 함수   |     DATE     |    NUMBER    |           MONTHS_BETWEEN            |
| 타입변환 함수 |     ANY      |     ANY      |     TO_CHAR, TO_DATE, TO_NUMBER     |
|   기타 함수   |     ANY      |     ANY      |             NVL, DECODE             |

#### 문자열 함수 LENGTH

- 주어진 컬럼 값/문자열 길이(문자 개수)를 반환하는 함수

```sql
LENGTH(string)    NUMBER     CHARACTER 타입의 컬럼 또는 임의 문자열
    [구문]       [반환타입]              [파라미터]
```

**[구문 특징]**

- CHAR 데이터 타입 컬럼 값을 입력 받은 경우
  - 실제 데이터 길이(문자 개수)에 상관없이 컬럼 전체 길이(문자 개수)를 반환

- VARCHAR2 데이터 타입 컬럼 값을 입력 받은 경우
  - VARCHAR2 데이터 타입 컬럼 값을 입력 받은 경우

##### 문자열 함수 LENGTH 사용 예

| Name        | Type         |
| ----------- | ------------ |
| CHARTYPE    | CHAR(20)     |
| VARCHARTYPE | VARCHAR2(20) |

| CHARTYPE     | VARCHARTYPE  |
| ------------ | ------------ |
| PE ENG       | PE ENG       |
| 펭수펭하펭빠 | 펭수펭하펭빠 |

```sql
SELECT	LENGTH(CHARTYPE),
		LENGTH(VARCHARTYPE)
FROM	COLUMN_LENGTH;
```

| LENGTH(CHARTYPE) | LENGTH(VARCHARTYPE) |
| ---------------- | ------------------- |
| 20               | 6                   |
| 14               | 6                   |

- CHAR 타입 컬럼에 한글이 포함되었을 때 LEGNTH 함수 적용 결과
  - 한글 1문자=2바이트, 6문자이므로 6*2 = 12바이트
  - 컬럼 길이(20바이트) – 데이터 길이(12바이트) = 8바이트
  - 데이터 6문자 + 여유 공간 8문자(영문자 기준으로) = 14

#### 문자열 함수 LENGTHB

- 주어진 컬럼 값/문자열 길이(Byte)를 반환하는 함수

```sql
LENGTHB(string)      NUMBER    CHARACTER 타입의 컬럼 또는 임의 문자열
  [구문]            [반환 타입]              [파라미터]
```

**[구문 특징]**

- Single-byte Character Set 경우
  - LENGTH 함수 반환 값과 동일(문자 개수 = 바이트 수)
- Multi-byte Character Set 경우
  - Character Set에 따라 동일한 데이터에 대한 바이트 처리 결과가 다름

##### 문자열 함수 LENGTHB 사용 예

| Name        | Type         |
| ----------- | ------------ |
| CHARTYPE    | CHAR(20)     |
| VARCHARTYPE | VARCHAR2(20) |

| CHARTYPE     | VARCHARTYPE  |
| ------------ | ------------ |
| PE ENG       | PE ENG       |
| 펭수펭하펭빠 | 펭수펭하펭빠 |

```sql
SELECT	LENGTHB(CHARTYPE),
		LENGTHB(VARCHARTYPE)
FROM	COLUMN_LENGTH;
```

| LENGTB(CHARTYPE) | LENGTB(VARCHARTYPE) |
| ---------------- | ------------------- |
| 20               | 6                   |
| 20               | 12                  |

#### 문자열 함수 INSTR

- 찾는 문자(열)이 지정한 위치부터 지정한 회수만큼 나타난 시작 위치를 반환하는 함수 

```
INSTR(string, substring, [ position, [ occurrence ] ] )    NUMBER
         [구문]											[반환 타입]
```

**[파라미터]**

|  파라미터  |                             설명                             |
| :--------: | :----------------------------------------------------------: |
|   string   |                  문자 타입 컬럼 또는 문자열                  |
| substring  |                      찾으려는 문자(열)                       |
|  position  | • 어디부터 찾을지를 결정하는 시작 위치(기본값 1)<br/>• POSITION > 0 : STRING의 시작부터 끝 방향으로 찾는 의미<br/> • POSITION < 0 : STRING의 끝부터 시작 방향으로 찾는 의미 |
| occurrence | • SUBSTRING이 반복될 때의 지정하는 빈도(기본값 1)<br/>• 음수 값은 사용할 수 없음 |

##### 문자열 함수 INSTR 사용 예

- EMAIL 컬럼 값의 '@vcc.com' 문자열 중 "." 바로 앞의 문자 'c' 위치를 구하시오

**[구문 1]**

```sql
SELECT 	EMAIL,
		INSTR( EMAIL,'c',-1,2 ) 위치
FROM 	EMPLOYEE ;
```

- 'POSITION'은 음수(-1)이므로 끝부터 시작 방향으로 찾음
- 'OCCURRENCE'는 2 이므로 두 번째 나타나는 문자 위치를 의미

**[결과(일부)]**

| EMAIL           | 위치 |
| --------------- | ---- |
| aa_aaa@acc.com  | 10   |
| bb_bbbb@acc.com | 11   |

**[구문 2]**

```sql
SELECT 	EMAIL,
		INSTR( EMAIL, 'c', INSTR( EMAIL,'.' )-1 ) 위치
FROM 	EMPLOYEE ;
```

- 'POSITION'은 INSTR(EMAIL, '.')-1 결과(≒ 마침표 핚 자리 앞 의미)
- 'OCCURRENCE'는 생략되었으므로 기본 값 1 적용(첫 번째 나타나는 문자 의미)

#### 문자열 함수 LPAD/RPAD

- 주어진 컬럼/문자열에 임의의 문자(열)을 왼쪽/오른쪽에 덧붙여 길이 N의 문자열을 반환하는 함수

| 구문                     | 반환타입  |
| ------------------------ | --------- |
| LPAD( string, N, [str] ) | CHARACTER |
| RPAD( string, N, [str] ) | CHARACTER |

| 파라미터 | 설명                                                         |
| -------- | ------------------------------------------------------------ |
| string   | 문자 타입 컬럼 또는 문자열                                   |
| N        | • 반환할 문자(열)의 길이(바이트)<br/>• 원래 STRING 길이보다 작다면 N만큼만 잘라서 표시 |
| str      | • 덧붙이려는 문자(열) <br/>• 생략하면 공백 핚 문자           |

##### 문자열 함수 LPAD 사용 예

- EMAIL 컬럼 왼쪽에 '.' 을 덧붙여 길이를 20으로 맞추시오

```sql
SELECT	EMAIL AS 원본데이터,
		LENGTH(EMAIL) AS 원본길이,
		LPAD(EMAIL, 20, '.') AS 적용결과,
		LENGTH(LPAD(EMAIL, 20, '.')) AS 결과길이
FROM 	EMPLOYEE;
```

**[결과(일부)]**

| 원본데이터      | 원본길이 | 적용결과             | 결과길이 |
| --------------- | -------- | -------------------- | -------- |
| aa_aaa@acc.com  | 14       | ......aa_aaa@acc.com | 20       |
| bb_bbbb@acc.com | 15       | .....bb_bbbb@acc.com | 20       |

- 오른쪽 정렬 효과를 나타냄

##### 문자열 함수 RPAD 사용 예

- EMAIL 컬럼 오른쪽에 '.' 을 덧붙여 길이를 20으로 맞추시오

```sql
SELECT	EMAIL AS 원본데이터,
		LENGTH(EMAIL) AS 원본길이,
		RPAD(EMAIL, 20, '.') AS 적용결과,
		LENGTH(RPAD(EMAIL, 20, '.')) AS 결과길이
FROM 	EMPLOYEE;
```

**[결과(일부)]**

| 원본데이터      | 원본길이 | 적용결과             | 결과길이 |
| --------------- | -------- | -------------------- | -------- |
| aa_aaa@acc.com  | 14       | aa_aaa@acc.com...... | 20       |
| bb_bbbb@acc.com | 15       | bb_bbbb@acc.com..... | 20       |

- 왼쪽 정렬 효과를 나타냄

#### 문자열 함수 LTRIM/RTRIM

- 주어진 컬럼/문자열의 왼쪽/오른쪽에서 지정한 STR에 포함된 모든 문자를 제거한 나머지를 반환하는 함수

- 패턴을 제거하는 의미가 아님

| 구문                                          | 반환타입  |
| --------------------------------------------- | --------- |
| LTRIM( string, str )<br/>RTRIM( string, str ) | CHARACTER |

| 파라미터 | 설명                                                         |
| :------: | ------------------------------------------------------------ |
|  string  | 문자 타입 컬럼 또는 문자열                                   |
|   str    | • 제거하려는 문자(열)<br/>• 생략하면 왼쪽/오른쪽 공백을 모두 제거 |

##### 문자열 함수 LTRIM 사용 예

| 구문 (•은 공백)                                              | 결과    |
| ------------------------------------------------------------ | ------- |
| SELECT LTRIM('•••tech') FROM DUAL;                           | tech    |
| SELECT LTRIM('•••tech', '•') FROM DUAL;                      | tech    |
| SELECT LTRIM('000123', '0') FROM DUAL;                       | 123     |
| SELECT LTRIM('123123Tech', '123') FROM DUAL;                 | Tech    |
| SELECT LTRIM('123123Tech123', '123') FROM DUAL;              | Tech123 |
| SELECT LTRIM('xyxzyyyTech', 'xyz') FROM DUAL;<br/>'xyz'는 xyz가 한묶음이 아니라 or 개념이다. | Tech    |
| SELECT LTRIM('6372Tech', '0123456789') FROM DUAL;            | Tech    |

##### 문자열 함수 RTRIM 사용 예

| 구문 (•은 공백)                                              | 결과    |
| ------------------------------------------------------------ | ------- |
| SELECT RTRIM('tech•••') FROM DUAL;                           | tech    |
| SELECT RTRIM('tech•••', '•') FROM DUAL;                      | tech    |
| SELECT RTRIM('123000', '0') FROM DUAL;                       | 123     |
| SELECT RTRIM('Tech123123', '123') FROM DUAL;                 | Tech    |
| SELECT RTRIM('123Tech123', '123') FROM DUAL;                 | Tech123 |
| SELECT RTRIM('Techxyxzyyy', 'xyz') FROM DUAL;<br/>'xyz'는 xyz가 한묶음이 아니라 or 개념이다. | Tech    |
| SELECT RTRIM('Tech6372', '0123456789') FROM DUAL;<br/>'0123456789'도 or 개념 | Tech    |

#### 문자열 함수 TRIM

- 주어진 컬럼/문자열의 앞/뒤/양쪽에 있는 지정한 문자를 제거한 나머지를 반환하는 함수

| 구문                                                         | 반환타입  |
| ------------------------------------------------------------ | --------- |
| TRIM( trim_source )                                          | CHARACTER |
| TRIM( trim_char FROM trim_source )                           | CHARACTER |
| TRIm( LEADING\|TRAILING\|BOTH [trim_char] FROM trim_source ) | CHARACTER |

|           파라미터            |                             설명                             |
| :---------------------------: | :----------------------------------------------------------: |
|          trim_source          |                  문자 타입 컬럼 또는 문자열                  |
|           trim_char           | • 제거하려는 하나의 문자<br/>• 생략하면 기본값으로 공백 한 문자 |
| LEADING<br/>TRAILING<br/>BOTH | • trim_char 위치 지정<br/>• 앞/뒤/양쪽 지정 가능(기본값은 양쪽) |

##### 문자열 함수 TRIM 사용 예

| 구문(•은 공백)                                     | 결과   |
| -------------------------------------------------- | ------ |
| SELECT TRIM('••tech••') FROM DUAL;                 | tech   |
| SELECT TRIM('a' FROM 'aatechaaa') FROM DUAL;       | tech   |
| SELECT TRIM(LEADING '0' FROM '000123') FROM DUAL;  | 123    |
| SELECT TRIM(TRAILING '1' FROM 'Tech1') FROM DUAL;  | Tech   |
| SELECT TRIM(BOTH '1' FROM '123Tech111') FROM DUAL; | 23Tech |
| SELECT TRIM(LEADING FROM '••Tech••') FROM DUAL;    | Tech•• |

#### 문자열 함수 SUBSTR

|                  구문                  | 반환 타입 |
| :------------------------------------: | :-------: |
| SUBSTR( string, position, [ length ] ) | CHARACTER |

| 파라미터 |                             설명                             |
| :------: | :----------------------------------------------------------: |
|  string  |                  문자 타입 컬럼 또는 문자열                  |
| position | • 잘라내는 시작 위치<br/>• position = 0 or 1 : 시작 위치(1번째)<br/>• position > 0 : 끝 방향으로 지정한 수만큼 위치<br/>• position < 0 : 시작 방향으로 지정한 수만큼 위치 |
|  length  | • 반환할 문자 개수<br/>• 잘라내는 방향은 항상 끝 방향<br/>• 생략하면 position부터 문자열 끝까지를 의미<br/>• length < 0 : NULL 반환 |

##### 문자열 함수 SUBSTR 사용 예

| 구문(•은 공백)                                      | 결과      |
| --------------------------------------------------- | --------- |
| SELECT SUBSTR('This•is•a•test', 6, 2) FROM DUAL;    | is        |
| SELECT SUBSTR('This•is•a•test', 6) FROM DUAL;       | is•a•test |
| SELECT SUBSTR('이것은•연습입니다', 3, 4) FROM DUAL; | 은•연습   |
| SELECT SUBSTR('TechOnTheNet', 1, 4) FROM DUAL;      | Tech      |
| SELECT SUBSTR('TechOnTheNet', -3, 3) FROM DUAL;     | Net       |
| SELECT SUBSTR('TechOnTheNet', -8, 2) FROM DUAL;     | On        |

#### 숫자 함수 ROUND

- 지정한 자릿수에서 반올림 하는 함수

| 구문                                  | 반환 타입 |
| ------------------------------------- | --------- |
| **ROUND**( number, [decimal_places] ) | NUMBER    |

|    파라미터    |                             설명                             |
| :------------: | :----------------------------------------------------------: |
|     number     |                숫자 타입 컬럼 또는 임의 숫자                 |
| decimal_places | • 반올림되어 표시되는 소수점 자리<br/> • 반드시 정수 값 사용<br/>• 생략되면 0 의미<br/>• decimal_places > 0 : 소수점 이하 자리 의미<br/>• decimal_places < 0 : 소수점 이상 자리 의미 |

##### 숫자 함수 ROUND 사용 예

|                 구문                 |  결과   |
| :----------------------------------: | :-----: |
|   SELECT ROUND(125.315) FROM DUAL;   |   125   |
| SELECT ROUND(125.315, 0) FROM DUAL;  |   125   |
| SELECT ROUND(125.315, 1) FROM DUAL;  |  125.3  |
| SELECT ROUND(125.315, -1) FROM DUAL; |   130   |
| SELECT ROUND(125.315, 3) FROM DUAL;  | 125.315 |
| SELECT ROUND(-125.315, 2) FROM DUAL; | -125.32 |

#### 숫자 함수 TRUNC

- 지정한 자릿수에서 버림 하는 함수

|                 구문                  | 반환 타입 |
| :-----------------------------------: | :-------: |
| **TRUNC**( number, [decimal_places] ) |  NUMBER   |

|    파라미터    |                             설명                             |
| :------------: | :----------------------------------------------------------: |
|     number     |                숫자 타입 컬럼 또는 임의 숫자                 |
| decimal_places | • 버림 결과가 표시되는 소수점 자리<br/>• 반드시 정수 값 사용<br/>• 생략되면 0 의미<br/>• decimal_places > 0 : 소수점 이하 자리 의미<br/>• decimal_places < 0 : 소수점 이상 자리 의미 |

##### 숫자 함수 TRUNC 사용 예

|                 구문                  |  결과   |
| :-----------------------------------: | :-----: |
|   SELECT TRUNC(125.315) FROM DUAL;    |   125   |
|  SELECT TRUNC(125.315, 0) FROM DUAL;  |   125   |
|  SELECT TRUNC(125.315, 1) FROM DUAL;  |  125.3  |
| SELECT TRUNC(125.315, -1) FROM DUAL;  |   120   |
|  SELECT TRUNC(125.315, 3) FROM DUAL;  | 125.315 |
| SELECT TRUNC(-125.315, -3) FROM DUAL; |    0    |

#### 날짜 함수 SYSDATE

- 지정된 형식으로 현재 날짜와 시간을 표시하는 함수

- 한글 버전으로 설치된 경우 기본 설정 형식은 '년도(2자리)/월(2자리)/일(2자리)'

|  구문   | 반환 타입 |
| :-----: | :-------: |
| SYSDATE |   DATE    |

```sql
SELECT SYSDATE
FROM DUAL;
```

| SYSDATE  |
| -------- |
| 09/12/31 |

#### 날짜 함수 ADD_MONTHS

- 지정한 만큼의 달 수를 더한 날짜를 반환하는 함수

|           구문            | 반환 타입 |
| :-----------------------: | :-------: |
| **ADD_MONTHS**( date, N ) |   DATE    |

| 파라미터 |         설명          |
| :------: | :-------------------: |
|   date   |   기준이 되는 날짜    |
|    N     | date에 더하려는 월 수 |

##### 날짜 함수 ADD_MONTHS 사용 예

- 직원 별로 입사일 기준으로 근무한 지 20년이 되는 일자를 조회하시오.

```sql
SELECT 	EMP_NAME,
		HIRE_DATE,
		ADD_MONTHS( HIRE_DATE, 240 )
FROM 	EMPLOYEE;
```

| EMP_NAME | HIRE_DATE | ADD_MONTHS |
| -------- | --------- | ---------- |
| 펭하     | 05/07/31/ | 25/07/31   |

#### 날짜 함수 MONTHS_BETWEEN

- 지정한 두 날짜 사이의 월 수를 반환하는 함수

|                구문                | 반환 타입 |
| :--------------------------------: | :-------: |
| **MONTHS_BETWEEN**( date1, date2 ) |  NUMBER   |

|    파라미터     |                             설명                             |
| :-------------: | :----------------------------------------------------------: |
| date1<br/>date2 | • date1, date2 는 날짜<br/>• date1 > date 2 : 양수 반환<br/>• date1 < date 2 : 음수 반환 |

- 날짜가 크다는 의미는 더 나중 날짜라는 의미

##### 날짜 함수 MONTHS_BETWEEN 사용 예

|                            구문                            |       결과        |
| :--------------------------------------------------------: | :---------------: |
| SELECT MONTHS_BETWEEN( '09/01/01', '09/03/14' ) FROM DUAL; | -2.41935483870968 |
| SELECT MONTHS_BETWEEN( '09/07/01', '09/03/14' ) FROM DUAL; | 3.58064516129032  |
| SELECT MONTHS_BETWEEN( '09/03/01', '09/03/01' ) FROM DUAL; |         0         |
| SELECT MONTHS_BETWEEN( '09/08/02', '09/06/02' ) FROM DUAL; |         2         |

- 결과 중 정수가 아닌 부분은 달의 일부를 의미

- '2010년 1월 1일' 기준으로 입사핚지 10년이 넘은 직원들의 근무년수 조회

```sql
SELECT 	EMP_NAME,
		HIRE_DATE,
		MONTHS_BETWEEN('10/01/01', HIRE_DATE)/12 AS 근무년수
FROM 	EMPLOYEE
WHERE 	MONTHS_BETWEEN('10/01/01', HIRE_DATE) > 120;
```

| EMP_NAME | HIRE_DATE | 근무년수         |
| -------- | --------- | ---------------- |
| 펭하     | 95/12/30  | 14.005376344086  |
| 펭수     | 97/06/03  | 12.5779569892473 |

#### 데이터 타입 변환

- 오라클 DBMS는 데이터 타입을 변환하는 두 가지 방법을 제공

> SQL 구문의 신뢰성을 높이기 위해서는 명시적으로 변환하는 것을 권장

|       방법       |                             특징                             |
| :--------------: | :----------------------------------------------------------: |
| 암시적(Implicit) |            • 자동적으로 변환<br/>• 적용 규칙 존재            |
| 명시적(Explicit) | • 타입 변환 함수를 명시적으로 사용<br/>• 데이터 타입에 따라 적절한 변환 함수 필요 |

- 데이터 타입에 따라 사용핛 수 있는 변환 함수가 다름
- 상호 타입 변환이 되지 않는 데이터 타입 존재

|   타입    | 변환 함수 |   결과    |
| :-------: | :-------: | :-------: |
|  NUMBER   |  TO_CHAR  | CHARACTER |
| CHARACTER |  TO_DATE  |   DATE    |
|   DATE    |  TO_CHAR  | CHARACTER |
| CHARACTER | TO_NUMBER |  NUMBER   |

#### 데이터 타입 변환 함수 TO_CHAR

- NUMBER/DATE 타입을 CHARACTER 타입으로 변환하는 함수

| 입력타입    | 구문                                 | 반환 타입 |
| ----------- | ------------------------------------ | --------- |
| NUMBER/DATE | **TO_CHAR**( input_type [,format ] ) | CHARACTER |

**[함수 설명]**

- NUMBER 타입을 CHARACTER 타입으로 변환이 필요한 경우
  - 표현 형식을 변경 : 1000 -> 1,000
  - 숫자를 문자로 사용 : 100 -> '100'

- DATE 타입을 CHARACTER 타입으로 변환이 필요한 경우
  - "년,월,일" 표현 형식을 변경 : 09/12/30 -> 2009-12-30
  - 시간 정보 표시
  - CHARCATER 타입과 비교 : HIRE_DATE = '03/06/14'

- 'format' 파라미터는 변경하려는 표현 형식을 위해 제공되는 규칙

##### 데이터 타입 변환 함수 TO_CHAR 사용 예

- 사번이 100인 직원 이름과 급여 조회

```sql
SELECT 	EMP_NAME,
		SALARY
FROM 	EMPLOYEE
WHERE 	EMP_ID = 100;
```

- CHAR 타입과 NUMBER 타입 비교는 불가능

```sql
SELECT 	EMP_NAME,
		SALARY
FROM 	EMPLOYEE
WHERE 	EMP_ID = TO_CHAR(100);
```

- TO_CHAR 함수를 사용하여 NUMBER 타입을 CHAR 타입으로 변환 후 비교 -> 명시적 변환

**[제공되는 숫자 표현형식]**

|   형식   |            설명             |
| :------: | :-------------------------: |
|    9     |        자리 수 지정         |
|    0     |   남는 자리를 0으로 표시    |
| $ 또는 L |        통화기호 표시        |
| . 또는 , | 지정핚 위치에 . 또는 , 표시 |
|   EEEE   |      과학 지수 표기법       |

|                    구문                    |  결과   |
| :----------------------------------------: | :-----: |
|  SELECT TO_CHAR(1234, '99999') FROM DUAL;  |  1234   |
|  SELECT TO_CHAR(1234, '09999') FROM DUAL;  |  01234  |
| SELECT TO_CHAR(1234, 'L99999') FROM DUAL;  | ￦1234  |
| SELECT TO_CHAR(1234, '99,999') FROM DUAL;  |  1,234  |
| SELECT TO_CHAR(1234, '09,999') FROM DUAL;  | 01,234  |
| SELECT TO_CHAR(1000, '9.9EEEE') FROM DUAL; | 1.0E+03 |
|   SELECT TO_CHAR(1234, '999') FROM DUAL;   |  ####   |

**[제공되는 날짜 표현형식]**

| 형식            | 설명                              |
| --------------- | --------------------------------- |
| YYYY/YY/YEAR    | 년도(4자리숫자/뒤 2자리숫자/문자) |
| MONTH/MON/MM/RM | 달(이름/약어/숫자/로마 기호)      |
| DDD/DD/D        | 일(1년 기준/1달 기준/1주 기준)    |
| Q               | 분기(1, 2, 3, 4)                  |
| DAY/DY          | 요일(이름/약어 이름)              |
| HH(12)/HH24     | 12시갂/24시갂 표시                |
| AM\|PM          | 오전,오후                         |
| MI              | 분(0~59)                          |
| SS              | 초(0~59)                          |

##### 데이터 타입 변환 함수 TO_CHAR 사용 예

| 구문                                                     | 결과             |
| -------------------------------------------------------- | ---------------- |
| SELECT TO_CHAR( SYSDATE, 'PM HH24:MI:SS' ) FROM DUAL;    | 오후 20:57:11    |
| SELECT TO_CHAR( SYSDATE, 'AM HH:MI:SS' ) FROM DUAL;      | 오후 08:57:11    |
| SELECT TO_CHAR( SYSDATE, 'MON DY, YYYY' ) FROM DUAL;     | 1월 월, 2020     |
| SELECT TO_CHAR( SYSDATE, 'YYYY-fmMM-DD DAY' ) FROM DUAL; | 2020-1-2 월요일  |
| SELECT TO_CHAR( SYSDATE, 'YYYY-MM-fmDD DAY' ) FROM DUAL; | 2020-01-2 월요일 |
| SELECT TO_CHAR( SYSDATE, 'Year, Q' ) FROM DUAL;          | Twenty Twenty, 1 |

- 'fm' 모델을 사용하면 '01' 형식이 '1' 형식으로 표현됨

```sql
SELECT	EMP_NAME AS 이름,
		TO_CHAR(HIRE_DATE, 'YYYY-MM-DD') AS 입사일
FROM 	EMPLOYEE
WHERE 	JOB_ID = 'J7';
```

| 이름 | 입사일     |
| ---- | ---------- |
| 펭하 | 2004-07-15 |

```sql
SELECT 	EMP_NAME AS 이름,
		TO_CHAR(HIRE_DATE, 'YYYY"년" MM"월" DD"일"') AS 입사일
FROM 	EMPLOYEE
WHERE 	JOB_ID = 'J7';
```

| 이름 | 입사일           |
| ---- | ---------------- |
| 펭하 | 2004년 07월 15일 |

```sql
SELECT 	EMP_NAME AS 이름,
		SUBSTR(HIRE_DATE,1,2)||'년 '||
		SUBSTR(HIRE_DATE,4,2)||'월 '||
		SUBSTR(HIRE_DATE,7,2)||'일' AS 입사일
FROM 	EMPLOYEE
WHERE 	JOB_ID = 'J7';
```

| 이름 | 입사일         |
| ---- | -------------- |
| 펭하 | 04년 07월 15일 |

- DATE 타입과 CHARACTER 타입 비교

```sql
SELECT 	EMP_NAME AS 이름,
		HIRE_DATE AS 기본입사일,
		TO_CHAR(HIRE_DATE,
		'YYYY/MM/DD HH24:MI:SS') AS 상세입사일
FROM 	EMPLOYEE
WHERE 	JOB_ID IN ('J1','J2');
```

| 이름 | 기본입사일 | 상세입사일           |
| ---- | ---------- | -------------------- |
| 펭하 | 04/07/15   | 2004/07/15  00:00:00 |

- DATE 타입에 시간정보없이 날짜만 입력하면 자동으로 시간은 해당 날짜의 '00시 00분 00초'로 저장

```sql
SELECT 	EMP_NAME
FROM 	EMPLOYEE
WHERE 	HIRE_DATE = '04/04/30';
```

| EMP_NAME |      |
| -------- | ---- |
| 펭하     |      |

- 시간 정보가 없는 경우에는 기본날짜 형식으로 비교 가능

```sql
SELECT	EMP_NAME
FROM 	EMPLOYEE
WHERE 	HIRE_DATE = '90/04/01';
```

| EMP_NAME |      |
| -------- | ---- |
|          |      |

- 시간 정보가 있는 경우에는 기본 날짜 형식으로는 비교 불가능

#### 데이터 타입 변환 함수 TO_DATE

- CHARACTER 타입을 DATE 타입으로 변환하는 함수

| 입력 타입 | 구문                                  | 반환 타입 |
| --------- | ------------------------------------- | --------- |
| CHARACTER | **TO_DATE** ( input_type [,format ] ) | DATE      |

**[함수 설명]**

- 기본 날짜 형식이 아닌 문자열을 DATE 타입으로 인식시켜야 하는 경우
  - 2010-01-04 : 기본 날짜 형식이 아니므로 문자열로 인식
- DATE 타입과 비교하는 경우
  - HIRE_DATE = '90/04/01 13:30:30'
- 제공되는 날짜 표현 형식을 이용

##### 데이터 타입 변환 함수 TO_DATE 사용 예

| 구문                                                         | 결과                     |
| ------------------------------------------------------------ | ------------------------ |
| SELECT TO_DATE( '20100101', 'YYYYMMDD') FROM DUAL;           | 10/01/01                 |
| SELECT TO_CHAR( '20100101', 'YYYY, MON') FROM DUAL;          | N/A(오류)                |
| SELECT TO_CHAR( TO_DATE( '20100101', 'YYYYMMDD'),<br/>                                  'YYYY, MON') FROM DUAL; | 2010, 1월                |
| SELECT TO_DATE( '041030 143000', 'YYMMDD HH24MISS' ) FROM DUAL; | 04/10/30                 |
| SELECT TO_CHAR( TO_DATE( '041030 143000', 'YYMMDD HH24MISS' ),<br/>                                 'DD-MON-YY HH:MI:SS PM' ) FROM DUAL; | 30-10월-04 02:30:00 오후 |
| SELECT TO_DATE( '980630', 'YYMMDD' ) FROM DUAL;              | 98/06/30                 |
| SELECT TO_CHAR( TO_DATE( '980630', 'YYMMDD' ),<br/>'YYYY.MM.DD') FROM DUAL; | 2098.06.30               |

- 98년을 2000년대로 인식해서 1998년으로하려면 다른 과정을 거쳐야 한다.

```sql
SELECT 	EMP_NAME,
		HIRE_DATE
FROM 	EMPLOYEE
WHERE 	HIRE_DATE =
		TO_DATE('900401 133030','YYMMDD HH24MISS');
```

```sql
SELECT 	EMP_NAME,
		HIRE_DATE
FROM 	EMPLOYEE
WHERE 	TO_CHAR(HIRE_DATE, 'YYMMDD') = '900401';
```

| EMP_NAME | HIRE_DATE |
| -------- | --------- |
| 펭하     | 90/04/01  |

#### 데이터 타입 변환 함수 TO_DATE

- RR 날짜 형식
  - YY 날짜 형식과 유사
  - '지정한 년도'와 '현재 년도'에 따라 반환하는 '세기' 값이 달라짐
    - 여러 세기 지정 가능

|           |         | 지정한 년도(두자리) | 지정한 년도(두자리) |
| :-------: | :-----: | :-----------------: | :-----------------: |
|           |         |       0 - 49        |       50 - 99       |
| 현재 년도 | 0 - 49  |   현재 세기 반환    | **이전 세기** 반환  |
| (두자리)  | 50 - 99 | **다음 세기** 반환  |   현재 세기 반환    |

```sql
SELECT 	EMP_NAME,
		HIRE_DATE,
		TO_CHAR(HIRE_DATE, 'YYYY/MM/DD')
FROM 	EMPLOYEE
WHERE 	EMP_NAME = '한선기';
```

| EMP_NAME | HIRE_DATE | TO_CHAR(HIRE_DATE, 'YYYY/MM/DD') |
| -------- | --------- | -------------------------------- |
| 펭하     | 90/04/01  | 2090/04/01                       |

- 해당 데이터는 두 자리 년도를  YY 형식으로 사용
  - TO_DATE('90/04/01 13:30:30','YY/MM/DD HH24:MI:SS')
- YY 형식은 항상 현재 세기를 의미
  - 원래 의미 '1990' 대신 현재 세기인 '2090'으로 인식

##### 데이터 타입 변환 함수 TO_DATE 사용 예

| 현재 년도 | 지정 날짜 | RR 형식 | YY 형식 |
| :-------: | :-------: | :-----: | :-----: |
|   1995    | 95/10/27  |  1995   |  1995   |
|   1995    | 17/10/27  |  2017   |  1917   |
|   2009    | 17/10/27  |  2017   |  2017   |
|   2009    | 95/10/27  |  1995   |  2095   |

```sql
SELECT 	'2009/10/14' AS 현재,
		'95/10/27' AS 입력,
		TO_CHAR(TO_DATE('95/10/27','YY/MM/DD'),'YYYY/MM/DD') AS YY형식1,
		TO_CHAR(TO_DATE('95/10/27','YY/MM/DD'),'RRRR/MM/DD') AS YY형식2,
		TO_CHAR(TO_DATE('95/10/27','RR/MM/DD'),'YYYY/MM/DD') AS RR형식1,
		TO_CHAR(TO_DATE('95/10/27','RR/MM/DD'),'RRRR/MM/DD') AS RR형식2
FROM 	DUAL;
```

| 현재       | 입력     | YY형식1    | YY형식2    | RR형식1    | RR형식2    |
| ---------- | -------- | ---------- | ---------- | ---------- | ---------- |
| 2009/10/14 | 95/10/27 | 2095/10/27 | 2095/10/27 | 1995/10/27 | 1995/10/27 |

#### 데이터 타입 변환 함수 TO_NUMBER

- CHARACTER 타입을 NUMBER 타입으로 변환하는 함수

| 입력 타입 | 구문                                    | 반환 타입 |
| --------- | --------------------------------------- | --------- |
| CHARACTER | **TO_NUMBER** ( input_type [,format ] ) | NUMBER    |

**[함수 설명]**

- 숫자로 변환될 때 의미 있는 형태의 문자열인 경우
  - '100' -> 100 : 문자열 '100'을 숫자 100으로 변환

##### 데이터 타입 변환 함수 TO_NUMBER 사용 예

```sql
SELECT 	EMP_NAME, EMP_NO,
		SUBSTR(EMP_NO,1,6)AS 앞부분,
		SUBSTR(EMP_NO,8) AS 뒷부분,
		TO_NUMBER( SUBSTR(EMP_NO,1,6) ) + TO_NUMBER( SUBSTR(EMP_NO,8) ) AS 결과
FROM 	EMPLOYEE
WHERE 	EMP_ID = '101';
```

| EMP_NAME | EMP_NO         | 앞부분 | 뒷부분  | 결과    |
| -------- | -------------- | ------ | ------- | ------- |
| 펭하     | 621136-1006405 | 621136 | 1006405 | 1627541 |

- 결과 : 1006405 + 621136 = 1627541

