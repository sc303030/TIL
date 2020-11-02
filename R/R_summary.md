# R 정리_01

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

### 제어문 

- 주어진 명령을 수행하는데 있어서 조건에 따라 수행여부를 정하고자 하는 경우
  - 조건문 if문을 사용
- 필요한 만큼 반복 수행하려는 경우 모두 제어문 사용
  - 반복문 for, while, repeat 문을 사용

- 제어문을 적용하여 수행하려는 명령이 여려개 인 경우
  - { } 블록으로 구성

#### if 문

```R
if(조건):
	수행명령문장
```

```R
if(조건):
	수행명령문장1
else:
	수행명령문장2
```

```R
if(조건):
	수행명령문장1
else if(조건2):
	수행명령문장2
else if(조건3):
	수행명령문장3
    .
    .
	.
else:
    수행명령문장n
```

#### ifelse 함수

```R
ifelse(조건, 조건이 참일 때 명령문1, 조건이 거짓일 때 명령문2)
```

#### switch 함수

```R
switch(EXPR=수치데이터, 식1, 식2, 식3, ...)
```

```R
switch(EXPR=문자열데이터, 비교값1=식1, 비교값2=, 비교값3=,비교값4=식3 ..., 식4)
```

#### 반복문

```R
for (변수 in 데이터셋):
	반복수행명령문장
```

```R
while(조건):
	반복수행명령문장
```

```R
repeat 명령문(while (TRUE)와 동일)
```

- 적어도 한 번 이상 명령문을 실행
- 무한 루프에서 벗어나기 위한 분기문을 반드시 포함

#### 분기문

```R
break
	해당 루프(반복문)를 종료
next
	현재 반복을 종료하고 실행 위치를 다음 반복문으로 이동
```

- 반복문 내에서는 화면에 결과 출력시 출렴함수 print(), cat() 을 사용해야 함

### 함수

- R 프로그램의 주요 구성 요소로 특정 작업을 독립적으로 수행하는 프로그램 코드 집합
- 함수를 사용하여 반복적인 연산을 효과적으로 할 수 있음

**[함수 처리 과정]**

- 시작(입력) : 매개변수를 통해 아규먼트 값 받음
- 실행(연산) : 연산, 변환 등...
- 종료(출력) : 수행결과를 데이터넷으로 반환, 출력 등...

**함수 정의**

```R
함수명 <- function([매개변수]) {
    함수의 수행코드(수행명령문장들...)
    [return(리턴하려는 값)]
}
```

**함수 호출**

```R
변수명 <- 함수명()
변수명 <- 함수명(아규먼트)
함수명()
함수명(아규먼트)
```

- 호출시 함수가 정의하고 있는 매개변수(기본값이 없는) 사양에 맞춰서 아규먼트 전달
- 리턴 값이 없는 함수는 NULL 리턴
- 리턴 값은 return()이라는 함수를 호출하여 처리하며 return() 문이 생략된 경우에는 마지막으로 출력된 데이터값이 자동으로 리턴
  - 리턴함수를 사용허여 명확히 구현하는 것이 필요
- 아큐먼트의 타입을 제한하려는 경우에는 is.xxxx() 함수를 활용
- 기본값을 갖는 매개변수 선언하여 선택적으로 전달되는 아규먼트 처리 가능
- 아큐먼트의 개수와 타입을 가변적으로 처리 가능
  - 리턴값의 경우에도 선택적으로 처리 가능
- 함수 안에서 만들어진 변수는 지역변수이며, 지역변수는 함수내에서만 사용 가능
- 함수 안에서 만들어지지 않은 변수를 사용할 때는 전역 변수를 사용하는 결과가 됨
  - 전역변수에도 존재하지 않으면 오류 발생
- 함수내에서 전역변수에 값을 지정하려는 경우 대입연산자로 <<- 을 사용

**[함수의 정의와 호출 예제들]**

- f1()로 호출

```R
f1 <- function() 
```

- f2(100) 로 호출

```R
f2 <- function(num)
```

- f3(), f3(p='PYTHON'), f3('java')로 호출

