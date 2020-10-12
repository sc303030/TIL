# Numpy_02

### reshape() _ 배열의 크기 변형

```python
import numpy as np
```

```python
def arrayinfo(ary):
    print('type : .{}'.format(type(ary)))
    print('shape : {}'.format(ary.shape))
    print('dimension : {}'.format(ary.ndim))
    print('dtype : {}'.format(ary.dtype))
    print('Array Data : \n', ary)
```

- 배열의 정보를 계속 확인하기 위해서 함수를 만들어 놓는다.

```python
arr = np.arange(0,12,1)
arrayinfo(arr)
>
type : .<class 'numpy.ndarray'>
shape : (12,)
dimension : 1
dtype : int32
Array Data : 
 [ 0  1  2  3  4  5  6  7  8  9 10 11]
```

- `arange` 는 순열, 0부터 11까지 1씩 만들어준다.

```python
arr1 = arr.reshape(4,3)
arrayinfo(arr1)
>
type : .<class 'numpy.ndarray'>
shape : (4, 3)
dimension : 2
dtype : int32
Array Data : 
 [[ 0  1  2]
 [ 3  4  5]
 [ 6  7  8]
 [ 9 10 11]]
```

- 1차원인 `arr` 을 `4행 3열` 로 변형한다.
- `2차원` 으로 바뀐다.

```python
arr1 = arr.reshape(2,2,-1) 
arrayinfo(arr1)
>
type : .<class 'numpy.ndarray'>
shape : (2, 2, 3)
dimension : 3
dtype : int32
Array Data : 
 [[[ 0  1  2]
  [ 3  4  5]]

 [[ 6  7  8]
  [ 9 10 11]]]
```

- 앞은 차원 ,행, 열이다.
  - `-1` 은 행을 `2` 로 주어줘서 알아서 거기에 맞춰서 열을 만들라는 뜻이다.

### 무조건 1차원의 배열로 변형하고 싶다면?

#### - flatten()

#### - ravel()

```python
arr = np.arange(5)
arrayinfo(arr)
>
type : .<class 'numpy.ndarray'>
shape : (5,)
dimension : 1
dtype : int32
Array Data : 
 [0 1 2 3 4]
```

- 1차원의 배열을 생성한다.

```python
arr1 = arr.reshape(1,5) 
arrayinfo(arr1)
>
type : .<class 'numpy.ndarray'>
shape : (1, 5)
dimension : 2
dtype : int32
Array Data : 
 [[0 1 2 3 4]]
```

- 배열 변형을 통해 2차원으로 행과 열을 바꾼다. 

```python
arr1 = arr.reshape(5,1) 
arrayinfo(arr1)
>
type : .<class 'numpy.ndarray'>
shape : (5, 1)
dimension : 2
dtype : int32
Array Data : 
 [[0]
 [1]
 [2]
 [3]
 [4]]
```

- 5행 1열로 변경되었다.

```python
arr1.flatten()
> array([0, 1, 2, 3, 4])
```

- `flatten()` 으로 1차원의 배열로 변경하였다.

```python
arr1.ravel()
> array([0, 1, 2, 3, 4])
```

- `revel` 로 1차원의 배열로 변경하였다.

```python
arr2 = arr.reshape(1,5).copy()
arrayinfo(arr2)
>
type : .<class 'numpy.ndarray'>
shape : (1, 5)
dimension : 2
dtype : int32
Array Data : 
 [[0 1 2 3 4]]
```

- `reshape` 는 뷰를 만드는 것이다.  물리적인 메모리를 받지 않는다. 
- `copy()` 는 새로운 array가 생긴다. 배열이라는 물리적 메모리가 생성됨

### 배열 연결(concatenate)

- hstack
- vstack
- dstack
- stack
- r_
- c_
- tile

#### hstack() : 행의 수가 같은 두 개 이상의 배열은 옆으로 연결,열의 수가 더 많은 배열을 만들때

```python
h_arr = np.ones((2,3))
arrayinfo(h_arr)
print('*'*50)
h_arr02 = np.zeros((2,2))
arrayinfo(h_arr02)
print(h_arr02)
print('*'*50)
np.hstack([h_arr, h_arr02])
>
type : .<class 'numpy.ndarray'>
shape : (2, 3)
dimension : 2
dtype : float64
Array Data : 
 [[1. 1. 1.]
 [1. 1. 1.]]
**************************************************
type : .<class 'numpy.ndarray'>
shape : (2, 2)
dimension : 2
dtype : float64
Array Data : 
 [[0. 0.]
 [0. 0.]]
[[0. 0.]
 [0. 0.]]
**************************************************
array([[1., 1., 1., 0., 0.],
       [1., 1., 1., 0., 0.]])
```

