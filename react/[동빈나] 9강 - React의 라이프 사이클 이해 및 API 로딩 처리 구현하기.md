# [동빈나] 9강 - React의 라이프 사이클 이해 및 API 로딩 처리 구현하기

### 리액트 라이브러리가 처음 컴포넌트를 실행하는 순서

1. constructor()를 불러온다.
2. componentWillMount()
   1. 컴포넌트를 마운트 하기 전에 이 함수를 불러온다.
3. render()
   1. 실제로 컴포넌트를 화면에 그린다.
4. componentDidMount()
   1. 그 다음에 이 함수가 불러와진다.

5. props or state -> shouldComponentUpdate()
   1. 값이 변경되었을 경우 이 함수를 불러온다.

```react
import CircularProgress from '@material-ui/core/CircularProgress';

const styles = theme => ({
  root:{
    width:'100%',
    marginTop:theme.spacing.unit * 3,
    overflowX:'auto'
  },
  table:{
    minWidth:1080
  },
  progress:{
    margin:theme.spacing.unit * 2
  }
})
-------------------
class App extends React.Component{

  state ={
    customers:"",
    completed:0
  }
```

- progress를 사용하기 위해서 패키지를 import하고 progress의 스타일을 부여한다.
- 또한 progress는 100까지 게이지가 차기때문에 completed에 0이란 값을 넣어준다.

```react
progress = () => {
  const {completed} = this.state;
  this.setState({completed:completed >= 100 ? 0: completed + 1});
}
--------
componentDidMount(){
    this.timer = setInterval(this.progress, 20);
    this.callApi()
      .then(res => this.setState({customers:res}))
      .catch(err => console.log(err));
  }r
```

- progress를 만들어서 완료상태를 받아오소 해당 상태에 따라 계속 값을 업데이트 한다.
- 그리고 timer를 줘서 간격을 조정한다.

```react
 <TableBody>
                {this.state.customers ? this.state.customers.map(c=>{ return( <Customer key={c.id}  id={c.id}  image={c.image}  name={c.name}  birthday={c.birthday}  gender={c.gender}  job={c.job} />)}) 
                : 
                <TableRow>
                  <TableCell colSpan="6" align="center">
                    <CircularProgress className={classes.progress} variant="determinate" value={this.state.completed}/>

                  </TableCell>
                </TableRow>
                }
                </TableBody>
```

- 테이블을 줘서 하나의 공간을 만들어주고 값을 넘겨주기 위해 value를 주었다.