```R
f3 <- function (p='A')
```

- f4(p1='abc', p2=3), f4('abc', 3) 로 호출
  - f4(5) 는 f4(p2=5) 로 호출

```R
f4 <- function (p1= 'zzz', p2) for(i in 1:p2)
```

- f5(10, 20, 30), f5(“abc”, T, 10, 20) 로 호출

```R
f5<- function(...) { print("TEST"); data <- c(...); print(length(data))}
```

- f6()
  f6(10)
  f6(10,20)
  f6(10,20,30)
  f6(10,’abc’, T, F)

```R
f6<- function(...) {
	print("수행시작")
	data<- c(...)
	for(item in data) {
	print(item)
	}
	return(length(data))
}
```

- f7(10,20,30) f8(10,20,30)
  f7(10,20,"test", 30,40)

```R
f7<- function(...) {
	data<- c(...)
	sum<- 0;
	for(item in data) {
	if(is.numeric(item))
	sum<- sum + item
	else
	print(item)
	}
	return(sum)
}
```

- f8(10,20,30)

​       f8(10,20,"test", 30,40)

```R
f8<- function(...) {
	data<- list(...)
	sum<- 0;
	for(item in data) {
	if(is.numeric(item))
	sum<- sum + item
	else
	print(item)
	}
	return(sum)
}
```

### apply 계열 함수

- 벡터, 행렬 또는 데이터 프레임에 임의의 함수를 적용한 결과를 얻기 위한 함수

- 데이터 전체에 함수를 한 번에 적용하는 벡터 연산을 수행하므로 속도도 빠르고 구현도 간단

|   함수    |                             설명                             |           다른 함수와 비교했을 때의 특징           |
| :-------: | :----------------------------------------------------------: | :------------------------------------------------: |
| apply( )  | 배열 또는 행렬에 주어진 함수를 적용한 뒤 그 결과를 벡터, 배열 또는 리스트로 반환 |               배열 또는 행렬에 적용                |
| lapply( ) | 벡터, 리스트 또는 표현식에 함수를 적용하여 그 결과를 리스트로 반환 |                   결과가 리스트                    |
| sapply( ) |   lapply와 유사하나 결과를 가능한 심플한 데이터셋으로 반환   |                결과가 심플데이터셋                 |
| tapply( ) | 벡터에 있는 데이터를 특정 기준에 따라 그룹으로 묶은 뒤 각 그룹마다 주어진 함수를 적용하고 그 결과를 반환 |       데이터를 그룹으로 묶은 뒤 함수를 적용        |
| mapply( ) | sapply의 확장된 버전으로, 여러 개의 벡터 또는 리스트를 인자로 받아 함수에 각 데이터의 첫째 요소들을 적용한 결과, 둘째 요소들을 적용한 결과, 셋째 요소들을 적용한 결과 등을 반환 | 여러 데이터셋의 데이터를 함수의 인자로 적용한 결과 |

#### apply()

- 행렬의 행 또는 열 방향으로 특정 함수를 적용하는 데 사용
- 배열 또는 행렬에 함수 FUN을 MARGIN 방향으로 적용하여 결과를 벡터, 배열 또는 리스트로 반환

```R
apply(
	X, # 배열 또는 행렬
	MARGIN, # 함수를 적용하는 방향. 1은 행 방향, 2는 열 방향
	FUN # 적용할 함수
)
```

- 반환 값은 FUN이 길이 1인 벡터들을 반환한 경우 벡터, 1보다 큰 벡터들을 반환한 경우 행렬, 서로 다른 길이의 벡터를 반환한 경우 리스트
- apply( )가 적용된 결과가 벡터, 배열, 리스트 중 어떤 형태로 반환될 것인지는 데이터 X의 데이터 타입과 함수 FUN의 반환 값에 따라 대부분 자연스럽게 예상가능

**[sum( )을 apply( )에 적용]**

```R
> d <- matrix(1:9, ncol=3)
> d
> apply(d, 1, sum)
[1] 12 15 18
> apply(d, 2, sum)
[1] 6 15 24
```

