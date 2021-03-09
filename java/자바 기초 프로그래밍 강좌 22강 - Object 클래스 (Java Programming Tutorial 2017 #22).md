# 자바 기초 프로그래밍 강좌 22강 - Object 클래스 (Java Programming Tutorial 2017 #22)

- Object라는 클래스가 있다.
- 모든 객체의 조상
- 그 이유는 모든 클래스가 공통으로 포함하고 있어야 하는 기능을 제공하기 위함

```java
public class Archer {

	String name;
	String power;
	
	public Archer(String name, String power) {
		this.name = name;
		this.power = power;
	}
	
	public boolean equals(Object obj) {
		
		Archer temp = (Archer) obj;
		if(name == temp.name && power == temp.power) {
			return true;
		}
		else {
			return fasle;
		}
		
	}
	
}
```

- 아처라는 것과 지금 오브젝트로 받은 값이 같은지 아닌지 확인하다. temp로 아쳐를 다시 받은 다음에 확인한다.
- 오브젝트가 아쳐보다 더 높은 부모이기 때문에 상속받을 수 있다. 
- 그래서 temp로 바꿀 수 있다.

```java
public class Main {

	public static void main(String[] args) {

		Archer archer1 = new Archer("궁수1", "상");
		Archer archer2 = new Archer("궁수2", "중");
		System.out.println(archer1 == archer2);

	}

}
>
false    
```

```java
public class Main {

	public static void main(String[] args) {

		Archer archer1 = new Archer("궁수1", "상");
		Archer archer2 = new Archer("궁수1", "상");
		System.out.println(archer1 == archer2);

	}

}
>
false 
```

- 값이 같은데도 false를 반환한다.
- 두 개의 인스턴스는 다른것이기 때문에 같냐고 물으면 false를 반환한다.

```java
public class Main {

	public static void main(String[] args) {

		Archer archer1 = new Archer("궁수1", "상");
		Archer archer2 = new Archer("궁수1", "상");
		System.out.println(archer1 == archer2);
		System.out.println(archer1.equals(archer2));

	}

}
>
false
true
```

- 아까 만들었던 함수에 대입하면 true를 반환한다.
- 왜냐하면 인스턴스를 비교한게 아니라 인스턴스의 내부의 값이 같은지 물어본 것이기때문이다.