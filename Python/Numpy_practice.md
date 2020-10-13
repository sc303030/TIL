# Numpy 실습

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
```

```python
weather_df = pd.read_csv('./data/weather_20201012.csv', sep=',',encoding='cp949')
weather_df.head()
>
		   날짜  지점평균기온(℃)	최저기온(℃)	최고기온(℃)
0	1907-10-01	108	13.5				7.9			20.7
1	1907-10-02	108	16.2				7.9			22.0
2	1907-10-03	108	16.2				13.1		21.3
3	1907-10-04	108	16.5				11.2		22.0
4	1907-10-05	108	17.6				10.9		25.4
```

```python
weather_df.info()
>
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 40854 entries, 0 to 40853
Data columns (total 5 columns):
날짜         40854 non-null object
지점         40854 non-null int64
평균기온(℃)    40098 non-null float64
최저기온(℃)    40097 non-null float64
최고기온(℃)    40096 non-null float64
dtypes: float64(3), int64(1), object(1)
memory usage: 1.6+ MB
```

#### 위 데이터에서 기온이 가장 높았던 날은 언제이고 몇도인지를 데이터 프레임으로 출력해 보자

```python
weather_df[weather_df['최고기온(℃)'] == weather_df['최고기온(℃)'].max()]
> 
				날짜	지점	평균기온(℃)	최저기온(℃)	최고기온(℃)
40051	2018-08-01	  108	    33.6	   27.8	      39.6
```

일반 파일경우에는 루프돌려서 하나씩 비교해줘야함

```python
year2020_df = pd.read_csv('./data/year2020_baby_name.csv', sep=',',encoding='cp949')
year2020_df.head()
>
		NAME	GENDER	COUNT
0	Isabella		F	22731
1	Sophia			F	20477
2	Emma			F	17179
3	Olivia			F	16860
4	Ava				F	15300
```

#### 정렬 : sort_values(by='기준', ascending=True(오름차순), False(내림차순))

```python
sort_df = year2020_df.sort_values(by='COUNT', ascending=True).head(10)
sort_df
>
          NAME	GENDER	COUNT
16918	Adwoa		F		5
18460	Lakisha		F		5
18461	Lalena		F		5
```

```python
sort_df = year2020_df.sort_values(by='COUNT', ascending=False).head(10)
sort_df
>
       NAME		GENDER	COUNT
0	Isabella		F	22731
19698	Jacob		M	21875
```

#### 타입변한 : astype(type)

```python
rank_df = year2020_df.sort_values(by='COUNT', ascending=False)['COUNT'].rank(ascending=False)
rank_df # 순위가 리턴됨
rank = rank_df.astype('int64')
rank

sort_df['RANK'] = rank
sort_df
>
		NAME	GENDER	COUNT	RANK
0	Isabella		F	22731	1
19698	Jacob		M	21875	2
1	Sophia			F	20477	3
```

- 순위를 매겨서 그걸 다시 정수로 변환하고 열에 추가하였다.

#### gender를 기준으로 M 데이터 프레임을 만들어라

```python
year2020_df_M =year2020_df[year2020_df['GENDER'] == 'M']
year2020_df_M.head(5)
>
		NAME	GENDER	COUNT
19698	Jacob		M	21875
19699	Ethan		M	17866
19700	Michael		M	17133
19701	Jayden		M	17030
19702	William		M	16870
```

#### gender를 기준으로 F 데이터 프레임을 만들어라

```python
year2020_df_F =year2020_df[year2020_df['GENDER'] == 'F']
year2020_df_F.head(5)
>
		NAME	GENDER	COUNT
0	Isabella		F	22731
1	Sophia			F	20477
2	Emma			F	17179
3	Olivia			F	16860
4	Ava				F	15300
```

#### 두 개의 데이터를 합치고 싶다. 그러나 인덱스가 다르기 때문에 인덱스를 리셋시켜야한다.

**reset_index(drop=True)**

```python
year2020_df_M = year2020_df_M.reset_index(drop=True)
year2020_df_M
>
		NAME	GENDER	COUNT	RANK
0	Jacob			M	21875	2
1	Ethan			M	17866	4
2	Michael			M	17133	6
3	Jayden			M	17030	7
4	William			M	16870	8
5	Alexander		M	16634	10
```

- drop=Ture하면 원래 인덱스가 버려진다.

**pd.merge(data1, data2,left_index=True, right_index=True)**

```python
gender_df = pd.merge(year2020_df_M, year2020_df_F, left_index=True, right_index=True)
gender_df 
>
	NAME_x	GENDER_x	COUNT_x	RANK_x	NAME_y	GENDER_y	COUNT_y	RANK_y
0	Jacob			M	21875		2	Isabella		F	22731	1
1	Ethan			M	17866		4	Sophia			F	20477	3
2	Michael			M	17133		6	Emma			F	17179	5
3	Jayden			M	17030		7	Olivia			F	16860	9
```

- `left_index=True, right_index=True` 하나라도 빠지면 에러가 뜬다.

```python
rank = gender_df['COUNT_x'].rank(ascending=False)
rank = rank.astype('int64')
gender_df['RANK'] = rank
gender_df
>
	NAME_x	GENDER_x	COUNT_x	RANK_x	NAME_y	GENDER_y	COUNT_y	RANK_y	RANK
0	Jacob			M	21875		2	Isabella		F	22731		1	1
1	Ethan			M	17866		4	Sophia			F	20477		3	2
2	Michael			M	17133		6	Emma			F	17179		5	3
3	Jayden			M	17030		7	Olivia			F	16860		9	4
```

- 다시 한 번 순위를 매겼다. 순위가 실수형이라 다시 정수로 바꿔주고 열에 추가하였다. 





