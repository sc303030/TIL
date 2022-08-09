# 크롤링 후 DB에 데이터 삽입하기

> 크롤링_into_db 스크립트 파일의 위치는 최상단 즉, manage.py파일의 위치와 동일하다.

### 기본 세팅

1. 크롤링에 필요한 셀레니옴 모듈을 불러온다.
2. db에 데이터를 삽입하기 위해 장고의 환경설정을 다시 잡아줘야 해서 settings를 다시 설정한다.

```python
from selenium import webdriver
from selenium.webdriver.chrome.service import Service as ChromeService

from nba_app.models import Player

from dataclasses import dataclass
from selenium.webdriver.common.by import By

import os

os.environ['DJANGO_SETTINGS_MODULE'] = 'nba_predict_django.settings'
import django

django.setup()

options = webdriver.ChromeOptions()
service = ChromeService(executable_path='./chromedriver.exe')
driver = webdriver.Chrome(service=service, options=options)
```

### 크롤링 진행

- 은퇴한 nba 선수의 정보를 가져오는 크롤링이다.
- 셀레니옴 4부터는 find_elements의 방법이 BY로 가져오는 것으로 변경되었으므로 주의해야 한다.

```python
def craw():
    time_list = ['2010-11', '2011-12', '2012-13', '2013-14', '2014-15', '2015-16', '2016-17', '2017-18', '2018-19',
                 '2019-20', '2020-21']
    day_list = ['2010', '2011', '2012', '2013', '2014', '2015', '2016', '2017', '2018', '2019', '2020']
    name_list = []
    age_list = []
    year_list = []
    cnt = 0
    for day in time_list:
        driver.get('https://en.wikipedia.org/wiki/List_of_' + str(day) + '_NBA_season_transactions')
        page = driver.find_elements(By.CSS_SELECTOR, '.wikitable')
        print(page[0])
        page = page[0]
        for i in page.find_elements(By.TAG_NAME, 'tbody'):
            k = i.find_elements(By.TAG_NAME, 'tr')
            for idx, j in enumerate(k):
                if idx == 0:
                    continue
                else:
                    td_list = j.find_elements(By.TAG_NAME, 'td')
                    if len(td_list) == 6:
                        name_list.append(j.find_elements(By.TAG_NAME, 'td')[1].text)
                        age_list.append(j.find_elements(By.TAG_NAME, 'td')[3].text)
                        year_list.append(day_list[cnt])
                    elif len(td_list) == 5:
                        name_list.append(j.find_elements(By.TAG_NAME, 'td')[0].text)
                        age_list.append(j.find_elements(By.TAG_NAME, 'td')[2].text)
                        year_list.append(day_list[cnt])
            cnt += 1
            print(cnt)
    return name_list, age_list, year_list


name_list, age_list, year_list = craw()
```

### DB에 삽입하기

1. 다수의 정보를 한 번에 저장하기 위해 bulk_create를 사용하였다.
2. 핵심은 리스트 생성 후 해당 리스트에 정보를 저장하고, 그 리스트를 bulk_create하면 된다.

```python
@dataclass
class PlayerIntoDb:
    name_list: list
    age_list: list
    year_list: list

    def into_db(self):
        players = []
        for name, age, year in zip(self.name_list, self.age_list, self.year_list):
            players.append(Player(name=name, age=age, retire_year=year))
        Player.objects.bulk_create(players)


print('Player 시작')
player_info_db = PlayerIntoDb(name_list, age_list, year_list)
player_info_db.into_db()
print('Player 종료')
```

### 결과

- db에 데이터가 잘 들어갔다.

![craw_01](image/craw_01.jpg)