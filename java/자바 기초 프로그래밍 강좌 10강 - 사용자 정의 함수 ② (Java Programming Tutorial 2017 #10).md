# 자바 기초 프로그래밍 강좌 10강 - 사용자 정의 함수 ② (Java Programming Tutorial 2017 #10)

### 약수 중 K번째로 작은 수를 찾는 프로그램

```java
public class Main {
	
	public static int function(int number, int k) {
		for(int i=1; i<= number; i++){
			if(number % i == 0) {
				k--;
				if(k==0) {
					return i;
				}
			}
		}
		return -1;
	}
	
	public static void main(String[] args) {
		int result = function(3050,10);
		if(result == -1) {
			System.out.println("3050의 10번째 약수는 없습니다.");	
		}else {
			System.out.println("3050의 10번째 약수는"+ result +"입니다.");
		}
		
	}

}
>
3050의 10번째 약수는610입니다.    
```



### 문자열에서 마지막 단어를 반환하는 함수

```java
public class Main {
	
	public static char function(String input) {
		return input.charAt(input.length() - 1);
	}
	
	public static void main(String[] args) {
		System.out.println("Hello Wordl의 마지막 단어는" +function("Hello World"));
	}

}
>
Hello Wordl의 마지막 단어는d    
```

- charAt : 문자열의 특정 인덱스의 값을 가져옴
  - 0부터 시작한다.  그래서 `input.length()` 에서 1을 빼준것



### max()를 이용하여 최대값을 저장하는 프로그램

```java
public class Main {
	
	public static int max(int a, int b) {
		return (a > b)? a:b;
	}
	
	public static int function(int a, int b, int c) {
		int result = max(a,b);
		result = max(result, c);
		return result;
	}
	
	public static void main(String[] args) {
		
		System.out.println("(345, 567, 789) 중에서 가장 큰 값은" + function(345,567,789)); 
		
	}

}
>
(345, 567, 789) 중에서 가장 큰 값은789    
```

# 자바 기초 프로그래밍 강좌 11강 - 반복 함수와 재귀 함수 ① (Java Programming Tutorial 2017 #11)

### 반복함수

- 단순히 while 혹은 for문법을 이용하여 특정한 처리를 반복하는 방식

### 재귀함수

- 자신의 함수 내부에서 자기 자신을 스스로 호출함으로써 재귀적으로 문제를 해결

### 팩토리얼을 재귀 함수로 구현

- 반복함수

```java
public class Main {
	
	// 5! = 5 * 4 * 3 * 2 * 1 = 120
	public static int factorial(int number) {
		int sum = 1;
		for(int i = 2; i <= number; i++) {
			sum *= i;
		}return sum;
	}	
	
	public static void main(String[] args) {
		

			System.out.println("10 팩토리얼은 "+factorial(10));
	}

}
>
10 팩토리얼은 3628800    
```

- 재귀함수

```java
public class Main {
	
	// 5! = 5 * 4 * 3 * 2 * 1 = 120
	public static int factorial(int number) {
		if(number == 1)
			return 1;
		else
			return number * factorial(number - 1);
		// 5! = 5 * 4! 와 같고 
		// 5! =5 * 4 * 3와 같으니  계속 팩토리얼곱하기
	}	
	
	public static void main(String[] args) {
		

			System.out.println("10 팩토리얼은 "+factorial(10));
	}

}
>
10 팩토리얼은 3628800    
```

- 5! = 5 * 4!
- 5! = 5 * 4 * 3!
- 5! = 5 * 4 * 3 * 2!
- 5! = 5 * 4 * 3 * 2 * 1
- 이러한 순서여서 1이 될때까지 계속 곱해나가면 된다.
- 재귀는 어떠한 값이 들어가면 그 값이 필요한 가장 작은 단위까지 갔다가 거슬러 올라와서 본인 값이 완료되면 다음으로 넘어간다.
- f(5)가 되려면 f(4)가 필요하고 f(4)는 f(3)이 필요하고 f(3)은 f(2)가 필요하고 f(2)는 f(1)이 필요하다. 이렇게 f(1)까지 구했으면 f(2)가 구해지고 그러면 f(5)까지 구할수 있다.