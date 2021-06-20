# while문

- 조건이 맞는 동안 수행문을 반복적으로 수행
- 조건이 맞지 않으면 수행 중단
- 수행문을 수행하기 전 조건을 확인하고 조건이 true인 동안 반복적으로 수행

 ```java
 package ch17;
 
 public class WhileTest {
 
 	public static void main(String[] args) {
 		
 		int num = 1;
 		int sum;
 		
 		while(num <= 10) {
 			sum += num;
 			num++;
 		}
 		System.out.println(sum);
 		System.out.println(num);
 	}
 
 }
 >
 55
 11
 ```

- 지역변수는 자동으로 초기화가 되지 않기 때문에 sum에서 오류 발생

```java
int sum = 0;
```

- sum값을 변경

# do - while 문

- 조건을 먼저 확인하지 않고 수행문을 먼저 수행 한 후 조건을 확인 함
- 조건이 맞지 않으면 더 이상 수행하지 않음
- 1:53