# Pandas_06

### 그룹분석 및 피봇

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
```

#### Series Grouping

```python
df = pd.DataFrame({ "학과" : ["컴퓨터","체육교육과","컴퓨터","체육교육과","컴퓨터"],
                    "학년" : [1, 2, 3, 2, 3],
                    "이름" : ["홍길동","펭펭","최길동","펭하","신사임당"],
                    "학점" : [1.5, 4.4, 3.7, 4.5, 3.8]})
df
>
	학과	학년	이름	학점
0	컴퓨터	1	홍길동	1.5
1	체육교육과	2	펭펭	4.4
2	컴퓨터	3	최길동	3.7
3	체육교육과	2	펭하	4.5
4	컴퓨터	3	신사임당	3.8
```

**학과를 기준으로 grouping**

#### groupby()

```python
dept_series = df['학과'].groupby( df['학과'])
dept_series
>
<pandas.core.groupby.groupby.SeriesGroupBy object at 0x00000289C9DD1A58>
```

- 그룹하려는 기준을 넣어준다.

**get_group()**

```python
dept_series.get_group('컴퓨터')
>
0    컴퓨터
2    컴퓨터
4    컴퓨터
Name: 학과, dtype: object
```

- 원하는 그룹을 확인 하기위해 그룹 키워드를 입력하면 시리즈로 리턴한다.

**size()**

```python
dept_series.size()
>
학과
체육교육과    2
컴퓨터      3
Name: 학과, dtype: int64
```

- 그룹이 몇개있는지 알려준다.

```python
dept = df.groupby(df['학과'])
dept.mean()
>
				학년	학점
학과		
체육교육과	2.000000	4.45
컴퓨터	2.333333	3.00
```

- 시리즈가 인덱스로 들어갔다. 즉 value가 인덱스로 들어갔다.
- 이름은 빠져있다. 

**groups**

```python
dept.groups
>
{'체육교육과': Int64Index([1, 3], dtype='int64'),
 '컴퓨터': Int64Index([0, 2, 4], dtype='int64')}
```

- 각 그룹의 인덱스를 볼 수 있다.

```python
dept.sum()
>
			학년	학점
학과		
체육교육과	  4	   8.9
컴퓨터		   7	9.0
```

```python
df.groupby(df['학과']).sum()
>
			학년	학점
학과		
체육교육과		4	8.9
컴퓨터			7	9.0
```

- 이렇게 바로 집계함수 쓸 수 있다.

```python
df.groupby(['학과','학년']).mean()
>
				학점
학과		학년	
체육교육과	2	4.45
컴퓨터	 	 1	 1.50
		   3	3.75
```

- 여러개 컬럼을 구할 수 있다.

```python
import seaborn as sns
iris = sns.load_dataset('iris')
iris
>sepal_length	sepal_width	petal_length	petal_width	species
0	5.1	3.5	1.4	0.2	setosa
1	4.9	3.0	1.4	0.2	setos
```

```python
iris.describe()
>
sepal_length	sepal_width	petal_length	petal_width
count	150.000000	150.000000	150.000000	150.000000
mean	5.843333	3.057333	3.758000	1.199333
std	0.828066	0.435866	1.765298	0.762238
min	4.300000	2.000000	1.000000	0.100000
25%	5.100000	2.800000	1.600000	0.300000
50%	5.800000	3.000000	4.350000	1.300000
75%	6.400000	3.300000	5.100000	1.800000
max	7.900000	4.400000	6.900000	2.500000
```

#### 각 종별로 가장 큰 값과 가장 작은 값의 비율을 구한다면?

```python
def get_ratio(x):
    return x.max() / x.min()
```

- 함수를 만들어 놓는다.

```python
iris.groupby(iris.species).sum()
>
	sepal_length	sepal_width	petal_length	petal_width
species				
setosa	250.3	171.4	73.1	12.3
versicolor	296.8	138.5	213.0	66.3
virginica	329.4	148.7	277.6	101.3
```

```python
iris.groupby(iris.species).mean()
>
sepal_length	sepal_width	petal_length	petal_width
species				
setosa	5.006	3.428	1.462	0.246
versicolor	5.936	2.770	4.260	1.326
virginica	6.588	2.974	5.552	2.026
```

```python
iris.groupby(iris.species).max()
>
sepal_length	sepal_width	petal_length	petal_width
species				
setosa	5.8	4.4	1.9	0.6
versicolor	7.0	3.4	5.1	1.8
virginica	7.9	3.8	6.9	2.5
```

#### agg(집계함수)

```python
iris.groupby(iris.species).agg(get_ratio)
>
sepal_length	sepal_width	petal_length	petal_width
species				
setosa	1.348837	1.913043	1.900000	6.000000
versicolor	1.428571	1.700000	1.700000	1.800000
virginica	1.612245	1.727273	1.533333	1.785714
```

- 위에서 만든 함수를 넣으면 비율을 보여준다.
- 시리즈별로 적용된다.

```python
iris.groupby(iris.species).describe().T
>
	species	setosa	versicolor	virginica
petal_length	count	50.000000	50.000000	50.000000
mean	1.462000	4.260000	5.552000
petal_width	count	50.000000	50.000000	50.000000
mean	0.246000	1.326000	2.026000
sepal_length	count	50.000000	50.000000	50.000000
mean	5.006000	5.936000	6.588000
sepal_width	count	50.000000	50.000000	50.000000
mean	3.428000	2.770000	2.974000
```

- 전치행렬을 주면 종별로 정보 파악 가능하다.