# 더해서 n이 되는 자연수 집합 구하기 파이썬

- 최근 코딩테스트를 보면서 이러한 유형의 문제가 나왔었는데 알 것 같으면서 구현을 못해서 못풀었다.
- 그래서 앞으로 같은 유형의 문제가 등장하면 꼭 풀겠다고 다짐하며 찾아보았다.
- 합분해의 경우의 수를 구하는 알고리즘은 있었는데 내가 원하는 알고리즘이 없었다. 내가 원했던건 n이 되는 집합을 구하고 싶었기 때문이다.
- 겨우 찾은 알고리즘은 자바로 되어 있어서 해당 코드를 파이썬으로 변경하였다.

### 소스코드

```python
import sys
input = sys.stdin.readline

def solution(n):
    number = [n]
    answer = []
    while True:
        answer.append(number.copy())
        temp = number.pop()
        if temp != 1:
            number.append(temp - 1)
            number.append(1)
        else:
            sum = 2
            for _ in range(len(number)):
                if number and number[-1] == 1:
                    sum += 1
                    number.pop()
            if not number:
                break
            pivot = number.pop() - 1
            number.append(pivot)
            while sum > pivot:
                number.append(pivot)
                sum -= pivot
            number.append(sum)
    return answer

n = 5
print(solution(5))
>
[[5], [4, 1], [3, 2], [3, 1, 1], [2, 2, 1], [2, 1, 1, 1], [1, 1, 1, 1, 1]]
```

- 우선 n을 저장하고 내림차순으로 1씩 빼면서 구한다.
- 마지막에 더한 숫자가 1인지 아닌지로 먼저 구분 한 후 나머지를 저장한다.
- [참고블로그](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=ndb796&logNo=220912159147)
  - 여기에 자세한 설명이 있다. 이 블로그에 있는 코드를 파이썬으로 변경하였다.
- 앞으로 비슷한 유형이 나오면 이 코드를 사용해야 겠다.