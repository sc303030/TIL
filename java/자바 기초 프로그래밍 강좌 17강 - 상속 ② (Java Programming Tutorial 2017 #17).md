# 자바 기초 프로그래밍 강좌 17강 - 상속 ② (Java Programming Tutorial 2017 #17)

```java
public class Teacher1 extends Person{
	
	private String teacherID;
	private int monthSalary;
	private int workedYear;
	
	public String getTeacherID() {
		return teacherID;
	}
	public void setTeacherID(String teacherID) {
		this.teacherID = teacherID;
	}
	public int getMonthSalary() {
		return monthSalary;
	}
	public void setMonthSalary(int monthSalary) {
		this.monthSalary = monthSalary;
	}
	public int getWorkedYear() {
		return workedYear;
	}
	public void setWorkedYear(int workedYear) {
		this.workedYear = workedYear;
	}
	public Teacher1(String name, int age, int height, int weight, String teacherID, int monthSalary, int workedYear) {
		super(name, age, height, weight);
		this.teacherID = teacherID;
		this.monthSalary = monthSalary;
		this.workedYear = workedYear;
	}
	
	public void show() {
		System.out.println("-----------------------");
		System.out.println("교사 이름 : " + getName());
		System.out.println("교사 나이 : " + getAge());
		System.out.println("교사 키 : " + getHeight());
		System.out.println("교사 몸무게 : " + getWeight());
		System.out.println("교직원 번호 : " + getTeacherID());
		System.out.println("교사 월급 : " + getMonthSalary());
		System.out.println("교사 연차 : " + getWorkedYear());
	}
}
```

- private를 만들고 public를 만든다.
- 그리고 source에서 construct를 이용하여 초기화 함수를 삽입한다.
- 최종으로 출력할 show를 만든다.

```java
public class Main {

	public static void main(String[] args) {
		
		Student student1 = new Student("홍길동", 20, 175, 80, "20170101", 1, 4.5);
		Student student2 = new Student("이순신", 20, 175, 80, "20170101", 1, 4.0);
		student1.show();
		student2.show();
		Teacher1 teacher1 = new Teacher1("John Doe", 30, 170, 80, "20090505", 3000000, 5);
		teacher1.show();
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
-----------------------
교사 이름 : John Doe
교사 나이 : 30
교사 키 : 170
교사 몸무게 : 80
교직원 번호 : 20090505
교사 월급 : 3000000
교사 연차 : 5    
```

- 그럼 이렇게 출력된다.

#### 많은 양의 데이터 처리

```java
public class Main {

	public static void main(String[] args) {
		
		Student[] students = new Student[100];
		for(int i =0; i < 100; i++) {
			students[i] = new Student("홍길동", 20, 175, 70, i+"", 1, 4.5);
			students[i].show();
		}
	
	}

}
>
======================
학생 이름 : 홍길동
학생 나이 : 20
학생 키 : 175
학생 몸무게 : 70
학번 : 0
학년 : 1
학점 : 4.5
======================
학생 이름 : 홍길동
학생 나이 : 20
학생 키 : 175
학생 몸무게 : 70
학번 : 1
학년 : 1
학점 : 4.5    
======================
학생 이름 : 홍길동
학생 나이 : 20
학생 키 : 175
학생 몸무게 : 70
학번 : 98
학년 : 1
학점 : 4.5
======================
학생 이름 : 홍길동
학생 나이 : 20
학생 키 : 175
학생 몸무게 : 70
학번 : 99
학년 : 1
학점 : 4.5    
```

- 배열을 만들어서 관리하는게 훨씬 효율적이다.

#### 입력받아서 출력하기

```java
import java.util.Scanner;

public class Main {

	public static void main(String[] args) {
		
		Scanner scan = new Scanner(System.in);
		System.out.print("총 몇 명의 학생이 존재합니까?");
		int number = scan.nextInt();
		Student[] students = new Student[number];
		for(int i = 0; i < number; i ++) {
			
			String name;
			int age;
			int height;
			int weight;
			String studentID;
			int grade;
			double gPA;
			System.out.print("학생의 이름을 입력하세요 : ");
			name = scan.next();
			System.out.print("학생의 나이를 입력하세요 : ");
			age = scan.nextInt();
			System.out.print("학생의 키를 입력하세요 : ");
			height = scan.nextInt();
			System.out.print("학생의 몸무게 입력하세요 : ");
			weight = scan.nextInt();
			System.out.print("학생의 학번을 입력하세요 : ");
			studentID = scan.next();
			System.out.print("학생의 학년을 입력하세요 : ");
			grade = scan.nextInt();
			System.out.print("학생의 점수를 입력하세요 : ");
			gPA = scan.nextDouble();
			students[i] = new Student( name,  age,  height,  weight,  studentID,  grade,  gPA);
		}
		for(int i = 0; i < number; i++) {
			students[i].show();
		}
	}
}
>
총 몇 명의 학생이 존재합니까?1
학생의 이름을 입력하세요 : 홍길동
학생의 나이를 입력하세요 : 20
학생의 키를 입력하세요 : 180
학생의 몸무게 입력하세요 : 75
학생의 학번을 입력하세요 : 20210303
학생의 학년을 입력하세요 : 3
학생의 점수를 입력하세요 : 4.12
======================
학생 이름 : 홍길동
학생 나이 : 20
학생 키 : 180
학생 몸무게 : 75
학번 : 20210303
학년 : 3
학점 : 4.12    
```

- 사용자에게 입력받아서 인스턴스와 배열을 만든다.
- 그 다음에 받았던 정보를 우리가 만든 class에 다시 넣어서 출력한다.