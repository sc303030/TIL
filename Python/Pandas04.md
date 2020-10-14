#### 인덱스에 대한 슬라이싱 및 인덱싱

- 인덱스에서 5개만 꺼내오기

```python
titanic.index[:5].values
>
array([3, 4, 5, 6, 7], dtype=int64)
```

- 6 인덱스를 꺼내고 싶다면?

```python
titanic.index[6]
>
9
```

```python
series_fair = titanic['fare']
print('series value ',series_fair)
print('type', type(series_fair))
>
series value  3       53.1000
4        8.0500
5        8.4583
          ... 
890      7.7500
Name: fare, Length: 888, dtype: float64
type <class 'pandas.core.series.Series'>
```

#### max, min, sum

```python
print(series_fair.max())
print(series_fair.min())
print(series_fair.sum())
>
512.3292
0.0
28607.491
```

```python
print('DC 10%', series_fair * 0.9)
>
DC 10% 3       47.79000
4        7.24500
5        7.61247
6       46.67625
			...
890      6.97500
Name: fare, Length: 888, dtype: float64
```

```python
print(np.sum(series_fair))
```

- 이렇게 해도 된다. 

#### reset_index() : 새로운 인덱스를 할당하고, 기존 인덱스는 인덱스라는 새로운 컬럼명으로 추가

```python
titanic_reset_index_df = titanic.reset_index(inplace=False)
titanic_reset_index_df.head()
>
	index	survived	pclass	sex	age	sibsp	parch	fare	embarked	class	who	adult_male	deck	embark_town	alive	alone
0	3	1	1	female	35.0	1	0	53.1000	S	First	woman	False	C	Southampton	yes	False
1	4	0	3	male	35.0	0	0	8.0500	S	Third	man	True	NaN	Southampton	no	True
2	5	0	3	male	NaN	0	0	8.4583	Q	Third	man	True	NaN	Queenstown	no	True
3	6	0	1	male	54.0	0	0	51.8625	S	First	man	True	E	Southampton	no	True
4	7	0	3	male	2.0	3	1	21.0750	S	Third	child	False	NaN	Southampton	no	False
```

- `inplace` 를 주면 원본이 바뀐다. `false` 를 사용하면 새로운 변수에 다시 담아주면 된다.
  - 디폴트 값이 `false` 라 안 줘도 다시 변수에 담아야 한다.