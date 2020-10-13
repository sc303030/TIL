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

