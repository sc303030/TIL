# 15강: 구현 유형 문제 풀이 + 통나무 건너뛰기

### <문제> 시각 : 문제 설명

- 정수 N이 입력되면 00시 00분 00초부터 N시 59분 59초까지의 **모든 시각 중에서 3이 하나라도 포함되는 모든 경우의 수를 구하는 프로그램을 작성**하라. 예를 들어 1을 입력했을 때 다음은 3이 하나라도 포함되어 있으므로_세어야 하는 시각_ 이다.
  - 00시 00분 03초
  - 00시 13분 30초
- 반면에 다음은 3이 하나라도 포함되어 있지 않으므로 _세면 안되는 시각_이다.
  - 00시 02분 55초
  - 01시 27분 45초

- 난이도 : 1개, 풀이 시간 15분, 시간제한 2초, 메모리 제한 128MB
- 입력 조건
  - 첫째 줄에 정수 N이 입력된다. (0 <= N <= 23)
- ​	출력 조건
  - 00시 00분 00초부터 N시 59분 59초까지의 모든 시각 중에서 3이 하나라도 포함되는 모든 경우의 수를 출력한다.
- 입력 예시

```
5
```

- 출력 예시

```
11475
```

### <문제> 시각 : 문제 해결 아이디어

- **가능한 모든 시각의 경우를 하나씩 모두 세서 풀 수 있는 문제**
- 하루는 86,400초이므로, 00시 00분 00초부터 23시 59분 59초까지의 모든 경우는 86,400가지다.
  - 24 * 60 * 60 = 86,400
- 단순히 시각을 1씩 증가시키면서 3이 하나라도 포함되어 있는지를 확인하면 된다.
- **완전 탐색(Brute Forcing)** 문제 유형
  - _가능한 경우의 수를 모두 검사해보는 탐색 방법_을 의미

### <문제> 시각 : 답안 예시 (Python)

```python
n = int(input())

hour = range(0,n+1)
minute = range(0,60)
second = range(0,60)

cnt = 0

for h in hour:
    for m in minute:
        for s in second:
            if '3' in str(h) + str(m) + str(s):
                cnt +=1
print(cnt)
```

### <문제> 시각 : 답안 예시 (C++)

```c++
#include <bits/stdc++.h>

using namespace std;

int h, cnt;

// 특정한 시각 안에 '3'이 포함되어 있는지의 여부
bool check(int h, int m, int s) {
    if (h % 10 == 3 || m / 10 == 3 || m % 10 == 3 || s / 10 == 3 || s % 10 == 3)
        return true;
    return false;
}

int main(void) {
    // H를 입력받기 
    cin >> h;
    for (int i = 0; i <= h; i++) {
        for (int j = 0; j < 60; j++) {
            for (int k = 0; k < 60; k++) {
                // 매 시각 안에 '3'이 포함되어 있다면 카운트 증가
                if (check(i, j, k)) cnt++;
            }
        }
    }
    cout << cnt << '\n';
    return 0;
}

```

### <문제> 시각 : 답안 예시 (Java)

```java
import java.util.*;

public class Main {

    // 특정한 시각 안에 '3'이 포함되어 있는지의 여부
    public static boolean check(int h, int m, int s) {
        if (h % 10 == 3 || m / 10 == 3 || m % 10 == 3 || s / 10 == 3 || s % 10 == 3)
            return true;
        return false;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        // H를 입력받기 
        int h = sc.nextInt();
        int cnt = 0;

        for (int i = 0; i <= h; i++) {
            for (int j = 0; j < 60; j++) {
                for (int k = 0; k < 60; k++) {
                    // 매 시각 안에 '3'이 포함되어 있다면 카운트 증가
                    if (check(i, j, k)) cnt++;
                }
            }
        }

        System.out.println(cnt);
    }

}
```

### <문제> 왕실의 나이트 : 문제 설명

- 행복 왕국의 왕실 정원은 체스판과 같은 **8 x 8 좌표 평면**이다. 왕실 정원의 특정한 한 칸에 나이트가 서있다. 나이트는 매우 충성스러운 신하로서 매일 무술을 연마한다.
- 나이트는 말을 타고 있기 때문에 이동을 할 때는 L자 형태로만 이동할 수 있으며 정원 밖으로는 나갈 수 없다.
- 나이트는 특정 위치에서 다음과 같은 2가지 경우로 이동할 수 있다.
  - 수평으로 두칸 이동한 뒤에 수직으로 한 칸 이동하기
  - 수직으로 두칸 이동한 뒤에 수평으로 한 칸 이동하기
