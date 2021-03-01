# 자바 기초 프로그래밍 강좌 16강 - 상속 ① (Java Programming Tutorial 2017 #16)

### 상속

- 다른 클래스가 가지고 있는 정보를 자신이 포함하겠다는 의미

```java
public class Person {
	
	private String name;
	private int age;
	private int height;
	private int weight;
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public int getAge() {
		return age;
	}
	
	public void setAge(int age) {
		this.age = age;
	}
	public int getHeight() {
		return height;
	}
	public void setHeight(int height) {
		this.height = height;
	}
	public int getWeight() {
		return weight;
	}
	public void setWeight(int weight) {
		this.weight = weight;
	}
	
	public Person(String name, int age, int height, int weight) {
		super();
		this.name = name;
		this.age = age;
		this.height = height;
		this.weight = weight;
	}
}
```

- java는 보안산 private를 만들고 public 함수를 만들어야하기때문에 set와 get을 java에서 private만 만들어도 get, set을 만들어주는 기능을 제공한다.

```java
public class Student extends Person {

	private String studentID;
	private int grade;
	private double GPA;
	
	public String getStudentID() {
		return studentID;
	}
	public void setStudentID(String studentID) {
		this.studentID = studentID;
	}
	public int getGrade() {
		return grade;
	}
	public void setGrade(int grade) {
		this.grade = grade;
	}
	public double getGPA() {
		return GPA;
	}
	public void setGPA(double gPA) {
		GPA = gPA;
	}
	public Student(String name, int age, int height, int weight, String studentID, int grade, double gPA) {
		super(name, age, height, weight);
		this.studentID = studentID;
		this.grade = grade;
		GPA = gPA;
	}
		
}
```

- super는 부모가 가지고 있는 생성자를 실행하겠다는 뜻이다.

```java
public class Main {

	public static void main(String[] args) {
		
		Student student1 = new Student("홍길동", 20, 175, 80, "20170101", 1, 4.5);
		Student student2 = new Student("이순신", 20, 175, 80, "20170101", 1, 4.0);
		student1.show();
		student2.show();
	}

}
>
======================
학생 이름 : 홍길동
학생 나이 : 20
학생 키 : 175
학생 몸무게 : 80
학번 : 20170101
학년 : 1
학점 : 4.5
======================
학생 이름 : 이순신
학생 나이 : 20
학생 키 : 175
학생 몸무게 : 80
학번 : 20170101
학년 : 1
학점 : 4.0    
```

- 이렇게 preson의 정보를 가지고 student를 만들어서 출력해보았다.