### 날짜와 시간관련 함수들

- 현재날짜: Sys.Date()
- 현재날짜 및 시간:Sys.time()
- 미국식 날짜 및 시간: date()
- 년월일 시분초 타입의 문자열을 시간으로 변경

```R
as.Date("년-월-일 시:분:초") 또는 as.Date("년/월/일 시:분:초")
```

- 특정 포맷을 이용한 날짜 및 시간

```R
as.Date("날짜 및 시간 문자열", format="포맷")
```

- 날짜 데이터끼리 연산 가능
  - 날짜끼리 뺄셈가능, 날짜와 정수의 덧셈뺄셈 가능
    - 하루를 1로 간주, 소숫점 생략
  - 날짜 데이터끼리 연산할 때 소숫점을 표현하고자 하는 경우는 as.Date 대신에 as.POSIXct 함수를 이용

```R
(today <- Sys.Date())
format(today, "%d %B %Y")
weekdays(today); months(today); quarters(today)
unclass(today) # 1970-01-01을 기준으로 얼마나 날짜가 지났지는지의 값
Sys.Date();Sys.time()
Sys.timezone()
as.Date('1/15/2018',format='%m/%d/%Y')
as.Date('4월 26, 2018',format='%B %d, %Y')
as.Date('22118',format='%d%b%y')
```

```R
x1 <- "2018-06-01"
as.Date(x1, "%Y-%m-%d")
strptime(x1, "%Y-%m-%d")
x2 <- "20130601"
as.Date(x2, "%Y%m%d")
strptime(x2, "%Y%m%d")
```

```R
as.Date("2018/01/01 08:00:00") - as.Date("2018/01/01 05:00:00")
as.POSIXct("2018/01/01 08:00:00") - as.POSIXct("2018/01/01 05:00:00")
as.POSIXlt("2018/01/01 08:00:00") - as.POSIXlt("2018/01/01 05:00:00")
t<-Sys.time()
ct<-as.POSIXct(t); lt<-as.POSIXlt(t)
unclass(ct); unclass(lt); lt$mon; lt$hour; lt$year+1900
as.POSIXct(1449894437,origin="1970-01-01")
as.POSIXlt(1449894437,origin="1970-01-01")
```

| Code |               Value               |
| :--: | :-------------------------------: |
|  %d  | Day of the month (decimal number) |
|  %m  |      Month (decimal number)       |
|  %b  |        Month (abbreviated)        |
|  %B  |         Month (full name)         |
|  %y  |          Year (2 digit)           |
|  %Y  |          Year (4 digit)           |

### 데이터 전처리(Preprocessing) - dplyr 패키지

```R
install.packages('dplyr')
library(dplyr)
```

|    함수     |       기능        |
| :---------: | :---------------: |
|  filter()   |      행 추출      |
|  select()   |   열(변수) 추출   |
|  arrange()  |       정렬        |
|  mutate()   |     변수 추가     |
| summarise() |    통계치 산출    |
| group_by()  |  집단별로 나누기  |
| left_join() | 데이터 합치기(열) |
| bind_rows() | 데이터 합치기(행) |

#### 조건에 맞는 데이터만 추출하기

```R
sample <- read.csv("csv_sample.csv")
# exam에서 class가 1인 경우만 추출하여 출력 
sample %>% filter(class == 1) # [참고] 단축키 [Ctrl+Shit+M]으로 %>% 기호 입력
# 2반인 경우만 추출 
sample %>% filter(class == 2)
# 1반이 아닌 경우 
sample %>% filter(class != 1)
# 3반이 아닌 경우 
sample %>% filter(class != 3)
# 수학 점수가 50점을 초과한 경우 
sample %>% filter(math > 50)
# 수학 점수가 50점 미만인 경우 
sample %>% filter(math < 50)
```

