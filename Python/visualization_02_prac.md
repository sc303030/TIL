# visualization_02_실습

- 데이터 빈도(히스토그램, 박스)
- 데이터 전처리
- 변수 검토
- 변수간 관계 분석 및 시각화

```python
import pandas as pd
import numpy as np
import matplotlib as mpl
import matplotlib.pyplot as plt
import matplotlib.pylab as plt
import datetime as dt
%matplotlib inline
import matplotlib
matplotlib.rcParams['axes.unicode_minus'] = False
```

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
koweps_df = xls.parse(xls.sheet_names[0])
```

```python
data_df = koweps_df.copy()
data_df.head()
```

- 원본 훼손될 수 있으니 카피해서 사용하자.

#### 해당 데이터 프레임에서 제공해 드린 컬럼들만 추출하여 변수명을 사용하고자 하는 컬럼들만 rename하세요.

```python
data_df.rename(columns={'h12_g3': '성별', 'h12_g4':'태어난 연도','h12_g10':'혼인상태','h12_g11':'종교','h12_eco9':'직종','p1202_8aq1':'일한달의 월 평균 임금','h12_reg7':'7개 권역별 지역구분'},inplace=True)
```

- rename으로 컬럼명을 변경하였다.

```python
data_df_col = data_df[['성별','태어난 연도','혼인상태','종교','직종','일한달의 월 평균 임금','7개 권역별 지역구분']]
data_df_col.head()
>
	성별	태어난 연도	혼인상태	종교	직종	일한달의 월 평균 임금	7개 권역별 지역구분
0	2		1936		2		2	NaN			NaN							1
1	2		1945		2		2	NaN			NaN							1
2	1		1948		2		2	NaN			NaN							1
3	1		1942		3		1	762.0		108.9						1
4	2		1923		2		1	NaN			NaN							1
```

- 사용할 컬럼만 가져온다.

#### 데이터 분석

- 성별의 데이터 분포 확인
-  성별을 비율순으로 정렬
- 데이터 시각화

```python
data_df_col.info()
>
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 15422 entries, 0 to 15421
Data columns (total 7 columns):
성별              15422 non-null int64
태어난 연도          15422 non-null int64
혼인상태            15422 non-null int64
종교              15422 non-null int64
직종              7131 non-null float64
일한달의 월 평균 임금    4507 non-null float64
7개 권역별 지역구분     15422 non-null int64
dtypes: float64(2), int64(5)
memory usage: 843.5 KB
```

- null값 있는지 없는지 확인하기

```python
data_df_gender = data_df_col['성별'].value_counts()
data_df_gender.plot.pie(autopct='%d%%',
                   startangle=45,
                   legend=True,
                   shadow=True,
                   labels=data_df_gender.index)
plt.axis('equal')
plt.show()
```

- 파이차트로 비율 시각화 하기

![vi40](./img/vi40.png)

```python
gender_filter_df = data_df_col.filter(['성별'])
gender_filter_df
>
	성별
0	2
1	2
2	1
3	1
4	2
```

- 이렇게 해서 진행 할 수 있다.

- `대괄호[]` 를 넣어줘여 값과 인덱스를 가져올 수 있다.

#### 성별값을 남, 여로 변경

```python
gender_filter_df['성별2'] = np.where(gender_filter_df['성별'] == 1 , '남', '여자')
```

- `np.where` 로 논리적인 제안을 줄 수 있다.

##### 결측값 확인

```python
gender_filter_df.isna().sum()
>
성별     0
성별2    0
dtype: int64
```

#### 데이터 분포 확인

```python
gender_cnt = gender_filter_df['성별'].value_counts()
gender_cnt
>
2    8440
1    6982
Name: 성별, dtype: int64
```

```python
gender_cnt = gender_filter_df['성별2'].value_counts()
gender_cnt
>
여자    8440
남     6982
Name: 성별2, dtype: int64
```

##### 시리즈를 데이터 프레임으로 변환

```python
gender_cnt_df = pd.DataFrame(gender_cnt)
gender_cnt_df.head()
>
	성별2
여자	8440
남	6982
```

```python
gender_cnt_df.rename(columns={'성별2' : '명'},inplace=True)
gender_cnt_df
>
		명
여자	8440
남	6982
```

##### 비율순으로 정렬

```python
gender_cnt_df.sort_values('명',inplace=True)
gender_cnt_df
>
		명
남	6982
여자	8440
```

##### 성별 분포를 시각화

```python
gender_cnt_df.plot.bar()
plt.title('성별본포')
plt.grid()
plt.xlabel('성별')
plt.ylabel('명')
for idx, value in enumerate(list(gender_cnt_df['명'])):
    txt = '%d명' % value
    plt.text(idx,value,txt,horizontalalignment='center',
            verticalalignment='bottom')
plt.show()
```

- 중간에 그래프에 데이터를 나태내기 위해 추가하였다.
  - bottom은 바닥이 아니라 그래프마지막이 bottom이다.

![vi41](./img/vi41.png)

```python
gender_cnt.plot.pie(autopct='%d%%',
                   startangle=90,
                   legend=True,
                   shadow=True,
                   labels=gender_cnt_df.index)
plt.axis('equal')
```

![vi42](./img/vi42.png)

- 이렇게 파이차트도 가능하다.

