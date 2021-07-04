# [동빈나] 15강 - 고객(Customer) 정보 삭제 기능 구현하기

- DB에서 삭제되었다는 표시를 주려고 테이블 업데이트를 수행

```sql
ALTER TABLE CUSTOMER ADD createDate DATETIME;
ALTER TABLE CUSTOMER ADD isDeleted INT;xxxxxxxxxx UPALTER TABLE CUSTOMER ADD createDate DATETIME;ALTER TABLE CUSTOMER ADD isDeleted INT;
```

```sql
UPDATE CUSTOMER SET createDate = NOW();
UPDATE CUSTOMER SET isDeleted = 0;
```

- 그러면 테이블이 업데이터되어서 값이 들어가있음

```react
class CustomerDelete extends React.Component {
        deleteCustomer(id) {
        const url = '/api/customers/' + id;
        fetch(url, {
            method: 'DELETE'
        });
        this.props.stateRefresh();
    }
	render() {
        return (
            <div>
                <Button variant="contained" color="secondary" onClick={this.handleClickOpen}>삭제</Button>)}
}
```

- CustomerDelete를 만들어주고 삭제하는 api도 만들어 줌
- 삭제 후 새롭게 바뀐 목록을 비동기로 다시 표시

- 버튼을 클릭하면 해당 함수가 실행되도록 함

```react
<TableCell><CustomerDelete stateRefresh={this.props.stateRefresh} id={this.props.id}/></TableCell>
```

- Customer.js에 다음을 추가함
- 부모에서 넘어온 stateRefresh를 그대로 넘겨준다.

```react
app.delete('/api/customers/:id', (req, res) => {
    let sql = 'UPDATE CUSTOMER SET isDeleted = 1 WHERE id = ?';
    let params = [req.params.id];
    connection.query(sql, params,
        (err, rows, fields) => {
            res.send(rows);
        }
    )
});
```

- server.js에 삭제하는 구문을 추가
- 쿼리를 클라이언트에 넘겨줌

```react
app.get('/api/customers', (req, res) => {
    connection.query(
      "SELECT * FROM CUSTOMER WHERE isDeleted = 0",
      (err, rows, fields) => {
          res.send(rows);
      }  
    );
});
```

- 가져올때 0인것만 가져오도록 변경

```react
app.post('/api/customers', upload.single('image'), (req, res) => {
    let sql = 'INSERT INTO CUSTOMER VALUES (null, ?, ?, ?, ?, ?, now(), 0)';
    let image = '/image/' + req.file.filename;
    let name = req.body.name;
    let birthday = req.body.birthday;
    let gender = req.body.gender;
    let job = req.body.job;
    let params = [image, name, birthday, gender, job];
    console.log(params)
    connection.query(sql, params, 
        (err, rows, fields) => {
            res.send(rows);
        }
    );
});
```

- 생성할 때도 기본값으로 현재 시간과 0을 넣어줌