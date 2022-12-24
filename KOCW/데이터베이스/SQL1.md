# SQL1

분기: 4/4
최종 편집일시: 2022년 12월 24일 오후 6:48

### 기본 sql 쿼리

- SELECT : 관계 대수의 projection연산에 해당
    - 결과로 얻고 싶은 속성
- DISTINCT
    - 중복 제거
    - 첫 번째 나오는 행을 선택
    - 만약 컬럼을 2개 썼는데 하나만 중복이면 출력되고 2개 컬럼 모두 중복이어야 제거
      
        ```sql
        SELECT DISTINCT s.sid, s.name
        ```
    
- FROM : 관계 대수의 cartesian product 연산
    - 릴레이션
- WHERE : 관계 대수의 selection 연산에 해당

### 순서

1. FROM
2. WHERE
3. SELECT

### 수식

- 글자 비교
    - `_%`
        - `_` 는 1개의 문자
        - `%` 0 또는 더 많이
    
    ```sql
    WHERE S.name LIKE 'A_%A'
    ```
    
- UNION은 중복이 있으면 제거
    - 중복 허용은 UNION ALL
- INTERSECT를 사용하면 교집합으로 값 구할 수 있음
- MINUS는 차집합