# [동빈나] 13강 - Node.js Express에서 파일 업로드 요청 처리 및 DB에 데이터 삽입

```react
handleFormSubmit = (e) => {
        e.preventDefault()
        this.addCustomer()
            .then((response) => {
                console.log(response.data);
                this.props.stateRefresh();
            })
        this.setState({
            file: null,
            userName: '',
            birthday: '',
            gender: '',
            job: '',
            fileName: '',
            open: false
        })
    }
```

- CosromerAdd.js를 수정해줌

```
npm install --save multer
```

- 사용자 이미지도 업로드해야하기 때문에 새로운 패키지 설치

```react
const multer = require('multer');
const upload = multer({dest: './upload'})
```

- 추가해줌

```react
app.use('/image', express.static('./upload'));
```

- 업로드 폴더 공유함
- 업로드 폴더 접근 가능사용자는 image로 서버는 upload로 인식

```react
app.post('/api/customers', upload.single('image'), (req, res) => {
    let sql = 'INSERT INTO CUSTOMER VALUES (null, ?, ?, ?, ?, ?)';
    let image = '/image/' + req.file.filename;
    let name = req.body.name;
    let birthday = req.body.birthday;
    let gender = req.body.gender;
    let job = req.body.job;
    let params = [image, name, birthday, gender, job];
    connection.query(sql, params, 
        (err, rows, fields) => {
            res.send(rows);
        }
    );
});
```

- null은 id값이기 때문에

- 사용자는 image로 접근하기때문에
  - filename은 multer이 알아서 겹치지 않게 저장
- params에 있는 값이 차례대로 들어감

