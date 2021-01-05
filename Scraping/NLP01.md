# KoNLPY

> 한국어 처리 패키지

### 말뭉치 

#### kolaw(헌법에 있는 말뭉치)

#### kobill(국회법에 있는 말뭉치)

```python
import konlpy
from konlpy.corpus import kolaw
from konlpy.corpus import kobill
```

- 분석에 필요한 패키지들을 불러온다. 

```python
kobill.fileids()
>
['1809890.txt',
 '1809891.txt',
 '1809892.txt',
 '1809893.txt',
 '1809894.txt',
 '1809895.txt',
 '1809896.txt',
 '1809897.txt',
 '1809898.txt',
 '1809899.txt',]
```

```python
txt = kobill.open('1809890.txt').read()
txt
>
'지방공무원법 일부개정법률안\n\n(정의화의원 대표발의 )\n\n 의 안\n 번 호\n\n9890\n\n발의연월일 : 2010.  11.  12. ...
```

- `kobill` 로 열 수 있다. 

### 형태소 분석

- 명사 : nonus()
- 형태소 : morphs()
- 품사 : pos()

```python
from konlpy.tag import *
hannanum = Hannanum() # 카이스트
kkma = Kkma() # 서울대
```

- `Hannanum()`  : 카이스트에서 만든 것

- `Kkma()` :  서울대에서 만든 것

 #### 명사추출

```python
hannanum.nouns(txt[:40])
>
['지방공무원법', '일부개정법률안', '정의화의원', '대표발', '의', '번']
```

```python
kkma.nouns(txt[:40])
>
['지방',
 '지방공무원법',
 '공무원',
 '법',
 '일부',
 '일부개정법률안',
 '개정',
 '법률안',
 '정의',
 '정의화의원',
 '화',
 '의원',
 '대표',
 '대표발의',
 '발의',
 '의',
 '안',
 '호']
```

#### 형태소 추출 : 단어중에서 더 이상 쪼갤 수 없는 것

```python
hannanum.morphs(txt[:40])
>
['지방공무원법', '일부개정법률안', '(', '정의화의원', '대표발', '의', ')', '의', '안', '번', '호']
```

```python
kkma.morphs(txt[:40])
>
['지방',
 '공무원',
 '법',
 '일부',
 '개정',
 '법률안',
 '(',
 '정의',
 '화',
 '의원',
 '대표',
 '발의',
 ')',
 '의',
 '안',
 '벌',
 'ㄴ',
 '호']
```

#### 품사 추출

```python
hannanum.pos(txt[:40])
>
[('지방공무원법', 'N'),
 ('일부개정법률안', 'N'),
 ('(', 'S'),
 ('정의화의원', 'N'),
 ('대표발', 'N'),
 ('의', 'J'),
 (')', 'S'),
 ('의', 'N'),
 ('안', 'M'),
 ('번', 'N'),
 ('호', 'I')]
```

```python
kkma.pos(txt[:40])
>
[('지방', 'NNG'),
 ('공무원', 'NNG'),
 ('법', 'NNG'),
 ('일부', 'NNG'),
 ('개정', 'NNG'),
 ('법률안', 'NNG'),
 ('(', 'SS'),
 ('정의', 'NNG'),
 ('화', 'NNG'),
 ('의원', 'NNG'),
 ('대표', 'NNG'),
 ('발의', 'NNG'),
 (')', 'SS'),
 ('의', 'NNG'),
 ('안', 'NNG'),
 ('벌', 'VV'),
 ('ㄴ', 'ETD'),
 ('호', 'NNG')]
```

```python
from konlpy.tag import Twitter
t = Twitter()
```

```python
token_ko = t.nouns(txt)
token_ko
>
['지방공무원법',
 '일부',
 '개정',
 '법률',
 '안',
 '정의화',...]
```

- 명사를 추출한다. 

```python
import nltk
ko = nltk.Text(token_ko, name='국회법안')
```

```python
print(len(ko.tokens))
print(len(set(ko.tokens))) 
>
735
250
```

- 위해서 만들었던 token_ko를  다시한번 분석해준다. 
-  그 다음에 중복을 제거한다. 

####  많이 나온 순으로 그래프를 그려보자.

```python
import matplotlib.pyplot as plt
%matplotlib inline

import platform

from matplotlib import font_manager, rc
# plt.rcParams['axes.unicode_minus'] = False

if platform.system() == 'Darwin':
    rc('font', family='AppleGothic')
elif platform.system() == 'Windows':
    path = "c:/Windows/Fonts/malgun.ttf"
    font_name = font_manager.FontProperties(fname=path).get_name()
    rc('font', family=font_name)
else:
    print('Unknown system... sorry~~~~') 
```

```python
plt.figure(figsize=(12,6))
ko.plot(50)
plt.show()
```

- 단어 50개만 가져온다.

![nlp01](./img/nlp01.png)

- 여기서 불용어들을 찾아서 다시 만들어 보자.

```python
sw = ['만','액','세','제','위','의','호','이','수','것','략','생','및','명']
```

```python
ko = [ word  for word in ko if word not in sw ]
ko
>
['지방공무원법',
 '일부',
 '개정',
 '법률',
 '안',
 '정의화',...]
```

- 불용어에 없는 것들만 저장한다. 

```python
ko = nltk.Text(ko, name='국회법안')
```

```python
plt.figure(figsize=(12,6))
ko.plot(50) #단어 50개만 가져온다.
plt.show()
```

![nlp02](./img/nlp02.png)

- 불용어를 제외하고 출력되었다. 

```python
data = ko.vocab().most_common(150)
```

- `ko` 를 내림차순으로 정렬한다. 
- 자주 등장하는 상위 `ko` 값 150개를 뽑는다.

```python
from wordcloud import WordCloud, STOPWORDS
wordcloud = WordCloud(font_path='c:/Windows/Fonts/malgun.ttf',
                      relative_scaling = 0.2,
                      background_color='white',
                      ).generate_from_frequencies(dict(data))
plt.figure(figsize=(12,8))
plt.imshow(wordcloud)
plt.axis("off")
plt.show()
```

![nlp03](./img/nlp03.png)