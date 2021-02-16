# 자바 기초 프로그래밍 강좌 1강 - Hello World! (Java Programming Tutorial 2017 #1)

- 이클립스에서 프로젝트 생성 후 클래스를 생성한다.
  - 프로젝트 생성 시 첫 글자는 대문자, 공백없이 하는것을 추천
- 클래스에서 가장 먼저 실행되는 이름이 Main
  - public ~ 자동으로 Main를 만들어 준다.

# 자바 기초 프로그래밍 강좌 2강 - 변수(Variable) (Java Programming Tutorial 2017 #2)

### 변수

- 프로그램이 실행되는 동안에 언제든지 저장된 값이 변경될 수 있는 공산
- ex)열린 상자
  - 값이 계속 들어가거나 나올 수 있다.

### 상수

- 한 번 정해지면 값을 변경할 필요가 없는 데이터
- ex) 파이값 3.14~
- ex) 닫힌 상자
  - 값이 못들어가고 못나옴

### 변수 선언하기

```java
public class Main {
	public static void main(String[] args) {
	
		int intType = 100;
		double doubleType = 150.5;
        String stringType - "펭수"
            
       System.out.println(intType);
	}
}
```

- int : 정수자료형
- intType : 변수 이름
- double : 실수형
- String : 문자형

- println : ln은 줄바꿈하라는 뜻

### 상수 선언하기

```java
public class Main {
    
    final static double PI = 3.141592; 
        
	public static void main(String[] args) {
	
        	int r = 30;
        	System.out.printin(r * r * PI);
		
	}
}
```

- 상수는  메인 함수 밖에 만든다.
- final : 한 번 선언되면 절대 바뀔 수 없다는 뜻
- static : 클래스에서 공유되는 자원

### 오버플로우

```java
public class Main {
    
    final static int INT_MAX = 2147483647;
        
	public static void main(String[] args) {
	
        	int a = INT_MAX;
        	System.out.printin(a + 1);
		
	}
}
```

- INT_MAX = 2147483647 : int가 가질 수 있는 가장 큰값

- a를 출력 할 시 `2147483647`.
- a+1을 출력하면 `-214783648`
- int 는 -21 ~ 21억까지
  - 범위를 넘어서면 컨트롤 할 수 없어서 최댓값보다 크면 최솟값보다 작아진다.

### 사칙연산 프로그램

```java
public class Main {
        
	public static void main(String[] args) {
	
        	int a = 1;
        	int b = 2;
			System.out.printin("a + b=" + (a + b));
	        System.out.printin("a - b=" + (a - b));
        	System.out.printin("a * b=" + (a * b));	
        	System.out.printin("a / b=" + (a / b));
	}
}
```

- ("a+b=" + (a+b)) 
  - 앞에 ""안에 값은 문자열로 +뒤에 ()은 우리가 선언한 변수들의 연산 결과값이 출력된다.
- 나누기는 몫만 출력

# 자바 기초 프로그래밍 강좌 3강 - 자료형(Data Type) (Java Programming Tutorial 2017 #3)

- 자바는 변수 초기화를 하지 않으면 사용할 수 없다.

```java
public class Main {
        
	public static void main(String[] args) {
	
        	int a = (int) 0.5;
        	
	}
}
```

- 오류 발생
  - int는 정수형이라 실수를 넣으면 오류
  - (int)로 형변환 해주면 사용 가능
    - 0으로 나옴 
    - 실수 -> 정수로 바꾸면 소수점 뒤에 자리 버려짐

```java
public class Main {
        
	public static void main(String[] args) {
			
        	double b = 0.5;
        	int a = (int) (b + 0.5);
        
        	
	}
}
```

- 실수 값을 반올림 할 때는 변수에 0.5를 더한 뒤에 정수형으로 형변환하여 계산
- 항상 반올림 된 값이 출력된다. 

### 자료형

- 원시형

- 비원시형

```java
public class Main {
        
	public static void main(String[] args) {
			
        	double a = 10.3;
        	double b = 9.6;
        	double c = 10.1;
        
        	System.out.printin((a + b + c ) / 3);
        	
	}
}
```

- `출력값 : 10.0`

```java
public class Main {
        
	public static void main(String[] args) {
			
        	for(char i = 'a'; i <= 'z'; i++){
        		System.out.printin(i + " ");
        	}
	}
}
```

- `출력값 : a b c d e f g h i j k l n m o p q r x t u v w x y z`
-  내부적으로 아스키코드가 사용되어서 1씩 증가하여 알파벳으로 보여짐

```java
public class Main {
        
	public static void main(String[] args) {
			
        	int a = 200;
        	System.out.printin("10진수 : "+ a);
        
	}
}
```

- `출력값 : 10진수 200`

```java
public class Main {
        
	public static void main(String[] args) {
			
        	int a = 200;
        	System.out.printin("10진수 : "+ a);
        	System.out.format("8진수 : %o\n : ", a);
	        System.out.format("16진수 : %x : ", a);
        
	}
}
```

- a가 %o에 들어간다.
  - 200을 8진수로 바꿔서 출력하라.

- a가 %x에 들어간다.
  - 200을 16진수로 바꿔준다.
- \n : 개행인자

```java
public class Main {
        
	public static void main(String[] args) {
			
        	String name = 'pengsoo'
            System.out.printin(name);    
        
	}
}
```

- `출력값 : pengsoo`

```java
public class Main {
        
	public static void main(String[] args) {
			
        	String name = 'pengsoo'
            System.out.printin(name.subString(0,1));    
        
	}
}
```

- `출력값 : p`

- 0부터 증가해서 1까지만 출력이니 p만 출력
- 파이썬 리스트 접근이랑 똑같이 마지막 -1까지 출력된다. 