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

```python
titanic_reset_index_df[titanic_reset_index_df['pclass']==3].head()
>
	index	survived	pclass	sex	age	sibsp	parch	fare	embarked	class	who	adult_male	deck	embark_town	alive	alone
1	4	0	3	male	35.0	0	0	8.0500	S	Third	man	True	NaN	Southampton	no	True
2	5	0	3	male	NaN	0	0	8.4583	Q	Third	man	True	NaN	Queenstown	no	True
4	7	0	3	male	2.0	3	1	21.0750	S	Third	child	False	NaN	Southampton	no	False
5	8	1	3	female	27.0	0	2	11.1333	S	Third	woman	False	NaN	Southampton	yes	False
7	10	1	3	female	4.0	1	1	16.7000	S	Third	child	False	G	Southampton	yes	False
```

- 3등석인 것 들만 추출해서 가져올 수 있다.

```python
titanic_reset_index_df.iloc[[4,6,8],[2,4,6]]
>
	pclass	age	parch
4	3		2.0		1
6	2		14.0	0
8	1		58.0	0
```

- 특정 행과 열만 가져올 수 있다. 

#### age > 60 이상인 정보만 추출하고 싶다면?

```python
titanic_reset_index_df[titanic_reset_index_df['age'] >= 60]
>
index	survived	pclass	sex	age	sibsp	parch	fare	embarked	class	who	adult_male	deck	embark_town	alive	alone
30	33	0	2	male	66.0	0	0	10.5000	S	Second	man	True	NaN	Southampton	no	True
51	54	0	1	male	65.0	0	1	61.9792	C	First	man	True	B	Cherbourg	no	False
93	96	0	1	male	71.0	0	0	34.6542	C	First	man	True	A	Cherbourg	no	True
113	116	0	3	male	70.5	0	0	7.7500	Q	Third	man	True	NaN	Queenstown	no	True
167	170	0	1	male	61.0	0	0	33.5000	S	First	man	True	B	Southampton	no	True
```

#### age > 60 이상인 pclass, survived, who만 추출한다면?

```python
titanic_reset_index_df[titanic_reset_index_df['age'] > 60 ].iloc[:,[1,2,10]]
>
	survived	pclass	who
30		0			2	man
51		0			1	man
93		0			1	man
113		0			3	man
```

```python
titanic_reset_index_df.loc[titanic_reset_index_df['age'] > 60,[ 'pclass', 'survived', 'who']].head()
>
		pclass	survived	who
30			2			0	man
51			1			0	man
93			1			0	man
113			3			0	man
167			1			0	man
```

```python
titanic_reset_index_df[titanic_reset_index_df['age'] > 60][[ 'pclass', 'survived', 'who']].head()
>
		pclass	survived	who
30			2			0	man
51			1			0	man
93			1			0	man
113			3			0	man
167			1			0	man
```

#### 나이 60보다크고 선실등급이 1등급이고 성별이 여자인 데이터를 추출한다면?

```python
titanic_reset_index_df[(titanic_reset_index_df['age'] > 60) & (titanic_reset_index_df['pclass'] == 1) & (titanic_reset_index_df['who'] == 'woman')]
>
index	survived	pclass	sex	age	sibsp	parch	fare	embarked	class	who	adult_male	deck	embark_town	alive	alone
272	275	1	1	female	63.0	1	0	77.9583	S	First	woman	False	D	Southampton	yes	False
826	829	1	1	female	62.0	0	0	80.0000	NaN	First	woman	False	B	NaN	yes	True
```

```python
x = titanic_reset_index_df['age'] > 60
y = titanic_reset_index_df['pclass'] == 1
z = titanic_reset_index_df['sex'] == 'female'
titanic_reset_index_df[x & y & z]
>
	index	survived	pclass	sex	age	sibsp	parch	fare	embarked	class	who	adult_male	deck	embark_town	alive	alone
272	275	1	1	female	63.0	1	0	77.9583	S	First	woman	False	D	Southampton	yes	False
826	829	1	1	female	62.0	0	0	80.0000	NaN	First	woman	False	B	NaN	yes	True
```

#### 정렬

- sort_index
- sort_values

```python
np.random.seed(100)
sort_df  = pd.DataFrame(np.random.randint(0,10,(6,4)))
sort_df
>
	0	1	2	3
0	8	8	3	7
1	7	0	4	2
2	5	2	2	2
3	1	0	8	4
4	0	9	6	2
5	4	1	5	3
```

