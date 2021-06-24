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

7:20