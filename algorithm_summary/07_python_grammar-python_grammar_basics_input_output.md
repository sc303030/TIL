# 7강: 파이썬 문법 - 기본 입출력 

### 기본 입출력

- 모든 프로그램은 적절한 입출력 양식 존재
- 프로그램 동작의 첫 번째 단계는 데이터를 입벽받거나 생성하는 것
  - ex) 학생의 성적 데이터가 주어지고, 이를 내림차순으로 정렬한 결과를 출력하는 프로그램

```python
5
65 90 75 34 99
>
99 90 75 65 34
```

### 자주 사용되는 표준 입력 방법

- input() : 한 줄의 문자열을 입력 받는 함수
- map() : 리스트의 모든 원소에 각각 특정한 함수를 적용할 때 사용
  - 예시 )  공백을 기준으로 구분된 데이터를 입력 받을 때
    - list(map(int, input().split()))
  - 예시 ) 공백을 기준으로 구분된 데이터의 개수가 많지 않다명, 단순히 다음과 같이 사용한다.
    - a, b, c = map(int, input().split())
    - map(int, input().split()) : 패킹
    - a, b, c : 언패킹
    - a, b, c =/= map(int, input().split()) 오류발생

### 입력을 위한 전형적인 소스코드 1)

```python
# 데이터의 개수 입력
n = int(input())

# 각 데이터를 공백을 기준으로 구분하여 입력
data = list(map(int, input().split()))

data.sort(reverse=True)
print(data)
>
5
65 90 75 34 99
[99, 90, 75, 65, 34]
```

### 입력을 위한 전형적인 소스코드 2)

```python
# n, m, k를 공백을 기준으로 구분하여 입력
n, m, k = map(int, input().split())

print(n, m, k)
>
3 5 7
3 5 7
```

### 빠르게 입력 받기

- sys 라이브러리에 정의되어 있는 sys.stdin.readline() 메서드 이용
  - 입력 후 엔터가 줄 바꿈 기호로 입력되므로 rstrip() 메서드를 함께 사용
    - 입력의 개수가 많을 때 시간초과 발생 가능하니 sys.stdin.readline() 사용하면 좋다.
    - 이진탐색, 정렬, 그래프 관련 문제에서 자주 사용되는 테크닉

```python
import sys

# 문자열 입력 받기
data = sys.stdin.readline().rstrip() 
print(data)
```

### 자주 사용되는 표준 출력 방법

- 파이썬에서 기본 출력은 print() 함수를 이용
  - 각 변수를 콤마를 이용하여 띄어쓰기로 구분하여 출력 가능
- print()는 기본적으로 출력 이후에 줄 바꿈 수행
  - 줄 바꿈을 원치 않으면 `end` 속성 이용 가능

```python
# 출력할 변수들
a = 1
b = 2
print(a, b)
> 1 2

print(7, end=" ")
print(8, end=" ")
>
7 8

# 출력할 변수
answer = 7
print('정답은' + str(answer) + '입니다.')
>
정답은 7입니다.
```

### f-string 예제

- 파이썬 3.6부터 사용가능하며, 문자열 앞에 접두사 'f' 를 붙여 사용
- 중괄호 안에 변수명을 기입하여 간단히 문자열과 정수를 함께 넣을 수 있음

```python
answer = 7
print(f'정답은 {answer}입니다.")
> 
정답은 7입니다.
```

