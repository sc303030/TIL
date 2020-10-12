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
  - 열로 만들어서 열로 결합한다.

```python
arrayinfo(np.tile(np.array([[1,2,3],[4,5,6]]),(3,2)))
>
type : .<class 'numpy.ndarray'>
shape : (6, 6)
dimension : 2
dtype : int32
Array Data : 
 [[1 2 3 1 2 3]
 [4 5 6 4 5 6]
 [1 2 3 1 2 3]
 [4 5 6 4 5 6]
 [1 2 3 1 2 3]
 [4 5 6 4 5 6]]
```

- 행을 3번 반복하고 열을 2번 반복한다. 
  - [1,2,3] [1,2,3]
  - [4,5,6] [4,5,6]

### ndarray delete() 함수

- axis 기준 행과 열을 삭제 할 수 있다.
- axis 지정하지 않으면 1차원 변환 삭제
- 원본 배열을 변경하지 않고 새로운 배열 return

```python
arr = np.random.randint(0,10,(3,4))  
arrayinfo(arr)
>
type : .<class 'numpy.ndarray'>
shape : (3, 4)
dimension : 2
dtype : int32
Array Data : 
 [[2 2 8 1]
 [5 5 2 6]
 [4 2 3 2]]
```

- 0부터 10까지의 수를 무작위로 3행 4열로 생성

```python
result = np.delete(arr,1) 
arrayinfo(result)
print('*'*50)
arrayinfo(arr)
print('*'*50)
result = np.delete(arr,1,axis=0) #축을 지정하면 원래 데이터 차원에서 지워버린다.axis=0 행을 기준
arrayinfo(result)
result = np.delete(arr,1,axis=1)#축을 지정하면 원래 데이터 차원에서 지워버린다.axis=1 열을 기준
arrayinfo(result)
>
type : .<class 'numpy.ndarray'>
shape : (11,)
dimension : 1
dtype : int32
Array Data : 
 [2 8 1 5 5 2 6 4 2 3 2]
**************************************************
type : .<class 'numpy.ndarray'>
shape : (3, 4)
dimension : 2
dtype : int32
Array Data : 
 [[2 2 8 1]
 [5 5 2 6]
 [4 2 3 2]]
**************************************************
type : .<class 'numpy.ndarray'>
shape : (2, 4)
dimension : 2
dtype : int32
Array Data : 
 [[2 2 8 1]
 [4 2 3 2]]
type : .<class 'numpy.ndarray'>
shape : (3, 3)
dimension : 2
dtype : int32
Array Data : 
 [[2 8 1]
 [5 2 6]
 [4 3 2]]
```

- 축을 지정하지 않으면 2차원을 1차원으로 변환시켜서 인덱스가 1인 것을 삭제한다.

- `axis=0` 을 지정하면 행을 기준으로 삭제한다.
- `axis=1` 을 지정하면 열을 기준으로 삭제한다.

#### 배열의 연산

- vector operation (명시적으로 반복문을 사용하지 않더라도 모든 원소에 대해서 연산 가능)

```python
x = np.arange(1, 10001)
print(x)
y = np.arange(10001, 20001)
print(y)
>
[    1     2     3 ...  9998  9999 10000]
[10001 10002 10003 ... 19998 19999 20000]
```

##### 연산 시간 측정

```python
%%time
z = np.zeros_like(x)
print(z)
for i in range(10000):
    z[i] = x[i] + y[i]
print(z[:10])
>
[0 0 0 ... 0 0 0]
[10002 10004 10006 10008 10010 10012 10014 10016 10018 10020]
Wall time: 6.01 ms
```

- 루프구문을 돌려 하나씩 더하면 6.01ms 걸린다.

```python
%%time 
z = x + y
print(z[:10])
>
[10002 10004 10006 10008 10010 10012 10014 10016 10018 10020]
Wall time: 0 ns
```

- 백터연산이 가능하니깐 시간적인 측면에서 매우 효율적이다. 

#### 비교, 논리 연산도 가능하다. 단, 길이가 맞아야 연산이 가능하다.

```python
x = np.array([1,2,3,4])
y = np.array([4,2,2,4])
z = np.array([1,2,3,4])

print(x == y)
print(x >= y)
# 배열의 모든 원소가 같은지  다른지를 판단하고 싶다면?
print(np.all(x == y)) # 요소에 대해서 전체 검사해서 동일하면 Ture, 틀리면 False
print(np.all(x == z))

# 스칼라 연산
print(x * 10)
>
[False  True False  True]
[False  True  True  True]
False
True
[10 20 30 40]
```

