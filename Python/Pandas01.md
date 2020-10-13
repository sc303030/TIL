# Pandas_01

- 분석하려는 데이터는 대부분 시계열(Serise) 이거나 표(table) 형태로 정의해야 한다.
- Serise 클래스와 DataFrame 클래스를 제공한다.

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
```

### Series 클래스

- 넘파이의 1차원배열과 비슷하지만 각 데이터의 의미를 표시하는 인덱스를 붙일 수 있다.
- series = index + value
- 넘파이랑 비교해볼 때 시리즈는 = vector, 데이터프레임 = array

#### Series 와 Numpy array 비교

```python
arr = np.array([1,2,3,4,'pengsoo'], dtype=np.object)
print(arr)
print(arr.dtype)
>
[1 2 3 4 'pengsoo']
object
```

- 중간에 문자열이 들어갔기 때문에 `dtype` 를 `np.object` 로 준다.

```python
s = pd.Series([1,2,3,4], dtype=np.float64)
print(s) # 판다스의 시리즈
print(s.values) # 값만 가져오기
print(type(s.values)) 
print(s.index)
print(type(s.index))
>
0    1.0
1    2.0
2    3.0
3    4.0
dtype: float64
[1. 2. 3. 4.]
<class 'numpy.ndarray'>
RangeIndex(start=0, stop=4, step=1)
<class 'pandas.core.indexes.range.RangeIndex'>
```

- 시리즈의 값과 인덱스등을 추출할 수 있다.

```python
def seriesInfo(s):
    print('value : ',s.values) 
    print('value type : ',type(s.values)) 
    print('index : ',s.index)
    print('index type : ',type(s.index))
    print('index + value : ',s) 
```

- `arrayInfo` 처럼 시리즈 정보를 지속적으로 확인하기 위해 함수를 만든다.

```python
s = pd.Series([344234,2342300,345300,4567400],
              dtype = np.int32 , 
             index = ['서울','부산','대구','경주'])
seriesInfo(s)
>
value :  [344234  23423   3453  45674]
value type :  <class 'numpy.ndarray'>
index :  Index(['서울', '부산', '대구', '경주'], dtype='object')
index type :  <class 'pandas.core.indexes.base.Index'>
index + value :  서울    344234
부산     23423
대구      3453
경주     45674
dtype: int32
```

- 인덱스를 따로 설정할 수 있다.
  - 문자열뿐만 아니라 날짜 ,시간, 정수 등 가능하다.
  - 데이터 길이랑 맞아야한다.

```python
s.index.name = '지역별'
print(s)
>
지역별
서울    344234
부산     23423
대구      3453
경주     45674
dtype: int32
```

- 인덱스에 이름을 줄 수 있다.

```python
s / 100000
>
지역별
서울    3.44234
부산    0.23423
대구    0.03453
경주    0.45674
dtype: float64
```

- 데이터에만 영향을 주고 인덱스에는 영향이 없다.

#### Series indexing

```python
s['서울':'대구']
>
지역별
서울    344234
부산     23423
대구      3453
dtype: int32
```

- 인덱스가 이름이 있어서 이름으로 인덱싱 할 수 있다.

```python
s[['서울','대구']] 
>
지역별
서울    344234
대구      3453
dtype: int32
```

- 원하는 컬럼 이름으로 가져오려면 이렇게 키 형식으로 해야한다.

```python
'강원' in s
> False
```

#### Series 루핑

```python
for key, value in s.items():
    print('key : {}, value : {}'.format(key,value))
>
key : 서울, value : 344234
key : 부산, value : 23423
key : 대구, value : 3453
key : 경주, value : 45674
```

- 키와 벨류를 가져올 수 있다. 

#### Series 만들 때 딕셔너리로 만들기

```python
s2 = pd.Series({'c' : 1, 'b' : '5', 'a' : -8, 'k' : 10}, dtype=np.float64)
seriesInfo(s2)
>
value :  [ 1.  5. -8. 10.]
value type :  <class 'numpy.ndarray'>
index :  Index(['c', 'b', 'a', 'k'], dtype='object')
index type :  <class 'pandas.core.indexes.base.Index'>
index + value :  c     1.0
b     5.0
a    -8.0
k    10.0
dtype: float64
```

- 위의 것들과 똑같이 `key`가 인덱스로 들어간다.

```python
# Fancy Indexing & Boolean Indexing
print('fancy [0,2] indexing : {} '.format(s2[[0,2]]))
# boolean indexing 2의 배수인 것
print(s2[s2 %2 == 0])
>
fancy [0,2] indexing : c    1.0
a   -8.0
dtype: float64 
a    -8.0
k    10.0
dtype: float64
```

```python
s3 = pd.Series({'서울' : 344234,'부산' : 2342300,'인천':345300,'대전':4567400},
              dtype = np.int32 , 
             index = ['광주','대전','부산','서울'])
