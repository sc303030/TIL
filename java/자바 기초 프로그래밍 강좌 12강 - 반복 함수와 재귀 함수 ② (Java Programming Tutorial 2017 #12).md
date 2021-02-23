# 자바 기초 프로그래밍 강좌 12강 - 반복 함수와 재귀 함수 ② (Java Programming Tutorial 2017 #12)

### 피보나치 수열을 반복 함수로 구현

```java
public class Main {
	
	public static int fibonacci(int number) {
		int one = 1;
		int two = 1;
		int result = -1;
		if(number == 1) {
			return one;
		}else if(number == 2){
			return two;
		}else {
			for(int i = 2; i < number; i++) {
				result = one + two;
				one = two;
				two = result;
			}
		}
		return result;
	}
	
	public static void main(String[] args) {
		
		System.out.println("피보나치 수열의 10번재 원소는" + fibonacci(10)+"입니다.");
		
	}

}
>
피보나치 수열의 10번재 원소는55입니다.    
```

- result = one + two; 
  - 앞원소랑 해당원소 더하기
- one = two;
  - 그래야 다음 원소 구할때 지금 해당원소가 다음 원소의 앞 원소가 됨
- two = result;
  - 누적된 값이 들어가야 하니깐 result를 부여한다.

### 피보나치 수열을 재귀 함수로 구현

```java
public class Main {
	
	public static int fibonacci(int number) {
		if(number == 1) {
			return 1;
		}else if(number == 2) {
			return 1;
		}else {
			return fibonacci(number -1) + fibonacci(number - 2);
		}
	}
	
	public static void main(String[] args) {
		
		System.out.println("피보나치 수열의 10번재 원소는" + fibonacci(10)+"입니다.");
		
	}

}
>
피보나치 수열의 10번재 원소는55입니다.      
```

- 저번처럼 작은 값의 값을 알아가면서 진행된다.
- 그러면 문제점은?
  - 이걸 하나씩 타고 올라가면 반복되는 연산이 많아진다. -> 비효율적으로 변해버림.
  - 이걸 해결하기위해 동적프로그래밍이 있다.
  - 적은 연산 개수에는 좋지만 많을수록 비효율적