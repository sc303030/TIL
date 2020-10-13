# Pandas_02

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
```

- 기본적인 패키지 로드

```python
def seriesInfo(s):
    print('value : ',s.values) 
    print('value type : ',type(s.values)) 
    print('index : ',s.index)
    print('index type : ',type(s.index))
    print('index + value : ',s) 
```

- 시리즈정보확인하기 위한 함수도 로드

```python
price_series = pd.Series([4000,3000,3500,2000],
                        index=['a','b','c','d'])
seriesInfo(price_series)
>
value :  [4000 3000 3500 2000]
value type :  <class 'numpy.ndarray'>
index :  Index(['a', 'b', 'c', 'd'], dtype='object')
index type :  <class 'pandas.core.indexes.base.Index'>
index + value :  a    4000
b    3000
c    3500
d    2000
dtype: int64
```

- 시리즈와 인덱스로 이루어져 있다.

### 인덱스로 데이터 추출, 변경

**라벨인덱싱**

```python
price_series['a'] = 5000
>
value :  [5000 3000 3500 2000]
value type :  <class 'numpy.ndarray'>
index :  Index(['a', 'b', 'c', 'd'], dtype='object')
index type :  <class 'pandas.core.indexes.base.Index'>
index + value :  a    5000
b    3000
c    3500
d    2000
dtype: int64
```

**배열인덱싱**

```python
price_series['0'] = 4000
seriesInfo(price_series)
>
value :  [5000 3000 3500 2000 4000]
value type :  <class 'numpy.ndarray'>
index :  Index(['a', 'b', 'c', 'd', '0'], dtype='object')
index type :  <class 'pandas.core.indexes.base.Index'>
index + value :  a    5000
b    3000
c    3500
d    2000
0    4000
dtype: int64
```

#### 값 추가

```python
price_series['e'] = 1000
seriesInfo(price_series)
>
value :  [5000 3000 3500 2000 4000 1000]
value type :  <class 'numpy.ndarray'>
index :  Index(['a', 'b', 'c', 'd', '0', 'e'], dtype='object')
index type :  <class 'pandas.core.indexes.base.Index'>
index + value :  a    5000
b    3000
c    3500
d    2000
0    4000
e    1000
dtype: int64
```

- 라벨인덱싱아나 배열인덱싱으로 값 추가 가능하다.

#### 값 삭제

```python
del price_series['e']
seriesInfo(price_series)
>
value :  [5000 3000 3500 2000 4000]
value type :  <class 'numpy.ndarray'>
index :  Index(['a', 'b', 'c', 'd', '0'], dtype='object')
index type :  <class 'pandas.core.indexes.base.Index'>
index + value :  a    5000
b    3000
c    3500
d    2000
0    4000
dtype: int64
```

- 라벨인덱싱아나 배열인덱싱으로 값 삭제 가능하다.

#### set 으로 Series 만들기

```python
set = pd.Series(list({10,20,30,40,50}) ) 
seriesInfo(set)
>
value :  [40 10 50 20 30]
value type :  <class 'numpy.ndarray'>
index :  RangeIndex(start=0, stop=5, step=1)
index type :  <class 'pandas.core.indexes.range.RangeIndex'>
index + value :  0    40
1    10
2    50
3    20
4    30
dtype: int64
```

- 순서가 있는 리스트로 캐스팅해서 만들어야 한다.

#### null 확인

```python
pd.isnull(set)
>
0    False
1    False
2    False
3    False
4    False
dtype: bool
```

- `True` 와 `False` 로 반환한다.

#### null 값 삽입

```python
set[0] = None
seriesInfo(set)
>
value :  [nan 10. 50. 20. 30.]
value type :  <class 'numpy.ndarray'>
index :  RangeIndex(start=0, stop=5, step=1)
index type :  <class 'pandas.core.indexes.range.RangeIndex'>
index + value :  0     NaN
1    10.0
2    50.0
3    20.0
4    30.0
dtype: float64
```

- `None` 를 줘서 널 값을 준다.

**널 값을 넣기 위해서는 numpy에서 제공하는 `np.NaN` 사용해야 한다.** 

```python
set[0] = np.NaN
seriesInfo(set)
>
value :  [nan 10. 50. 20. 30.]
value type :  <class 'numpy.ndarray'>
index :  RangeIndex(start=0, stop=5, step=1)
index type :  <class 'pandas.core.indexes.range.RangeIndex'>
index + value :  0     NaN
1    10.0
2    50.0
3    20.0
4    30.0
dtype: float64
```

- 이 방법이 더 낫다.
- 데이터 타입이 바뀌는 것도 확인해야 한다.

```python
ser01 = pd.Series([100,200,300,350],
                 index=['a','o','k','m'])
