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
> [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```

- 리스트 안에 값에 2씩 곱해지는 것이 아니라 리스트가 2개가 된다. 

```python
result = []
for d in data:
    result.append(d * 2)
result
> [0, 2, 4, 6, 8, 10, 12, 14, 16, 18]
```

- 이렇게 해야 `data` 값에 2씩 곱해진다. 

```python
result2 = [d * 2 for d in data]
result2
> [0, 2, 4, 6, 8, 10, 12, 14, 16, 18]
```

- 리스트컴프리헨션을 이용해서 바로 변수에 저장한다. 

```python
result3 = firstAry * 2
result3
> array([ 0,  2,  4,  6,  8, 10, 12, 14, 16, 18])
```

- 행렬에 대한 연산이 바로 가능하다. 

#### 벡터 연산은 비교, 산술, 논리 연산을 포함하는 모든 수학연사에 적용됨

```python
xArray = np.array([1,2,3])
yArray = np.array([10,20,30])
```

```python
xArray + yArray
> array([11, 22, 33])
```

- 각자의 첨자번지끼지 더해진다.

```python
2 * xArray + yArray
> array([12, 24, 36])
```

- 연산에 우선순위가 있는 것을 알 수 있다. 

```python
xArray == 2 
> array([False,  True, False])
```

- 그냥 리스트였으면 `if` 를 써야 하지만 배열이라 연산이 가능하다.

```python
yArray > 20
> array([False, False,  True])
```

```python
(xAry == 2) & (yAry > 20) 
> array([False, False, False])
```

- `True` 가 하나 있더라도 `False` 가 있으면 `False` 다. 비교 연산자가 `&` 이기 때문이다.

### 2차원 배열 생성

- ndarray(N-Dimensional Array)
- 2차원, 3차원(다차원 배열 자료구조)
- 2차원 배열은 행렬(Matrix)
- list of list
- list od list of list

**2개의 행과 3개의 열을 가지는 배열 만든다면?**

```python
secondAry = np.array([[1,2,3],[4,5,6]], dtype=np.float64)
arrayinfo(secondAry)
> 
type : .<class 'numpy.ndarray'>
shape : (2, 3)
dimension : 2
dtype : float64
Array Data : 
 [[1. 2. 3.]
 [4. 5. 6.]]
```

- `dtype` 을 주면 형태를 지정할 수 있다. 

```python
secondAry = np.array([[1,2,3],[4,5,'6']])
arrayinfo(secondAry)
>
type : .<class 'numpy.ndarray'>
shape : (2, 3)
dimension : 2
dtype : <U11
Array Data : 
 [['1' '2' '3']
 ['4' '5' '6']]
```

- `dtype` 가 `<U11` 이다. 배열중에 `'6'` 이 들어있어서 문자로 인식한다. 

**행의 개수, 열의 개수**

```python
print(len(secondAry))
print(len(secondAry[0]))
print(len(secondAry[1]))
> 
2
3
3
```

- `print(len(secondAry))` : 행의 개수를 리턴한다.
- `print(len(secondAry[0]))` : 처음 행의 개수를 리턴하니 열의 개수를 알 수 있다. 

### 3차원 배열 생성

**2차원, 3행, 4열**

```python
thirdAry = np.array([ [[1,2,3,4],
                      [5,6,7,8],
                       [9,10,11,12]],
                      [[11,12,13,14],
                      [15,16,17,18],
                      [19,20,21,22]] ]
                     )
arrayinfo(thirdAry)
>
type : .<class 'numpy.ndarray'>
shape : (2, 3, 4)
dimension : 3
dtype : int32
Array Data : 
 [[[ 1  2  3  4]
  [ 5  6  7  8]
  [ 9 10 11 12]]

 [[11 12 13 14]
  [15 16 17 18]
  [19 20 21 22]]]
```

```python
print('dept' ,len(thirdAry)) 
print('row' , len(thirdAry[0]))
print('row' , len(thirdAry[1]))
print('row[0] col' , len(thirdAry[0][0]))
>
dept 2
row 3
row 3
row[0] col 4
```

- 차원을 알 수 있고, `[0]` 은 처음 차원에 들어가서 행의 개수를 알 수 있다. `[0][0]` 을 하면 열의 개수를 알 수 있다. 

**요소의 타입을 변경할 때는 astype()**

```python
tyChange = thirdAry.astype(np.float64)
aryinfo(tyChange)
>
type : .<class 'numpy.ndarray'>
shape : (2, 3, 4)
dimension : 3
dtype : float64
Array Data : 
 [[[ 1.  2.  3.  4.]
  [ 5.  6.  7.  8.]
  [ 9. 10. 11. 12.]]

 [[11. 12. 13. 14.]
  [15. 16. 17. 18.]
  [19. 20. 21. 22.]]]
```

- `int32` 에서 형태를 `float64` 로 바꾼다. 

```python
indexArray = np.array([1,2,3,4,5,6,7])
aryinfo(indexArray)
>
type : .<class 'numpy.ndarray'>
shape : (7,)
dimension : 1
dtype : int32
Array Data : 
 [1 2 3 4 5 6 7]
```

```python
indexArray[2]
> 3
```

- 인덱싱을 할 수 있다. 

```python
indexArray[-1]
> 7
```

- 맨 뒤의 값을 인덱싱 할 수 있다. 

**secondAry**

- 첫번 째 행의 첫번 째 열

```python
print(secondAry[0,0])
> 1.0
```

- 첫번 째 행의 두번 째 열

```python
print(secondAry[0,1])
> 2.0
```

- 마지막 행의 마지막 열

```python
print(twoAry[-1,-1])
> 6.0
```

**slicingArray **

```python
slicingArray = np.array([[1,2,3,4],[5,6,7,8]])
```

- 첫번 째 행의 전체

```python
print(slicingAry[0,:])
> [1 2 3 4]
```

- 두번 째 열의 전체

```python
print(slicingAry[:,1])
> [2 6]
```

- 두번 째 행의 두번 째 열부터 끝까지

```python
print(slicingAry[1,1:])
> [6 7 8]
```

**mArray**

```python
mArray = np.array([[ 0,  1,  2,  3,  4],
            [ 5,  6,  7,  8,  9],
            [10, 11, 12, 13, 14]])
aryinfo(mArray)
>
type : .<class 'numpy.ndarray'>
shape : (3, 5)
dimension : 2
dtype : int32
Array Data : 
 [[ 0  1  2  3  4]
 [ 5  6  7  8  9]
 [10 11 12 13 14]]
```

- 행렬에서 값 7을 인덱싱한다. 

```python
print(mArray[1,2])
> 7
```

- 행렬에서 값 14를 인덱싱 한다.

```python
print(mArray[1,2])
> 14
```

- 행렬에서 배열 [6,7]을 슬라이싱 한다.

```python
print(mArray[1,1:3])
> [6 7]
```

- 행렬에서 배열 [7,12]를 인덱싱한다.

```python
print(mArray[1:3,2])
> [ 7 12]
```

- 행렬에서 배열 [ [3,4] , [8,9] ] 를 슬라이싱 한다. 

```python
print(mArray[0:2,-2:])
> [[3 4]
 [8 9]]
```

### Fancy indexing

**boolean indexing = sql의 쿼리로 생각하면 된다.**

```python
arrr = np.array([0,1,2,3,4,5,6,7,8,9])
arrayinfo(arrr)
idx = np.array([True,False,True,False,True,False,True,False,True,False ]) 
print(arrr[idx])
>
type : .<class 'numpy.ndarray'>
shape : (10,)
dimension : 1
dtype : int32
Array Data : 
 [0 1 2 3 4 5 6 7 8 9]
> [0 2 4 6 8]
```

- 짝수만 뽑는 경우다. 
  -  `index` 에 `True` 와 `False` 를 준다.
  - `arrr` 에 인덱스를 `idx` 로 줘서  `True` 값만 가져오게 하면 짝수만 가져온다. 

```python
arr % 2 == 0
> array([ True, False,  True, False,  True, False,  True, False,  True, False])
```

- 배열이라 연산이 가능하다. 

```python
arrr[arrr % 2 == 0]
> array([0, 2, 4, 6, 8])
```

- boolean 인덱싱이 가능하다. 

```python
cnidx = np.array([0,2,4,6,8])
print(arrr[cnidx]) 
```

- 배열로 인덱스를 만들어서 다시 인덱스로 줄 수 있다. 

```python
x = np.array([1, 2, 3, 4, 5, 6, 7, 8, 9, 10,
             11, 12, 13, 14, 15, 16, 17, 18, 19, 20])
```

**배열에서 3의 배수를 찾아라**

```python
print(x[x % 3 == 0])
> [ 3  6  9 12 15 18]
```

- `True` 와 `False` 값으로 인덱싱을 하는것이다. 

**배열에서 4로 나누면 1이 남는 수를 찾아라**

```python
print(x[x % 4 == 1])
> [ 1  5  9 13 17]
```

**배열에서 3으로 나누면 나누어지고 4로 나누면 1이 남는 수를 찾아라**

```python
print(x[(x % 3 == 0) & (x % 4 == 1)])
> [9]
```

**배열에 index배열을 전달하여 배열 요소를 참조해 보자**

**정수값 인덱스가 들어가서 fancy인덱싱하자**

```python
fancyArray = np.arange(0,12,1).reshape(3,4)
arrayinfo(fancyArray)
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

- `arange` : 0에서 12까지 1씩 증가시켜서 값을 만든다.
- `reshape` 를 통해 3행 4열을 만든다.

**10값을 가져온다면?**

```python
fancyArray[2,2]
> 10
```

- 스칼라 타입 인덱싱이다.

**6값을 가져온다면?**

```python
arrayinfo(fancyArray[1:2,2])
> 
type : .<class 'numpy.ndarray'>
shape : (1,)
dimension : 1
dtype : int32
Array Data : 
 [6]
```

- 배열 형식으로 리턴된다. 

```python
aryinfo(fancyArray[1:2,:])
>
type : .<class 'numpy.ndarray'>
shape : (1, 4)
dimension : 2
dtype : int32
Array Data : 
 [[4 5 6 7]]
```

- 2차원으로 리턴된다. 

```python
aryinfo(fancyArray[1:2,:1:2])
> 
type : .<class 'numpy.ndarray'>
shape : (1, 1)
dimension : 2
dtype : int32
Array Data : 
 [[4]]
```

- 2차원으로 리턴된다.

```python
fancyArray[[0,2],2]
aryinfo(fancyArray[[0,2],2:3])
>
type : .<class 'numpy.ndarray'>
shape : (2, 1)
dimension : 2
dtype : int32
Array Data : 
 [[ 2]
 [10]]
```

#### fancy인덱싱을 하는 이유는 차원에 대한 이해가 없느면 data핸들링이 어렵다.

##### 배열에서 슬라이싱 할 때 배열의 요소를 넘겨서 작업을 하는것

##### 1:2 , 2 이건 그냥 인덱스를 넘긴것

##### [0,2], 2:3 배열의 인덱스 번지를 넘긴 것

**fancyArray에서 0,2,8,10을 찾자**

```python
fancyArray[[0,2],[[0],[2]]]
> array([[ 0,  8],
       [ 2, 10]])
```

```python
rowidx = np.array([0,2])
colidx = np.array([0,2])
print(fancyArray[ [rowidx]] [:,colidx] )
> [[ 0  2]
 [ 8 10]]
```

- 행과 열을 뽑을 배열을 만드다.
  - 이 배열을 인덱스에 지정한다. 
  - 행을 우선 뽑고 열을 뽑것이다. 즉 0번째 행을 뽑고 거기서 다시 열 인덱스를 찾아서 값을 리턴 받는다. 

### 배열 변형( 타입, 형태)

```python
x = np.array([1,2,3], dtype='U')
arrayinfo(x)
> 
type : .<class 'numpy.ndarray'>
shape : (3,)
dimension : 1
dtype : <U1
Array Data : 
 ['1' '2' '3']
```

- 데이터 형식을 문자로 주었다. 

```python
x[0] + x[1]
> '12'
```

- 문자 형식이라 연산이 되는게 아니라 이어서 써진다. 

#### Inf vs NaN

- `Inf ` : 무한대,  `NaN` : 정할수 없을때, 숫자로 정의 못하고

```python
np.array([0,1,-1,0] / np.array([1,0,0,0]))
> array([  0.,  inf, -inf,  nan])
```

- `inf` 와`nan` 이 어떤 상황에서 등장하는지 보았다. 

#### 넌파이 - 다른속성

- zeros : 0으로 세팅,크기가 정해져 있고 모든 값이 0인 배열을 생성하려면
- ones : 1로 세팅
- zeros_like : 카피해온다. 0으로 세팅, ones_like : 쉐잎만 카피해서 1로 세팅
- empty : 비어있는거
- arange : 순열
- linspace, logspace

```python
z = np.zeros(5, dtype='i')
arrayinfo(z)
>
type : .<class 'numpy.ndarray'>
shape : (5,)
dimension : 1
dtype : int32
Array Data : 
 [0 0 0 0 0]
```

- 0으로 구성된 배열이 생성된다. 

```python
z = np.zeros(5, dtype='U')
arrayinfo(z)
>
type : .<class 'numpy.ndarray'>
shape : (5,)
dimension : 1
dtype : <U1
Array Data : 
 ['' '' '' '' '']
```

- 데이터 타입이 문자여서 공백으로 구성된 배열이 생성된다. 

```python
z = np.zeros((2,3), dtype='i') 
arrayinfo(z)
>
type : .<class 'numpy.ndarray'>
shape : (2, 3)
dimension : 2
dtype : int32
Array Data : 
 [[0 0 0]
 [0 0 0]]
```

- 2행과 3열로 0으로 채운 배열을 만든다.

```python
o = np.ones((2,3,4), dtype='i8') 
arrayinfo(o)
>
type : .<class 'numpy.ndarray'>
shape : (2, 3, 4)
dimension : 3
dtype : int64
Array Data : 
 [[[1 1 1 1]
  [1 1 1 1]
  [1 1 1 1]]

 [[1 1 1 1]
  [1 1 1 1]
  [1 1 1 1]]]
```

- `dtype='i8'` : 64비트
- 1로 채운 2차원 3행 4열이 생성된다.

```python
o_like = np.ones_like(o, dtype='f')
arrayinfo(o_like)
>
type : .<class 'numpy.ndarray'>
shape : (2, 3, 4)
dimension : 3
dtype : float32
Array Data : 
 [[[1. 1. 1. 1.]
  [1. 1. 1. 1.]
  [1. 1. 1. 1.]]

 [[1. 1. 1. 1.]
  [1. 1. 1. 1.]
  [1. 1. 1. 1.]]]
```

- 실수 1로 2차원 3행 4열이 생성된다. 

```python
z_like = np.zeros_like(z)
arrayinfo(z_like)
>
type : .<class 'numpy.ndarray'>
shape : (5,)
dimension : 1
dtype : <U1
Array Data : 
 ['' '' '' '' '']
```

- `z` 배열을 복사한다. 

```python
e = np.empty((4,3))
arrayinfo(e) 
>
type : .<class 'numpy.ndarray'>
shape : (4, 3)
dimension : 2
dtype : float64
Array Data : 
 [[0.0078125 0.0078125 0.0078125]
 [0.0078125 0.0078125 0.0078125]
 [0.0078125 0.0078125 0.0078125]
 [0.0078125 0.0078125 0.0078125]]
```

- `0078125` : 메모리의 주소값, 랜덤하게 주어진다. 

```python
a = np.arange(10)
arrayinfo(a)
>
type : .<class 'numpy.ndarray'>
shape : (10,)
dimension : 1
dtype : int32
Array Data : 
 [0 1 2 3 4 5 6 7 8 9]
```

- 0...n-1 까지 나열한다. 

```python
a = np.arange(3,21,2)
arrayinfo(a)
>
type : .<class 'numpy.ndarray'>
shape : (9,)
dimension : 1
dtype : int32
Array Data : 
 [ 3  5  7  9 11 13 15 17 19]
```

- 3부터 시작해서 20까지 2단계식 띄어서 부여한다.

### 전치행렬

-  행렬의 행은 열로, 열은 행으로 바꾼 행렬을 의미한다.

- 속성 : T(transpose opertion)

```python
arr = np.array([[1,2,3],[4,5,6]])
arrayinfo(arr)
>
type : .<class 'numpy.ndarray'>
shape : (2, 3)
dimension : 2
dtype : int32
Array Data : 
 [[1 2 3]
 [4 5 6]]
```

```python
arr_transpose = arr.T
arrayinfo(arr_transpose)
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

- 2행 3열이 3행 2열로 바뀌었다. 

**Vector에 Transpose가 가능할까?**

```python
vec = np.arange(10)
arrayinfo(vec)
>
type : .<class 'numpy.ndarray'>
shape : (10,)
dimension : 1
dtype : int32
Array Data : 
 [0 1 2 3 4 5 6 7 8 9]
```

```python
vec_transpose = vec.T
arrayinfo(vec_transpose)
>
type : .<class 'numpy.ndarray'>
shape : (10,)
dimension : 1
dtype : int32
Array Data : 
 [0 1 2 3 4 5 6 7 8 9]
```

- 벡터로 할 수는 있지만 의미는 없다. 

```python
vec_transpose = vec.reshape(1,10).T
arrayinfo(vec_transpose)
>
type : .<class 'numpy.ndarray'>
shape : (10, 1)
dimension : 2
dtype : int32
Array Data : 
 [[0]
 [1]
 [2]
 [3]
 [4]
 [5]
 [6]
 [7]
 [8]
 [9]]
```

- 이렇게 하면 된다. 

### 배열의 원소를 순차적으로 접근하고자 한다면?

1. for(Vector, Matrix)
2. iterator(순환반복) : internext(), finished 속성을 이용해서 ndarray 모든 요소를 순차적으로 접근할 수 있다. 

```python
arr = np.array([1,2,3,4,5])
```

#### for

```python
for tmp in arr : 
    print(tmp, end=' ')
> 1 2 3 4 5 
```

```python
for idx in range(len(arr)):
    print(arr[idx], end=' ')
>  1 2 3 4 5 
```

#### iterator

```python
ite = np.nditer(arr, flags=['c_index']) 
# finished 면 True기 때문에 not 을 줘야지만 빠져나갈 수 있다.
while not ite.finished:
    print(arr[ite.index], end=' ')
    ite.iternext()
```

- `ite ` : 객체를 이용해서 접근 할 수 있다.순환 반복 객체가 만들어졌다. 

- `ite.finished` : 만약에 `ite` 가 끝났으면 `True` 가 된다. 그러니 `not` 를 붙여서 `False` 로 조건을 준다.
- `ite.iternext()` : 하나가 끝났으면 그 다음으로 넘어간다.

```python
arr = np.array([[1,2,3],[4,5,6]])
arrayinfo(arr)
>
type : .<class 'numpy.ndarray'>
shape : (2, 3)
dimension : 2
dtype : int32
Array Data : 
 [[1 2 3]
 [4 5 6]]
```

**2차원 배열에 대한 순차적 접근 코드 작성**

```python
for a in arr:
    for i in a:
        print(i)
>
1
2
3
4
5
6
```

##### print(arr.shape[0]) : row

##### print(arr.shape[1]) : col

```python
for i in range(arr.shape[0]):
    for j in range(arr.shape[1]):
        print(arr[i,j], end='\t')
    print('\n')
```

- 먼저 행의 개수를 불러온다.
- 그 다음에 다시 열을 부른다. 거기거 인덱스로 행과 열을 준다.

```python
ite = np.nditer(arr, flags=['multi_index'])
while not ite.finished:
    print(arr[ite.multi_index], end=' ')
    ite.iternext()
> 1 2 3 4 5 6 
```

- 여기서는 `'multi_index'` 로 주어야한다.
- 속도면에서 훨씬 이득이다. 

