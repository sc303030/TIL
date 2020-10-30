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

- 팩터 생성 방법 
  - factor(벡터)
  - factor(벡터[, levels=레벨벡터])
  - factor(벡터[, levels=레벨벡터] , ordered=TRUE)
- 팩터의 레벨 정보 추출
  - levels(팩터변수)

#### 행렬

- 2차원의 벡터
- 동일 타입의 데이터만 저장 가능
- 인덱싱
  - [행인덱싱 , 열인덱싱] 
  - [행인덱싱, ]
  - [ ,열인덱싱]
  - drop속성 : 행렬 구조 유지 여부
- 행렬 생성방법
  - matrix(data=벡터, nrow=행의 갯수, ncol=열의 갯수)
  - matrix(data=벡터, nrow=행의 갯수, ncol=열릐 갯수, byrow=TRUE)
  - rbind(벡터들)
  - cbind(벡터들)
- dim(m)
  - 행렬이 몇 차원인지 체크
  - nrow(행렬)
  - ncol(행렬)
- 그 외
  - colnames(m)
  - rownames(m)
  - rowSums(m)
  - colSums(m)
  - rowMeans(m)
  - colMeans(m)

#### 배열

- 3차원 벡터
- 동일 타입의 벡터만 저장 가능
- 인덱싱
  - [행인덱싱, 열인덱싱, 층(명) 인덱스]

#### 데이터프레임

- 2차원 구조
- 열단위로 서로 다른 타입의 데이터들로 구성 가능
- 모든 열의 데이터 개수(행의 개수)는 동일해야 함
- 데이터 프레임 생성 방법
  - data.frame(벡터..), data.frame(열이름=벡터,..)
  - data.frame(벡터들..[ , stringsAsfactors=FALSE])
  - as.data.frame(벡터 또는 행렬 등)
- 데이터 프레임 변환
  - rbind(df, 벡터)
  - cbind(df, 벡터)
- 데이터프레임의 구조 확인
  - str(df)
- 인덱싱
  - [행 인덱싱, 열인덱싱]
  - [얄 인덱싱]
  - df$컬럼이름
  - [[열인덱싱]]
  - subset(df, select=컬럼명들, subset=(조건))

#### 리스트

- 저장 가능한 데이터의 타입, 데이터 셋 종류에 제한이 없음
- 벡터, 행렬, 배열, 데이터 프레임 등의 서로 다른 구조의 데이터를 하나로 묶을 수 있는 자료 구조

- R에서는 통계 분석 결과가 리스트 구조로 제시되는 경우가 많으며 서로 다른 구조의 다수의 데이터 객체를 개별로 따로 따로 관리하는 것보다 이것들을 리스트에 담아서 관리하는 것이 편리
- list() 함수로 리스트를 생성하고, [, [[ , $ 을 통해 부분집합 뽑기
- [ :  리스트가 포함한 하위 리스트 뽑기
- [[ , $ : 하위 리스트가 포함한 원소를 추출, 계층구조 수준을 한단계 제거

```R
a <- list(
	a = 1:3,
    b = 'a string',
    c = pi,
    d = list(-1,-5)
)
```

- a[[4]] : [-1, -5]
- a[[4]]\[1] : [-1]
- a[[4]]\[[1]] : -1

#### unlist()

- 리스트 해제
- 리스트를 벡터로 변환

### R의 연산자

| 연산자                           | 기능                         |
| -------------------------------- | ---------------------------- |
| {                                | 블록 정의                    |
| (                                | 괄호기능                     |
| $                                | 성분 추출                    |
| [       [[                       | 첨자 표현                    |
| ^      **                        | 제곱 연산자                  |
| -                                | 음의 부호 연산자             |
| :                                | 연속된 데이터 정의           |
| %*%   %/%     %%                 | 행렬의 곱, 몫, 나머지 연산자 |
| *          /                     | 곱셈, 나눗셈 연산자          |
| +      -                         | 더하기, 빼기 연산자          |
| <    >    <=      >=     ==   != | 비교 연산자                  |
| !                                | 부정 연산자                  |
| &   &&   \|   \|\|               | 논리 연산자                  |
| <<-                              | 전역 할당 연산자             |
| <-    =    ->                    | 할당 연산자                  |

### R의 데이터 입출력

#### 데이터 출력 함수

- print(x,...)

  - print(출력데이터  [,옵션들])

- cat()

  - cat(.....,옵션들....)

- 예시들

  - print(100)
  - print(pi)
  - data <- '가나다'
  - print(data)
  - print(data, quote=FALSE)
  - vi <-('사과', '바나나', "포도")
  - print(vi)
  - print(vi, print.gap=10)
  - cat(100)
  - cat(100,200)
  - cat(100,200,'\n')
  - cat('aaa','bbb','ccc','ddd','\n')
  - cat(vi,'\n')
  - cat(vi, sep='-','\n')
  - print(paste('R', '은 통계분석','전용 언어입니다.'))
  - cat('R','은 통계분석','전용 언어입니다.','\n')

  - [ 지금까지 만들어진 데이터셋과 함수 저장하기]

- 모두저장 : save(list=ls(), file='all.rda')

- 읽어오기 : load('all.rda')

- 한 개 저장 : save(변수명 , file='one.rda')

### 파일에서 데이터 읽어들이기 1

- nums <- scan('sample_num.txt')
- words_ansi <- scan('sample_ansi.txt', what='')
- words_utf8 <- scan('sample_utf8', what='', encoding='UTF-8')
- lines_ansi <- readLines('sample.txt')\
- lines_utf-8 <- readLines('sample.txt', encoding='UTF-8')
- df1 <- read.csv('CSV파일 또는 CSV를 응답하는 URL')
- df2 <- read.table('일정한 단위(공백 또는 탭등)로 구성되어 있는 텍스트 파일 도는 URL')
  - 필요에 따라서 stringsAsFactors 속성 사용

### 제어문 5p



