# 11강 - 고객(Customer) DB 테이블 구축 및 Express와 연동하기

### 테이블 생성

```sql
CREATE TABLE CUSTOMER (
	id INT PRIMARY KEY AUTO_INCREMENT;
	image VARCHAR(1024),
	name VARCHAR(64),
	birthady VARCHAR(64),
	gender VARCHAR(64),
	job VARCHAR(64)
) DEFAULT CHARACTER SET UTF8 COLLATE UTF8_GENERAL_CI;
```

- /* SQL 오류 (1064): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near '' at line 2 */
- 오류가 발생함

```sql
CREATE TABLE CUSTOMER (
	id INT PRIMARY KEY AUTO_INCREMENT,
	image VARCHAR(1024),
	name VARCHAR(64),
	birthady VARCHAR(64),
	gender VARCHAR(64),
	job VARCHAR(64)
) DEFAULT CHARACTER SET UTF8 COLLATE utf8_general_ci;
```

- `id INT PRIMARY KEY AUTO_INCREMENT,` 로 바꿔주었다. 처음에는 콤마대신에 ; 이게 들어가있었다.....

```sql
INSERT INTO CUSTOMER VALUES (1, 'https://img1.daumcdn.net/thumb/R720x0/?fname=https://t1.daumcdn.net/news/201910/29/iMBC/20191029021632844hejm.png',
'이길동', '999999','남자','대학생');
INSERT INTO CUSTOMER VALUES (2, 'https://img1.daumcdn.net/thumb/R720x0/?fname=https://t1.daumcdn.net/news/201910/29/iMBC/20191029021632844hejm.png',
'홍길동', '999999','남자','대학생');
INSERT INTO CUSTOMER VALUES (3, 'https://img1.daumcdn.net/thumb/R720x0/?fname=https://t1.daumcdn.net/news/201910/29/iMBC/20191029021632844hejm.png',
'하길동', '999999','여자','대학생');
```

- db에 데이터를 추가한다.

```
# database
/database.json
```

- db에 관련된 정보는 깃에 올라가면 안되기 때문에 gitignore에 추가한다.

```
{
    "host":데이터베이스주소,
    "user":데이터베이스사용자이름,
    "password":데이터베이스비밀번호,
    "port":3306,
    "database":데이터베이스이름
}
```

- database.json 내용

```react
# database.json 불러오는 라이브러리 불러오기
const fs = require('fs');

# 파일 읽기
const data = fs.readFileSync('./database.json');

#database.json파일이니 json으로 파싱하기
const conf = JSON.parse(data);

# mysql과 연결하기
const mysql = require('mysql');

# 저장했던 환경설정 지정해주기
# mysql에 필요한 속성값들을 위에서 불러온 파일에서 가져와 지정
const connection = mysql.createConnection({
  host:conf.host,
  user:conf.user,
  password:conf.password,
  port:conf.port,
  database:conf.database
})

# 실제로 연결하기
connection.connect();

# 접속해서 쿼리문 날리기
app.get('/api/customers', (req, res) => {
  connection.query(
      # 쿼리문 날리기
    "SELECT * FROM CUSTOMER",
      # 실제로 가져온 데이터는 rows에 담김
    (err, rows, fields) => {
      # 사용자에게 보여주기
      res.send(rows);
    }
  );
});
```

- serve.js 수정

10분
