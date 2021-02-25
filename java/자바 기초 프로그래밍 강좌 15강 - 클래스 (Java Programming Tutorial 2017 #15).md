# 자바 기초 프로그래밍 강좌 15강 - 클래스 (Java Programming Tutorial 2017 #15)

### 클래스

- 객체 지향의 기본
- 대표적으로 많이 사용되는게 Node 클래스
- 사물들의 속성을 묶는게 클래스 -> 이걸 변수로 사용하는게 인스턴스

### Node 만들기

```java
public class Node {
	
	private int x;
	private int y;
	
	public int getX() {
		return x;
	}
	public void setX(int x) {
		this.x = x;
	}
	public int getY() {
		return y;
	}
	public void setY(int y) {
		this.y = y;
	}
	public Node(int x, int y) {
		this.x = x;
		this.y = y;
	}
	public Node getCenter(Node other) {
		return new Node((this.x + other.getX()) / 2, (this.y + other.getY()) / 2);	
	}
}
```

- private 
  - 외부에서 변경 못하도록 private으로 지정한다.
- public
  - 그래서 이렇게 만들어서 외부에서도 사용가능하게 만든다.

- this.x = x
  - 이 함수에서 들어온 x의 값을 기존에 있던 x의 값에 변경시킬때 this를 사용

- public Node()
  - 생성자는 클래스랑 이름이 같다.
  - this.x = x;
    - 초기화해준다.

- public Node getCenter(Node other) 
  - Node의 값과 다른 값을 비교하여 중심을 알려준다.

### Main에서 실행하기

```java
public class Main {

	public static void main(String[] args) {
		
		Node one = new Node(10,20);
		Node two = new Node(30,40);
		
		Node result = one.getCenter(two);
		System.out.println("x : " + result.getX() + ", y : " + result.getY()); 

	}

}
```

- (10,20) 하나랑 (30,40)하나 만들어서 중앙을 구한다.
- one.getCenter했으니 one의 값이 Node가 되고 two가 other가 되어서 계산된다.