##### 컬럼과 인덱스 넣기

```python
sort_df.columns = ['A','B','C','D']
sort_df.index = pd.date_range('20201014',periods=6)        
sort_df
>
			A	B	C	D
2020-10-14	8	8	3	7
2020-10-15	7	0	4	2
2020-10-16	5	2	2	2
2020-10-17	1	0	8	4
2020-10-18	0	9	6	2
2020-10-19	4	1	5	3
```

- 이렇게 컬럼과 인덱스에 값을 나중에 추가할 수 있다.

#### permutation() : 순열 랜덤 치환

```python
random_date = np.random.permutation(sort_df.index)
random_date
>
array(['2020-10-18T00:00:00.000000000', '2020-10-19T00:00:00.000000000',
       '2020-10-16T00:00:00.000000000', '2020-10-14T00:00:00.000000000',
       '2020-10-17T00:00:00.000000000', '2020-10-15T00:00:00.000000000'],
      dtype='datetime64[ns]')
```

#### reindex() : 인덱스 재할당

```python
sort_df2 = sort_df.reindex(index=random_date)
sort_df2
>
			A	B	C	D
2020-10-18	0	9	6	2
2020-10-19	4	1	5	3
2020-10-16	5	2	2	2
2020-10-14	8	8	3	7
2020-10-17	1	0	8	4
2020-10-15	7	0	4	2
```

```python
sort_df2 = sort_df.reindex(index=random_date, columns= ['B','A','D','C'])
sort_df2
>
			B	A	D	C
2020-10-18	9	0	2	6
2020-10-19	1	4	3	5
2020-10-16	2	5	2	2
2020-10-14	8	8	7	3
2020-10-17	0	1	4	8
2020-10-15	0	7	2	4
```

#### axis = 0 : row, axis = 1 : col

```python
sort_df2.sort_index(axis=1, ascending = False)
>
			D	C	B	A
2020-10-18	2	6	9	0
2020-10-19	3	5	1	4
2020-10-16	2	2	2	5
2020-10-14	7	3	8	8
2020-10-17	4	8	0	1
2020-10-15	2	4	0	7
```

```python
sort_df2.sort_index(axis=0)
>
			B	A	D	C
2020-10-14	8	8	7	3
2020-10-15	0	7	2	4
2020-10-16	2	5	2	2
2020-10-17	0	1	4	8
2020-10-18	9	0	2	6
2020-10-19	1	4	3	5
```

#### 특정 컬럼 값으로 행 정렬

```python
sort_df2.sort_values(by='B', ascending=False)
>
			B	A	D	C
2020-10-18	9	0	2	6
2020-10-14	8	8	7	3
2020-10-16	2	5	2	2
2020-10-19	1	4	3	5
2020-10-17	0	1	4	8
2020-10-15	0	7	2	4
```

```python
sort_df2.sort_values(by=['B','A'])
>
			B	A	D	C
2020-10-17	0	1	4	8
2020-10-15	0	7	2	4
2020-10-19	1	4	3	5
2020-10-16	2	5	2	2
2020-10-14	8	8	7	3
2020-10-18	9	0	2	6
```

- B값이 똑같으면 A로 오름차순 한다.

#### 행/열의 합을 구할 대는 sum(axis=)

```python
sort_df2.sum(axis=1)
>
2020-10-18    17
2020-10-19    13
2020-10-16    11
2020-10-14    26
2020-10-17    13
2020-10-15    13
dtype: int64
```

```python
sort_df2['row_sum'] = sort_df2.sum(axis=1)
sort_df2
>
			B	A	D	C	row_sum
2020-10-18	9	0	2	6	17
2020-10-19	1	4	3	5	13
2020-10-16	2	5	2	2	11
2020-10-14	8	8	7	3	26
2020-10-17	0	1	4	8	13
2020-10-15	0	7	2	4	13
```

- 디폴트는 행방향

```python
sort_df2.loc['col_sum', :] = sort_df2.sum(axis=0)
sort_df2
>
					B	A	D	C	row_sum	col_sum
2020-10-18 00:00:00	9.0	0.0	2.0	6.0	17.0	NaN
2020-10-19 00:00:00	1.0	4.0	3.0	5.0	13.0	NaN
2020-10-16 00:00:00	2.0	5.0	2.0	2.0	11.0	NaN
2020-10-14 00:00:00	8.0	8.0	7.0	3.0	26.0	NaN
2020-10-17 00:00:00	0.0	1.0	4.0	8.0	13.0	NaN
2020-10-15 00:00:00	0.0	7.0	2.0	4.0	13.0	NaN
col_sum	20.0	25.0	20.0	28.0	93.0	0.0
```