seriesInfo(s3) 
>
value :  [     nan 4567400. 2342300.  344234.]
value type :  <class 'numpy.ndarray'>
index :  Index(['광주', '대전', '부산', '서울'], dtype='object')
index type :  <class 'pandas.core.indexes.base.Index'>
index + value :  
광주          NaN
대전    4567400.0
부산    2342300.0
서울     344234.0
dtype: float64
```

- 딕셔너리로 키를 주고, 인덱스를 따로 주면 인덱스가 우선한다.
- 그러나 인덱스가 딕셔너리 키에 없으면 값으 `NaN` 으로 뜬다. 
- 이름을 맞춰야 인덱싱이 가능하다.

```python
diff_s = s - s2
print(diff_s)
>
a    NaN
b    NaN
c    NaN
k    NaN
경주   NaN
대구   NaN
부산   NaN
서울   NaN
dtype: float64
```

- 시리즈끼리 연산은 배열의 인덱스 기반으로 하는게 아니라 우리가 주었던 인덱스 등 그런걸로 한다.
- 이름이 같지 않아서 연산이 안 되었다.
- 인덱스가 같아야 한다.

#### A공장의 2019-01-01 부터 10일간의 생산량을 Series 저장

##### 생산량은 평균이 50이고 편차가 5인 정규분포 생성(정수)

#### B 공장의 2019-01-01 부터 10일간의 생산량을 Series 저장

##### 생산량은 평균이 70이고 편차가 8인 정규분초 생성(정수)

##### 날짜별로 모든 공장의 생산향 합계를 구한다면 ?

```python
import numpy as np
import pandas as pd
from datetime import date, datetime, timedelta
from dateutil.parser import parse
```

```python
start_day = datetime(2019,1,1) # parse('2019-01-01')이것도 가능
print(start_day)
facA = pd.Series([int(x) for x in np.random.normal(50,5,(10,))],
                index=[ start_day + timedelta(days=i) for i in range(10) ]) #이렇게 하면 array 리스트로 놓고 싶다.
print(facA)
print('*'*50)
facB = pd.Series([int(x) for x in np.random.normal(70,8,(10,))],
                index=[ start_day + timedelta(days=i) for i in range(10) ])
print(facB)
print('*'*50)
print(facA + facB)
>
2019-01-01 00:00:00
2019-01-01    53
2019-01-02    47
2019-01-03    56
2019-01-04    45
2019-01-05    58
2019-01-06    57
2019-01-07    48
2019-01-08    54
2019-01-09    52
2019-01-10    43
dtype: int64
**************************************************
2019-01-01    78
2019-01-02    62
2019-01-03    77
2019-01-04    75
2019-01-05    68
2019-01-06    67
2019-01-07    73
2019-01-08    76
2019-01-09    65
2019-01-10    67
dtype: int64
**************************************************
2019-01-01    131
2019-01-02    109
2019-01-03    133
2019-01-04    120
2019-01-05    126
2019-01-06    124
2019-01-07    121
2019-01-08    130
2019-01-09    117
2019-01-10    110
dtype: int64
```

- 평균과 표준편차 자리에 각각 주어진 값을 넣는다.
- 그 다음에 `start` 에 하루씩 더한다.

```python
np.random.normal(50,5,(10,))
print(start_day + timedelta(days=1)
>
2019-01-02 00:00:00
```

- 하루씩 증가한다.

```python
print(facA.index)
print(facB.index)
>
DatetimeIndex(['2019-01-01', '2019-01-02', '2019-01-03', '2019-01-04',
               '2019-01-05', '2019-01-06', '2019-01-07', '2019-01-08',
               '2019-01-09', '2019-01-10'],
              dtype='datetime64[ns]', freq=None)
DatetimeIndex(['2019-01-01', '2019-01-02', '2019-01-03', '2019-01-04',
               '2019-01-05', '2019-01-06', '2019-01-07', '2019-01-08',
               '2019-01-09', '2019-01-10'],
              dtype='datetime64[ns]', freq=None)
```

- 인덱스가 같은 것을 확인하였다.

```python
print(facA + facB)
>
2019-01-01    131
2019-01-02    109
2019-01-03    133
2019-01-04    120
2019-01-05    126
2019-01-06    124
2019-01-07    121
2019-01-08    130
2019-01-09    117
2019-01-10    110
dtype: int64
```

- 연산이 가능하다.

