# 자바 기초 프로그래밍 강좌 18강 - 추상 (Java Programming Tutorial 2017 #18)

### 자바 객체지향의 활용

- 추상과 인터페이스의 개념이 존재

### 추상

- 자바에서는 일종의 미완성의 클래스라고 할 수 있는 추상 클래스 제공
- 직접적으로 객체 인스턴스를 생성할 수 없음
- 설계로서 틀을 갖추고 클래스를 작성할 수 있게 한다는 특징
- 꼭 상속을 받아야 하고 모든 추상 메소드는 반드시 구현해야한다.

```java
abstract class Player {

}
```

- public를 abstract로 바꿔서 추상이라는 의미를 알려줌.

```java
public class Main extends Player{

	public static void main(String[] args) {
		
		Main main = new Main();
		main.play("펭수 - 펭하");
		main.pause();
		main.stop();

	}

	@Override
	void play(String songName) {
		
		System.out.println(songName + "곡을 재생합니다.");
		
	}

	@Override
	void pause() {
		System.out.println("곡을 일시정지합니다.");
		
	}

	@Override
	void stop() {
		System.out.println("곡을 정지합니다.");
		
	}

}

```

- 추상으로 만들었던걸 반드시 상속 받아야함

- Main을 안에서 지정해줘서 만들어야함

```java
abstract class Animal {
	abstract void crying();

}
```

```java
public class Dog extends Animal{

	@Override
	 void crying() {
		
		System.out.println("월월");
		
	}

}
```

```java
public class Cat extends Animal{

	@Override
	void crying() {
		
		System.out.println("야옹");
		
	}

}
```

```java
public class Main{

	public static void main(String[] args) {
		
		Dog dog = new Dog();
		Cat cat = new Cat();
		dog.crying();
		cat.crying();
		
	}

}
>
월월
```

- 이렇게 Crying는 무조건 상속받아서 작성해줘야 한다.