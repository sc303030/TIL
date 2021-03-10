# 자바 기초 프로그래밍 강좌 23강 - 객체 지향 기법의 활용 (Java Programming Tutorial 2017 #23)

```java
public class Hero {
	
	String name;
	
	public Hero(String name) {
		this.name = name;
	}
	
	public void attack() {
		System.out.println("주먹 지르기!");
	}
}
```

```java
public class Warrior extends Hero{

	public Warrior(String name) {
		super(name);
		
	}
	
	public void groundCutting() {
		System.out.println("대지가르기!");
	}
}
```

- super은 자신의 부모 클래스의 기본적인 생성자를 초기화 해준다.

```java
public class Archer extends Hero{

	public Archer(String name) {
		super(name);
		
	}
	
	public void fireArrow() {
		System.out.println("불화살!");
	}

}
```

```java
public class Wizard extends Hero{

	public Wizard(String name) {
		super(name);
	}
	
	public void freezing() {
		System.out.println("얼리기!");
	}

}
```

```java
public class Main {

	public static void main(String[] args) {
		
		Hero[] heros = new Hero[3];
		
		heros[0] = new Warrior("전사");
		heros[1] = new Archer("궁수");
		heros[2] = new Wizard("마법사");
		
		for(int i = 0; i < heros.length; i++) {
			heros[i].attack();
			
			if(heros[i] instanceof Warrior) {
				Warrior temp = (Warrior) heros[i];
				temp.groundCutting();
			}
			else if(heros[i] instanceof Archer) {
				Archer temp = (Archer) heros[i];
				temp.fireArrow();
			}
			else if(heros[i] instanceof Wizard) {
				Wizard temp = (Wizard) heros[i];
				temp.freezing();
			}
		}
	}

}
>
주먹 지르기!
대지가르기!
주먹 지르기!
불화살!
주먹 지르기!
얼리기!    
```

- 배열을 생성해서 거기에 직업을 하나씩 넣어준다. 인스턴스를 초기화 해준다.

- `if(heros[1] instanceof Warrior)` 
  - 지금 접근하고 있는 인스턴스가 전사가 맞는지 확인하기

- `Warrior temp = (Warrior) heros[i];`
  - temp에 담아서 만들기

- 배열이 돌면서 해당 인덱스에 해당되는 캐릭터의 속성을 실행한다.