ser02 = pd.Series([400,200,350,450],
                 index=['o','a','h','m'])
```

```python
ser03 = ser01 + ser02
seriesInfo(ser03)
>
value :  [300.  nan  nan 800. 600.]
value type :  <class 'numpy.ndarray'>
index :  Index(['a', 'h', 'k', 'm', 'o'], dtype='object')
index type :  <class 'pandas.core.indexes.base.Index'>
index + value :  a    300.0
h      NaN
k      NaN
m    800.0
o    600.0
dtype: float64
```

**NaN을 본래의 데이터의 값으로 출력하고 싶다면?**

- 연산 `+` 대신 함수를 이용하자

```python
ser04 = ser01.add(ser02, fill_value=0)
seriesInfo(ser04)
>
value :  [300. 350. 300. 800. 600.]
value type :  <class 'numpy.ndarray'>
index :  Index(['a', 'h', 'k', 'm', 'o'], dtype='object')
index type :  <class 'pandas.core.indexes.base.Index'>
index + value :  a    300.0
h    350.0
k    300.0
m    800.0
o    600.0
dtype: float64
```

- 함수를 이용하면 본래의 값을 출력할 수 있다.

#### fillna() : 결측값을 채워넣는 함수

```python
zser = ser03.fillna(0)
seriesInfo(zser)
mser = ser03.fillna(ser03.mean())
seriesInfo(mser)
>
value :  [300.   0.   0. 800. 600.]
value type :  <class 'numpy.ndarray'>
index :  Index(['a', 'h', 'k', 'm', 'o'], dtype='object')
index type :  <class 'pandas.core.indexes.base.Index'>
index + value :  a    300.0
h      0.0
k      0.0
m    800.0
o    600.0
dtype: float64
value :  [300.         566.66666667 566.66666667 800.         600.        ]
value type :  <class 'numpy.ndarray'>
index :  Index(['a', 'h', 'k', 'm', 'o'], dtype='object')
index type :  <class 'pandas.core.indexes.base.Index'>
index + value :  a    300.000000
h    566.666667
k    566.666667
m    800.000000
o    600.000000
dtype: float64
```

- 결측값을 0으로 채우거나 평균등으로 채울 수 있다. 

#### 결측값 제거

```python
pd.notnull(ser03)
>
a     True
h    False
k    False
m     True
o     True
dtype: bool
```

- `True` 는 결측값이 아니고 `False` 가 결측값이다. 이것을 `boolean` 인덱싱으로 하면 된다.

```python
subset = ser03[pd.notnull(ser03)]
seriesInfo(subset)
>
value :  [300. 800. 600.]
value type :  <class 'numpy.ndarray'>
index :  Index(['a', 'm', 'o'], dtype='object')
index type :  <class 'pandas.core.indexes.base.Index'>
index + value :  a    300.0
m    800.0
o    600.0
dtype: float64
```

- `True` 인 값들만 뽑으면 제거가 된다.

## DataFrame

- 2차원 행렬 데이터에 인덱스를 붙인 것과 동일하다.
- 행 인덱스 열 인덱스 붙일 수 있다.

**년도에 해당하는 도시별 인구수를 정의한다면?**

```python
data = {
    '2020' : [9910293, 8384050, 2938486, 1203948],
    '2018' : [8910293, 7384050, 5938486, 3203948]
}
pop_df = pd.DataFrame(data)
pop_df
>
	   2020	   2018
