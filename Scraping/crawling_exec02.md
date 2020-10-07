# Selenium 시작하기

> 동적 크롤링을 위한 필수 라이브러리

- 브라우저 동작을 자동화 해주는 프로그램
- 브라우저 동작 시킨다는 의미는 javascript가 동작하면서 비동기적으로 서버로부터 콘텐츠를 가져오는 의미
- 비동기적 : ajax
  - json형식

```python
from selenium import webdriver
```

- `selenium` 을 시작한다. 

```python
path = './driver/chromedriver.exe'
driver = webdriver.Chrome(path)
driver
```

```
<selenium.webdriver.chrome.webdriver.WebDriver(session="d73eb4426d6968f8b998b2cf0ac89daf")>
```

- 미리 크롬의 버전을 확인해서 버전이랑 맞는 드라이버를 다운 받아서 넣어놓자.

![cl15](./img/cl15.PNG)

- 이렇게 크롬 창이 뜬다. 

```python
driver.get('http://www.google.com')
```

![cl16](./img/cl16.PNG)

- 이렇게 하면 구글이 들어가진다. 

```
driver.close()
```

- 현재 켜져있는 브라우저가 닫힌다. 

### Json형식의 파일 크롤링

```python
import json, re
from urllib.request import urlopen
from html import unescape
```

- re : 정규표현식

```python
request = urlopen('http://www.hanbit.co.kr/store/books/full_book_list.html')
encoding = request.info().get_content_charset('utf-8')
html = request.read().decode(encoding)
html
```

- `utf-8`  을 줘서 글자가 깨지는 걸 방지한다.
- 디코딩이 없으면 한글이 다 깨진다. 

```
'<!DOCTYPE html>\r\n<html lang="ko">\r\n<head>\r\n<!--[if lte IE 8]>\r\n<script>\r\n  location.replace(\'/support/explorer_upgrade.html\');\r\n</script>\r\n<![endif]-->\r\n<meta charset="utf-8"/>\r\n<title>
```

---

- json.dumps() : 데이터를 json형태로 인고딩(문자열)
- json.dump() : 데이터를 json형태로 인코딩하여 파일에 출력(파일)
- ensure_ascii = False : \xxxxx 형태로 이스케이프 하지 않고 출력
- [{key : value},{key : value}] indent = size 들여쓰기를 몇 칸할 것인지 사이즈를 줄 수 있다.

---

- 정규표현식
- . 모든문자
- \* 0번이상 반복
- ? 있어도 되고 없어도 된다.
- `<a href='(.*?)'>` 모든 문자이거나 반복되거나 링크가 없어도 된다.
- `<td class='left'><a.*?</td>` a태그로 시작해서 모든 문자 반복,없어도되고 td태그로 끝나는 모든것

#### json 파일 생성

```python
with open('./data/booklist_json.json', mode='w' , encoding='utf-8') as file:
    data=[]
    for partial_html in re.findall(r'<td class="left"><a.*?</td>', html):
        search = re.search(r'<a href="(.*?)">' ,partial_html ).group(1) 
        url = 'http://www.hanbit.co.kr'+search

        title = re.sub(r'<.*?>','' , partial_html )
        data.append({'bookName' : title, 'link' : url})
        print(json.dumps(data, ensure_ascii=False, indent=2))
    json.dump(data, file,ensure_ascii=False, indent=2 )
```

- 매칭되는것만 가져오려면 group()하면 된다.

- group(1) 하면 속성에 있는 값만 가져온다.

- `r'<td class="left"><a.*?</td>'` : 로우데이터를 쓰려면 `r` 을 붙인다. 

- `re.search(r'<a href="(.*?)">'` : 해당되는 것을 찾는다. 

- `re.sub(r'<.*?>','' , partial_html )`  : `r'<.*?>'` 이걸 `''` 이걸로 바꾼다.

- `print(json.dumps(data, ensure_ascii=False, indent=2))` : 아스키로 변환 안 하면 한글이 다 깨진다. 그러니 변환하자 . 인덴트는 들여쓰기.그러면 좀 더 이뻐진다. 

```
[
  {
    "bookName": "나는 꼭 필요한 것만 남기기로 했다",
    "link": "http://www.hanbit.co.kr/store/books/look.php?p_code=B7269609529"
  }
]
```

```python
import json
```

```python
with open('./data/booklist_json.json', mode='r',encoding='utf-8') as file:
    json_data = json.load(file)
```

```python
print(json.dumps(json_data, ensure_ascii=False, indent='\t'))
```

- 파일을 저장하고 출력해본다. 

#### 원하는 데이터만 가져오자.

```python
print(json.dumps(json_data[0]['bookName'], ensure_ascii=False, indent='\t'))
```

```
"나는 꼭 필요한 것만 남기기로 했다"
```

