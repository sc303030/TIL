# Pandas_실습02

#### 1. 각 파일을 pd.read_csv() 로 읽어들인다
- 헤더 및 구분자 확인요망

```python
ratings_df = pd.read_csv('./data/ratings.dat',sep='::',header=None)
display(ratings_df.head())
movies_df = pd.read_csv('./data/movies.dat',sep='::',header=None)
display(movies_df.head())
users = pd.read_csv('./data/users.dat',sep='::',header=None)
display(users.head())
>
	0	1	2	3
0	1	1193	5	978300760
1	1	661	3	978302109
2	1	914	3	978301968
3	1	3408	4	978300275
4	1	2355	5	978824291

	0	1	2
0	1	Toy Story (1995)	Animation|Children's|Comedy
1	2	Jumanji (1995)	Adventure|Children's|Fantasy
2	3	Grumpier Old Men (1995)	Comedy|Romance
3	4	Waiting to Exhale (1995)	Comedy|Drama
4	5	Father of the Bride Part II (1995)	Comedy

	0	1	2	3	4
0	1	F	1	10	48067
1	2	M	56	16	70072
2	3	M	25	15	55117
3	4	M	45	7	02460
4	5	M	25	20	55455
```

#### 2. 데이터 확인 및 구조분석과 각 데이터 프레임에 헤더를 추가

ratings.dat
- userId movieId rating timestamp

movies.data

- movieId title genres

users.data

- userId gender age

```python
ratings_df.info()
movies_df.info()
users.info()
```

```python
users = users.iloc[:,0:3]
# users.drop([3,4],axis=1,inplace=True)
```

```python
ratings_df.columns = ["userId",  "movieId", "rating", "timestamp"]
movies_df.columns = ["movieId", "title", "genres"]
users.columns = ["userId", "gender"," age"]
```

- 각 데이커에 컬럼이름을 부여하였다.

#### 3. 공통 컬럼을 이용한 데이터 병합

```python
movies_df = pd.merge(pd.merge(ratings_df,movies_df),users_df)
>
userId	movieId	rating	timestamp	title	genres	gender	age
0	1	1193	5	978300760	One Flew Over the Cuckoo's Nest (1975)	Drama	F	1
1	1	661	3	978302109	James and the Giant Peach (1996)	Animation|Children's|Musical	F	1
```

- 한 번에 merge를 하면 편리하다.

#### 4.user 평점 갯수 순위 확인

```python
user_rating = movies_df.groupby('userId')[['rating']].count()
user_rating.sort_values(by='rating',ascending=False)
>		rating
userId	
4169	2314
1680	1850
```

- 그룹을 통해 유저의 영화 개수를 구했고 다시 내림차순해서 순위를 구하였다.

#### 5. 각 영화의 성별 평균 평점

```python
movies_df.pivot_table('rating',"gender","movieId",aggfunc=np.mean)
>
movieId		1				2	
gender																					
F		4.187817       3.278409
M		4.130552	   3.175238
```

- 피벗테이블로 바로 하면 이렇게 나온다.

#### 6.  평점이 220개 이상인 영화만 필터링

```python
mo_220 = movies_df.groupby('movieId')["rating"].agg(['count']).reset_index()
m_data = mo_220[mo_220['count'] >= 220]
movie_rating_220 = pd.merge(m_data,movies_df,on="movieId",how='inner')
movie_rating_220
>
movieId	count	userId	rating	timestamp	title	genres	gender	age
0	1	2077	1	5	978824268	Toy Story (1995)	Animation|Children's|Comedy	F	1
1	1	2077	18	4	978154768	Toy Story (1995)	Animation|Children's|Comedy	F	18
```

- 영화아이디로 그룹하고 평점의 개수를 센다.
- 그 다음에 평점의 개수가 220개 이상인 것들을 다시 뽑는다.
- 머지해서 다른 정보들도 출력해본다.

#### 7. 평점이 200개 이상인 영화들의 성별 평균 평점

