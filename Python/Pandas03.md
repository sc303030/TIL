

# Pandas_03

####  공백이 들어 있는 경우, 공백 제거 및 대소문자 처리

```python
empty_df = pd.DataFrame({
    
    'col01' : ['abcd     ', ' FFFaht    ', 'abCCe     '],
    'col02' : ['     fgHAij', '   fhhhij   ', 'lmnop    ']
})
empty_df
>
	col01	col02
0	abcd	fgHAij
1	FFFaht	fhhhij
2	abCCe	lmnop
```

##### **strip() : 양쪽 공백 제거**

```python
test_strip = empty_df['col01'].str.strip()
test_strip.iloc[1]
>
'FFFaht'
```

**iloc : indexlocation으로 인덱스의 번호를 찾는 것**

**lstrip() : 왼쪽 공백 제거**

```python
test_lstrip = empty_df['col01'].str.lstrip()
test_lstrip.iloc[1]
>
'FFFaht    '
```

**rstrip() : 오른쪽 공백 제거**

```python
test_rstrip = empty_df['col01'].str.rstrip()
test_rstrip.iloc[1]
>
' FFFaht'
```

**lower() : 소문자로**

```python
empty_df['col01'].str.lower()
>
0      abcd     
1     fffaht    
2     abcce     
Name: col01, dtype: object
```

**upper() : 대문자로**

```python
empty_df['col01'].str.upper()
>
0      ABCD     
1     FFFAHT    
2     ABCCE     
Name: col01, dtype: object
```

**swapcase() : 소문자는 대문자, 대문자는 소문자로**

```python
empty_df['col01'].str.swapcase()
>
0      ABCD     
1     fffAHT    
2     ABccE     
Name: col01, dtype: object
```

### 인덱싱, 데이터 조작, 인덱스 조작

- loc() : 라벨값(index) 기반의 2차원 인덱싱
- iloc() : 순서를 나타내는 정수 기반의 2차원 인덱싱

#### df.loc[]

- df.loc[행 인덱싱값]
- df.loc[행 인덱싱값, 열 인덱싱값]

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
```

```python
sample_df = pd.DataFrame(np.arange(10,22).reshape(3,4),
                        index=['a','b','c'],
                        columns=['A','B','C','D'])
sample_df
>
	A	B	C	D
a	10	11	12	13
b	14	15	16	17
c	18	19	20	21
```

#### 행에 대한 접근

```python
sample_df.loc['a']
>
A    10
B    11
C    12
D    13
Name: a, dtype: int32
```

```python
type(sample_df.loc['a'])
>
pandas.core.series.Series
```

- 인덱스를 찾으면 시리즈 형식이다.

```python
sample_df.loc['a'].values
>
array([10, 11, 12, 13])
```

```python
type(sample_df.loc['a'].values)
>
numpy.ndarray
```

- 넘파이 배열형태로 리턴한다.

```python
sample_df.loc['b' : 'c']
>
	A	B	C	D
b	14	15	16	17
c	18	19	20	21
```

```python
sample_df['b' : 'c']
>
	A	B	C	D
b	14	15	16	17
c	18	19	20	21
```

```python
sample_df.loc[['b' , 'c']]
>
	A	B	C	D
b	14	15	16	17
c	18	19	20	21
```

- 3가지는 동일안 문법이다.

#### 열에 대한 접근

```python
sample_df.A
>
a    10
b    14
c    18
Name: A, dtype: int32
```

```python
sample_df['A']
>
a    10
b    14
c    18
Name: A, dtype: int32
```

```python
type(sample_df.A)
> 
pandas.core.series.Series
```

- 2가지는 같은 결과를 리턴한다. 1차원인 시리즈를 리턴한다. 넘파이에서 vector인다.

```python
sample_df.A > 15
>
a    False
b    False
c     True
Name: A, dtype: bool
```

```python
type(sample_df.A > 15)
> pandas.core.series.Series
```

```python
sample_df.loc[sample_df.A > 15]
>
	A	B	C	D
