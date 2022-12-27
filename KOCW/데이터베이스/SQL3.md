# SQL3

### Outer Join

- 오라클
    - NATURAL : join기준을 알아서 잡는다.
    - LEFT OUTER JOIN
      
        ```sql
        SELECT sid. bid
        FROM Student NATURAL LEFT OUTER JOIN Reserves;
        ```
        
        ```sql
        SELECT sid. bid
        FROM Student S LEFT OUTER JOIN Reserves R ON S.SID = R.SID;
        ```
        
        ```sql
        SELECT S.sid, R.bid
        FROM Strdent S, Reserves R
        WHERE S.sid=R.sid(+);
        ```
        
    - RIGHT OUTER JOIN
        - LEFT에서 LEFT만 RIGHT로 바꾸면 됨
        
        ```sql
        SELECT S.sid, R.bid
        FROM Strdent S, Reserves R
        WHERE S.sid(+)=R.sid;
        ```
        
    - FULL OUTER JOIN
        - LEFT에서 LEFT만 FULL만 바꾸면 됨
        - 양쪽 (+)는 허용하지 않음
- mysql 기준
    - LEFT OUTER JOIN
      
        ```sql
        SELECT sid. bid
        FROM Student NATURAL LEFT OUTER JOIN Reserves;
        ```
        
        ```sql
        SELECT sid. bid
        FROM Student NATURAL LEFT OUTER JOIN Reserves;
        ```
        
    - (+)는 허용하지 않음
    - RIGHT OUTER JOIN은 LEFT에서 RIGHT만 바꾸면 됨
    - FULL OUTER JOIN을 허용하지 않기에 UNION을 사용해서 결합해야 함

### VIEW

- 사용자에게 접근이 허용된 자료만을 제한적으로 보여주기 위해 하나 이상의 기본 테이블로부터 유도된 이름을 가진 가상 테이블
- 저장 장치내에 물리적으로 존재하지 않지만, 사용자에게는 있는 것처럼 간주됨

```sql
CREATE VIEW B-Students (name, sid)
			AS SELECT S.name, S.sid
					FROM Student S
```

- 하나의 뷰를 삭제하면 그 뷰를 기초로 정의된 다를 뷰도 자동으로 삭제됨.

```sql
DROP VIEW B-Students
```

- 보안과 편리성을 위해 사용
- update
  
    ```sql
    CREATE VIEW branch_load as 
    				select loan_number
    				from loan
    ```
    
    ```sql
    insert into branch_loan
    				values ('aa', 'bb')
    ```
    
    - 필요한 값이 없으면 null이 들어감
- 하나의 테이블로만 된 것 가능
- 합으로 이루어진 테이블 view 불가
- primary key가 없으면 view 불가

### DROP TABLE

- CASCADE
    - 같이 삭제
- RESTRICT
    - 제한두기
        - 참고하고 있는 것이 있으면 삭제 불가