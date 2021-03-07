# 자바 기초 프로그래밍 강좌 20강 - 인터페이스 (Java Programming Tutorial 2017 #20)

- 얼핏보기에는 추상 클래스와 매우 흡사한 개념으로 느껴질 수 있다.
- 인터페이스르 사용하면 다중 상속을 구한하게 해준다.
- 추상화의 정도가 더 높다.

```java
public interface Dog {
	
	abstract void crying();
	public void show() {
		System.out.println("hello world");
	}

}
```

- `public void show()` : 여기서 오류발생
  - 인터페이스는 미리 어떤 코드를 작성하면 오류

```java
abstract class Dog {
	
	abstract void crying();
	public void show() {
		System.out.println("hello world");
	}

}
```

- `public`을 `abstract class`로 바꾸면 오류 발생 안 함.

```java
abstract interface Dog {
	
	abstract void crying();
	public void show();
}
```

- 이렇게 어떤 함수가 존재한다는 것만 알려줘야 한다.

```java
public class Main implements Dog{

	public static void main(String[] args) {
		
		Main main = new Main();
		main.crying();
		main.show();

	}

	@Override
	public void crying() {
		
		System.out.println("월월");
		
	}

	@Override
	public void show() {
		
		System.out.println("hello world");
		
	}

}
>
월월
hello world    
```

- Main 클래스에서 `implements Dog` 로 불러온다.

```java
public interface Dog {
	
	abstract void crying();
	public void one();

}
```

```java
public interface Cat {
	
	abstract void crying();
	public void two();

}
```

```java
public class Main implements Dog, Cat{

	public static void main(String[] args) {
		
		Main main = new Main();
		main.crying();
		main.one();
		main.two();

	}

	@Override
	public void crying() {
		
		System.out.println("월월");
		
	}

	@Override
	public void two() {
		System.out.println("Two");
		
	}

	@Override
	public void one() {
		System.out.println("One");
		
	}


}
>
월월
One
Two
```

- interface를 사용하면 다중 클래스 상속을 받을 수 있다.