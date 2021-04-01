# [동빈나] 5강 - React의 State

### state

- 내부적으로 변경될 수 있는 데이터를 처리할 때 효율적으로 사용

```react
class Clock extends React.Component {
  constructor(props){
    super(props);
    this.state=
  }
  render(){
  return (
  <h3>현재 시각은 [{this.props.date.toLocaleTimeString()}] 입니다.</h3>
  );
}
}
function tick() {
  ReactDOM.render(<Clock date={new Date()}/>, document.getElementById('root'));
}
      
setInterval(tick, 1000)
```

- `this.props.` : class를 사용하면 안에 props가 있어서 this를 붙여줘야 한다.
- `super(props)` : constructor를 사용해서 인자를 준다면 super를 해서 props를 초기화해준다.

```react
class Clock extends React.Component {
  constructor(props){
    super(props);
    this.state= {
      date : new Date()
    };
  }
  componentDidMount(){
    
  }
  render(){
  return (
  <h3>현재 시각은 [{this.state.date.toLocaleTimeString()}] 입니다.</h3>
  );
}
}

ReactDOM.render(<Clock/>,document.getElementById('root'));
```

- 더이상 다른 함수는 필요없다. class 안에서 다 할거다.
- state를 줬지만 새로고침하는 명령이 없어서 정적으로 되어있다.

```react
class Clock extends React.Component {
  constructor(props){
    super(props);
    this.state= {
      date : new Date()
    };
  }
  tick() {
    this.setState({
      date: new Date()
    })
  }
  componentDidMount(){
    this.timerID = setInterval(() => this.tick(),1000 );
  }
  componentWillUnmount(){
    clearInterval(this.timerID);
  }
  render(){
  return (
  <h3>현재 시각은 [{this.state.date.toLocaleTimeString()}] 입니다.</h3>
  );
}
} 

ReactDOM.render(<Clock/>,document.getElementById('root'));
```

- state를 바꾸려면 setState를 줘서 값을 다시 준다.

```react
class Clock extends React.Component {
  constructor(props){
    super(props);
    this.state= {
      date : new Date()
    };
  }
goBack () {
  let nextDate = this.state.date;
  nextDate.setSeconds(nextDate.getSeconds() -10);
  this.setState({
    date:nextDate
  });
}
  render(){
  return (
    <div>
  <h3>현재 시각은 [{this.state.date.toLocaleTimeString()}] 입니다.</h3>
      <button onClick={this.goBack.bind(this)}>10초 뒤로가기</button>
      </div>
  );
}
} 

ReactDOM.render(<Clock/>,document.getElementById('root'));
```

- 버튼을 눌러서 setSstate값을 뒤로 돌릴 수 있다.