c	18	19	20	21
```

- `True` 인 값인 행의 값을 가져온다. 

- loc를 사용할 때는 존재하는 인덱스를 사용해야한다.

- 배열 인덱스를 사용할 수 없다.

---

##### 배열 인덱스를 사용 할 수 있는 경우

```python
sample_df2 = pd.DataFrame(np.arange(10,26).reshape(4,4),
                        columns=['A','B','C','D'])
sample_df2
>
	A	B	C	D
0	10	11	12	13
1	14	15	16	17
2	18	19	20	21
3	22	23	24	25
```

```python
sample_df2.loc[1:2]
>
	A	B	C	D
1	14	15	16	17
2	18	19	20	21
```

- 인덱스가 숫자일 때ㅡ 인덱스를 따로 주지 않았다.

##### df.loc[행 인덱싱값, 열 인덱싱값]

```python
sample_df.loc['a','A']
>
10
```

```python
sample_df.loc['b':,'A']
>
b    14
c    18
Name: A, dtype: int32
```

- b와 c만 가져온다.

```python
sample_df.loc['a',:]
>
A    10
B    11
C    12
D    13
Name: a, dtype: int32
```

- a행의 모든열을 가져온다.

```python
type(sample_df.loc['a',:])
>
pandas.core.series.Series
```

```python
sample_df.loc['b':,'C':]
>
	C	D
b	16	17
c	20	21
```

```python
sample_df.loc[['b', 'c'],['C','D']]
>
	C	D
b	16	17
c	20	21
```

- 같은 결과이다. 

```python
sample_df.loc[sample_df.A >10,['C','D']]
>
	C	D
b	16	17
c	20	21
```

#### df.iloc()

```python
sample_df.iloc[0,1]
>
11
```

```python
sample_df.iloc[:,1]
>
a    11
b    15
c    19
Name: B, dtype: int32
```

```python
sample_df.iloc[0,2:]
>
C    12
D    13
Name: a, dtype: int32
```

```python
sample_df.iloc[0,-2:]
>
C    12
D    13
Name: a, dtype: int32
```

```python
sample_df.iloc[2,1:3]
>
B    19
C    20
Name: c, dtype: int32
```

```python
sample_df.iloc[-1]
>
A    18
B    19
C    20
D    21
Name: c, dtype: int32
```

- 마지막 행만 출력

```python
sample_df.iloc[-1] = sample_df.iloc[-1] * 2
sample_df
>
	A	B	C	D
a	10	11	12	13
b	14	15	16	17
c	36	38	40	42
```

- 마지막 행만 바꿔서 저장하였다. 

### 데이터 갯수를 세어보자

#### count()

- 결측치를 제외하고 출력한다.

```python
s = pd.Series(range(10))
s
>
0    0
1    1
2    2
3    3
4    4
5    5
6    6
7    7
8    8
9    9
dtype: int64
```

```python
s[5] = np.NaN
s[2] = np.NaN
s.count()
>
8
```

```python
np.random.seed(2)
count_df = pd.DataFrame(np.random.randint(5,size=(4,4)), dtype=np.float)
count_df
>
	0	1	2	3
0	0.0	0.0	3.0	2.0
1	3.0	0.0	2.0	1.0
2	3.0	2.0	4.0	4.0
3	4.0	3.0	4.0	2.0
```

```python
count_df.count()
>
0    4
1    4
2    4
3    4
dtype: int64
```

- 각 열에대한 count임

##### NaN 값 줘서 확인해보기

```python
count_df.iloc[1,0] = np.NaN
count_df.iloc[3,0] = np.NaN
count_df.iloc[2,3] = np.NaN
display(count_df)
count_df.count()
>
	0	1	2	3
