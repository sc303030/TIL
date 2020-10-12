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