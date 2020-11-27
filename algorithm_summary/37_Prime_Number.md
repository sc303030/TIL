# 37강: 소수 판별 알고리즘 + 백준문제(베르트랑 공준, 소수)

### 소수(Prime Number)

- **소수**란 <u>1보다 큰 자연수 중에서 1과 자기 자신을 제외한 자연수로는 나누어 떨어지지 않는 자연수</u>
  - **6**은 1, 2, 3, 6으로 나누어떨어지므로 소수가 아니다.
  - **7**은1과 7을 제외하고는 나누어떨어지지 않으므로 소수이다.

### 소수의 판별 : 기본적인 알고리즘 (Python)

```python
import math

# 소수 판별 함수
def is_prime_number(x):
    # 2부터 x의 제곱근까지의 모든 수를 확인하며
    for i in range(2, int(math.sqrt(x)) + 1):
        # x가 해당 수로 나누어떨어진다면
        if x % i == 0:
            return False # 소수가 아님
    return True # 소수임

print(is_prime_number(4)) # 4는 소수가 아님
print(is_prime_number(7)) # 7은 소수임
```

### 소수의 판별 : 기본적인 알고리즘 (C++)

```c++
#include <bits/stdc++.h>

using namespace std;

// 소수 판별 함수(2이상의 자연수에 대하여)
bool isPrimeNumber(int x) {
    // 2부터 x의 제곱근까지의 모든 수를 확인하며
    for (int i = 2; i <= (int) sqrt(x); i++) {
        // x가 해당 수로 나누어떨어진다면
        if (x % i == 0) {
            return false; // 소수가 아님
        }
    }
    return true; // 소수임
}

int main() {
    cout << isPrimeNumber(4) << '\n';
    cout << isPrimeNumber(7) << '\n';
}
```

### 소수의 판별 : 기본적인 알고리즘 (Java)

```java
import java.util.*;

class Main {
    // 소수 판별 함수(2이상의 자연수에 대하여)
    public static boolean isPrimeNumber(int x) {
        // 2부터 x의 제곱근까지의 모든 수를 확인하며
        for (int i = 2; i <= Math.sqrt(x); i++) {
            // x가 해당 수로 나누어떨어진다면
            if (x % i == 0) {
                return false; // 소수가 아님
            }
        }
        return true; // 소수임
    }

    public static void main(String[] args) {
        System.out.println(isPrimeNumber(4));
        System.out.println(isPrimeNumber(7));
    }
}
```

### 소수의 판별 : 기본적인 알고리즘 성능 분석

- 2부터 X - 1까지의 모든 자연수에 대하여 연산을 수행해야 한다.
  - 모든 수를 하니씩 확인한다는 점에서 시간 복잡도는 **O(X)**이다.

### 약수의 성질

- **모든 약수가 가운데 약수를 기준으로 곱셈 연산에 대해 대칭**을 이루는 것을 알 수 있다.
  - 예를 들어 16의 약수는 1, 2, 4, 8, 16이다.
  - 이때 2 X 8 = 16은 8 X 2 = 16과 대칭이다.
- 따라서 우리는 특정한 자연수의 모든 약수를 찾을 때 <u>가운데 약수(제곱근)까지만 확인</u>하면 된다.
  - 예들 들어 16이 2로 나누어떨어진다는 것은 8로도 나누어떨어진다는 것을 의미힌다.

### 소수의 판별 : 개선된 알고리즘 (Python)

```python
import math

# 소수 판별 함수 (2이상의 자연수에 대하여)
def is_prime_number(x):
    # 2부터 x의 제곱근까지의 모든 수를 확인하며
    for i in range(2, int(math.sqrt(x)) + 1):
        # x가 해당 수로 나누어떨어진다면
        if x % i == 0:
            return False # 소수가 아님
    return True # 소수임

print(is_prime_number(4)) # 4는 소수가 아님
print(is_prime_number(7)) # 7은 소수임
```

### 소수의 판별 : 개선된 알고리즘 성능 분석

- 2부터 X의 제곱근(소수점 이하 무시)까지의 모든 자연수에 대하여 연산을 수행해야 한다.	
  - 시간 복잡도는 **O(N<sup>½</sup>)**이다.

### <문제> 베르트랑 공준

```python
N = 123456 * 2 + 1
sieve = [True] * N
for i in range(2, int(N**0.5)+1):
    if sieve[i]:
        for j in range(2*i, N, i):
            sieve[j] = False

def prime_cnt(val):
    cnt = 0
    for i in range(val + 1, val * 2 + 1):
        if sieve[i]:
            cnt += 1
    print(cnt)

while True:
    val = int(input())
    if val == 0:
        break
    prime_cnt(val)
```

### <문제> 소수

```python
def is_prime_number(x):
    for i in range(2, int(x ** 0.5) + 1):
        if x % i == 0:
            return False
    return True

num_list = []
for i in range(2,10000+1):
    if is_prime_number(i) == True:
        num_list.append(i)
m = int(input())
n = int(input())
sum_num = []
for i in range(len(num_list)):
    if m <= num_list[i] <= n:
        sum_num.append(num_list[i])

if len(sum_num) == 0:
    print(-1)
else:
    print(sum(sum_num))
    print(min(sum_num))
```

