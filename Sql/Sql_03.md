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

13p