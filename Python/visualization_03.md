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