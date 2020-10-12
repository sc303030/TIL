# Numpy_03

## 난수 발생 및 카운팅, 통계계산 함수 소개

- count
- mean, average()
- variance(분산)
- standard deviation(표준편차)
- max, min, median, quartile
- np.random.rand : 0부터 1사이의 균일한 값을 리턴하는 분포
- np.random.randn : 정규분포
- np.random.randint : 정수의 난수를 리턴한다

```python
import numpy as  np
x = np.array([18,5,10,23,19,-8,10,0,0,5,2,15,8,2,5,4,15,-1,4,-7,-24,7,9,-6,23,13])
x
>
array([ 18,   5,  10,  23,  19,  -8,  10,   0,   0,   5,   2,  15,   8,
         2,   5,   4,  15,  -1,   4,  -7, -24,   7,   9,  -6,  23,  13])
```

```python
len(x)
> 26
```

**mean() : 표본평균** 

```python
np.mean(x)
> 5.8076923076923075
```

**var() : 분산**

```python
np.var(x)
> 104.61686390532545
```

**std() : 표준편차**

```python
np.std(x)
> 10.22823855340329
```

**max() : 최댓값**

```python
np.max(x)
> 23
```

**min() : 최솟값**

```python
np.min(x)
> -24
```

**median() : 중간값**

```python
np.median(x)
> 5.0
```

#### 사분위수

```python
np.percentile(x,0)
np.percentile(x,25)
np.percentile(x,50)
np.percentile(x,75)
np.percentile(x,100)
```

- 퍼센트를 주면 된다.
- 