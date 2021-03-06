# Sql_시험대비끄적임

- 도메인
  - 엔터티 내에서 속성에 대한 데이터 타입과 크기 그리고 제약사항을 지정하는 것
  - 엔터티 내에서 속성에 대한 NOT NULL을 지정
  - 엔터티 내에서 속성에 대한 Check 조건을 지정
- 반정규화 설명
  - 데이터를 조회할 때 디스트 I/O 량이 많아서 성능이 저하되거나 경로가 너무 멀어 조인으로 인한 성능저하가 예상될 때 수행
  - 컬럼을 계산하여 읽을 때 성능이 저하될 것이 예상되는 경우 수행
  - 기본적으로 데이터 무결성이 깨질 가능성이 많이 있으므로 데이터 무결성을 보장할 수 있는 방법 고려
- 반정규화 기법
  - 반정규화 전에 테이블 추가(통계 테이블, 중복 테이블, 이력테이블 추가)를 통해 반정규화를 회피
  - 중복칼럼을 추가 - 조인감소를 위해 여러 테이블에 동일한 컬럼을 가짐
  - 파생컬럼 추가 - 조회 성능을 우수하게 하기 위해 미리 계산된 컬럼을 갖도록 함
  - 이력 테이블에 기능 컬럼 추가 : FK관계에 해당하는 속성을 추가하여 조인 성능 높임
- 반정규화 대상
  - 자주 사용되는 테이블에 접근하는 프로세스의 수가 많고 항상 일정한 범위만을 조회하는 경우
  - 테이블의 대량의 데이터가 있고 대량의 데이터 범위를 자주 처리하는 경우에 처리범위를 일정하게 줄이지 않으면 성능을 보장할 수 없을 경우
  - 통계성 프로세스에 의해 통계 정보를 필요로 할 때 별도의 테이블을 생성해야 하는 경우
- 발생 시점에 따른 엔터티 분류
  - 매출, 계약, 주문
  - 사원
    - 기본 엔터티
- 발생시점에 따른 엔터티 분류2
  - 기본/키 엔터티 : 조직, 사원, 상품, 부서
  - 중심 엔터티 : 주문 상품
  - 행위 엔터티 : 주문내역, 계약 진행
- 논리적 모델링
  - 데이터 모델링이 최종적으로 완료된 상태라고 정의할 수 있는, 즉 물리적인 스키마 설계를 하기 전 단계를 가리키는 말
- 식별자 분류 체계
  - 스스로 생성여부에 따라 분류되는 식별자는 내부 식별자와 외부 식별자
  - 둘 이상의 속성으로 구성된 식별자를 복합식별자라 하며 속성의 수에 따라 식별자 분류
  - 업무적으로 만들어지지는 않지만 필요에 따라 인위적으로 만든 식별자는 인조 식별자
- 위치 투명성
  - 분산 데이터베이스의 특징 중 사용하려는 데이터의 저장 장소 명시가 불필요하다는 특징
- Row migration과 Row Chaining
  - 로우 길이가 너무 길어서 데이터 블록 하나에 데이터가 모두 저장되지 않고 두 개 이상의 블록에 걸쳐 하나의 로우가 저장되는 형상을  Row Chaining 이라고 함
- Trigger
  - DELETE ON TRIGGER 의 경우 :OLD는 삭제 전 데이터를, :NEW는 삭제후 데이터를 나타냄
  - 특정 테이블에 DML 문이 수행되었을 때 자동으로 동작하도록 작성된 프로그램
  - UPDATE TRIGGER에서 :OLD에는 수정 전, :NEW 에는 수정 후 값이 들어감
- DML, DCL, DDL
  - DDL : CREATE
  - DML : UPDATE
  - DCL : ROLLBACK

- TCL 명렁어
  - COMMIT
- CHARACTER
  - 고정 길이 문자열 정보로 S만큼 최대 길이를 갖고 고정 길이를 가지고 있으므로 할당된 변수 값의 길이가 S보다 작을 경우에는 그 차이 길이 만큼 공간으로 채워진다.

- UNION ALL
  - Set Operation에서 중복 제거를 위해 정렬 작업을 하지 않는 집합 연산자