```R
# 영어점수가 80점 이상인 경우 
sample %>% filter(english >= 80)
# 영어점수가 80점 이하인 경우 
sample %>% filter(english <= 80)
# 1반 이면서 수학 점수가 50점 이상인 경우 
sample %>% filter(class == 1 & math >= 50)
# 2반 이면서 영어점수가 80점 이상인 경우 
sample %>% filter(class == 2 & english >= 80)
# 수학 점수가 90점 이상이거나 영어점수가 90점 이상인 경우 
sample %>% filter(math >= 90 | english >= 90)
# 영어점수가 90점 미만이거나 과학점수가 50점 미만인 경우 
sample %>% filter(english < 90 | science < 50)
```

```R
# 목록에 해당되는 행 추출하기
sample %>% filter(class == 1 | class == 3 | class == 5) # 1, 3, 5 반에 해당되면 추출
# %in% 연산자 이용하기
sample %>% filter(class %in% c(1,3,5)) # 1, 3, 5 반에 해당하면 추출
# 추출한 행으로 데이터 만들기
class1 <- sample %>% filter(class == 1) # class가 1인 행 추출, class1에 할당 
class2 <- sample %>% filter(class == 2) # class가 2인 행 추출, class2에 할당 
mean(class1$math) # 1반 수학 점수 평균 구하기
mean(class2$math) # 2반 수학 점수 평균 구하기
```

#### 필요한 변수만 추출하기

```R
sample %>% select(math) # math 추출
sample %>% select(english) # english 추출
```

```R
# 여러 변수 추출하기
sample %>% select(class, math, english) # class, math, english 변수 추출
# 변수 제외하기
sample %>% select(-math) # math 제외
sample %>% select(-math, -english) # math, english 제외
# dplyr 함수 조합하기
# class가 1인 행만 추출한 다음 english 추출 
sample %>% filter(class == 1) %>% select(english)
# 가독성 있게 줄 바꾸기
sample %>% 
	filter(class == 1) %>%  # class가 1인 행 추출 
		select(english) # english 추출
# 일부만 출력하기
sample %>% 
	select(id, math) %>% # id, math 추출 
		head # 앞부분 6행까지 추출
# 일부만 출력하기
sample %>% 
	select(id, math) %>% # id, math 추출
		head(10) # 앞부분 10행까지 추출
```

#### 순서대로 정렬하기

```R
# 오름차순으로 정렬하기
sample %>% arrange(math) # math 오름차순 정렬
# 내림차순으로 정렬하기
sample %>% arrange(desc(math)) # math 내림차순 정렬
# 정렬 기준 변수 여러개 지정
sample %>% arrange(class, math) # class 및 math 오름차순 정렬
```

#### 파생변수 추가하기

```R
sample %>% 
	mutate(total = math + english + science) %>% # 총합 변수 추가 
		head # 일부 추출
```

```R
#여러 파생변수 한 번에 추가하기
exam %>% 
	mutate(total = math + english + science, mean = (math + english + science)/3) %>% # 총합 변수 추가, # 총평균 변수 추가 
		head # 일부 추출
```

```R
# mutate()에 ifelse() 적용하기
exam %>% 
	mutate(test = ifelse(science >= 60, "pass", "fail")) %>% 
		head
```

```R
#추가한 변수를 dplyr 코드에 바로 활용하기
exam %>% 
	mutate(total = math + english + science) %>% # 총합 변수 추가 
		arrange(total) %>% # 총합 변수 기준 정렬 
			head # 일부 추출
```

#### 집단별로 요약하기

```R
# 전체 요약하기
exam %>% 
	summarise(mean_math = mean(math)) # math 평균 산출
```

```R
# 집단별로 요약하기
exam %>% 
	group_by(class) %>% # class별로 분리 
		summarise(mean_math = mean(math)) # math 평균 산출
```

```R
#여러 요약통계량 한 번에 산출하기
exam %>% 
	group_by(class) %>% # class별로 분리 
		summarise(mean_math = mean(math),sum_math = sum(math), median_math = median(math), n = n())  # math 평균,  # math 합계,  # math 중앙값, # 학생 수
```

