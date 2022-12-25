# SQL2

### 중첩 쿼리

- IN을 사용해서 중첩
- 괄호안에 있는 구문을 먼처 처리
- ex) s.sid가 결과로 리턴받은 값에 포함 되는지
  
    ```sql
    SELECT s.name
    FROM Student s
    WHERE s.sid IN (SELECT R.sid
    								FROM Reserves R
    								WHERE R.bid = 103)
    ```
    
- 포함되지 않은 것은 NOT IN
- EXISTS
    - 존재하는지 찾는 것의 또 다른 방법
    
    ```sql
    SELECT S.name
    FROM Student S
    WHERE EXISTS (SELECT *
    						  FROM Reserves R
    							WHERE R.bid=103 AND S.sid=R.sid)
    ```
    
- 존재하지 않는 것이면 NOT EXISTS
- 연산자 옆에 ANY 나 ALL 사용 가능
    - any는 집합 중 하나만 성립해도 TRUE
      
        ```sql
        SELECT *
        FROM Student S
        WHERE S.rating > ANY (SELECT S.rating
        											FROM Student S
        											WHERE S.name='aaa')
        ```
        
    - ALL은 모두 성립해야 TRUE
      
        ```sql
        SELECT *
        FROM Student S
        WHERE S.rating > ALL (SELECT S.rating
        											FROM Student S
        											WHERE S.name='aaa')
        ```
        

### Aggregate 연산

- AVG(평균)
  
    ```sql
    SELECT AVG(S.rating)
    FROM Student S
    ```
    
- 최댓값
  
    ```sql
    SELECT S.name
    FROM Student S
    WHERE S.age = (SELECT MAX(S.age)
    								FROM Student S)
    ```
    
- 개수
  
    ```sql
    SELECT COUNT(*)
    FROM Student S
    ```
    
- 중복제거
  
    ```sql
    SELECT COUNT(DISTINCT S.name)
    FROM Student S
    ```
    

### GROYP BY, HAVING

- 그룹짓기. 그룹의 조건
- group by
    - select에 언급된 속성으로만 그룹핑 가능
    
    ```sql
    SELECT S.rating, S.name
    FROM Student S
    GROUP BY S.rating
    ```
    
- having
  
    ```sql
    SELECT S.rating, S.name
    FROM Student S
    GROUP BY S.rating
    HAVING S.rating > 80
    ```
    
- 순서
    1. FROM
    2. WHERE
    3. GROUP BY
    4. HAVING
    5. SELECT

### ORDER BY

- 오름차순, 내림차순
    - 기본은 오름차순
- 오름차순(ASE)
    - ASE는 써도 되고 안 써도 되고
    
    ```sql
    SELECT S.rating
    FROM Student S
    ORDER BY S.raing
    ```
    
- 내림차순
  
    ```sql
    SELECT S.rating
    FROM Student S
    ORDER BY S.raing DESC
    ```