# useEffect가 2번 호출 될 때

1. root에 있는 `<React.StrictMode>` 지우기

```react
const root = ReactDOM.createRoot(
  document.getElementById('root') as HTMLElement
);
root.render(
  // <React.StrictMode> -> 주석 처리 혹은 지우기
    <App />
  // </React.StrictMode>
);
```

### StrictMode란?

https://ko.reactjs.org/docs/strict-mode.html

- 애플리케이션 내의 잠재적인 문제를 알아내기 위한 도구
- 개발모드에서만 적용되고 서비스 환경에서는 적용 안 됨

```
- 안전하지 않은 생명주기를 사용하는 컴포넌트 발견
- 레거시 문자열 ref 사용에 대한 경고
- 권장되지 않는 findDOMNode 사용에 대한 경고
- 예상치 못한 부작용 검사
- 레거시 context API 검사
```

