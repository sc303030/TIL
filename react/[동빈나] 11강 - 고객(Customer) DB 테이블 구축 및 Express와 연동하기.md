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

