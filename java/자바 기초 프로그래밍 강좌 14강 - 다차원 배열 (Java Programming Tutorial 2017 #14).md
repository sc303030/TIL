# 자바 기초 프로그래밍 강좌 14강 - 다차원 배열 (Java Programming Tutorial 2017 #14)

### 다차원배열

- 배열이 배열의 원소로 들어가는 구조

```java
public class Main {

	public static void main(String[] args) {
		
		int N = 50;
		int[][] array = new int[N][N];
		for(int i = 0; i < N;i++) {
			for(int j = 0;j < N; j++) {
				array[i][j] = (int)(Math.random() * 10);
			}
		}
		for(int i =0; i < N; i++) {
			for(int j = 0; j < N; j++) {
				System.out.print(array[i][j]+" ");
			}
			System.out.println();
		}
		
	}

}
>
8 5 8 3 9 4 7 2 1 6 2 4 0 6 2 7 5 6 0 9 8 1 7 0 8 5 1 1 1 0 2 3 8 4 7 7 3 0 6 7 4 6 3 0 1 6 2 5 8 9 
1 1 7 4 7 9 5 8 5 9 2 0 9 8 7 4 9 3 5 6 6 9 6 5 5 0 9 9 1 7 1 7 5 2 2 7 3 8 8 0 2 3 2 6 4 1 1 0 3 8 ...
```

- 2D게임 좌표 찾거나 할때 이차원배열이 많이 쓰인다.

- for문을 2번 돌려서 한번은 바깥쪽인덱스를 먼저 지정하고 그 다음에 내부 인덱스를 돌면서 값을저장한다.