# R 정리

### R의 자료형

- 문자형 ( character) : 문자, 문자열
- 수치형 ( numeric)   : 정수 ( integer), 실수 (double)
- 복소수형 (complex) : 실수 + 허수
- 논리형 (logical) : 참값과 거짓값

### R의 리터럴 (literal)

- 문자형 리터럴 : '가나다', "가나다", "", '', '123', "abd"
- 수치형 리터럴 : 200, ,3.14,0
- 논리형 리터럴 : TRUE(T), FALSE(F)

- NULL : 데이터 셋이 비어있음을 의미
- NA : 데이터 셋의 내부에 존재하지 않는 값(결측치)을 의미
- NaN (Not a NUmber)  : 숫자가 아님
- Inf : 무한대값

### 타입체크 함수

- is.character(x) : 문자형 / 결측값 확인 : is.null(x)
- is.logical(x) : 논리형 / 결측값 확인 is.na(x)
- is.numeric(x) : 수치형 / 결측값 확인 : is.nan(x)
- is.double(x) : 실수형 / 결측값 확인 : is.finite(x)
- is.integer(x) : 정수형 / 결측값 확인 : is.infinite(x)

### 자동 형변환 룰

- 문자형(character) > 복소수형 (complex) > 수치형 (numeric) > 논리형 (logical)

### 강제 형변환 함수

- as.character(x)
- as.complex(x)
- as.numeric(x)
- as.double(x)
- as.integer(x)
- as.logical(x)

### 자료형 확인 함수

- class(x)

- str(x)
- mode(x)

### R의 데이터셋

- 벡터(팩터)
- 행렬
- 배열
- 데이터프레임
- 리스트

#### 백터

- R에서 다루는 가장 기초적인 데이터셋(데이터 구조)로서 1차원으로 사용됨
- 하나의 데이터 값도 벡터로 취급
- **동일 타입의 데이터**로 구성
  - 문자형(character) > 수치형 (numeric) > 논리형 (logical)
- 벡터 생성 방법
  - c()
  - seq()
  - rep()
- 미리 정의된 내장 상수 벡터
  - LETTERS
  - letters
  - month.name
  - month.abb
  - pi
- 인덱싱
  - 1부터 시작하는 인덱스값과 [인덱스] 연산자 사용
- 주요 함수
  - length() : 길이를 반환
  - names() : 인덱스의 이름을 반환
  - sort() : 정렬
  - order() : 정렬

#### 팩터

- 가능한 범주값 (level)만으로 구성되는 벡터