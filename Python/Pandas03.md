

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