- 이처럼 8 x 8좌표 평면상에서 나이트의 위치가 주어졌을 때 나이트가 이동할 수 있는 경우의 수를 출력하는 프로그램을 작성하시오. 왕실의 정원에서 행 위치를 표현할 때는 1부터 8로 표현하며, 열 위치를 표현할 대는 a부터 h로 표현한다.
  - **c2**에 있을 때 이동할 수 있는 경우의 수는 **6가지**이다.
  - **a1**에 있을 때 이동할 수 있는 경우의 수는 **2가지**이다.

- 난이도 : 1개, 풀이 시간 : 20분, 시간 제한 : 1초, 메모리 제한 : 128MB
- 입력 조건
  - 첫째 줄에 8 x 8 좌표 평면상에서 현재 나이트가 위치한 곳의 좌표를 나타내는 두 문자로 구성된 문자열이 입력된다. 입력 문자는 a1처럼 열과 행으로 이뤄진다.
- 출력 조건
  - 첫째 줄에 나이트가 이동할 수 있는 경우의 수를 출력하시오
- 입력 예시

```
a1
```

- 출력 예시

```
2
```

### <문제> 왕실의 나이트 : 문제 해결 아이디어

- 나이트의 8가지 경로를 하나씩 확인하며 각 위치로 이동이 가능한지 확인한다.
  - 리스트를 이용하여 8가지 방향에 대한 방향 벡터를 정의한다.

### <문제> 왕실의 나이트 : 답안 예시 (Python)

```python
col,row = input()
row = int(row)
column = int(ord(col)) - int(ord('a')) + 1

steps = [(-2,-1),(-1,-2),(1,-2),(2,-1),(2,1),(1,2),(-1,2),(-2,1)]

cnt = 0
for step in steps:
    next_row = row = step[0]
    next_col = column + step[1]
    
    if next_row >= 1 and next_row <=8 and next_col >=1 and next_col <= 8:
        cnt +=1
print(cnt)
```

### <문제> 왕실의 나이트 : 답안 예시 (C++)

```c++
#include <bits/stdc++.h>

using namespace std;

string inputData;

// 나이트가 이동할 수 있는 8가지 방향 정의
int dx[] = {-2, -1, 1, 2, 2, 1, -1, -2};
int dy[] = {-1, -2, -2, -1, 1, 2, 2, 1};

int main(void) {
    // 현재 나이트의 위치 입력받기
    cin >> inputData;
    int row = inputData[1] - '0';
    int column = inputData[0] - 'a' + 1;

    // 8가지 방향에 대하여 각 위치로 이동이 가능한지 확인
    int result = 0;
    for (int i = 0; i < 8; i++) {
        // 이동하고자 하는 위치 확인
        int nextRow = row + dx[i];
        int nextColumn = column + dy[i];
        // 해당 위치로 이동이 가능하다면 카운트 증가
        if (nextRow >= 1 && nextRow <= 8 && nextColumn >= 1 && nextColumn <= 8) {
            result += 1;
        }
    }

    cout << result << '\n';
    return 0;
}

```

### <문제> 왕실의 나이트 : 답안 예시 (Java)

```java
import java.util.*;

public class Main {

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        // 현재 나이트의 위치 입력받기
        String inputData = sc.nextLine();
        int row = inputData.charAt(1) - '0';
        int column = inputData.charAt(0) - 'a' + 1;

        // 나이트가 이동할 수 있는 8가지 방향 정의
        int[] dx = {-2, -1, 1, 2, 2, 1, -1, -2};
        int[] dy = {-1, -2, -2, -1, 1, 2, 2, 1};

        // 8가지 방향에 대하여 각 위치로 이동이 가능한지 확인
        int result = 0;
        for (int i = 0; i < 8; i++) {
            // 이동하고자 하는 위치 확인
            int nextRow = row + dx[i];
            int nextColumn = column + dy[i];
            // 해당 위치로 이동이 가능하다면 카운트 증가
            if (nextRow >= 1 && nextRow <= 8 && nextColumn >= 1 && nextColumn <= 8) {
                result += 1;
            }
        }

        System.out.println(result);
    }

}
```