0	9910293	8910293
1	8384050	7384050
2	2938486	5938486
3	1203948	3203948
```

- `2020` 과 `2018` 이 열의 인덱스로 들어간다. 

값을 추가하자.

```python
data = {
    '2020' : [9910293, 8384050, 2938486, 1203948],
    '2018' : [8910293, 7384050, 5938486, 3203948],
    '2016' : [7910293, 5384050, 7938486, 6203948],
    '2014' : [5910293, 3384050, 4938486, 4203948],
    '지역' : ['수도권', '경상권', '수도권', '경상권'],
    '증가율' : [0.2343, 0.0434, 0.0944, 0.0034] 
}
pop_df = pd.DataFrame(data)
pop_df
>
	   2020	   2018	   2016	   2014	 지역   증가율
0	9910293	8910293	7910293	5910293	수도권	0.2343
1	8384050	7384050	5384050	3384050	경상권	0.0434
2	2938486	5938486	7938486	4938486	수도권	0.0944
3	1203948	3203948	6203948	4203948	경상권	0.0034
```

**행 인덱스를 추가하자.**

```python
data = {
    '2020' : [9910293, 8384050, 2938486, 1203948],
    '2018' : [8910293, 7384050, 5938486, 3203948],
    '2016' : [7910293, 5384050, 7938486, 6203948],
    '2014' : [5910293, 3384050, 4938486, 4203948],
    '지역' : ['수도권', '경상권', '수도권', '경상권'],
    '증가율' : [0.2343, 0.0434, 0.0944, 0.0034] 
}
pop_df = pd.DataFrame(data, index=['서울', '부산', '경기', '대구'])
pop_df
>
	    2020	2018	2016	2014  지역   증가율
서울	9910293	8910293	7910293	5910293	수도권	0.2343
부산	8384050	7384050	5384050	3384050	경상권	0.0434
경기	2938486	5938486	7938486	4938486	수도권	0.0944
대구	1203948	3203948	6203948	4203948	경상권	0.0034
```

**열 순서를 변경해보자**

```python
data = {
    '2020' : [9910293, 8384050, 2938486, 1203948],
    '2018' : [8910293, 7384050, 5938486, 3203948],
    '2016' : [7910293, 5384050, 7938486, 6203948],
    '2014' : [5910293, 3384050, 4938486, 4203948],
    '지역' : ['수도권', '경상권', '수도권', '경상권'],
    '증가율' : [0.2343, 0.0434, 0.0944, 0.0034] 
}
columns = ['지역','2014','2016','2018','2020','증가율']
pop_df = pd.DataFrame(data, index=['서울', '부산', '경기', '대구'], columns = columns)
pop_df
>
	  지역	 2014	 2016	 2018	 2020  증가율
서울	수도권	5910293	7910293	8910293	9910293	0.2343
부산	경상권	3384050	5384050	7384050	8384050	0.0434
경기	수도권	4938486	7938486	5938486	2938486	0.0944
대구	경상권	4203948	6203948	3203948	1203948	0.0034
```

- 새로운 열 리스트를 만들어서 `columns` 에 적용하자.

**데이터만 가져오기**

```python
pop_df.values
>
array([['수도권', 5910293, 7910293, 8910293, 9910293, 0.2343],
       ['경상권', 3384050, 5384050, 7384050, 8384050, 0.0434],
       ['수도권', 4938486, 7938486, 5938486, 2938486, 0.0944],
       ['경상권', 4203948, 6203948, 3203948, 1203948, 0.0034]], dtype=object)
