# 자바 기초 프로그래밍 강좌 13강 - 배열 (Java Programming Tutorial 2017 #13)

### 배열

- 쉽게 말해 데이터가 많을 때 사용하는 것

```java
import java.util.Scanner;

public class Main {
	
	public static int max(int a, int b) {
		return (a > b) ? a : b;
	}
	
	public static void main(String[] args) {

		Scanner scanner = new Scanner(System.in);
		System.out.print("생설할 배열의 크기를 입력하세요 : ");
		int number = scanner.nextInt();
		int[] array = new int[number];
		for(int i = 0;i < number; i++) {
			System.out.print("배열에 입력할 정수를 하나씩 입력하세요 : ");
			array[i] = scanner.nextInt();
		}
		int result = -1;
		for(int i = 0; i < number; i ++) {
			result = max(result, array[i]);
		}
		System.out.println("입력한 모든 정수 중에서 가장 큰 값은 : " + result + "입니다.");
	}
}
>
생설할 배열의 크기를 입력하세요 : 5
배열에 입력할 정수를 하나씩 입력하세요 : 7
배열에 입력할 정수를 하나씩 입력하세요 : 10
배열에 입력할 정수를 하나씩 입력하세요 : 5
배열에 입력할 정수를 하나씩 입력하세요 : 4
배열에 입력할 정수를 하나씩 입력하세요 : 3
입력한 모든 정수 중에서 가장 큰 값은 : 10입니다.    
```

- Scanner scanner = new Scanner(System.in);
  - 자바가 가지고 있는기본 라이브러리가 아니라서 ctrl + shift + f5를 눌러서 import를 시킨다.
  - 모든 라이브러리를 가지면 자바가 무거워지기때문에 필요할때 이렇게 import 한다.

- return (a > b) ? a : b;
  - a가 b보다 크면  x:y에서 x를 반환, b가 더 크면 y를 반환

### 100개의 랜덤 정수의 평균

```java
public class Main {
	

	public static void main(String[] args) {

		int[] array = new int[100];
		for(int i =0; i < 100;i++) {
			array[i] = (int) (Math.random() * 100 + 1);
		}
		int sum = 0;
		for(int i = 0; i < 100; i++) {
			sum += array[i];
		}
		System.out.println("100개의 랜덤 정수의 평균 값은" + sum/100+"입니다");
	}
}
>
100개의 랜덤 정수의 평균 값은53입니다
```

- Math.random()
  - Math에서 지원하는 랜덤한 수를 뽑아주는 함수

- (int) (Math.random() * 100 + 1)
  - 1을 더하는 이유?
    - 0 <= random() <=1 이다.
    - 여기에 100을 곱하면 1<= x < 101
    - int로 바꿔주면 소수점이 날아간다.
    - 그래서 1을 더해야 0.xx이 1이된다.

