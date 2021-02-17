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

