# 영화평점에 대한 평점 변화(시각화)[실습]

```python
from urllib.request import urlopen
from bs4 import BeautifulSoup
from urllib.request import urlopen
from urllib.error   import HTTPError
from urllib.error   import URLError
```

```python
base_url = 'https://movie.naver.com/'
sub_url = '/movie/sdb/rank/rmovie.nhn?sel=cur&date=20170501'

try:
    html = urlopen(base_url+sub_url)
except HTTPError as he:
    print('http error')
except URLError as ue:
    print('url error')
else:
    soup = BeautifulSoup(html.read(), 'html.parser')
```

- `base url` 을 해놓으면 나중에 `sub_url` 만 바꾸면 편하다.

### 영화 제목 찾기

![sc01](./img/sc01.jpg)

- 영화 랭킹에서 영화 제목과 평점을 가져올 것이다.
  - 영화 평점 변화를 살펴보자.

![sc02](./img/sc02.jpg)

- 월-E를 담고있는 `div` 에 `class` 가 있다. `class` 를 찾아서 들어가자.

```python
soup.find_all('div', 'tit5')
```

```
[<div class="tit5">
 <a href="/movie/bi/mi/basic.nhn?code=147092" title="히든 피겨스">히든 피겨스</a>
 </div>, <div class="tit5">
 <a href="/movie/bi/mi/basic.nhn?code=10102" title="사운드 오브 뮤직">사운드 오브 뮤직</a> ...
 <a href="/movie/bi/mi/basic.nhn?code=17875" title="로미오와 줄리엣">로미오와 줄리엣</a>
 </div>]
```

- 제목이 다른 이유는 현재 날짜가 아니라 링크에 과거 날짜를 입력하였기 때문이다.

```python
soup.find_all('div', 'tit5')[0].a.text 
```

```
'히든 피겨스'
```

- `string` ,` get_text()` 을 써도 문자를 가져올 수 있다.
- 리스트로 값이 넘어오기 때문에 인덱스를 써서 텍스트를 가져온다.

### 평점찾기

![sc03](./img/sc03.jpg)

- 평점을 담고있는 `td` 에 `class` 가 있다. `class` 를 타고 들어가자.

```python
soup.find_all('td', 'point')[0].text
```

```
'9.38'
```

### DataFrame을 만들자

> 데이터 프레임을 만들기 위해서는 셀의 길이가 동일해야하므로 확인절차가 필요하다.

```python
print(len(soup.find_all('div', 'tit5')))
print(len(soup.find_all('td', 'point')))
```

```
50
50
```

- 셀의 길이가 동일하다.

#### 영화 제목 저장

```python
movie_names = [soup.find_all('div', 'tit5')[i].a.text for i in range(50) ]
print(len(movie_names))
print(movie_names)
```

- 굳이 `for` 구문을 돌리지 않아서 리스트 안에서 실행하면 값이 리스트 안에 저장된다. 

```
50
['히든 피겨스', '사운드 오브 뮤직', '시네마 천국', '미스 슬로운', '잉여들의 히치하이킹', '나, 다니엘 블레이크', '바람과 함께 사라지다', '오즈의 마법사', '벤허', '흑집사 : 북 오브 더 아틀란틱', '우리들', '일 포스티노', '댄서', '라이언', '코알라', '로건', '더 플랜', '분노의 질주: 더 익스트림', '시카고', '10분', '해리가 샐리를 만났을 때', '미녀와 야수', '너의 이름은.', '그랑블루', '한공주', '연애담', '포켓몬 더 무비 XY&Z; 「볼케니온 : 기계왕국의 비밀」', '리틀 프린세스 소피아: 엘레나와 비밀의 아발로 왕국', '분노', '맨체스터 바이 더 씨', '행복 목욕탕', '스머프: 비밀의 숲', '부당거래', '파닥파닥', '아비정전', '패션 오브 크라이스트', '라라랜드', '뽀로로 극장판 슈퍼썰매 대모험', '족구왕', '가디언즈 오브 갤럭시', '자전거 탄 소년', '오두막', '성실한 나라의 앨리스', '원라인', '존 윅 - 리로드', '사일런스', '클로저', '임금님의 사건수첩', '문라이트', '로미오와 줄리엣']
```

#### 영화 평점 저장

