# R 정리_02

### 데이터 정제

- 빠진 데이터(결측치), 이상한 데이터(이상치) 제거하기

- 결측치(Missing Value) : 누락된 값, 비어있는 값
  - 함수 적용 불가, 분석 결과 왜곡
    -  제거 후 분석 실시
  - 결측치 표기 - 대문자 **NA**

```R
df <- data.frame(sex = c("M", "F", NA, "M", "F"), score = c(5, 4, 3, 4, NA))
```

#### 결측치 확인하기

```R
is.na(df) # 결측치 확인
table(is.na(df)) # 결측치 빈도 출력
```

```R
# 변수별로 결측치 확인하기
table(is.na(df$sex)) # sex 결측치 빈도 출력
table(is.na(df$score)) # score 결측치 빈도 출력
```

```R
# 결측치 포함된 상태로 분석
mean(df$score) # 평균 산출
sum(df$score) # 합계 산출
```

```R
# 결측치 있는 행 제거하기
library(dplyr) # dplyr 패키지 로드 
df %>% 
	filter(is.na(score)) # score가 NA인 데이터만 출력
df %>% 
	filter(!is.na(score)) # score 결측치 제거
```

```R
# 결측치 제외한 데이터로 분석하기
df_nomiss <- df %>% 
				filter(!is.na(score)) # score 결측치 제거 
mean(df_nomiss$score) # score 평균 산출
sum(df_nomiss$score) # score 합계 산출
```

23p