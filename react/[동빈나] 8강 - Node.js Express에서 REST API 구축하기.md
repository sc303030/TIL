# [동빈나] 8강 - Node.js Express에서 REST API 구축하기 

```react
yarn dev
```

- 서버와 클라이언트가 설치된다.
- 윈도우가 한글이름이면 오류가 발생할 수 있다.

- yarn의 환경변수를 등록해줘도 오류를 해결할 수 있다.

- 데이터를 클라이언트가 가지고 있는게 아니라 필요할 때마다 불러와서 써야한다.

- 제이슨 형태로 보낸다.

### 5000 포트가 열리지 않을 때

```react
yarn add http-proxy-middleware
```

- cilent > src에 setupProxy.js를 생성하고 아래의 내용을 넣어준다.

```react
const createProxyMiddleware  = require('http-proxy-middleware');

module.exports = function(app) {
app.use( '/api', createProxyMiddleware({ target: 'http://localhost:5000', changeOrigin: true,}));};
```

- 바깥폴더에 있는 package.json에 proxy가 있다면 지워주자.

```react
$ yarn add concurrently
```

```react
    "scripts": {
        "server": "nodemon server.js",
        "client": "yarn run start --prefix client",
        "dev": "concurrently \"yarn run server\" \"yarn run client\""
    },
```

- 다음과 같이 패키지를 설치하고 스크립트를 수정하자.

```react
$ yarn run dev
```

- 위와 같은 명령어로 실행하면 드디어 제이슨 파일이 보인다.

### 다시 본문으로

- props는 기본적으로  변경될 수 없는 값을 쓸 때
- state는 변경되는 값을 쓸 때 사용한다.

- componentDidMount
  - 모든 컴포넌트가 마운트 되었을 때 실행되는 것

```react
componentDidMount(){
    this.callApi()
      .then(res => this.setState({customers:res}))
      .catch(err => console.log(err));
  }
  callApi = async () => {
    const response = await fetch('/api/customers')
    const body = await response.json()
    return body;
  }
```

- response
  - 접속하고자 하는 주소의 api를 넣는다.
- body
  - 해당 정보를 json형태로 받겠다.

- this.callApi()
  - return된 body가 this.callapi로 불러와져서 res로 변수이름이 바뀌고 customers변수에 담는다.