```python
movie_num =  [soup.find_all('td', 'point')[i].text for i in range(50) ]
print(len(movie_num))
print(movie_num)
```

```
50
['9.38', '9.36', '9.29', '9.26', '9.25', '9.25', '9.24', '9.23', '9.22', '9.20', '9.18', '9.17', '9.14', '9.07', '9.07', '9.06', '9.04', '9.02', '8.92', '8.89', '8.89', '8.85', '8.81', '8.78', '8.78', '8.76', '8.75', '8.73', '8.73', '8.72', '8.70', '8.67', '8.66', '8.65', '8.59', '8.59', '8.59', '8.56', '8.56', '8.56', '8.54', '8.48', '8.39', '8.29', '8.28', '8.26', '8.20', '8.17', '8.12', '8.10']
```

#### pandas 불러오기

```python
import pandas as pd
```

#### 날짜 불러오기

```python
date = pd.date_range('2017-5-1',periods=100,freq='D')
date
```

```
DatetimeIndex(['2017-05-01', '2017-05-02', '2017-05-03', '2017-05-04',
               '2017-05-05', '2017-05-06', '2017-05-07', '2017-05-08',
               '2017-05-09', '2017-05-10', '2017-05-11', '2017-05-12',
               '2017-05-13', '2017-05-14', '2017-05-15', '2017-05-16',
               '2017-05-17', '2017-05-18', '2017-05-19', '2017-05-20',
               '2017-05-21', '2017-05-22', '2017-05-23', '2017-05-24',
               '2017-05-25', '2017-05-26', '2017-05-27', '2017-05-28',
               '2017-05-29', '2017-05-30', '2017-05-31', '2017-06-01',
               '2017-06-02', '2017-06-03', '2017-06-04', '2017-06-05',
               '2017-06-06', '2017-06-07', '2017-06-08', '2017-06-09',
               '2017-06-10', '2017-06-11', '2017-06-12', '2017-06-13',
               '2017-06-14', '2017-06-15', '2017-06-16', '2017-06-17',
               '2017-06-18', '2017-06-19', '2017-06-20', '2017-06-21',
               '2017-06-22', '2017-06-23', '2017-06-24', '2017-06-25',
               '2017-06-26', '2017-06-27', '2017-06-28', '2017-06-29',
               '2017-06-30', '2017-07-01', '2017-07-02', '2017-07-03',
               '2017-07-04', '2017-07-05', '2017-07-06', '2017-07-07',
               '2017-07-08', '2017-07-09', '2017-07-10', '2017-07-11',
               '2017-07-12', '2017-07-13', '2017-07-14', '2017-07-15',
               '2017-07-16', '2017-07-17', '2017-07-18', '2017-07-19',
               '2017-07-20', '2017-07-21', '2017-07-22', '2017-07-23',
               '2017-07-24', '2017-07-25', '2017-07-26', '2017-07-27',
               '2017-07-28', '2017-07-29', '2017-07-30', '2017-07-31',
               '2017-08-01', '2017-08-02', '2017-08-03', '2017-08-04',
               '2017-08-05', '2017-08-06', '2017-08-07', '2017-08-08'],
              dtype='datetime64[ns]', freq='D')
```

- `date_range` 를 하면 원하는 날짜의 길이를 가져올 수 있다.

#### for 루프 시간 확인하기

```python
import urllib
from tqdm import tqdm_notebook
import time
```

```python
for n in tqdm_notebook(range(100)):
    time.sleep(0.1)
```

![sc04](./img/sc04.gif)

- 다음과 같이 `for` 루프가 얼마나 남았는지 알 수 있다.

- `sleep` 는 일정시간동안 프로세스를 일시정지 할 수 있다. 

```python
for n in tqdm_notebook(range(2), desc='outer'):
    for y in tqdm_notebook(range(36), desc='innter'):
        time.sleep(0.1)
```

![sc05](./img/sc05.gif)

- 안에 있는 `for` 가 끝나야 다음 `for` 가 실행되고 최종 마무리 되는 것을 볼 수 있다.
- `desc` 는 본인이 원하는 구별 텍스트를 기입하면 된다.

#### 데이터를 리스트에 저장하자

```python
names_result = []
point_result = []
date_result  = []
```