- Sort Merge Join
  - Set Operation에서 중복 제거를 위해 정렬 작업을 하지 않는 집합 연산자
  - 대용량 데이터를 정렬하여 조인
  - 동등 조인, 비동등 조인에서 모두 사용가능
  - 각 테이블을 정렬한 후 조인
- Cross Join 와 Natural Join 에 대한 차이점
  - Natural Join에서는 특정 Join 컬럼을 명시적으로 적을 수 없음
  - Cross Join은 Join에 참여하는 테이블의 Join Key가 없을 경우 발생
  - Natural Join에서  Join Key는 컬럼명으로 결정
- List
  - 데이터의 양이 매우 많은 대용량 테이블에서 데이터의 생성일자를 구분짓는 특정 컬럼이 없는 형태일 때 적절한 파티셔닝 방법
- FIRST_VALUE () OVER
  - 특정 그룹에서 특정 컬럼으로 정렬된 결과에서 첫 번째 값을 구하는 Window Function
- View
  - 복잡한 질의를 단순하게 작성 가능
  - SQL문을 자주 사용할 때 이용하면 편리하게 사용 가능
  - 사용자에게 정보 숨김 가능
  - 실제 데이터 보유하지 않음
- UPPER
  - 대문자로 변경해주는 함수

- 주식별자 도출하기 위한 기준
  - 해당 업무에서 자주 이용되는 속성을 주식별자로 지정
  - 명칭, 내역 등과 같이 이름으로 기술되는 것들은 가능하면 주식별자로 지정하지 않음
  - 복합으로 주식별자로 구성할 경우 너무 많은 속성이 포함되지 않도록 함

- 주식별자 특징
  - 유일성 : 주식별자에 의해 엔터티 내에서 모든 인스터스들을 유일하게 구분
  - 최소성  : 주식별자를 구성하는 속성의 수는 유일성을 만족하는 최소의 수가 되어야 함
  - 불변성 : 주식별자가 한 번 특정 엔터티에 지정되면 그 식별자의 값은 변하지 않아야 함

- 속성의 특징
  - 엔터티를 설명하고 인스턴스의 구성요소가 됨

- TRUNCATE TABLE 명렁어
  - 특정 로우를 선택하여 지울 수 없음

- PROCEDURE, TRIGGER 
  - PROCEDURE, TRIGGER 모두 CREATE 명령어로 생성함
  - PROCEDURE는 COMMIT, ROLLBACK명령어 사용 할 수 있음
  - TRIGGER 는 COMMIT, ROLLBACK 명령어를 사용할 수 없음

- GROUP 함수 설명
  - CUBE는 결합 가능한 모든 값에 대하여 다차원 집계 생성

- 트랜잭션
  - 원자성 : 트랜잭션에서 정의된 연산들은 모두 성공적으로 실행 되던지 아니면 전혀 실행되지 않은 상태로 남아 있어야함
  - 고립성 : 트랜잭션이 실행되는 도중에 다른 트랜잭션의 영향을 받아 잘못된 결과를 만들어서는 안 됨
  - 지속성 : 트랜잭션이 성공적으로 수행되면 그 트랜잭션이 갱신한 데이터베이스의 내용은 영구적으로 저장됨
  - 일관성 : 트랜잭션이 실행 되기 전의 데이터베이스 내용이 잘못 되어 있지 않다면 트랜잭션이  실행된 이후에도 데이터베이스의 내용에 잘못이 있으면 안 됨
- 순번을 구하는 그룹 함수
  - RANK
  - ROW_NUMBER
  - DENSE_RANK
- 반올림 함수
  - ROUND

- ORDER BY 특징
  - SELECT 구문에 사용되지 않은 컬럼도 OEDER BY 구문에서 사용할 수 있음
  - ORDER BY 1, COL 1과 같이 숫자와 컬럼을 혼용하여 사용 가능
  - ORACLE은 NULL을 가장 큰 값으로 취급하여 ORDER BY시 맨 뒤로 정렬되고 SQL SERVER 반대로 가장 앞으로 정렬함

- 조인 기법
  - Hash Join은 정렬 작업이 없어 정렬이 부담되는 대량배치작업에 유리

- INTERSECT
  - 결과의 교집합으로 중복된 행을 하나의 행으로 표시

- Window Function
  - Sum,max, min 등과 같은 집계 window function을 사용할 때 window 절과 함께 사용하면 집계의 대상이 되는 레코드 범위 지정 가능