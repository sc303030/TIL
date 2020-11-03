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

24p