```python
mo_200 = mo_220[mo_220['count']>= 200]
mo_200_df =  pd.merge(mo_200,movies_df,on="movieId",how='inner')
mo_200_df.groupby(['title','gender'])['rating'].agg(['mean'])
>
														mean
title								gender	
'burbs, The (1989)					F				2.793478
									M				2.962085
10 Things I Hate About You (1999)	F				3.646552
									M				3.311966
```

- 위에서 만들었던 mo_200에서 다시 200넘는것들만 추출한다.
- 그걸 원본 데이터와 합쳐서 정보를 가져온다.
- 영화 제목과 성별로 그룹나누고 평점 평균을 구한다.

#### 8. 여성이 가장 좋아하는 영화 순위

```python
movies_df_f = movies_df[movies_df['gender']=='F']
movies_df_f_no = movies_df_f.groupby('movieId')['rating'].agg(['count']).reset_index()
movies_df_f_no = movies_df_f_no.sort_values(by='count',ascending=False)
movies_df_f_no
>
	movieId		count
2503	2858	946
2070	2396	798
559		593		706
```

- 원본 데이터에서 여성인 데이터만들 추출하였다.
- 영화 제목과 평점으로 갯수를 카운트하고 내림차순으로 정리하여 출력하였다.

#### 9. 남녀간의 평균 평점 차이

```python
gender_diff = movies_df.groupby('gender')['rating'].agg(['mean']).reset_index()
gender_diff.loc[0,'mean'] - gender_diff.loc[1,'mean'] 
>
0.05148748291259997
```

- 성별로 그룹지어 평점의 평균을 구했다.
- 그 다음에 여성에서 남성을 뺐다.
- 여성이 조금 높다.

#### 10. 평점의 표준편차가 큰 영화

```python
movie_std = movies_df.groupby('movieId')['rating'].agg(['std']).reset_index()
movie_std.sort_values(by='std',ascending=False)
>
		movieId		std
558		572		2.828427
3557	3800	2.309401
```

- 집계함수에 표준편차를 줘서 내림차순으로 정리하였다.

#### 11 . Comedy영화 중 평점이 낮은 영화의 제목

```python
comedy_movie = movies_df[movies_df['genres']=='Comedy']
comedy_movie.groupby('title')['rating'].agg(['min']).reset_index()
>
				title		min
0	'burbs, The (1989)		1
1	20 Dates (1998)			1
2	28 Days (2000)			1
```

- 코미디 장르만 추출하였다.
- 제목으로 그룹지어 평점이 가장 낮은 영화들을 출력하였다.

#### 12. 평균 평점이 가장 높은 영화의 제목(동률이 있을 경우 모두 출력)

```python
movie_rs_max = movies_df.groupby('title')['rating'].agg(['mean']).reset_index()
movie_rs_max[movie_rs_max['mean'] == movie_rs_max['mean'].max()]
>
											title	mean
249									Baby, The (1973)	5.0
407							Bittersweet Motel (2000)	5.0
1203							Follow the Bitch (1998)	5.0
1297				Gate of Heavenly Peace, The (1995)	5.0
2025									Lured (1947)	5.0
2453						One Little Indian (1973)	5.0
2903		Schlafes Bruder (Brother of Sleep) (1995)	5.0
3044							Smashing Time (1967)	5.0
3087							Song of Freedom (1936)	5.0
3477							Ulysses (Ulisse) (1954)	5.0
```

- 제목으로 그룹을 지어 평점의 평균을 구한다.
- 그 다음 데이터에 max조건을 줘서 출력한다.

#### 13. 각 영화별 평균 평점

```python
movies_df.groupby('title')['rating'].agg(['mean'])
>
							mean
				title	
$1,000,000 Duck (1971)	3.027027
'Night Mother (1986)	3.371429
```

- 제목으로 그룹짓고 평점에 평균함수를 주었다.

- 데이터 프레임으로 만들고 싶어서 []을 해줬다.

  