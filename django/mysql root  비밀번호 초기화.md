### mysql root  비밀번호 초기화

1. mysql 서비스 종료

2. ```
   $ where mysql 
   C:\Program Files\MySQL\MySQL Server 8.0\bin\mysql.exe
   ```

3. cd `C:\Program Files\MySQL\MySQL Server 8.0\bin`

4. ```
   $ mysqld.exe --skip-grant-tables --console --shared-memory
   ```

5. 이동한 경로에서

```
mysqld.exe --skip-grant-tables --console --shared-memory
```

5.1	에러가 발생하면

```
mysqld --initialize --console
```

```
;MdOyd1XyTwU
```

다시 

```
mysqld.exe --skip-grant-tables --console --shared-memory
```

6. ```
   mysql -u root
   ```

7. ```
   use mysql;
   UPDATE user SET authentication_string=null WHERE User='root';
   select authentication_string from user;
   flush privileges;
   ```

8. ```
   mysql -u root
   ```

   ```
   ALTER USER 'root'@'localhost' IDENTIFIED WITH caching_sha2_password BY 'test1234';
   ```

### 유저 생성하기

```
create user '유저이름'@'%' identified by '비밀번호';
```

### db 생성하기

```
CREATE DATABASE 테이블이름 default CHARACTER SET UTF8;
CREATE DATABASE tossdb default CHARACTER SET UTF8;
```

### 유저에게 db 권한주기

```
grant all privileges on 테이블명.* to '유저명'@'localhost' WITH GRANT OPTION;
grant all privileges on tossdb.* to 'root'@'localhost' WITH GRANT OPTION;
```

### 변경된 내용 저장

```
flush privileges;
```

### 테이블 삭제, 유저 삭제

```
drop user '사용자ID'@'localhost';  
drop database DB명;
```

### 계속 저장하기

```
select authentication_string,host from mysql.user where user='root';
UPDATE mysql.user SET authentication_string='' WHERE user='root';

alter user 'root'@'localhost' identified with mysql_native_password by 'test1234';
```

### 껐다 켜도 비번 변경되지 않게 하기

```
net stop MySql80

mysqld --datadir="C:/ProgramData/MySQL/MySQL Server 8.0/Data" --skip-grant-tables --console --shared-memory

mysql -u root

use mysql;
UPDATE user SET authentication_string=null WHERE User='root';
select authentication_string from user;
flush privileges;

mysql -u root
ALTER USER 'root'@'localhost' IDENTIFIED WITH caching_sha2_password BY 'test1234';
flush privileges;

net start mysql80
```

```
update mysql.user set authentication_string='1234' where user='root';
```

### 도커 mysql 실행

```
docker run -d -p 3306:3306 -e MYSQL_ROOT_PASSWORD=1234 --name mysql8 mysql:latest --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci90e995884173014b15ff0db0035de80eaf62965f5631d35e71870564068e2c4e
```

