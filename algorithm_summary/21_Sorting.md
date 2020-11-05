# 21강: 선택 정렬

- 데이터를 특정한 기준에 따라 순서대로 나열

|  7   |  5   |  9   |  0   |  3   |  1   |  6   |  2   |  4   |  8   |
| :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: |
|      |      |      |      |      |      |      |      |      |      |

- 위의 숫자를 어떻게 정렬할 수 있을까?

### 선택 정렬

- 처리되지 않은 데이터 중에서 **가장 작은 데이터를 선택해 맨 앞에 있는 데이터와 바꾸는 것을 반복**

### 선택 정렬 동작 예시

- **[Step 0]** 처리되지 않은 데이터 중 가장 작은 '0'을 선택해 가장 앞의 '7'과 바꾼다

|  7   |  5   |  9   |  0   |  3   |  1   |  6   |  2   |  4   |  8   |
| :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: |
|  0   |  5   |  9   |  7   |  3   |  1   |  6   |  2   |  4   |  8   |

- **[Step 1]** 처리되지 않은 데이터 중 가장 작은 '1'을 선택해 가장 앞의 '1'과 바꾼다

|  0   |  5   |  9   |  7   |  3   |  1   |  6   |  2   |  4   |  8   |
| :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: |
|  0   |  1   |  9   |  7   |  3   |  5   |  6   |  2   |  4   |  8   |

- **[Step 2]** 처리되지 않은 데이터 중 가장 작은 '2'를 선택해 가장 앞의 '9'와 바꾼다

|  0   |  1   |  9   |  7   |  3   |  5   |  6   |  2   |  4   |  8   |
| :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: |
|  0   |  1   |  2   |  7   |  3   |  5   |  6   |  9   |  4   |  8   |

- **[Step 3]** 처리되지 않은 데이터 중 가장 작은 '3'을 선택해 가장 앞의 '7'과 바꾼다

|  0   |  1   |  2   |  7   |  3   |  5   |  6   |  9   |  4   |  8   |
| :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: |
|  0   |  1   |  2   |  3   |  7   |  5   |  6   |  9   |  4   |  8   |

- 이러한 과정을 반복하면 정렬 완료

|  0   |  1   |  2   |  3   |  4   |  5   |  6   |  7   |  8   |   9    |
| :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :----: |
|      |      |      |      |      |      |      |      |      | 마지막 |

- 마지막 경우에는 처리하지 않아도 성공적으로 정렬된다.
- 탐색 범위만큼 이중 반복문을 돌린다.

### 선택 정렬 소스코드 (Python)

```python
array = [7, 5, 9, 0, 3, 1, 6, 2, 4, 8]

for i in range(len(array)):
    min_index = i #가장 작은 원소의 인덱스
    for j in range(i + 1, len(array)):
        if array[min_index] > array[j]:
            min_index = j
    array[i], array[min_index] = array[min_index], array[i] # 스와프
print(array)
>
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```

### 선택 정렬 소스코드 (C++)

```c++
#include <bits/stdc++.h>

using namespace std;

int n = 10;
int arr[10] = {7, 5, 9, 0, 3, 1, 6, 2, 4, 8};

int main(void) {
    for (int i = 0; i < n; i++) {
        int min_index = i; // 가장 작은 원소의 인덱스 
        for (int j = i + 1; j < n; j++) {
            if (arr[min_index] > arr[j]) {
                min_index = j;
            }
        }
        swap(arr[i], arr[min_index]); // 스와프
    }
    for(int i = 0; i < n; i++) {
        cout << arr[i] << ' ';
    }
}
```

### 선택 정렬 소스코드 (Java)

```java
import java.util.*;

public class Main {

    public static void main(String[] args) {

        int n = 10;
        int[] arr = {7, 5, 9, 0, 3, 1, 6, 2, 4, 8};

        for (int i = 0; i < n; i++) {
            int min_index = i; // 가장 작은 원소의 인덱스 
            for (int j = i + 1; j < n; j++) {
                if (arr[min_index] > arr[j]) {
                    min_index = j;
                }
            }
            // 스와프
            int temp = arr[i];
            arr[i] = arr[min_index];
            arr[min_index] = temp;
        }

        for(int i = 0; i < n; i++) {
            System.out.print(arr[i] + " ");
        }
    }

}
```

### 선택 정렬의 시간 복잡도

- N번 만큼 가장 작은 수를 찾아서 맨 앞으로 보내야 한다.
- 전체 연산 횟수
  - N + (N -1) + (N -2) + ... + 2
- (N<sup>2</sup> + N - 2) / 2로 표현할 수 있는데, 빅오 표기법에 따라서 O(N<sup>2</sup>)이라고 작성

### <문제> 수 정렬하기 1, 2

```python
n= int(input())

num_list = []
for i in range(n):
    num_list.append(int(input()))
    
num_list = sorted(num_list)
for a in num_list:
    print(a)
```

- 간단히 수를 입력받고 sorted해서 출력한다.

```python
n= int(input())

num_list = []
for i in range(n):
    num_list.append(int(input()))
    
num_list = sorted(num_list)
result = [ str(x) for x in num_list]

print('\n'.join(result))
```

- 루프를 돌리지 않기위해 리스트 컴프리헨션으로 숫자를 문자로 바꾸고 출력한다.

### <문제> 소트인사이드

```python
num_list = list(input())

for i in range(len(num_list)):
    max_index = i
    for h in range(i,len(num_list)):
        if num_list[i] > num_list[h]:
            continue
        else:
            max_index = h
            num_list[i], num_list[h] = num_list[h],num_list[i]

print(''.join(num_list))
```

- 숫자대신 문자로 받아서 하나씩 인덱싱하고 , 인덱싱 한 숫자는 알아서 숫자형식으로 비교해줘서 순서를 바꿔준다.