```python
x = np.arange(12).reshape(3,4)
arrayinfo(x)
>
type : .<class 'numpy.ndarray'>
shape : (3, 4)
dimension : 2
dtype : int32
Array Data : 
 [[ 0  1  2  3]
 [ 4  5  6  7]
 [ 8  9 10 11]]
```

```python
print(x * 100) 
>
[[   0  100  200  300]
 [ 400  500  600  700]
 [ 800  900 1000 1100]]
```

- 스칼라연산이 가능하다.

#### broadcasting

- 크기가 다를 때 넘파이에서 연산이 가능하다. 이걸 브로드캐스팅이라고 한다.

```python
x = np.vstack([ range(7)[i:i+3] for i in range(5)])
arrayinfo(x)
>
type : .<class 'numpy.ndarray'>
shape : (5, 3)
dimension : 2
dtype : int32
Array Data : 
 [[0 1 2]
 [1 2 3]
 [2 3 4]
 [3 4 5]
 [4 5 6]]
```

- 리스트 컴프리헨션을 통해 바로 `vstack` 를 적용해 보았다.

- 0~6에서 i,i+3까지만 행으로 정한다.

```python
y = np.arange(5)[:, np.newaxis] 
arrayinfo(y)
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

- `np.newaxis` : 열을 추가해준다.
  - 열을 지정하지 않고 추가하는 식으로한다.

```python
x + y
>
array([[ 0,  1,  2],
       [ 2,  3,  4],
       [ 4,  5,  6],
       [ 6,  7,  8],
       [ 8,  9, 10]])
```

- 0,1,2,3,4가 x의 각각의 행과 열에 더해진다.

#### 최대/최소 : min, max, argmin, argmax

#### 통계 : sum, mean, median, std, var

```python
x = np.array([1,2,3,4])
arrayinfo(np.sum(x))
>
type : .<class 'numpy.int32'>
shape : ()
dimension : 0
dtype : int32
Array Data : 
 10
```

- 합계를 리턴한다.

```python
arrayinfo(x.sum())
>
type : .<class 'numpy.int32'>
shape : ()
dimension : 0
dtype : int32
Array Data : 
 10
```

- 이렇게 해도 합계가 리턴된다.

```python
x.min()
>1
```

```python
x.max()
> 4
```

- 최소와 최대를 리턴한다.

```python
print('최솟값인덱스 : ', x.argmin())
print('최댓값인덱스 : ', x.argmax())
print('최솟값 : ', x[x.argmin()]) 
print('최댓값 : ', x[x.argmax()])
print('중위수값 : ', np.median(x))
print('평균값 : ', np.mean(x))
print('합 : ', np.sum(x))

print('*'*50)

print(np.all([True, True, True]))  #and연산자랑 비슷하게 생각하면 된다.
print(np.any([True, True, False])) #or연산자랑 비슷하게 생각하면 된다.
>
최솟값인덱스 :  0
최댓값인덱스 :  3
최솟값 :  1
최댓값 :  4
중위수값 :  2.5
평균값 :  2.5
합 :  10
**************************************************
True
True
```

- `arg~` 은 인덱스를 반환한다. 그래서 이것을 인덱싱하면 값이 나온다.

```python
all_matrix =np.zeros((100,100), dtype=np.int)
arrayinfo(all_matrix)

print(np.all( all_matrix == all_matrix))
>
type : .<class 'numpy.ndarray'>
shape : (100, 100)
dimension : 2
dtype : int32
Array Data : 
 [[0 0 0 ... 0 0 0]
 [0 0 0 ... 0 0 0]
 [0 0 0 ... 0 0 0]
 ...
 [0 0 0 ... 0 0 0]
 [0 0 0 ... 0 0 0]
 [0 0 0 ... 0 0 0]]
True
```

- `dtype` 를 주면 타입을 정해줄 수 있다. 

```python
x_vector = np.array([1,2,3,2])
y_vector = np.array([2,2,3,2])
z_vector = np.array([6,4,4,5])

print(((x_vector <= y_vector) & (y_vector <= z_vector)).all())
> True
```

