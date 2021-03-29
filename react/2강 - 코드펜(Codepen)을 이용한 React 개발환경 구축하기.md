# 2강 - 코드펜(Codepen)을 이용한 React 개발환경 구축하기

- [코드펜](https://codepen.io/)
  - react,react-dom을 불러온다.
  - 전처리는 babel을 선택한다.

```ㅗtml
<div id='root'></div>
```

- 우선 html에 div태그를 만든다. 

```
class HelloReact extends React.Component {
  render(){
    return (
      <h1>Hello world</h1>
    )
  }
}

ReactDOM.render(<HelloReact/>,
               document.getElementById('root'));
```

- react를 상속받아서 클래스를 만들고 그 클래스 안에 render를 써서 내가 보여줄 태그를 만든다.
- 만든 class를 화면에 띄우려면 dom을 써서 render을 한다.
  - 먼저 클래스를 찾고 그 다음에 어디에 띄울건지 태그를 찾아서 거기에 띄운다.
- react.dom은 그려주는 역할이고 component 는  그려질 대상이다.