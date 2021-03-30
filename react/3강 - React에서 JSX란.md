# 3강 - React에서 JSX란

- 리액트는 자바스크립트 문법의 확장판
- 항상 render를 해서 명시적으로 알려준다.

```react
 function formatInfo(student) {
   return student.name + "["+ student.id +"]"
 }

const student = {
  id: "201520000",
  name:'펭하',
  color:'blue'
}

const element = (
  <h3 class={student.color}>
    {formatInfo(student)}
  </h3>
);

ReactDOM.render(element, document.getElementById('root'));
```

- dom에서  element 를 root에 그려주겠다.
- 함수를 생성하고 이 함수를 불러와서 보여준다.
- element의 중괄호가 jsx다. 
  - 어떤게 자바스크립트인지 명시해준다.
  - 클래스 정의할때도 중괄호 표시해줘서 jsx라는 것을 명시적으로 알려준다.
- 그래서 안전하다.
- 그리고 가볍다.

```react
function tick() {
  const element = (
  <h3>현재 시각은 [{new Date().toLocaleTimeString()}] 입니다. </h3>
    );
  ReactDOM.render(element, document.getElementById('root'))
}

setInterval(tick, 1000);
```

- 시간 부분만 지속적으로 바뀐다.