# 자바 기초 프로그래밍 강좌 21강 - 다형성 (Java Programming Tutorial 2017 #21)

### 다형성

- 자바는 다형성을 그 특징으로 가지는 객체지향 프로그래밍 언어이다. 
- 자바는 이 다형성을 이용하여 객체를 사용할 때 사용하는 변수형태를 바꾸어 여러 타입의 객체를 참조할 수 있다.
- 부모 클래스 타입의 참조 변수로 하위 클래스의 객체를 참조할  수 있게 해준다.

```java
public class Fruit {
	
	String name;
	int price;
	int fresh;
	
	public void show() {
		System.out.println("이름 : "+ name);
		System.out.println("가격 : "+ price);
		System.out.println("신선도 : "+ fresh);
	}

}
```

```java
public class Peach extends Fruit{
	
	public Peach() {
		price = 1500;
		name = "복숭아";
		fresh = 75;
	}

}
```

```java
public class Main {

	public static void main(String[] args) {
		
		Fruit fruit = new Peach();
		fruit.show();		

	}

}
>
이름 : 복숭아
가격 : 1500
신선도 : 75    
```

- Peach를 만들 때 변수를 초기화 해줘야 한다.
- 자식 인스턴스 클래스를 다양하게 쓸 수 있다.

```java
public class Banana extends Fruit{
	
	public Banana() {
		price = 1000;
		name = "바나나";
		fresh = 80;
	}

}
```

```java
import java.util.Scanner;

public class Main {

	public static void main(String[] args) {
		
		Scanner scanner = new Scanner(System.in);
		System.out.print("바나나 : 1, 복숭아 : 2 ?");
		int input = scanner.nextInt();
		Fruit fruit;
		if(input == 1) {
			fruit = new Banana();
			fruit.show();
		}else if(input == 2){
			fruit = new Peach();
			fruit.show();
		}
				
	}

}
>
바나나 : 1, 복숭아 : 2 ?1
이름 : 바나나
가격 : 1000
신선도 : 80    
```

- 사용자에게 입력받아서 사용할 수 있다.
- 게임캐릭터라면 부모는 게임캐릭터라고 하고 자식 클래스는 전사, 마법사등으로 할 수 있다.
- 부모클래스에 어떠한 틀을 정해놓고 사용할 수 있다.