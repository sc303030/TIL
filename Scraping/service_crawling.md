# 비정형 데이터 활용하기

## 스크래핑(scraping)

- 컴퓨터 프로그램이 다른 프로그램으로부터 들어오는 인간이 읽을 수 있는 출력으로부터 데이터를 추출하는 기법이다.

```python
import requests
from bs4 import BeautifulSoup
```

- `html` 을 불러오려면 사용자의 `request` 를 받아야 하니 패키지를 불러온다.

```python
webpage = requests.get('https://www.daangn.com/hot_articles')
webpage.text
```

```
'<!DOCTYPE html>\n<html lang="ko">\n<head>\n  <meta charset="utf-8">\n  <meta http-equiv="X-UA-Compatible" content="IE=edge">\n  <meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=no">\n      <link rel="canonical" href="https://www.daangn.com/hot_articles" />\n\n  <title>당근마켓 중고거래 | 당신 근처의 당근마켓</title>\n<meta name="description" content="당근마켓에서 거래되는 인기 중고 매물을 소개합니다. 지금 당근마켓에서 거래되고 있는 다양한 매물을 구경해보세요.">\n<meta property="og:url" content="https://www.daangn.com/hot_articles">
```

- 그냥 `request` 로만 받아오면 분리되지도 않고 텍스트형대로 받아온다. 

- html로 파싱할 수 있게 받아야 한다.

###  BeautifulSoup

```python
webpage = requests.get('https://www.daangn.com/hot_articles')
soup = BeautifulSoup(webpage.content, 'html.parser')
soup
```

- `BeautifulSoup` 을 활용하여 파싱하여 가져온다.

```
<!DOCTYPE html>

<html lang="ko">
<head>
<meta charset="utf-8"/>
<meta content="IE=edge" http-equiv="X-UA-Compatible"/>
<meta content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=no" name="viewport"/>
<link href="https://www.daangn.com/hot_articles" rel="canonical"/>
<title>당근마켓 중고거래 | 당신 근처의 당근마켓</title>
<meta content="당근마켓에서 거래되는 인기 중고 매물을 소개합니다. 지금 당근마켓에서 거래되고 있는 다양한 매물을 구경해보세요." name="description"/>
```

- 그냥 `request` 만 받았을 때랑 다르다. 파싱할수 있도록 변형되어 받아온다.
- 이렇게 가져와야 탐색할 수 있다.

```python
soup.p
```

```
<p>당근마켓 앱에서 따뜻한 거래를 직접 경험해보세요!</p>
```

- 전체페이지에서 가장 첫 번째 `<p>` 태그를 찾아서 보여준다.

```python
print(soup.p.string)
```

```
당근마켓 앱에서 따뜻한 거래를 직접 경험해보세요!
```

- `string` 을 사용하면 태그안에 있는 텍스트만 추출할 수 있다.

```python
soup.h
```

```
<h1 id="fixed-bar-logo-title">
<a href="https://www.daangn.com/">
<span class="sr-only">당근마켓</span>
<img alt="당근마켓" class="fixed-logo" src="https://d1unjqcospf8gs.cloudfront.net/assets/home/base/header/logo-basic-24b18257ac4ef693c02233bf21e9cb7ecbf43ebd8d5b40c24d99e14094a44c81.svg"/>
</a> </h1>
```

- 처음 만나는 `<h1>` 을 가져온다. 하위요소들도 가져와버린다. 하위태그 접근이 필요하다면?

```python
for child in soup.h1.children :
    print(child)
```

```
<a href="https://www.daangn.com/">
<span class="sr-only">당근마켓</span>
<img alt="당근마켓" class="fixed-logo" src="https://d1unjqcospf8gs.cloudfront.net/assets/home/base/header/logo-basic-24b18257ac4ef693c02233bf21e9cb7ecbf43ebd8d5b40c24d99e14094a44c81.svg"/>
</a>
```

- for루프를 돌려서 하위태그들을 가져오게 하였다.

```python
for d in soup.div.children:
    print(d)
```

```
<h1 id="fixed-bar-logo-title">
<a href="https://www.daangn.com/">
               .
               .
               .
<div class="fixed-download-text">Google Play</div>
</a> </section>
```

- 처음 만나는  `<div>` 에는 다음과 같은 하위요소들이 있다.

### find_all()

- 원하는 부분을 모두 가져올 때 사용하는 함수

```python
soup.find_all('h2')
```

```
[<h2 class="card-title">버팔로 캠핑의자</h2>, <h2 class="card-title">스팸 선물세트 새것</h2>, ...  <h2 class="card-title">24인치 자전거</h2>]
```

- 태그를 입력하면 태그에 해당하는 모든 정보들을 가져온다.
  - 여기서는 `<h2>`  에 해당하는 값들을 모두 가져왔다.
- 또한 리스트로 구성되어 있다.

```python
type(soup.find_all('h2')
```

```
<class 'bs4.element.ResultSet'>
```

### 정규식을 활용하자

- `<ol> <ul>` 포함하는 값을 리스트로 읽어오고 싶다면?

```python
import re
```

- 필요한 패키지를 불러오자.

```python
soup.find_all(re.compile('[ou]l'))
```

```
[<ul class="nav navbar-nav">
 <li><a href="http://www.weather.gov">HOME</a></li>
 <li class="dropdown"><a class="dropdown-toggle" data-toggle="dropdown" href="http://www.weather.gov/forecastmaps">FORECAST <span class="caret"></span></a><ul class="dropdown-menu" role="menu"> ... <li><a href="https://www.weather.gov/careers">Career Opportunities</a></li>
 </ul>]
```

- `<ol>` , `<ul>` 에 해당하는 태그들만 불러온다.

```python
soup.find_all(re.compile('h[1-9]'))
```

```
[<h1 id="fixed-bar-logo-title">
 <a href="https://www.daangn.com/">
 <span class="sr-only">당근마켓</span>... <p>당근마켓 앱에서 따뜻한 거래를 직접 경험해보세요!</p>]
```

- `<h1>` ~ `<h9>` 에 해당하는 값들을 모두 불러온다.

**하나 이상의 태그들을 가져올 때 리스트로 만들면 원하는 태그들의 정보를 가져올 수 있다.**

#### attrs = {}

- 딕셔너리 형식으로 접근하면 해당 값들을 가져올 수 있다. 

```python
soup.find_all(attrs={'class':'card-title'})
```

