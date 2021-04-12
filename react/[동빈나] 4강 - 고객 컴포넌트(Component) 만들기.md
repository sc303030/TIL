# [동빈나] 4강 - 고객 컴포넌트(Component) 만들기 

### 리액트의 기본 구조

```js
import React from 'react';

class Customer extends React.Component {
    render() {
        return(
        <div>
        </div>
        )
    }
}

export default Customer;
```

- import로 패키지를 불러오고 export로 class를 내보낸다.

```js
import './App.css';
import Customer from './components/Customer'
import React from 'react';


class App extends React.Component {
  render() {
  return (
    <Customer />
  );
  }
}

export default App;
```

- 그러면 App.js에서 실제로 페이지에 그려지게 되니깐 여기서 만들었던 Customer을 불러서 넣어준다.

#### App.js

```js
const customer = {
  'name' : '홍길동',
  'birthday' : '0000',
  'gender' : '남자'
}

class App extends React.Component {
  render() {
  return (
    <Customer 
    name = {customer.name}
    birthday={customer.birthday}
    gender={customer.gender}/>
  );
  }
}
```

#### Customer.js

```js
class Customer extends React.Component {
    render() {
        return(
        <div>
            <h2>{this.props.name}</h2>
            <p>{this.props.birthday}</p>
            <p>{this.props.gender}</p>
        </div>
        )
    }
}
```

- 이렇게 변수를 선언해서 그 값을 배정하고 props를 받아서 값을 출력할 수 있다. 보통 하드코딩해서 하기보다 이렇게 props를 받아서 사용한다.

- 기존의 코드보다 구조화되었다.