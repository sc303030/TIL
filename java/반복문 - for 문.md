# 반복문 - for 문

```
for(초기화식;조건식;중감식){
	수행문;
}
```

- ex)

```java
int num;
for(num=1;num <= 5; num++){
	System.out.println(num);
}
```

	1. num=1 먼저 실행됨
 	2. num<=5를 확인
 	3. println(num)을 확인
 	4. num++을 함
      	1. 다시 2로 가서 반복

```java
public class ForTest {

	public static void main(String[] args) {
			
		int count = 1;
		int sum = 0;
		
		for(int i = 0; i<10; i++, count++) {
			sum += count;
		}
		
	}

}
```

- 증가식을 여러개 사용 가능
- for(int i = 0, count=1; i<10; i++, count++) 
  - 초기화식도 여려개 가능하지만 가독성 많이 떨어짐

- 초기화식 생략 가능

  - ```java
    int i = 0;
    for(;i<5;i++){}
    ```

- 조건식 생략 가능

  - ```java
    for(i=0;;i++){
    	sum += i;
    	if(sum>200)break;
    }
    ```

- 증감식 생략 가능

  - ```
    ```

  - 14:04