```python
base_url = 'https://movie.naver.com/'
sub_url = '/movie/sdb/rank/rmovie.nhn?sel=cur&date='
for day in tqdm_notebook(date):
    html = base_url + sub_url + '{date}'
    response = urlopen(html.format(date=urllib.parse.quote(day.strftime('%Y%m%d'))))
    soup = BeautifulSoup(response,'html.parser')
    end  = len(soup.find_all('td', 'point'))
    names_result.extend([soup.find_all('div', 'tit5')[n].a.text for n in range(0,end) ])
    point_result.extend([soup.find_all('td', 'point')[n].text for n in range(0,end) ])
    date_result.extend([day for i in range(0,end)])
```

- `date` 는 우리가 위에서 100일을 불러왔던 값이다. 
  - 그 `date` 를 `url` 에 붙이면 그 당시의 영화 제목과 평점을 얻어 올 수 있다. 
  - 양식이 `2017-5-1` 과 같은 형식이기 때문에 `urllib.parse.quote` 을 사용하여 `20170501` 과 같은 형식으로 바꿔줍니다
- `quote` : [URL 인용(quoting) 함수는 특수 문자를 인용하고 비 ASCII 텍스트를 적절히 인코딩하여 프로그램 데이터를 취해서 URL 구성 요소로 안전하게 사용할 수 있도록 하는 데 중점을 둡니다. 또한 해당 작업이 위의 URL 구문 분석 함수로 처리되지 않는 경우 URL 구성 요소의 내용에서 원래 데이터를 다시 만들기 위해 이러한 작업을 뒤집는 것도 지원합니다.](https://docs.python.org/ko/3/library/urllib.parse.html)

- `append` 를 사용하지 않고 `extend` 를 사용하는 이유 :
  - `append` : 리스트 끝에 x 1
  - `extend` : 리스트 끝에 iterable의 모든 항목
  - `append` 를 사용했다라고 가정하면 리스트안에 또 리스트가 만들어지는 셈이다. 우리가 원하는건 데이터 값만이다. 그래서 `extend` 를 사용하였다.

```python
print(len(names_result))
print(len(point_result))
print(len(date_result))
```

```
4723
4723
4723
```

- 길이를 다시 확인한다.

#### DataFrame 생성

```python
movieDF = pd.DataFrame({'date' : date_result, 'name' : names_result, 'point' : point_result})
```

```python
movieDF.head()
```

```
		  date	     name	    point
0	2017-05-01	히든 피겨스	      9.38
1	2017-05-01	사운드 오브 뮤직	 9.36
2	2017-05-01	시네마 천국	      9.29
3	2017-05-01	미스 슬로운	      9.26
4	2017-05-01	잉여들의 히치하이킹	9.25
```

```python
movieDF.info()
```

```
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 4723 entries, 0 to 4722
Data columns (total 3 columns):
date     4723 non-null datetime64[ns]
name     4723 non-null object
point    4723 non-null object
dtypes: datetime64[ns](1), object(2)
memory usage: 110.8+ KB
```

- point의 형식이 `object` 다. 숫자로 바꿔주자.

#### astype() 

> 컬럼의 타입을 변경할 수 있다.

```py
movieDF['point']  = movieDF['point'].astype(float)
```

```
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 4723 entries, 0 to 4722
Data columns (total 3 columns):
date     4723 non-null datetime64[ns]
name     4723 non-null object
point    4723 non-null float64
dtypes: datetime64[ns](1), float64(1), object(1)
memory usage: 110.8+ KB
```

- 정상적으로 변경되었다.

### 피벗테이블

- 내가 원하는 영화의 평점을 총점으로 확인하고 싶다면?
- 피벗테이블을 이용하여 볼 수 있다.

#### 필요한 패키지 설치

```python
import numpy as np
import matplotlib.pyplot as plt
%matplotlib inline
```

```python
movie_pivot = pd.pivot_table(movieDF, index=['name'], aggfunc=np.sum)
movie_pivot
```

- pd.pivot_table(데이터, 내가 그룹하고 싶은 컬럼, aggfunc=함수)

```
					  point
name	
10분					 124.46
47 미터				149.23
500일의 썸머			75.51
7년-그들이 없는 언론	 137.28
```

#### `point` 에 따라 정렬

