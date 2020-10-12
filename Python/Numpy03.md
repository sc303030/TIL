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

#### 랜덤한 수

- random : 서브패키지에 난수를 발생시키는 함수 제공
- np.random.seed(0) : 난수를 예측 할 수 있음 0부터 시작해서

```python
np.random.seed(0)
np.random.rand(5)
> array([0.5488135 , 0.71518937, 0.60276338, 0.54488318, 0.4236548 ])
```

-  `seed` 가 예측했기 때문에 계속 실행하다 보면 값이 똑같다.

#### 데이터 샘플링

- 트레인과 데이터셋 나눌 때
- choice(배열, size = 샘플링 숫자, replace=T(변경하겠다), p=각 데이터가 선택될 확률)

```python
np.random.choice(5, 10, p=[0.2, 0.1, 0.2 , 0.3, 0.2]) 
>
array([3, 4, 3, 4, 4, 0, 1, 0, 4, 3], dtype=int64)
```

- `np.random.choice(5)` : arange와 같이 5까지 생성 
- 10개를 뽑고 각각의 인덱스에 가중치를 준다.

**np.random.randn() : 표준정규분포 (평균 0. 표준편차 1) 확률에서 실수만 표본**

```python
arr = np.random.randn(3,5) 
print(arr)
>
[[-0.68153298 -1.51223065  2.19689103  1.04117502 -0.03323603]
 [ 0.06564128  0.26578572  1.15184215  0.13804288 -0.14780553]
 [ 1.68382745  0.97183213  1.60767401 -0.25712774  1.80981769]]
```

- 평균이 3이고 표준편차 5인 배열을 만든다.

**np.random.randint(low, high = None, size = None) : 분포가 균일한 정수의 난수값을 리턴**

```python
np.random.randint(10, size=5)
> array([0, 3, 8, 7, 7])
```

- high를 안 주면 0과 low의 사이를 정수값으로 리턴

```python
np.random.randint(10,20, size=10)
> array([11, 18, 14, 17, 10, 14, 19, 10, 16, 14])
```

- 10에서 20사이의 10개의 값

```python
np.random.randint(10,20, size=(3,5))
>
array([[12, 14, 16, 13, 13],
       [17, 18, 15, 10, 18],
       [15, 14, 17, 14, 11]])
```

- `size` 를 튜플로 주면 2차원으로 만들 수 있다.

**np.unique()**

```python
np.unique([11,11,2,2,34,34])
> array([ 2, 11, 34])
```

- 중복되는 값을 제거하고 중복되지 않는 값을 리스트 형식으로 출력
- 정수일때만 가능

```python
arr = np.array(['a','b','b','c','a'])
index, count = np.unique(arr, return_counts = True)
print('idx : ', index)
print('count : ', count)
```

- `return_counts = True` 를 주면 몇 번 중복되는지 알 수 있다.

그럼 주사위의 눈 수 중에서 몇개가 나오는지 알고 싶다면?

나오지 않는 값도 0으로 처리해서 알고 싶다면?

**bincount() : 데이터가 없을 경우에는 카운트 값이 0 이 리턴된다.**

```python
np.bincount([1, 1, 2, 2, 3], minlength = 6)
>
array([0, 2, 2, 1, 0, 0], dtype=int64)
```

- 0~ 5까지 몇 번 중복되는지 출력해준다. 
- 데이터가 없는 숫자에는 0을 리턴한다.

