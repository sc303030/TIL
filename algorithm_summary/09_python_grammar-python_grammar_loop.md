# 9강: 파이썬 문법 - 반복문

### 반복문

- 특정 소스코드를 반복적으로 실행하고자 할 때 사용하는 문법
- while , for
  - for 문이 더 간결한 경우가 많다.

#### 1부터 9까지 모든 정수의 합 구하기 예제 (while문)

```python
i = 1
result = 0

# i가 9보다 작거나 같을 때 아래 코드를 반복적으로 실행
while i <= 9 :
	result += i
	i += 1
print(result)
>
45
```

#### 1부터 9까지 홀수의 합 구하기 예제 (while문)

```python
i = 1
result = 0

# i가 9보다 작거나 같을 때 아래 코드를 반복적으로 실행
while i <= 9 :
	if i % 2 == 1:
		result += i
	i += 1
print(result)
>
25
```

#### 반복문에서의 무한 루프

- 무한루프 : 끊임없이 반복되는 반복 구문
  - 코딩 테스트에서 무한 루프를 구현할 일은 거의 없으니 유의
  - 반복문을 작성 한 후 항상 반복문을 탈출 할 수 있는지 확인해야 한다.

```python
x = 10
while x > 5:
	print(x)
>
10
...
(중략)
```

### 반복문 : for문

- 특정한 변수를 이용하여 'in' 뒤에 오는 데이터(리스트, 튜플 등)에 포함 되어 있는 원소를 첫 번째 인덱스부터 차례대로 하나씩 방문한다.

```python
for 변수 in 리스트:
	실행할 소스코드
```

```python
array = [9,8,7,6,5]

for x in array:
	print(x)
>
9
8
7
6
5
```

- range() : 연속적인 값을 차례대로 순회할 때
  - range(시작 값, 끝 값 +1) 형태로 사용
  - 인자를 하나만 넣으면 자동으로 시작 값은 0

```python
result = 0

# i는 1부터 9까지의 모든 값을 순회
for i in range(1,10):
	result += 1
	
print(result)
>45
```

#### 1부터 30까지의 모든 정수의 합 구하기 예제(for 문)

```python
result = 0
for i in range(1,31):
	result += i
print(result)
> 465
```

### 파이썬의 continue 키워드

- continue : 반복문에서 남은 코드의 실행을 건너뛰고, 다음 반복을 진행하고자 할 때 사용

```python
result = 0

for i in range(1,10):
	if i % 2 == 0:
		continue
	result += i
print(result)
> 25
```

### 파이썬의 break 키워드

- break : 반복문을 즉시 탈출하고자 할 때 사용

```python
i = 1

while True:
	print('현재 i의 값:', i)
	if i == 5:
		break
	i += 1
>
현재 i의 값: 1
현재 i의 값: 2
현재 i의 값: 3
현재 i의 값: 4
현재 i의 값: 5
```

#### 학생들의 합격 여부 판만 예제 1) 점수가 80점만 넘으면 합격

```python
scores = [90,85,77,65,97]

for i in range(5):
	if score[i] >= 80:
		print(i + 1, '번 학생은 합격입니다.')
>
1 번 학생은 합격입니다.
2 번 학생은 합격입니다.
5 번 학생은 합격입니다.
```

#### 학생들의 합격 여부 판단 예제  2) 특정 번호의 학생은 제외하기

```python
score = [90,85,77,65,97]
cheating_student_list = {2,4}
for i in range(5):
    if i + 1 in cheating_student_list:
        continue
    if scores[i] >= 80:
        print(i + 1,'번 학생은 합격입니다.')
>
1번 학생은 합격입니다.
5번 학생은 합격입니다.
```

#### 중첩된 반복문 : 구구단 예제

```python
for i in range(2,10):
	for j in range(1,10):
        print(i,'X',j,'=',i*j)
    print()
>
2 x 1 = 2 
(중략)
9 x 9 = 81
```

