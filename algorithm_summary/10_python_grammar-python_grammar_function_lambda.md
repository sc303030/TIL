# 10강: 파이썬 문법 - 함수와 람다 표현식

### 함수

- 특정한 작업을 하나의 단위로 묶어 놓은 것
- 함수를 사용하면 불필요한 소스코드 반복 줄일 수 있다.

### 함수의 종류

- 내장함수 : 파이썬이 기본적으로 제공하는 함수
  - ex) input(), print()와 같은 함수들
- 사용자 정의 함수 : 개발자가 직접 정의하여 사용할 수 있는 함수

### 함수 정의하기

- 함수를 사용하면 소스코드의 길이를 줄일 수 있다.
  - 매개변수 : 함수 내부에서 사용할 변수
  - 반환 값 : 함수에서 처리 된 결과를 반환

```python
def 함수명(매개변수):
	실행할 소스코드
	return 반환 값
```

### 더하기 함수 예시

**더하기 함수 예시 1)**

```python
def add(a,b):
	return a + b
print(add(3,7))
> 10
```

**더하기 함수 예시 2)**

```python
def add(a,b):
	print('함수의 결과:', a + b)
add(3,7)
>
함수의 결과: 10
```

### 파라미터 지정하기

- 파라미터의 변수 직접 지정 가능
  - 매개변수의 순서가 달라도 상관 없다.

```python
def add(a,b):
	print('함수의 결과:',a + b)
add(b = 3,a = 7)
```

### global 키워드

- global 키워드로 변수를 지정하면 해당 함수에서는 지역 변수를 만들지 않고, **함수 바깥에 선언된 변수를 바로 참조**

```python
a = 0

def func():
	global a
	a += 1
for i in range(10):
	func()
	
print(a)
```

- 전역변수로 선언된 리스트개체의 내부 개체의 메서드를 호출하는 것도 가능하다.
  - 함수안에 지역변수 == 전역변수 이면 함수안에서는 지역변수가 우선 적용된다.

### 여러 개의 반환 값

- 파이썬의 함수는 여러 개의 반환 값을 가질 수 있다.

```python
def operator(a,b):
	add_var = a + b
    subtract_var = a - b
    multiply_var = a * b
    divide_var = a / b
    return add_var, subtract_var, multiply_var, divide_var
a,b,c,d = operator(7,3)
print(a,b,c,d)
```

- 한꺼번에 담는 것 : 패킹
- 다시 변수에 담는 것 : 언패킹

### 람다 표현식

- 특정한 기능을 수행하는 함수를 한 줄에 작성 할 수 있다.

```python
def add(a,b):
	return a + b
# 일반적인 add() 메서드 사용
print(add(3,7))
>
10

# 람다 표현식으로 구현한 add() 메서드
print((lambda a,b: a + b)(3,7))
>
10
```

### 람다 표현식 예시 : 내장 함수에서 자주 사용되는 람다 함수

```python
array = [('홍길동',50),('이순신',32),('아무개',74)]

def my_key(x):
	return x[1]
print(sorted(array, key=my_key))
print(sorted(array, key=lambda x : x[1]))
>
[('이순신',32),('홍길동',50),,('아무개',74)]
[('이순신',32),('홍길동',50),,('아무개',74)]
```

### 람다 표현식 예시 : 여러 개의 리스트에 적용

```python
list1 = [1,2,3,4,5]
list2 = [6,7,8,9,10]

result = map(lambda a,b : a + b,list1, list2)
print(list(result))
>
[7,9,11,13,15]
```

- list1의 0번째 인덱스, list2의 0번째 인덱스부터 차례대로 더해진 값이 출력된다.