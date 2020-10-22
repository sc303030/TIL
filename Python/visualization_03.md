# visualization_03

### 지도 시각화를 위한 folium

```python
import folium as g
```

- 아나콘다 프롬포트에서 conda pip install folium -n conda-forge를 해준다.

```python
g_map = g.Map(location=[37.522208, 126.973734],zoom_start=10)
# option
marker01 = g.Marker([37.522208, 126.973734],
                 popup='국립중앙박물관',
                 icon = g.Icon(color='blue'))
marker01.add_to(g_map)

marker02 = g.CircleMarker([37.522208, 126.973734],
                         radius = 100,
                         color='skyblue',
                         popup='esu',
                         fill_color = 'skyblue')
marker02.add_to(g_map)
g_map
```

- location에 위도,경도를 넣고 zoom_start에 얼만큼 줌을 할 것인지 수치를 부여한다.
- 옵션에 marker를 준다.
  - 마커를 클릭하면 내가 popup에 지정한 위치 이름이 뜬다.
  - 마커를 g_map에 추가한다.
- 써클마커는 주변 위치를 포함하여 효시

![g01](./img/g01.jpg)

```python
g_map.save('./data/now_location.html')
```

- html로 지도를 저장할 수 있다.

```python
g_map = g.Map(location=[37.522208, 126.973734],zoom_start=18,tiles='Stamen Toner')
```

- tiles 로 지도의 모양을 바꿀 수 있다.

**서울 시청 지도**

```python
seoul_map = g.Map(location=[37.55,126.98],
                 tiles = 'Stamen Terrain',
                 zoom_start = 12)
seoul_map
```

#### 서울 지역 대학교 좌표 찍기

```python
xls = pd.ExcelFile('./data/seoul_loc.xlsx')
uni_loc = xls.parse(xls.sheet_names[0])
uni_df = uni_loc.copy()
uni_df.reset_index(inplace=True)
uni_df.rename({'index':'대학명'},axis=1,inplace=True)
uni_df.head()
		대학명					위도			경도
0	KAIST 서울캠퍼스			37.592573	127.046737
1	KC대학교				37.548345	126.854797
2	가톨릭대학교(성신교정)	37.585922	127.004328
3	가톨릭대학교(성의교정)	37.499623	127.006065
4	감리교신학대학교			37.567645	126.961610
```

- 대학교 좌표 정보를 불러온다.

```python
seoul_map = g.Map(location=[37.55,126.98],
                 tiles = 'Stamen Terrain',
                 zoom_start = 12)
# 대학교 위치정보로 Marker를 표시

for i in range(len(uni_df)):
    marker01 = g.Marker([uni_df['위도'][i], uni_df['경도'][i]],
                 popup=uni_df['대학명'][i],
                 icon = g.Icon(color='blue'))
    marker01.add_to(seoul_map)
seoul_map
```

```python
for i in range(len(uni_df)):
    marker01 = g.Marker(uni_df.loc[i, ['위도','경도']],
                 popup=uni_df.loc[i,'대학명'],
                 icon = g.Icon(color='blue'))
    marker01.add_to(seoul_map) 
```

```python
for name, lat, lng in zip(uni_df['대학명'],uni_df['위도'],uni_df['경도']):
    marker01 = g.Marker([lat,lng],
                 popup = name,
                 icon = g.Icon(color='blue'))
    marker01.add_to(seoul_map)
```

- zip함수로 묶어서 각각 가져와서 부여해줘도 된다.

- 이렇게 돌려도 된다.

![g02](./img/g02.jpg)

```python
pip install git+https://github.com/python-visualization/branca.git@master
```

- 아이콘 클릭시 한글이  깨지면 이걸 프롬포트에서 설치하면 된다.

```python
seoul_map = g.Map(location=[37.55,126.98],
                 tiles = 'Stamen Terrain',
                 zoom_start = 12)
# 대학교 위치정보로 Marker를 표시

for name, lat, lng in zip(uni_df['대학명'],uni_df['위도'],uni_df['경도']):
    marker01 = g.CircleMarker([lat,lng],
                 popup = name,
                 icon = g.Icon(color='blue'),
                 radius=10,
                 fill=True,
                 fill_color='coral',
                 fill_opacity=0.3)
    marker01.add_to(seoul_map)
    
seoul_map
```

![g03](./img/g03.jpg)

- 다양한 마커가 있다.