```

**컬럼 리스트 가져오기**

```python
pop_df.columns
> Index(['지역', '2014', '2016', '2018', '2020', '증가율'], dtype='object')
```

**인덱스 가져오기**

```python
pop_df.index
> Index(['서울', '부산', '경기', '대구'], dtype='object')
```

**열과 행의 인덱스에 이름 부여하기**

```python
pop_df.index.name = '도시'
pop_df.columns.name = '특성'
pop_df
>
특성	지역	2014	2016	2018	2020	증가율
도시						
서울	수도권	5910293	7910293	8910293	9910293	0.2343
부산	경상권	3384050	5384050	7384050	8384050	0.0434
경기	수도권	4938486	7938486	5938486	2938486	0.0944
대구	경상권	4203948	6203948	3203948	1203948	0.0034
```

- 행과 열에 이름이 부여되었다.

#### DataFrame 정보 확인하는 함수 만들기

```python
def dfInfo(df):
    print('df shape : {}'.format(df.shape))
    print('df size : {}'.format(df.size))
    print('df ndim : {}'.format(df.ndim))
    print('df index : {}'.format(df.index))
    print('df index  type : {}'.format(type(df.index)))
    print('df columns : {}'.format(df.columns))
    print('df columns  type : {}'.format(type(df.columns)))
    print('df values : {}'.format(df.values))
    print('df values  type : {}'.format(type(df.values)))
```

```python
dfInfo(pop_df)
>
df shape : (4, 6)
df size : 24
df ndim : 2
df index : Index(['서울', '부산', '경기', '대구'], dtype='object', name='도시')
df index  type : <class 'pandas.core.indexes.base.Index'>
df columns : Index(['지역', '2014', '2016', '2018', '2020', '증가율'], dtype='object', name='특성')
df columns  type : <class 'pandas.core.indexes.base.Index'>
df values : [['수도권' 5910293 7910293 8910293 9910293 0.2343]
 ['경상권' 3384050 5384050 7384050 8384050 0.0434]
 ['수도권' 4938486 7938486 5938486 2938486 0.0944]
 ['경상권' 4203948 6203948 3203948 1203948 0.0034]]
df values  type : <class 'numpy.ndarray'>
```

---

데이터를 프레임 형식으로 출력하고 싶으면 `print` 말고 그냥 `data` 를 실행하거나 `display(data)` 를 하면 된다.

---

```python
pop_df.T
>
도시	서울	부산	경기	대구
특성				
지역	수도권	경상권	수도권	경상권
2014	5910293	3384050	4938486	4203948
2016	7910293	5384050	7938486	6203948
2018	8910293	7384050	5938486	3203948
2020	9910293	8384050	2938486	1203948
증가율	0.2343	0.0434	0.0944	0.0034
```

- 전치행렬 가능하다.

#### 열 데이터의 갱신, 추가, 삭제

**열 추가**

```python
pop_df['2014-2016 증가율'] = ((pop_df['2016'] - pop_df['2014'] ) / pop_df['2014'] * 100).round(2)
pop_df
>
특성	지역	2014	2016	2018	2020	증가율	2014-2016 증가율
도시							
서울	수도권	5910293	7910293	8910293	9910293	0.2343			33.84
부산	경상권	3384050	5384050	7384050	8384050	0.0434			59.10
경기	수도권	4938486	7938486	5938486	2938486	0.0944			60.75
대구	경상권	4203948	6203948	3203948	1203948	0.0034			47.57
```

**열 삭제**

- 열에 대해서 배열 인덱스가 안 된다. 단, 컬럼 이름을 정수로 만들었으면 가능하다.

```python
del pop_df['2014-2016 증가율']
pop_df
>
특성	지역	2014	2016	2018	2020	증가율
도시						
서울	수도권	5910293	7910293	8910293	9910293	0.2343
부산	경상권	3384050	5384050	7384050	8384050	0.0434
경기	수도권	4938486	7938486	5938486	2938486	0.0944
대구	경상권	4203948	6203948	3203948	1203948	0.0034
```

**부분 인덱싱**

```python
pop_df[['지역','증가율']]
>
특성	지역	증가율
도시		
서울	수도권	0.2343
부산	경상권	0.0434
경기	수도권	0.0944
대구	경상권	0.0034
```

- 원하는 컬럼을 리스트로 만들어서 인덱싱 해야한다.

```python
type(pop_df[['지역','증가율']])
> pandas.core.frame.DataFrame
```

```python
test_df = pd.DataFrame(np.arange(12).reshape(3,4))
test_df
>
	0	1	2	3
