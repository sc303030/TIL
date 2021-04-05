# [동빈나] 6강 - React의 LifeCycle과 API 호출

```react
class ApiExample extends React.Component {
  constructor(props){
    super(props);
    this.state = {
      data: ''
    }
  }
  callApi = () => {
    fetch('https://jsonplaceholder.typicode.com/todos/1')
      .then(res => res.json())
      .then(json => {
      this.setState({
      data:json.title
    });
    });
  }
  componentDidMount(){
    this.callApi(); 
  }
render(){
  return (
  <h3>
    {this.state.data? this.state.data : '데이터를 불러오는 중입니다.'}
  </h3>
  );
}
}

ReactDOM.render(<ApiExample/>, document.getElementById('root'));
```

- render함수 다음에 componentDidMount 함수가 실행된다.
- 이러한 구조를 알면 api를 사용할 때 편하다.

