# syntaxerror: unexpected token < in json at position 0

- 리액트 강의를 따라 하는데 자꾸 이러한 메시지가 뜨면서 값이 나오지 않았다.
- 그래서 다음과 같은 코드를 추가하니 잘 나왔다.

```react
  callApi = async () => {
    const response = await fetch('/api/customers',{
      headers: {
        'Accept' : 'application / json'
      }
    });
    const body = await response.json();
    return body;
  }
```

```
{
      headers: {
        'Accept' : 'application / json'
      }
    }
```

- api 주소 다음에 이 부분을 추가하였다.