### <문제> 문자열 재정렬 : 문제 설명

- 알파벳 대문자와 숫자(0 ~ 9)로만 구성된 문자열이 입력으로 주어진다. 이때 모든 알파벳을 오름파순으로 정렬하여 이어서 출력한 뒤에, 그 뒤에 모든 숫자를 더한 값을 이어서 출력한다.
- 예를 들어 K1KA5CB7이라는 값이 들어오면 ABCKK13을 출력한다.

- 난이도 : 1개, 풀이 시간 : 20분, 시간 제한 : 1초, 메모리 제한 : 128MB, 기출 Facebook인터뷰
- 입력 조건
  - 첫째 줄에 하나의 문자열 S가 주어진다. (1 <= S의 길이 <= 10,000)
- 출력 조건
  - 첫째 줄에 문제에서 요구하는 정답을 출력한다.
- 입력 예시1

```
K1KA5CB7
```

- 출력 예시 1

```
ABCKK13
```

- 입력 예시 2

```
AJKDLSI412K4JSJ9D
```

- 출력 예시 2

```
ADDIJJJKKLSS20
```

**내가 작성한 코드**

```python
n = input()

n = sorted(n)

idx = 0

for i in range(9,0,-1):
    i = str(i)
    if  i in n:
        print(n.index(i))
        idx = n.index(i) +1
        break;
    else:
        continue
text_list = n[idx:]
num_list = sum(list(map(int,n[:idx])))
print(''.join(text_list) + str(num_list))
```

1. 문자열을 입력받아 정렬한다.
2. 9부터 역순으로 문자열에서 숫자를 찾아 인덱스를 찾는다.
3. 그 인덱스 +1 에 해당하는 곳을 나눈다.
4. 텍스트와 숫자를 나누어 숫자는 sum을해서 출력한다.

### <문제> 문자열 재정렬 : 문제 해결 아이디어

- 문자열이 입력되었을 때 문자를 하나씩 확인한다.
  - 숫자인 경우 따로 합계를 계산
  - 알파벳인 경우 별도의 리스트에 저장
- **리스트에 저장된 알파벳을 정렬해 출력하고, 합계를 뒤에 붙여 출력하면 정답**

### <문제> 문자열 재정렬 : 답안 예시 (Python)

```python
data = input()
result = []
value = 0

# 문자를 하나씩 확인하며
for x in data:
    #알파벳인 경우 결과 리스트에 삽입
    if x.isalpha():
        result.append(x)
    # 숫자를 따로 더하기
    else:
        value += int(x)
# 알파벳을 오름차순으로 정렬
result.sort()

# 숫자가 하나라도 존재하는 경우 가장 뒤에 삽입
if value != 0:
    result.append(str(value))
# 최종 결과 출력(리스트를 문자열로 변환하여 출력)
print(''.join(result))
```

### <문제> 통나무 건너뛰기

```python
n = int(input())
max_list = []
for i in range(n):
    m = int(input())
    namu = list(map(int,input().split()))
    namu_asc = sorted(namu)
    namu_desc = sorted(namu,reverse=True)
    if m % 2 != 0:
        namu_wan = namu_asc[::2] + namu_desc[1::2]
    else:
        namu_wan = namu_asc[::2] + namu_desc[::2]
    a = []
    for x in range(1,m):
        b = abs(namu_wan[x-1] - namu_wan[x])
        a.append(b)
    max_list.append(max(a))
for idx in range(n):
    print(max_list[idx], end='\n')
```

1. 총 케이스를 받는다.
2. 최대높이 차이를 담을 리스트를 만든다.
3. 데이터의 개수를 입력 받는다.
   1. 리스트를 앞에서 부터 짝수번째, 리버스해서 짝수번째로 입력받는다. 그래야 높이차가 가장 작다.
   2. 홀수일때와 짝수일때 인덱스가 달라야 하니 if를 줘서 다르게 건너뛴다.
4. 정렬된 리스트에서 0,1번째 처럼 계속 계산해서 높이의 최대값을 받는다.
5. 최종적으로 케이스만큼 높이를 출력한다.



