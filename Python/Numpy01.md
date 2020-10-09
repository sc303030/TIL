# Numpy_01

```python
import numpy as np
```

- 넘파이의 배열 : 모든 요소가 같은 자료형이여야 한다

- Vector(1차원 - 배열) - pandas (Series)
- Matrix(2차원 - 행렬) - pandas (DataFrame)

- 선형대수(행렬을 이용한 연산 가능)

```python
def arrayinfo(array):
    print('type : .{}'.format(type(array)))
    print('shape : {}'.format(array.shape))
    print('dimension : {}'.format(array.ndim))
    print('dtype : {}'.format(array.dtype))
    print('Array Data : \n', array)
    
```

- 만든 배열의 정보를 확인하기 위해 함수를 만들었다. 
  - `type` 는 배열의 타입
  - `shape` 는 크기
  - `ndim` 은 차원
  - `dtype` 는 정수인지 실수인지 문자열인지 알려준다.
  - `array` 는 데이터다.

### 1차원 배열 생성

#### array()

```python
firstAry = np.array([0,1,2,3,4,5,6,7,8,9])
arrayinfo(firstAry)
```

- 배열은 리스트로 만든다. 

```
type : .<class 'numpy.ndarray'>
shape : (10,)
dimension : 1
dtype : int32
Array Data : 
 [0 1 2 3 4 5 6 7 8 9]
```

- `type`  : n차원의 dimension 배열
- `shape` 10개의 요소가 들어있다.
- `dimension` : 1차원이다.
- `dtype` : 정수이다.

#### List vs Array 차이점(Vector opertaion)

```python
data = [0,1,2,3,4,5,6,7,8,9]
data * 2
```

```
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```

- 리스트 안에 값에 2씩 곱해지는 것이 아니라 리스트가 2개가 된다. 

```python
result = []
for d in data:
    result.append(d * 2)
result
```

```
[0, 2, 4, 6, 8, 10, 12, 14, 16, 18]
```

- 이렇게 해야 `data` 값에 2씩 곱해진다. 

```python
result2 = [d * 2 for d in data]
result2
```

```
[0, 2, 4, 6, 8, 10, 12, 14, 16, 18]
```

- 리스트컴프리헨션을 이용해서 바로 변수에 저장한다. 

```python
result3 = firstAry * 2
result3
```

```
array([ 0,  2,  4,  6,  8, 10, 12, 14, 16, 18])
```

- 행렬에 대한 연산이 바로 가능하다. 
- 