0	0.0	0.0	3.0	2.0
1	NaN	0.0	2.0	1.0
2	3.0	2.0	4.0	NaN
3	NaN	3.0	4.0	2.0
0    2
1    4
2    4
3    3
dtype: int64
```

- 열에 대한 count인 것을 알 수 있다.

```
import seaborn as sns 
```

- 시각화 라이브러리이며 데이터셋을 포함하고 있다.

#### describe()

- 요약정보

```python
titanic = sns.load_dataset('titanic')
titanic.describe()
>
		survived	pclass			age			sibsp	parch			fare
count	891.000000	891.000000	714.000000	891.000000	891.000000	891.000000
mean	0.383838	2.308642	29.699118	0.523008	0.381594	32.204208
std	0.486592	0.836071	14.526497	1.102743	0.806057	49.693429
min	0.000000	1.000000	0.420000	0.000000	0.000000	0.000000
25%	0.000000	2.000000	20.125000	0.000000	0.000000	7.910400
50%	0.000000	3.000000	28.000000	0.000000	0.000000	14.454200
75%	1.000000	3.000000	38.000000	1.000000	0.000000	31.000000
max	1.000000	3.000000	80.000000	8.000000	6.000000	512.329200
```

```python
titanic.count()
>
survived       891
pclass         891
sex            891
age            714
sibsp          891
parch          891
fare           891
embarked       889
class          891
who            891
adult_male     891
deck           203
embark_town    889
alive          891
alone          891
dtype: int64
```

##### value_counts()

- 특정 열에 대하여 count 가능하다.
- 다만 시리즈만 계산 할 수 있다. 

```python
titanic['pclass'].value_counts()
>
3    491
1    216
2    184
Name: pclass, dtype: int64
```

```python
titanic['survived'].value_counts().values
>
array([549, 342], dtype=int64)
```

#### 새로운 열 추가 age_0 일괄적으로 0 할당

```
titanic['age_0'] = 0
```

```python
titanic.columns
>
Index(['survived', 'pclass', 'sex', 'age', 'sibsp', 'parch', 'fare',
       'embarked', 'class', 'who', 'adult_male', 'deck', 'embark_town',
       'alive', 'alone', 'age_0'],
      dtype='object')
```

```python
titanic.head(5)
>
survived	pclass	sex	age	sibsp	parch	fare	embarked	class	who	adult_male	deck	embark_town	alive	alone	age_0
0	0	3	male	22.0	1	0	7.2500	S	Third	man	True	NaN	Southampton	no	False	0
1	1	1	female	38.0	1	0	71.2833	C	First	woman	False	C	Cherbourg	yes	False	0
2	1	3	female	26.0	0	0	7.9250	S	Third	woman	False	NaN	Southampton	yes	True	0
3	1	1	female	35.0	1	0	53.1000	S	First	woman	False	C	Southampton	yes	False	0
4	0	3	male	35.0	0	0	8.0500	S	Third	man	True	NaN	Southampton	no	True	0
```

####  age의 각 값에 10을 곱한 age_by_10 컬럼 생성

```python
titanic['age_by_10'] = titanic['age']* 10
titanic.head()
>
survived	pclass	sex	age	sibsp	parch	fare	embarked	class	who	adult_male	deck	embark_town	alive	alone	age_0	age_by_10
0	0	3	male	22.0	1	0	7.2500	S	Third	man	True	NaN	Southampton	no	False	0	220.0
1	1	1	female	38.0	1	0	71.2833	C	First	woman	False	C	Cherbourg	yes	False	0	380.0
2	1	3	female	26.0	0	0	7.9250	S	Third	woman	False	NaN	Southampton	yes	True	0	260.0
3	1	1	female	35.0	1	0	53.1000	S	First	woman	False	C	Southampton	yes	False	0	350.0
4	0	3	male	35.0	0	0	8.0500	S	Third	man	True	NaN	Southampton	no	True	0	350.0
```

