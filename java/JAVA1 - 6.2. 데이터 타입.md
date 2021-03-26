# JAVA1 - 6.2. 데이터 타입

```java
public class Datatype{
	public static void main(String[] args ) {
		System.out.println(6); //Number
		System.out.println("six"); //String
		
		System.out.println("6"); //String 6
		
		System.out.println(6+6); //12
		System.out.println("6"+"6"); //66
		
		System.out.println(6*6); //36
		//System.out.println("6" * "6"); //ERROR 문자열은 곱하기 연산 불가
		System.out.println("1111".length());
		System.out.println(1111);
	}
}
>
6
six
6
12
66
36
4
1111
```

- `length()`  : 문자열의 길이를 알려준다.

- 데이터 타입별로 연산방법이 있기 때문에 구별하는게 중요하다.