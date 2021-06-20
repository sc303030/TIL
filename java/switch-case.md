# 조건이 여러 개 일 때 간단히 표현되는 switch-case 문

### switch - case 

- if - else 문을 가독성 좋게 표현함
- break문을 사용하여 조건 만족 시 switch 탈출
- 비교 조건이 특정한 값이나 문자열인 경우

```java
switch(month) {
			case 1: day=31;
				break;
			case 2: day =28;
        		break;
```

- case가 끝난 후 break를 주지 않으면 밑에 있는 case를 또 들어가기 때문에 break를 주어야 함
- 그리고 중괄호를 하지 않고 콜론으로 지정

```java
	switch(month) {
			case 1: case 3: case 5: case 7: case 8: case 10: case 12: 
				day=31;
				break;
			case 2: day =28;
				break;
			case 4: case 6: case 9: case 11: 
				day =30;
				break;
		
			default:
				System.out.println("존재하지 않는 달입니다.");
				day = -1;
				
		}
```

- 같은 조건이면 나열해서 써도 됨
- 숫자뿐 아니라 문자열도 비교 가능

- 콜론이 아니라 쉼표로 구분해서 적용 가능해짐 (14부터)

```java
		int day = switch(month) {
			case 1, 3, 5, 7, 8, 10, 12 ->
				31;
			case 4, 6, 9, 11 ->
				30;
			case 2 ->
				28;
			default ->{
				System.out.println("존재하지 않는 달입니다.");
				yield -1;
			}
		};
		System.out.println(month + "월은 : "+ day + "일 입니다.");
```

- 이런식으로 바로 대입가능
- 쉼표로 구분해서 대입 가능

```java
case 1, 3, 5, 7, 8, 10, 12 ->{
				System.out.println("이번달은 31일까지 입니다.");
				yield 31;
			}
```

- 반환값 말고 더 쓰고 싶으면 중괄호를 줘서 안에다 쓰고 yield로 반환값을 지정하면 됨