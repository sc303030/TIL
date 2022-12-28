# SQL4

### 시스템 카탈로그

- 데이터베이스에 포함된 다양한 데이터 객체에 대한 정보들을 유지, 관리하기 위한 시스템 데이터베이스
- DDL로 구성되는 기본테이블, 뷰, 인덱스, 제약조건 등의 데이터베이스 구조 및 통계 정보를 저장
- 데이터 사전이라고도 함
- 카탈로그에 저장된 정보를 메타 데이터라고도 함
- 시스템 테이블로 구성되어 있어 일반 이용자도 SQL을 이용하여 내용을 검색해 볼 수 있음
- 카탈로그를 갱신하는 것은 허용되지 않음
- DBMS가 스스로 생성하고 유지함
- 종류
    - USER_OBJECTS
        - 테이블 뷰, 제약 조건, 인덱스, 프로시져, 트리거 등 검색
        
        ```sql
        SELECT * FROM USER_OBJECTS;
        ```
        
    - USER_TABLES
        - 생성된 테이블 검색
        
        ```sql
        SELECT * FROM USER_TABLES;
        ```
        
    - USER_VIEWS
        - 뷰 이름, 검색 범위, 컬럼이 참조한 테이블
        
        ```sql
        SELECT * FROM USER_VIEWS;
        ```
        
    - USER_CONSTRAINTS
        - 제약조건 이름, 적용된 테이블, 상태(활성화 유무)
        
        ```sql
        SELECT * FROM USER_CONSTRAINTS;
        ```
        
    - USER_INDEXES
        - 인덱스 이름, 적용된 테이블
        
        ```sql
        SELECT * FROM USER_INDEXES;
        ```
        

### 권한

- read, insert, update, delete
- 스키마
    - index, resources, alteration, drop
- 명렁어
  
    ```sql
    grant <pricilege list> on <relation name or view name> to <user list>
    ```
    
    - ex ) select
      
        ```sql
        grant select on branch to u1, u2, u3
        ```
    
- 회수
  
    ```sql
    revoke <privilege list> on <relation name or view name> from <user list>
    ```
    
    ```sql
    revoke select on branch from u1, u2, u3
    ```
    

### Embedded SQL

- 일반 응용 프로그램에 SQL을 삽입하여 데이터베이스 자료를 이용하고 다양하 조작을 할 수 있도록 한 것
- EXEC SQL문으로 시작하여 세미콜론으로 종료(일반적으로)
- 일반 SQL문은 실행 후 여러 자료를 얻을 수 있지만 내장 SQL문은 하나의 자료만 얻을 수 있음
    - 커서 : 튜플들의 집합을 처리하는데 사용되는 포인터 역할
- 호스트 언어에 데이터베이스의 자료를 불러와 기억하기 위한 호스트 변수가 필요함
- 호스트 변수를 사용하려면 BEGIN DECLARE SECTION ~ END DECLARE SECTION을 통해 선언된어야 함
- 호스트 변수는 구분을 위해 콜론을 변수명 앞에 붙임

### 일반적인 제약

```sql
CREATE TABLE Students
			(sid INTEGER
				CHECK (rating >=1 AND RATING <=10))
```

- 테이블 생성 시 제약조건을 적용하지 않았다면, 생성 이후에 필요에 의해서 제약조건을 추가 할 수 있음
  
    ```sql
    ALTER TABLE Player
    ADD CONSTRAINT Player_FK
    FOREIGN KEY (Team_ID) REFERENCES Team(Team_ID)
    ```
    
- 테이블 생성 시 혹은 생성 이후에 부여했던 제약조건을 삭제할 수 있음
  
    ```sql
    ALTER TABLE Player
    DROP CONSTRAINT Player_FK
    ```
    

### 트리거

- 특정한 동작이 수행되면 발생함
- Event
    - DB에 변화가 생기면 발생
- Condition
- Action

```sql
CREATE TRIGGER init_count BEFORE INSERT ON Student
	DECLARE
		count INTEGER;
	BEGIN
		count := 0;
	END 
```

```sql
CREATE TRIGGER incr_count AFTER INSERT ON Student
	WHEN (new.age < 20)
	FOR EACH ROW
	BEGIN
		count := count +1;
	END
```