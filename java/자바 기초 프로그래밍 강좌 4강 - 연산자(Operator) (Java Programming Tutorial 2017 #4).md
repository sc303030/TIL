# 자바 기초 프로그래밍 강좌 4강 - 연산자(Operator) (Java Programming Tutorial 2017 #4)

- 정수를 나타내는 자료형이 많은 이유는 자료형이 차지하는 메모리 공간의 크기가 다르기때문이다.

### 연산자

- 계산의 기본
- 사칙연산이나 나머지

#### 시분초 받아서 출력하기

```java
public class Main {
    final static int SECOND = 1000;
    
    public static void main(String[] args){
        
        int minute = SECOND / 60;
        int second = SECOND % 60;
        
        System.out.println(minute + "분" + second + "초");
    }
}

>
16분 40초    
```

#### 증감 연산자

```java
public class Main {

    public static void main(String[] args){
        
        int a = 10;
        
        System.out.println("현재의 a는" + a + "입니다.");
        a++;
        System.out.println("현재의 a는" + a + "입니다.");
        System.out.println("현재의 a는" + ++a + "입니다.");
        System.out.println("현재의 a는" + a++ + "입니다.");
        System.out.println("현재의 a는" + a + "입니다.");
    }
}

>
현재의 a는 10입니다.
현재의 a는 11입니다.
현재의 a는 12입니다.
현재의 a는 12입니다.
현재의 a는 13입니다.
```

- ++이 앞에 붙으면 출력되기전에 연산이 실행되고 뒤에 붙어있으면 출력된 이후에 연산이 수행된다.

#### %연산자

```java
public class Main {

    public static void main(String[] args){
        
       
        System.out.println(1 % 3);
        System.out.println(2 % 3);
        System.out.println(3 % 3);
        System.out.println(4 % 3);
        System.out.println(5 % 3);
        System.out.println(6 % 3);
       
        
    }
}

>
1    
2
0
1
2
0   
```

#### ==, > , <, &&, ||, ! 알아보기

```java
public class Main {

    public static void main(String[] args){
        
        int a = 50;
        int b = 50;
        
        System.out.println("a와 b가 같은가요?" + (a == b));
        System.out.println("a가 b보다 큰가요?" + (a > b));
        System.out.println("a가 b보다 작은가요?" + (a < b));
        System.out.println("a가 b와 같으면서 a가 30보다 큰가요?" + ((a == b) && (a > 30)));
        System.out.println("a가 50이 아닌가요?" + !(a == 50));
       
    }
}

>
a와 b가 같은가요? true
a가 b보다 큰가요? false    
a가 b보다 작은가요? false    
a가 b와 같으면서 a가 30보다 큰가요? false
a가 50이 아닌가요? false    
```

#### 조건, 참,거짓

```java
public class Main {

    public static void main(String[] args){
        
        int x = 50;
        int y = 60;
        
      
        System.out.println(max(a,y));
       
    }
    // 반환형, 함수이름, 매개 변수
    static int max(int a, int b){
        int result = (a > b) ? a: b;
        // a가 b보다 크다면 a를 넣고 b가 더 크가면 b를 넣는다. true : fasle
        return result;
    }
}

>
60    
```

#### pow() 제곱

```java
public class Main {

    public static void main(String[] args){
        
        dobule a = Math.pow(3.0,20.0);
        
        System.out.println((int) a);
       
    }

}
```

# 자바 기초 프로그래밍 강좌 5강 - 조건문 & 반복문 ① (Java Programming Tutorial 2017 #5)

- ++i 와 i++는 단순히 연산하는 것이면 동일하지만 출력문에서는 다르다.

```java
public class Main {

    public static void main(String[] args){
        
        int i = 20;
        i = i + 1; // i += 1과 동일
        
        System.out.println(i);
        System.out.println((100 < i) && (i < 200));
        System.out.println((100 < i) || (i < 200));
    }

}

>
21    
false   
true    
```

### 조건문 & 반복문

- 조건문
  - 조건에 따라 결정을 내리는 것
- 반복문
  - 반복적으로 같은 처리를 되풀이 하는 것

#### if

- 조건문에 해당되는 가장 기본적인 문법

```java
public class Main {

    public static void main(String[] args){
        
        string a = 'I Love You';
        if(a.contains("Love")){
            // 포함하는 경우
            System.out.println("Me too");
        }else{
            // 포함하지 않는 경우
            System.out.println("I Hate You");
        }

    }

}
>
Me too    
```

- contains : 특정 문자열이 (여기문자)를 포함하고 있는지 확인해주는 함수

- 현재는 "Love"라는 문자열을 포함하고 있기에 `Me too`를 출력

# 자바 기초 프로그래밍 강좌 6강 - 조건문 & 반복문 ② (Java Programming Tutorial 2017 #6)

### 정수에 따라서 다르게 출력

```java
public class Main {

    public static void main(String[] args){
        
        int score = 95;
        
        if(score >= 90){
            System.out.println("A+입니다.");
        }else if(score >= 80{
            System.out.println("B+입니다.");
        }else if(score >= 70{
            System.out.println("C+입니다.");    
        }else{
            System.out.println("F입니다.");   
        }
    }
}
>
A+입니다.
```

### 문자열과 정수형

```java
public class Main {

    public static void main(String[] args){
        
        String a = 'Man';
        
        int b = 0;
        if(a.equals('Man')){
            System.out.println("남자입니다.");   
        }else{
            System.out.println("남자가 아닙니다."); 
        }
        if(b == 3){
            System.out.println("b는 3입니다."); 
        }
        else{
            System.out.println('3이 아닙니다."); 
        }
        if(a.equalsIgnorCase('Man') && b == 0){
            System.out.println("참입니다."); 
        }else{
            System.out.println("거짓입니다."); 
        }
    }
}
>
남자입니다.
3이 아닙니다.
참입니다.                               
```

- 자바는 String을 비교할 때 equal을 이용한다.
- 그 이유는 String은 다른 자료형과 다른 문자열 자료형이다.

- equalsIgnorCase : 대소문자 구분하지 않는다.

### while문

```java
public class Main {

    public static void main(String[] args){
		int i = 1, sum = 0;
        while(i<=1000){
            sum += i++
        }
        System.out.println("1부터 1000까지의 합은"+sum+"입니다.");  
    }
}
>
1부터 1000까지의 합은 500500입니다.    
```

### for문

```java
public class Main {
	
    final static int N = 30;
    
    public static void main(String[] args){
        
		for(int i = N; i>0;i--){
            for(int j = i; j > 0; j--){
                System.out.print("*");
            }
            System.out.println();
        }
    }
}
```

- for문 : 초기화부분, 조건부분, 연산부분

- i는 처음에 30이기에 처음은 30개 별이 출력되고 그 다음에는 29여서 29개가 출력되고 이렇게 하나씩 감소하면서 출력한다.

### for문 원 출력하기

```java
public class Main {
	
    final static int N = 15;
    
    public static void main(String[] args){
        
		for(int i = -N; i <= N; i++){
            
            for(int j = -N; j <= N; j++){
                
                if(i * i + j * j<= N + N){
                    System.out.print('*');
                }else{
                    System.out.print();
                }
                System.out.println();
            }
            
        }
    }
}
```

- 원의 방정식 : x^2 + y^2 = r^2

- 이렇게 하면 원이 출력된다.

# 자바 기초 프로그래밍 강좌 7강 - 기본 입출력(Input & Output) (Java Programming Tutorial 2017 #7)

```java
public class Main {
	
    
    public static void main(String[] args){
        
		int coint = 0;
        
        for(;;){
            System.out.println('출력')
            count++;
            if(count == 10){
                
            }
        }
    }
}

>
출력
 .
 .
 .   
```

### 기본 입출력

- Scanner 글래스를 이용하여 상호작용 할 수 있다.

```java
import java.util.Scanner;
public class Main {
    
    public static void main(String[] args){
        
        Scanner sc = new Scanner(System.in);
        System.out.print('정수를 입력하세요 : ');
        int i = sc.nextInt();
        System.out.println('입력된 정수는 '+i+'입니다.');
        sc.close();

    }
}

>
정수를 입력하세요 : 100
입력된 정수는 100입니다.    
```

- 파이썬의 input()과 비슷하다.

```java
import java.io.File;
import java.io.FileNotFoundException;
import java.util.Scanner;
public class Main {
    
    public static void main(String[] args){
        
        File file = new File('input.txt');
        try {
        	Scanner sc = new Scanner(file);
            whilw(sc.hasNextInt()){
                System.out.println(sc.nextInt() * 100);
            }
        } catch (FileNotFoundException e){
            e.printStackTrace('파일을 읽어오는 도중에 오류가 발생하였습니다.');
        }

    }
}
```

- try, catch : 실행되면 try, 안되면 catch 실행

# 자바 기초 프로그래밍 강좌 9강 - 사용자 정의 함수 ① (Java Programming Tutorial 2017 #9)

### 사용자 정의 함수

- 정해진 특정한 기능을 수행하는 모듈

#### 3개의 수 최대 공약수 찾기

```java
public class Main {
	
	// 반환형, 함수명, 매개변수
	// 3개의 값을 넣어준다.
	public static int function(int a, int b, int c) {
		int min;
		if (a > b) {
			if(b>c) {
				min = c;
			}else {
				min=b;
			}
		}else {
			if (a>c) {
				min = c;
			}else {
				min = a;
			}
		}
		for(int i = min; i >0;i--) {
			if(a % i == 0 && b % i == 0 && c % i == 0) {
				return i;
			}
		}
		return -1;
	}

	public static void main(String[] args) {
		System.out.println("(400,300,750)의 최대 공약수 : " + function(400,300,750));

	}

}
>
(400,300,750)의 최대 공약수 : 50    
```

