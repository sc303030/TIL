# 자바 기초 프로그래밍 강좌 19강 - Final 키워드 (Java Programming Tutorial 2017 #19)

### 최종

- 절대로 변하지 않는 특정한 것을 정할 때 final을 쓴다.
- 변수, 메소드, 클래스에 모두 사용 가능
- 변수에 사용할 경우 변하지않는 상수

```java
public class Main {

	public static void main(String[] args) {
		 
		final int number = 10;
	}

}

```

- number은 완전한 상수가 된다.

```java
public class Parent {
	
	public void show() {
		System.out.println("hi");
	}

}
```

```java
public class Main extends Parent{
	
	public void show() {
		System.out.println("hello");
	}
	public static void main(String[] args) {
		 
		Main main = new Main();
		main.show();
		
	}

}
```

- 부모에서 상속받은걸 오버라이딩헤서 다시 바꿀수 있다. 오버라이딩 없으면 상속받은 문자가 출력된다.

```java
public class Parent {
	
	public final void show() {
		System.out.println("hi");
	}

}
```

- final을 붙이면 상속받은 main클래스에서 show의 재정의가 불가능해진다.

```java
final class Parent {
	
	public final void show() {
		System.out.println("hi");
	}

}
```

- class에 final을 붙이면 main에서 상속받을 때 오류가 발생한다.

![java01](../img/java01.png)

- final클래스는 파란색 삼각형으로 알려준다.