0	0	1	2	3
1	4	5	6	7
2	8	9	10	11
```

```python
test_df[2]
>
0     2
1     6
2    10
Name: 2, dtype: int32
```

- 정수면 배열 인덱싱을 할 수 있다.

#### row indexing

- 항상 슬라이싱을 해야 한다.
- 인덱스, 문자라벨 슬라이싱도 가능하다.

```python
display(pop_df[:1])
display(pop_df[:'서울'])
>
특성	지역	2014	2016	2018	2020	증가율
도시						
서울	수도권	5910293	7910293	8910293	9910293	0.2343
특성	지역	2014	2016	2018	2020	증가율
도시						
서울	수도권	5910293	7910293	8910293	9910293	0.2343
```

- 0~1 인덱스 이니깐 0에 해당하는 행의 값을 출력한다.

```python
display(pop_df[1:2])
>
특성	지역	2014	2016	2018	2020	증가율
도시						
부산	경상권	3384050	5384050	7384050	8384050	0.0434
```

```python
display(pop_df[0:3])
>
특성	지역	2014	2016	2018	2020	증가율
도시						
서울	수도권	5910293	7910293	8910293	9910293	0.2343
부산	경상권	3384050	5384050	7384050	8384050	0.0434
경기	수도권	4938486	7938486	5938486	2938486	0.0944
```

```python
display(pop_df['서울':'경기'])
>
특성	지역	2014	2016	2018	2020	증가율
도시						
서울	수도권	5910293	7910293	8910293	9910293	0.2343
부산	경상권	3384050	5384050	7384050	8384050	0.0434
경기	수도권	4938486	7938486	5938486	2938486	0.0944
```

- 문자 인덱싱은 문자까지 출력한다.

#### 개별 데이터 인덱싱 ( 특정 행, 특정 열)

- 열의 행을 건드려야 한다.

```python
display(pop_df['2020']['서울'])
> 9910293
```

```python
display(pop_df[['2020']][:'서울'])
>
특성	2020
도시	
서울	9910293
```

- 데이터 프레임으로 보고 싶으면 열을 `[]` 로 감싸면 된다.

```python
score_data = {
    'kor' : [80,90,70,30],
    'eng' : [90,70,60,40],
    'math' : [90,60,90,70]
    
}
columns = ['kor', 'eng', 'math']
index   = ['펭수', '뽀로로', '뿡뿡이', '물범']

exec_df = pd.DataFrame(score_data, index=index, columns = columns)
exec_df
>
	kor	eng	math
펭수	80	90	90
뽀로로	90	70	60
뿡뿡이	70	60	90
물범	30	40	70
```

- 위 데이터를 보고 모든 학생의 수학 점수를 시리즈로 출력하라

```python
display(exec_df['math'])
type(exec_df['math'])
>
펭수     90
뽀로로    60
뿡뿡이    90
물범     70
Name: math, dtype: int64
pandas.core.series.Series
```

- 모든 학생의  국어와 영어 점수를 데이터 프레임으로 만들어라

```python
display(exec_df[['kor','eng']])
type(exec_df[['kor','eng']])
>

	kor	eng
펭수	80	90
뽀로로	90	70
뿡뿡이	70	60
물범	30	40
```

- 모든 학생의 각 과목 평균 점수를 새로운 열로 추가하라

```python
exec_df['mean'] = np.mean(exec_df[['kor', 'eng', 'math']].T)
exec_df
>
	kor	eng	math	mean
