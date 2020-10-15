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
```

```python
ratings_df.columns = ["userId",  "movieId", "rating", "timestamp"]
movies_df.columns = ["movieId", "title", "genres"]
users.columns = ["userId", "gender"," age"]
```

- 각 데이커에 컬럼이름을 부여하였다.

#### 3. 공통 컬럼을 이용한 데이터 병합

```python
r_m_df = pd.merge(ratings_df,movies_df,on='movieId')
r_m_df.head()
>
	userId	movieId	rating	timestamp	title									genres
0		1		1193	5	978300760	One Flew Over the Cuckoo's Nest (1975)	Drama
1		2		1193	5	978298413	One Flew Over the Cuckoo's Nest (1975)	Drama
2		12		1193	4	978220179	One Flew Over the Cuckoo's Nest (1975)	Drama
3		15		1193	4	978199279	One Flew Over the Cuckoo's Nest (1975)	Drama
4		17		1193	5	978158471	One Flew Over the Cuckoo's Nest (1975)	Drama
```

```python
r_m_u_df = pd.merge(r_m_df, users, on='userId')
r_m_u_df.head()
>
userId	movieId	rating	timestamp	title	genres	ender	age
0	1	1193	5	978300760	One Flew Over the Cuckoo's Nest (1975)	Drama	F	1
1	1	661	3	978302109	James and the Giant Peach (1996)	Animation|Children's|Musical	F	1
2	1	914	3	978301968	My Fair Lady (1964)	Musical|Romance	F	1
3	1	3408	4	978300275	Erin Brockovich (2000)	Drama	F	1
4	1	2355	5	978824291	Bug's Life, A (1998)	Animation|Children's|Comedy	F	1
```

- 한 번에 3개의 데이터를 합칠 수 없어서 우선 `movieId` 를 가진 영화와 별점을 합쳤다.
- 그 다음에 `userId` 로 유저 데이터를 합쳤다.

#### 4.user 평점 갯수 순위 확인

```python
user_cnt = pd.DataFrame(r_m_u_df['userId'].value_counts())
user_cnt.index.name = 'userId'
user_cnt.columns = ['cnt']
display(user_cnt)
>
		cnt
userId	
4169	2314
1680	1850
4277	1743
```

- 유저 컬럼에서 중복이 몇개인지 확인하였다. 그 값이 평점 개수이다.

#### 7. 평점이 200개 이상인 영화들의 성별 평균 평점

```python
movie_cnt = pd.DataFrame(r_m_u_df['movieId'].value_counts())
movie_cnt.index.name = 'movieId'
movie_cnt.columns = ['cnt']
movie_cnt
>
		cnt
movieId	
2858	3428
260		2991
1196	2990
1210	2883
```

- 우선 영화마다 평점이 몇 개인지 뽑았다.

```python
m_200_id =  movie_cnt[movie_cnt['cnt'] >= 200].index
```

- 그 다음에 200이 넘는 것들의 영화id만 다시 변수에 담았다.

```python
a = r_m_u_df[ r_m_u_df['movieId'].isin(m_200_id)]
female = a[a['ender']=='F']['rating'].mean()
male = a[a['ender']=='M']['rating'].mean()
```

- 만약에 영화 아이디가 내가 만든 변수에 있다면 그 데이터들만 다시 새로운 변수에 담았다. 거기서 성별로 평균을 추출하였다.

```python
print('F : ',female)
print('M : ', male)
>
F :  3.688517650906957
M :  3.650922994518683
```

#### 8. 여성이 가장 좋아하는 영화 순위

```python
f_b = r_m_u_df[r_m_u_df['ender']=='F']
f_best = pd.DataFrame(f_b['movieId'].value_counts())
f_best.index.name = 'movieId'
f_best.columns = ['cnt']
f_best["rank"] = range(1,len(f_best)+1)
f_best.head()
>
		cnt	rank
movieId		
2858	946		1
2396	798		2
593		706		3
2762	664		4
1265	658		5
```

- 원본 데이터에서 여성인 데이터만들 추출하였다.
- 그 데이터로 다시 영화 갯수를 카운트 하였다.
- 순위를 매기고 출력하였다.

#### 9. 남녀간의 평균 평점 차이

```python
f_mean = r_m_u_df[r_m_u_df['ender']=='F']['rating'].mean()
m_mean = r_m_u_df[r_m_u_df['ender']=='M']['rating'].mean()
print('차이 : ', f_mean- m_mean)
> 
F :  3.6203660120110372
M :  3.5688785290984373
차이 :  0.05148748291259997
```

- 여자가 좀 더 높다.

#### 10 . Comedy영화 중 평점이 낮은 영화의 제목

```python
c_low = r_m_u_df[r_m_u_df['genres'] == 'Comedy']
c_titile = c_low[c_low['rating'] == c_low['rating'].min()]['title']
c_titile.value_counts()
>
Austin Powers: The Spy Who Shagged Me (1999)                108
Dumb & Dumber (1994)                                        105
Ace Ventura: When Nature Calls (1995)                        98
Baby Geniuses (1999)                                         96
```

- 장르가 코미디인을 먼저 추출하여 새로운 변수에 할당하였다.
- 그 다음에 평점이 최저인 것들의 제목만 뽑아서 중복되는 것들을 정리해서 제목들만 뽑았다.

#### 11. 평균 평점이 가장 높은 영화의 제목(동률이 있을 경우 모두 출력)

```python
title_list = pd.DataFrame(r_m_u_df['title'].value_counts()).index
title_list
t1 = [r_m_u_df[r_m_u_df['title']  == title_list[i]]["rating"].mean() for i in range(len(title_list)) ]
t1 = np.array(t1)
title_list[t1.argmax()]
> 'Gate of Heavenly Peace, The (1995)'
```

- 우선 제목을 리스트로 만든다.
- for루프를 통해 원본데이터와 리스트로 만든 영화 제목을 비교해 평균 평점을 리스트 컴프리헨션한다.
- 그러면 리스트에 담긴 평점이랑 타이틀 리스트랑 인덱스가 같으니깐 거기서 최대값의 리스트를 다시 제목 리스트에 부여하면 제목이 출력된다.

#### 12. 각 영화별 평균 평점

```python
movie_ra = pd.DataFrame({
    '제목' : title_list,
    '평균 평점' : t1
})
movie_ra
>
						제목								평균 평점
0	American Beauty (1999)								4.317386
1	Star Wars: Episode IV - A New Hope (1977)			4.453694
2	Star Wars: Episode V - The Empire Strikes Back...	4.292977
```

- 위에서 만들어 놓은 리스트덕분에 바로 데이터 프레임을 만들 수 있었다.

  