```R
# 각 집단별로 다시 집단 나누기
mpg %>% 
	group_by(manufacturer, drv) %>% # 회사별, 구방방식별 분리 
		summarise(mean_cty = mean(cty)) %>% # cty 평균 산출 
			head(10) # 일부 출력
```

**[자주 사용하는 요약통계량 함수]**

| 함수     | 의미     |
| -------- | -------- |
| mean()   | 평균     |
| sd()     | 표준편차 |
| sum()    | 합계     |
| median() | 중앙값   |
| min()    | 최솟값   |
| max()    | 최댓값   |
| n()      | 빈도     |

#### 데이터 합치기

##### 가로로 합치기

```R
# 중간고사 데이터 생성 
test1 <- data.frame(id = c(1, 2, 3, 4, 5), midterm = c(60, 80, 70, 90, 85)) 
# 기말고사 데이터 생성 
test2 <- data.frame(id = c(1, 2, 3, 4, 5), final = c(70, 83, 65, 95, 80))
```

```R
# id 기준으로 합치기
exam <- left_join(test1, test2, by = "id") # id 기준으로 합쳐 total에 할당
```

- by에 변수명을 지정할 때 변수명 앞 뒤에 겹따옴표 입력

```R
# 다른 데이터 활용해 변수 추가하기
# 반별 담임교사 명단 생성
name <- data.frame(class = c(1, 2, 3, 4, 5), teacher = c("peng", "soo", "park", "choi", "jung"))
```

```R
# class 기준 합치기
exam_new <- left_join(exam, name, by = "class")
```

##### 세로로 합치기

```R
# 학생 1~5번 시험 데이터 생성 
group_a <- data.frame(id = c(1, 2, 3, 4, 5), test = c(60, 80, 70, 90, 85))
```

```R
# 학생 6~10번 시험 데이터 생성 
group_b <- data.frame(id = c(6, 7, 8, 9, 10), test = c(70, 83, 65, 95, 80))
```

```R
#세로로 합치기
group_all <- bind_rows(group_a, group_b) # 데이터 합쳐서 group_all에 할당
```

#### 정리하기

```R
# 1.조건에 맞는 데이터만 추출하기 
exam %>% filter(english >= 80)
```

```R
# 여러 조건 동시 충족 
exam %>% filter(class == 1 & math >= 50)
```

```R
# 여러 조건 중 하나 이상 충족 
exam %>% 
	filter(math >= 90 | english >= 90) 
exam %>% 
	filter(class %in% c(1,3,5))
```

```R
# 2.필요한 변수만 추출하기 
exam %>% 
	select(math) 
exam %>% 
	select(class, math, english)
```

```R
# 3.함수 조합하기, 일부만 출력하기 
exam %>% 
	select(id, math) %>% 
		head(10)
```

```R
# 4.순서대로 정렬하기 
exam %>% 
	arrange(math) # 오름차순 정렬
exam %>% 
	arrange(desc(math)) # 내림차순 정렬 
exam %>% 
	arrange(class, math) # 여러 변수 기준 오름차순 정렬
```

```R
# 5.파생변수 추가하기 
exam %>% 
	mutate(total = math + english + science)
```

```R
# 여러 파생변수 한 번에 추가하기 
exam %>% 
	mutate(total = math + english + science, mean = (math + english + science)/3)
```

```R
# mutate()에 ifelse() 적용하기 
exam %>% 
	mutate(test = ifelse(science >= 60, "pass", "fail"))
```

```R
# 추가한 변수를 dplyr 코드에 바로 활용하기 
exam %>% 
	mutate(total = math + english + science) %>% 
		arrange(total)
```

```R
# 6.집단별로 요약하기 
exam %>% 
	group_by(class) %>% 
		summarise(mean_math = mean(math))
```

```R
# 각 집단별로 다시 집단 나누기 
mpg %>% 
	group_by(manufacturer, drv) %>% 
		summarise(mean_cty = mean(cty))
```

```R
# 7.데이터 합치기 
# 가로로 합치기 
total <- left_join(test1, test2, by = "id")
```

```R
# 세로로 합치기 
group_all <- bind_rows(group_a, group_b)
```