펭수	80	90	90	86.666667
뽀로로	100	70	100	90.000000
뿡뿡이	70	60	90	73.333333
물범	30	90	70	63.333333
```

- 물범 학생의 영어 점수를 90점으로 수정하고 평균 점수도 다시 계산하라

```python
exec_df['eng']['물범'] = 90
exec_df['mean'] = np.mean(exec_df[['kor', 'eng', 'math']].T)
exec_df
>
	kor	eng	math	mean
펭수	80	90	90	86.666667
뽀로로	100	70	100	90.000000
뿡뿡이	70	60	90	73.333333
물범	30	90	70	63.333333
```

- 펭수 학생의 점수를 데이터 프레임으로 만들어라

```python
display(exec_df[:'펭수'])
type(exec_df[:'펭수'])
>
	kor	eng	math	mean
펭수	80	90	90	86.6667
pandas.core.frame.DataFrame
```

- 뿡뿡이 학생의 점수를 시리즈로 출력하라

```python
exec_df.T['뿡뿡이']
>
kor     70.000000
eng     60.000000
math    90.000000
mean    73.333333
Name: 뿡뿡이, dtype: float64
```

- 뽀로로 학생의 국어점수와 수학점수를 100점으로 수정하고 평균 점수도 다시 계산하라

```python
exec_df['kor']['뽀로로'] = 100
exec_df['math']['뽀로로'] = 100
exec_df['mean'] = np.mean(exec_df[['kor', 'eng', 'math']].T)
exec_df
>
	kor	eng	math	mean
펭수	80	90	90	86.666667
뽀로로	100	70	100	90.000000
뿡뿡이	70	60	90	73.333333
물범	30	90	70	63.333333
```

## 데이터 입출력

- 매직명령어 - %%

```python
%%writefile sample01.csv
col01, col02, col03
1,1,1
2,2,2
3,3,3
4,4,4
>
Writing sample01.csv
```

- 간단히 매직 명령어로 csv 파일을 만들었다.

```python
court_df = pd.read_csv('./data/court_code.txt', sep='\t', encoding='cp949')
dfInfo(court_df)
>
df shape : (46180, 3)
df size : 138540
df ndim : 2
df index : RangeIndex(start=0, stop=46180, step=1)
df index  type : <class 'pandas.core.indexes.range.RangeIndex'>
df columns : Index(['법정동코드', '법정동명', '폐지여부'], dtype='object')
df columns  type : <class 'pandas.core.indexes.base.Index'>
df values : [[1100000000 '서울특별시' '존재']
 [1111000000 '서울특별시 종로구' '존재']
 [1111010100 '서울특별시 종로구 청운동' '존재']
 ...
 [5013032024 '제주특별자치도 서귀포시 표선면 가시리' '존재']
 [5013032025 '제주특별자치도 서귀포시 표선면 세화리' '존재']
 [5013032026 '제주특별자치도 서귀포시 표선면 토산리' '존재']]
df values  type : <class 'numpy.ndarray'>
```

**head()**

```python
court_df.head()
>
	법정동코드				법정동명	폐지여부
0	1100000000	서울특별시			    존재
1	1111000000	서울특별시 종로구		  존재
2	1111010100	서울특별시 종로구 청운동	존재
3	1111010200	서울특별시 종로구 신교동	존재
4	1111010300	서울특별시 종로구 궁정동	존재
```

- 상위 5개만 보여준다.

**tail()**

```python
court_df.tail()
>
			법정동코드	법정동명	                 폐지여부
