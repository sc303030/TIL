# [생활코딩] JAVA1 - 8.3. 데이터 타입의 변환 (casting)

```java
public class Casting {

	public static void main(String[] args) {
		
		double a = 1.1;
		double b = 1;
		System.out.println(b);
		
//		int c = 1.1;
		double d = 1.1;
		int e = (int) 1.1;
		System.out.println(e);
		
		// 1 to String
		String strI = Integer.toString(1);
		System.out.println(strI.getClass());
	}

}
>
1.0
1
class java.lang.String    
```

- 실수로 강제로 변환시키면 손실이 발생한다. 
- 다른 타입으로  변환시 명시적으로 입력해야 한다.