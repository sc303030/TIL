# [동빈나] 14강 - 부모 컴포넌트의 상태(State) 변경을 통한 고객 정보 갱신

- 필요한 부분만 새로고침 하도록 해야함
- 부모컴포넌트에서 자식에게 전달

```react

class App extends Component {

  constructor(props) {
    super(props);
    this.state = {
      customers: '',
      completed: 0,
      searchKeyword: ''
    }
  }

  stateRefresh = () => {
    this.setState({
      customers: '',
      completed: 0,
      searchKeyword: ''
    });
    this.callApi()
      .then(res => this.setState({customers: res}))
      .catch(err => console.log(err));
  }

```

- state를 초기화 해준다.

```react
<CustomerAdd stateRefresh={this.stateRefresh}/>
```

- pros 자체를 넘겨준다.

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

- 응답을 받고 나서 Refresh를 실행해주도록 변경

- 그러면 비동기로 새로고침 됨
- 고객데이터가 많으면 별로 좋지 않음