- 새로운 열을 주가한다.

#### vstack() : 열의 수가 같은 두 개 이상의 배열은 위,아래로 연결,행의 수가 더 많은 배열을 만들 때

```python
v_arr = np.ones((2,3))
arrayinfo(v_arr)
print('*'*50)
v_arr02 = np.zeros((3,3))
arrayinfo(v_arr02)
print(v_arr02)
print('*'*50)
np.vstack([v_arr, v_arr02])
>
type : .<class 'numpy.ndarray'>
shape : (2, 3)
dimension : 2
dtype : float64
Array Data : 
 [[1. 1. 1.]
 [1. 1. 1.]]
**************************************************
type : .<class 'numpy.ndarray'>
shape : (3, 3)
dimension : 2
dtype : float64
Array Data : 
 [[0. 0. 0.]
 [0. 0. 0.]
 [0. 0. 0.]]
[[0. 0. 0.]
 [0. 0. 0.]
 [0. 0. 0.]]
**************************************************
array([[1., 1., 1.],
       [1., 1., 1.],
       [0., 0., 0.],
       [0., 0., 0.],
       [0., 0., 0.]])
```

- 새로운 행을 추가한다.

#### dstack() :  맨 앞이 차원 , 행, 행을 지정했으니 열은 알아서 들어간다.

```python
v_arr = np.ones((3,4)) 
arrayinfo(v_arr)
print('*'*50)
v_arr02 = np.zeros((3,4))
arrayinfo(v_arr02)
print(v_arr02)
print('*'*50)
arrayinfo(np.dstack([v_arr, v_arr02]))
>
type : .<class 'numpy.ndarray'>
shape : (3, 4)
dimension : 2
dtype : float64
Array Data : 
 [[1. 1. 1. 1.]
 [1. 1. 1. 1.]
 [1. 1. 1. 1.]]
**************************************************
type : .<class 'numpy.ndarray'>
shape : (3, 4)
dimension : 2
dtype : float64
Array Data : 
 [[0. 0. 0. 0.]
 [0. 0. 0. 0.]
 [0. 0. 0. 0.]]
[[0. 0. 0. 0.]
 [0. 0. 0. 0.]
 [0. 0. 0. 0.]]
**************************************************
type : .<class 'numpy.ndarray'>
shape : (3, 4, 2)
dimension : 3
dtype : float64
Array Data : 
 [[[1. 0.]
  [1. 0.]
  [1. 0.]
  [1. 0.]]

 [[1. 0.]
  [1. 0.]
  [1. 0.]
  [1. 0.]]

 [[1. 0.]
  [1. 0.]
  [1. 0.]
  [1. 0.]]]
```

- 3차원의 4행 2열이 생성된다.
  - 1이 열로 쫙, 0도 열로 쫙 퍼져서 합쳐진다.

#### stack() 

```python
arrayinfo(np.stack([v_arr, v_arr02]))
>
type : .<class 'numpy.ndarray'>
shape : (2, 3, 4)
dimension : 3
dtype : float64
Array Data : 
 [[[1. 1. 1. 1.]
  [1. 1. 1. 1.]
  [1. 1. 1. 1.]]

 [[0. 0. 0. 0.]
  [0. 0. 0. 0.]
  [0. 0. 0. 0.]]]
```

- 그냥 변수가 가지고 있는 배열의 타입으로 만들어진다.

#### r_

```python
arrayinfo(np.r_[np.array([1,2,3]), np.array([4,5,6])])
>
type : .<class 'numpy.ndarray'>
shape : (6,)
dimension : 1
dtype : int32
Array Data : 
 [1 2 3 4 5 6]
```

- 인덱서(indexer), 좌우로 연결한다.

#### c_

```python
arrayinfo(np.c_[np.array([1,2,3]), np.array([4,5,6])])
>
type : .<class 'numpy.ndarray'>
shape : (3, 2)
dimension : 2
dtype : int32
Array Data : 
 [[1 4]
 [2 5]
 [3 6]]
```

- 1차원에서 차원을 추가해서 배열을 열로 결합한다.



