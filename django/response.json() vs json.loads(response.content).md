# response.json() vs json.loads(response.content)

- api에서 데이터를 가져오고 data를 추출할 때 두 개의 차이점이 궁금했다.
- input에서 차이점이 있었다.
- 공통의 response를 받아서 비교해보자.

```python
url = reverse('product-list')
response = self.client.get(url)
```

### .json()

```python
response_data = response.json()
print(type(response))
print(response_data)
print(type(response_data))

>
<class 'rest_framework.response.Response'>
[{'id': 1, 'name': '텀블러', 'cost': '10000'}]
<class 'list'>
```

- input으로 들어가야 하는 타입이 class이다.

### json.loads(response.content)

```python
response_data2 = json.loads(response.content)
print(type(response.content))
print(response_data2)
print(type(response_data2))

>
<class 'bytes'>
[{'id': 1, 'name': '텀블러', 'cost': '10000'}]
<class 'list'>
```

- input 타입이 bytes로 들어가야 한다.

### .json()에 response.content로 한다면?

```python
response_data3 = response.content.json()
print(response_data3)

>
AttributeError: 'bytes' object has no attribute 'json'
```

- bytes는 json속성이 없다고 오류를 반환한다.

### json.loads(response)으로 한다면?

```python
response_data3 = json.loads(response)
print(response_data3)

>
raise TypeError(f'the JSON object must be str, bytes or bytearray, '
TypeError: the JSON object must be str, bytes or bytearray, not Response
```

- class가 들어갔으니 다른 타입을 넣으라는 에러를 반환한다.

### 정리

| 공통점                                              | 차이점                    |
| --------------------------------------------------- | ------------------------- |
| 반환하는 값(여기서는 <class 'list'>)의 type는 같다. | input 값의 형태가 다르다. |

### 추가

- 만약 인코딩이 확실하게 `UTF-8`이면 `json.loads(response.content)`를 써보고, 인코딩에 상관없이 json으로 변경할거면 `.json()`을 사용하면 된다.