# break 문

- 제어문 빠져나오기
- 반복문 중첩 시 break가 있는 반복문만 빠져 나옴

```java
for(num = 1;sum<=100 ; num++) {
			sum += num;
}
```

- 이런식으로 하면 num이 ++된 다음에 끝나기 때문에

```java
for(num = 1; ; num++) {
			sum += num;
			if(sum >= 100) {
				break;
	}
}
```

- 이런식으로 내부에서 break를 걸어주면 그 순간 빠져나올 수 있음

# continue 문

- 반복문 값이 참이면 내부 블럭의 다른 내용문을 수행하지 않음
  - 다시 제어문으로 올라가라

```java
public class ContinueTest {

	public static void main(String[] args) {
			
		int num;
		
		for(num=1; num<=100; num++) {
			if(num % 3 !=0) continue;
			System.out.println(num);
		}
		
	}

}
>
9
12
15
18
21
24
27
```

- 3으로 나눠지는 값들을 출력
  - 아닌 것들은 수행하지 않고 다시 제어문으로 올라감