46175	5013032022	제주특별자치도 서귀포시 표선면 하천리	존재
46176	5013032023	제주특별자치도 서귀포시 표선면 성읍리	존재
46177	5013032024	제주특별자치도 서귀포시 표선면 가시리	존재
46178	5013032025	제주특별자치도 서귀포시 표선면 세화리	존재
46179	5013032026	제주특별자치도 서귀포시 표선면 토산리	존재
```

- 하위 5개를 보여준다.

**info()**

```python
court_df.info()
>
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 46180 entries, 0 to 46179
Data columns (total 3 columns):
법정동코드    46180 non-null int64
법정동명     46180 non-null object
폐지여부     46180 non-null object
dtypes: int64(1), object(2)
memory usage: 1.1+ MB
```

- 정보를 출력해준다.

##### 1. 폐지여부가 존재인 것들만  데이터 프레임으로 만들어 보자.

```python
subset_df = court_df[court_df['폐지여부'] == '존재']
subset_df.info()
>
<class 'pandas.core.frame.DataFrame'>
Int64Index: 20544 entries, 0 to 46179
Data columns (total 3 columns):
법정동코드    20544 non-null int64
법정동명     20544 non-null object
폐지여부     20544 non-null object
dtypes: int64(1), object(2)
memory usage: 642.0+ KB
```

- 인덱스에 `True` 와 `False` 가 들어가서 `True` 값만 담는다.

#### 판다스에서 문자열 전처리를 할 때는 반드시 str

**법정동명 앞 5자리까지만 추출**

```python
subset_df['법정동명'].str[:5].head()
>
0    서울특별시
1    서울특별시
2    서울특별시
3    서울특별시
4    서울특별시
Name: 법정동명, dtype: object
```

- 5글자만 출력할 수 있다.

**법정동명 마지막 한글자만 추출**

```python
subset_df['법정동명'].str[-1].head()
>
0    시
1    구
2    동
3    동
4    동
Name: 법정동명, dtype: object
```

**split() : 공백으로 구분할 때**

```python
subset_df['법정동명'].str.split(' ').head()
>
0              [서울특별시]
1         [서울특별시, 종로구]
2    [서울특별시, 종로구, 청운동]
3    [서울특별시, 종로구, 신교동]
4    [서울특별시, 종로구, 궁정동]
Name: 법정동명, dtype: object
```

- 공백을 기준으로 리스트로 만들어준다.

**split(expane=Ture) 추가**

```python
subset_df['법정동명'].str.split(' ',expand=True).head()
>
			0	  1	      2	       3	4
0	서울특별시	None	None	None	None
1	서울특별시	종로구	None	None	None
2	서울특별시	종로구	청운동	None	None
3	서울특별시	종로구	신교동	None	None
4	서울특별시	종로구	궁정동	None	None
```

- 데이터 프레임으로 만들어 준다.

#### 서울로 시작하는 데이터만 필터링 한다면?

**startswith()**

```python
subset_df[(subset_df['법정동명'].str).startswith('서울')]
>
		법정동코드	법정동명	폐지여부
0	1100000000	서울특별시	      존재
1	1111000000	서울특별시 종로구	존재
493 rows × 3 columns
```

#### 법정동명 중 '동'으로 끝나는 것들만 출력한다면?

**endswith()**

```python
subset_df[(subset_df['법정동명'].str).endswith('동')]
>
		법정동코드	    법정동명	폐지여부
2	1111010100	서울특별시 종로구 청운동	존재
3	1111010200	서울특별시 종로구 신교동	존재
3205 rows × 3 columns
```

#### 법정동명 중 강서구를 포함하는 데이터만 필터링

**contains()**

```python
subset_df[(subset_df['법정동명'].str).contains('강서구')]
>
		법정동코드	법정동명	폐지여부
737	1150000000	서울특별시 강서구	존재
740	1150010100	서울특별시 강서구 염창동	존재
```

#### ''법정동명'에서 공백을 다른 문자(_) 대체 하고 싶다면? 

**str.replace(old,new)**

```python
subset_df['법정동명'].str.replace(' ','_')
>
0                       서울특별시
1                   서울특별시_종로구
2               서울특별시_종로구_청운동
```



# 