##### 타이타닉호 승객의 평균 나이를 구하라.

```python
titanic_reset_index_df['age'].mean()
> 29.703473980309422
```

##### 타이타닉호 승객중 여성 승객의 평균나이를 구하라

```python
titanic_reset_index_df[titanic_reset_index_df['sex']=='female']['age'].mean()
> 27.884169884169886
```

```python
female_index= titanic_reset_index_df['sex'] == 'female'
titanic_reset_index_df.loc[ female_sex, 'age' ]
> 27.884169884169886
```

##### 타이타닉호 승객중 1등실 선실의 여성 승객의 평균 나이를 구하라

```python
x = titanic_reset_index_df['sex']=='female'
y = titanic_reset_index_df['pclass'] == 1
titanic_reset_index_df[x & y]['age'].mean()
> 34.57142857142857
```

```python
pclass_index = titanic_reset_index_df['pclass'] == 1
wanted_index = female_index & pclass_index
titanic_reset_index_df.loc[wanted_index ,'age]'.mean()
> 34.57142857142857
```

#### apply 변환

- 행이나 열 단위로 복잡한 데이터 가공이 필요한 경우
- lambda 식
- apply 함수는 인자로 함수를 넘겨받을 수 있다.

```python
def get_square(a):
    return a**2
print('제곱근 : ',get_square(3) )
>
제곱근 :  9
```

#### 위 코드를 람다식으로 바꾼다면?

- **lambda** a(함수의 인자값) : a**2(함수의 바디값)\

```python
lambda_square = lambda a : a**2
print('제곱근 : ', lambda_square(3))
> 제곱근 :  9
```

- a는 함수가 받는 데이터, a**2는 리턴하는 값

```python
np.random.seed(100)
apply_df = pd.DataFrame(np.random.randint(0,10,(6,4)))
apply_df.columns=['A','B','C','D']
apply_df.index = pd.date_range('20201014',periods=6)
apply_df
>
			A	B	C	D
2020-10-14	8	8	3	7
2020-10-15	7	0	4	2
2020-10-16	5	2	2	2
2020-10-17	1	0	8	4
2020-10-18	0	9	6	2
2020-10-19	4	1	5	3
```

- 각 행의 컬럼에 대해서 최댓값 - 최솟값을 구해 새로운 column 추가
- 각 컬럼 안에서 최댓값 - 최솟값을 구해 출력

```python
func = lambda x : x.max() - x.min()
apply_df.apply(func,axis=1 )
>
2020-10-14    5
2020-10-15    7
2020-10-16    3
2020-10-17    8
2020-10-18    9
2020-10-19    4
Freq: D, dtype: int64
```

- axis=1 은 행에 대한 접근이다.

```python
apply_df['row 최대 - 최소 '] = apply_df.apply(func,axis=1 )
apply_df
>
			A	B	C	D	row 최대 - 최소
2020-10-14	8	8	3	7				5
2020-10-15	7	0	4	2				7
2020-10-16	5	2	2	2				3
2020-10-17	1	0	8	4				8
2020-10-18	0	9	6	2				9
2020-10-19	4	1	5	3				4
```

- 루프를 돌리지 않아도 추가된다. 

##### embark_town의 문자열 개수를 별도의 컬럼인 embark_len 컬럼을 추가

```python
titanic_reset_index_df["embark_len"] = titanic_reset_index_df['embark_town'].apply(lambda x : len(str(x)))
titanic_reset_index_df
>
	embark_len	embark_town
0			11	Southampton
```

- 짧은 람다식은 바로 적용해도 된다.

##### if ~ else 절을 활용하여 나이가 15세 이하면 child 그렇지 않으면 adult로 구분하는 child_adult 추가

```python
titanic_reset_index_df["child_adult"]  = titanic_reset_index_df['age'].apply(lambda x : 'child' if x < 15 else 'adult')
titanic_reset_index_df[['age',"child_adult" ]]
>
	age		child_adult
0	35.0		adult
```

- 리스트 컴프리헨션처럼 `:`  `True` 조건 `else` 값

