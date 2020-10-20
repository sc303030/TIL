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

#### 성별에 따른 평균 급여 차이를 분석

- 성별과 월급 데이터만 추출
- 성별을 남자와 여자로 변환
- 데이터 정제(결측값 확인, 결측값 제거, 이상치 결측 처리)
- 데이터 분석(성별로 그룹화하여 그룹별 평균)
- 데이터 시각화

```python
data_df_ge_sa = data_df_col[['성별',"일한달의 월 평균 임금"]]
data_df_ge_sa['성별2'] = np.where(data_df_ge_sa['성별'] == 1 , '남', '여자')
```

- 우선 성별을 남자와 여자로 바꾸었다.

```python
data_df_ge_sa.dropna(inplace=True)
data_df_sa = data_df_ge_sa['일한달의 월 평균 임금']
```

- 결측값들을 제거하고 이상치를 확인하기 위해 월 평균 임슴만 따로 변수에 저장한다.

```python
data_df_ge_sa[['일한달의 월 평균 임금']].boxplot()
```

- 이상치 그래프를 그려본다.

![vi43](./img/vi43.png)

```python
quantile25 = data_df_sa.quantile(q=0.25)
quantile25
>
135.0

quantile75 = data_df_sa.quantile(q=0.75)
quantile75
>
336.0

iqr = quantile75 - quantile25
iqr
>
201.0
```

- 1사분위수와 3사분위수를 구하여 iqr을 계산한다.

```python
lower_fence = quantile25 - 1.5 * iqr
upper_fence = quantile75 + 1.5 * iqr
lower_outlier = data_df_sa[data_df_sa > lower_fence].min()
upper_outlier = data_df_sa[data_df_sa < upper_fence].max()
```

- 최저한계치와 최고한계치를 구해서 `data_df_sa` 에서 이 수치들보다 높은 것들을 뽑는다.

```python
outlier_clean_df = data_df_ge_sa.copy()
sa_out = data_df_ge_sa[data_df_ge_sa['일한달의 월 평균 임금'] > upper_outlier]
for idx in sa_out.index:
    outlier_clean_df.loc[idx,'일한달의 월 평균 임금'] = np.nan
outlier_clean_df.isna().sum()
>
성별                0
일한달의 월 평균 임금    207
dtype: int64
```

- `lower_outlier ` 가 boxplot에서 없었기 때문에 upper의 이상치만 nan으로 바꾸었다.

```python
outlier_clean_df.dropna().groupby('성별').mean()
>
		일한달의 월 평균 임금
성별	
남			289.125203
여자			170.066146
```

- 최종으로 그룹지어서 평균을 하면 된다.