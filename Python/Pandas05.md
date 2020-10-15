# Pandas_05

- 람다식에서 else 가 많아질수록 코드가 복잡해 지는 경우 발생
- 람다식에 함수 호출하는 식이 좋다.

**15세 이하는 child, 15 ~ 60세 까지는 adult , 61~이상은 elderly 로 분류를 해서 분류된 정보를 age_division 컬럼에 저장**

**lambda 식으로 apply 함수를 이용해서 구현한다면?**

```python
func_age = lambda x : 'child' if x <= 15 else ('adult' if x >= 16 and x <= 60 else 'elderly')
titanic_reset_index_df['age_division'] = titanic_reset_index_df['age'].apply(func_age)
titanic_reset_index_df['age_division'].value_counts()
>
adult      606
elderly    199
child       83
Name: age_division, dtype: int64
```

- 람다식으로 하였다. 람다식에서는 `elif` 가 안 되기 때문에 `else`안에 또 `if` 를 넣었다.

- lambda if ~ else 구문형식
- lambda 매개변수 : true if 조건식 else false

#### 나이에 따라서 세분화된 분류를 수행하는 함수 생성

```python
def get_category(age):
    catrgory = ''
    if age <= 5:
        category = 'baby'
    elif age <= 12:
        category = 'child'
    elif age <= 19:
        category = 'teenager'
    elif age <= 24:
        category = 'student'
    elif age <= 39:
        category = 'young adult'
    elif age <= 60:
        category = 'adult'
    else:
        category = 'elderly'
    return catrgory
```

```python
titanic_reset_index_df['age_category'] = titanic_reset_index_df['age'].apply(lambda x : get_category(x))
titanic_reset_index_df['age_category'].value_counts()
>
young adult    272
elderly        199
adult          141
student        112
teenager        95
baby            44
child           25
Name: age_division, dtype: int64
```

- 이렇게 함수를 만들어서 람다 함수 구현부에 만든 함수를 부여할 수 있다. 

#### apply() 함수에 DataFrame이 넘어온다면?

```python
titanic_reset_index_df['child/adult'] = titanic_reset_index_df.apply(lambda f : 'adult' if f.age >= 20 else 'child',  axis=1)
titanic_reset_index_df['child/adult'].value_counts()
>
adult    547
child    341
Name: child/adult, dtype: int64
```

- 데이터 프레임이 넘어오니깐 우리가 열에 접근하는 것처럼 접근한다.

- **열이나 행의 방향**을 무조건 잡아줘야 한다.

**승객에 대한 나이와 성별에 의한 카테고리를 cat 으로 추가한다.**

- 조건1. 20살이 넘으면 성별을 그대로 사용하고

- 조건2. 20살 이상니면 성별에 관계없이 'child' 라고 정의

```python
titanic_reset_index_df['cat'] = titanic_reset_index_df.apply(lambda f : f.sex if f.age >= 20 else 'child', axis=1)
titanic_reset_index_df['cat'].value_counts()
>
male      363
child     341
female    184
Name: cat, dtype: int64
```

#### fillna :  NaN을 원하는 값으로 변경할 수 있다.

```python
sample_df = pd.DataFrame({
    'A' : [1,3,4,3,4],
    'B' : [2,3,1,2,3],
    'C' : [1,5,2,4,4]
})
sample_df
>
	A	B	C
0	1	2	1
1	3	3	5
2	4	1	2
3	3	2	4
4	4	3	4
```

```python
sample_df.iloc[2,2] = np.nan
sample_df
>
	A	B	C
0	1	2	1.0
1	3	3	5.0
2	4	1	NaN
3	3	2	4.0
4	4	3	4.0
```

```python
sample_df.apply(pd.value_counts).fillna(0.0)
>
	A	B	C
1.0	1.0	1.0	1.0
2.0	0.0	2.0	0.0
3.0	2.0	2.0	0.0
4.0	2.0	0.0	2.0
5.0	0.0	0.0	1.0
```

- `pd.value_counts` : 이렇게 하면 `NaN` 값만 몇개인지 나오니 그걸 0.0으로 바꿔라.

#### astype() :  자료형 변환

```python
sample_df.apply(pd.value_counts).fillna(0).astype(int)
>
	A	B	C
1.0	1	1	1
2.0	0	2	0
3.0	2	2	0
4.0	2	0	2
5.0	0	0	1
```

#### 타이타닉 승객 중 나이를 명시하지 않은 고객은 나이를 명시한 고객의 평균 나이 값으로 대체

```python
titanic['age'] = titanic['age'].fillna(titanic['age'].mean()).astype(int)
```

**타이타닉 승객에 대해 나이와 성별에 의한 age_gender_cat 열을 만들고**

- 조건1. 성별을 나타내는 문자열 male 또는 female로 시작한다.
- 조건2. 성별을 나타내는 문자열 뒤에 나이를 나태나는 문자열로 변경하라
- 조건 3. 예시) 남성의 나이가 27이라면 -> male27

```python
titanic["age_gender_cat"] = titanic["sex"] + titanic["age"].apply(str)
titanic.head()
>
survived	pclass	sex	age	sibsp	parch	fare	embarked	class	who	adult_male	deck	embark_town	alive	alone	age_gender_cat
0	0	3	male	22	1	0	7.2500	S	Third	man	True	NaN	Southampton	no	False	male22
1	1	1	female	38	1	0	71.2833	C	First	woman	False	C	Cherbourg	yes	False	female38
2	1	3	female	26	0	0	7.9250	S	Third	woman	False	NaN	Southampton	yes	True	female26
3	1	1	female	35	1	0	53.1000	S	First	woman	False	C	Southampton	yes	False	female35
4	0	3	male	35	0	0	8.0500	S	Third	man	True	NaN	Southampton	no	True	male35
```

- 나이를 문자열로 바꾸고 결합하였다.

#### 데이터 프레임 인덱스 조작하는 방법

- set_index : 기존 행 인덱스를 제거하고 데이터 열 중 하나를 인덱스로 설정
- reset_index : 기존 행 인덱스를 제거하고 인덱스를 데이터 열로 추가

```python
np.random.seed(100)
index_df = pd.DataFrame(np.vstack([list('ABCDE'),
                                  np.round(np.random.rand(3,5),2)]).T,
                       columns=['col01','col02','col03','col04'])
index_df
>
	col01	col02	col03	col04
0		A	0.54	0.12	0.89
1		B	0.28	0.67	0.21
2		C	0.42	0.83	0.19
3		D	0.84	0.14	0.11
4		E	0.0		0.58	0.22
```

**col01을 인덱스로**

```python
index_df2 = index_df.set_index('col01')
index_df2
>
		col02	col03	col04
col01			
A		0.54	0.12	0.89
B		0.28	0.67	0.21
C		0.42	0.83	0.19
D		0.84	0.14	0.11
E		0.0		0.58	0.22
```

**col02를 인덱스로**

```python
index_df3 = index_df2.set_index('col02')
index_df3
>
		col03	col04
col02		
0.54	0.12	0.89
0.28	0.67	0.21
0.42	0.83	0.19
0.84	0.14	0.11
0.0		0.58	0.22
```

##### reset_index

```python
index_df2.reset_index()
>
	col01	col02	col03	col04
0	A		0.54	0.12	0.89
1	B		0.28	0.67	0.21
2	C		0.42	0.83	0.19
3	D		0.84	0.14	0.11
4	E		0.0		0.58	0.22
```

- 인덱스가 다시 생겼다.

##### reset_index(drop=True)

```python
index_df2.reset_index(drop=True)
>
	col02	col03	col04
0	0.54	0.12	0.89
1	0.28	0.67	0.21
2	0.42	0.83	0.19
3	0.84	0.14	0.11
4	0.0		0.58	0.22
```

- `col01` 인덱스가 새로운 열로 생기지 않고 삭제되었다.

#### 5명의 학생의 국어, 영어, 수학 점수를 나타내는 데이터프레임을 만든다.

- 학생 이름을 나타내는 열을 포함시키지 않고  데이터프레임 df_score1 을 생성한 후, df_score1.index 속성에 학생 이름을 나타내는 열을 지정하여 인덱스를 지정한다.  
- reset_index 명령으로 이 인덱스 열을 명령으로 일반 데이터열로 바꾸여 데이터프레임 df_score2을 만든다.

```python
df_score1 = pd.DataFrame({
    '국어' : [100,95,90],
    '영어' : [87,90,89],
    '수학' : [90,97,95]
})
df_score1.index = ['펭하','펭빠','펭펭']
df_score1.index.name = '이름'
df_score2 = df_score1.reset_index()
df_score2
>
	이름	국어	영어	수학
0	펭하	100	  87	90
1	펭빠	95	  90	97
2	펭펭	90	  89	95
```

- 학생 이름을 나타내는 열이 일반 데이터 열을 포함하는 데이터프레임 df_score2에 set_index 명령을 적용하여 다시 학생 이름을 나타내는 열을 인덱스로 변경한다.

```python
df_score2 = df_score2.set_index('이름')
df_score2
>
	국어	영어	수학
이름			
펭하	100	87	  90
펭빠	95	90	  97
펭펭	90	89	  95
```

- np.random.randint : 정수난수 1개 생성
- np.random.rand : 0 ~ 1 사이의 분포된 난수 생성
- np.random.randn : 가우시안 표준 정규분포에서 난수 생성

```python
np.random.randint(6) # 0~ 5
> 3
```

- 정수 난수

```python
np.random.randint(1, 20) # 1~ 19
> 13
```

- 범위를 줄 수도 있다.

```python
np.random.rand(6) # 0 ~ 1
>
array([0.81168315, 0.17194101, 0.81622475, 0.27407375, 0.43170418,
       0.94002982])
```

- 넘파이의 1차원array

```python
np.random.rand(3,2)
>
array([[0.81764938, 0.33611195],
       [0.17541045, 0.37283205],
       [0.00568851, 0.25242635]])
```

- 행렬의 매트릭스가 된다.

```python
np.random.randn(3,2)
>
array([[ 1.61898166,  1.54160517],
       [-0.25187914, -0.84243574],
       [ 0.18451869,  0.9370822 ]])
```

- 정규분포라 `-` 가 있을 수 있다.

```python
np.random.randn(6) # 가우시안 정규분포 난수
>
array([ 0.73100034,  1.36155613, -0.32623806,  0.05567601,  0.22239961,
       -1.443217  ])
```

### DataFrmae merge

```python
data1 = {
    '학번' : [1,2,3,4],
    '이름' : ['펭수','펭하','펭빠','펭펭'],
    '학년' : [2,4,1,3]
}
data2 = {
    '학번' : [4,3,2,1],
    '학과' : ['CS','Math','Math','CS'],
    '학점' : [2.4,4.5,3.4,3.9]
}
```

```python
stu_df = pd.DataFrame(data1)
major_df = pd.DataFrame(data2)
display(stu_df)
display(major_df)
>
	학번	이름	학년
0	1		펭수	2
1	2		펭하	4
2	3		펭빠	1
3	4		펭펭	3
	학번	학과		학점
0	4		CS		2.4
1	3		Math	4.5
2	2		Math	3.4
3	1		CS		3.9
```

#### pd.merge()

```python
pd.merge(stu_df,major_df)
>
	학번	이름	학년	학과		학점
0	1	펭수		2	CS		3.9
1	2	펭하		4	Math	3.4
2	3	펭빠		1	Math	4.5
3	4	펭펭		3	CS		2.4
```

- 학번 수정 후 해보기

```python
data2 = {
    '학번' : [1,2,4,5],
    '학과' : ['CS','Math','Math','CS'],
    '학점' : [2.4,4.5,3.4,3.9]
}
```

```python
pd.merge(stu_df,major_df)
>

	학번	이름	학년	학과		학점
0	1	펭수	2		CS		2.4
1	2	펭하	4		Math	4.5
2	4	펭펭	3		Math	3.4
```

- 학번 있는 데이터만 출력됨

**pd.merge(data1,data2, on='기준', how='inner')**

- on = 열 인덱스 디폴트 같이 있는거
- how  = 어떻게 합칠 것인지. 디폴트 inner

**how = 'left'**

```python
pd.merge(stu_df,major_df, on='학번', how='left')
>
		학번	  이름	학년	학과	    학점
0		1		펭수		2	CS		2.4
1		2		펭하		4	Math	4.5
2		3		펭빠		1	NaN		NaN
3		4		펭펭		3	Math	3.4
```

- 왼쪽데이터를 매칭되지 않더라도 모두 출력
  - 매칭되는 값이 없기에 NaN값으로 들어간다.

**how = 'right'**

```python
pd.merge(stu_df,major_df, on='학번', how='right')
>
	학번	이름	학년	학과	     학점
0	1	 펭수	 2.0	CS	    2.4
1	2	 펭하	 4.0	Math	4.5
2	4	 펭펭	 3.0	Math	3.4
3	5	 NaN   NaN	  CS	 3.9
```

- 오른쪽 데이터를 매칭되지 않더라도 모두 출력
  - 매칭되는 값이 없기에 NaN값으로 들어간다.

**how = 'outer'**

```python
pd.merge(stu_df,major_df, on='학번', how='outer')
>
	학번		이름	학년	 	학과		학점
0	1		펭수		2.0		CS		2.4
1	2		펭하		4.0		Math	4.5
2	3		펭빠		1.0		NaN		NaN
3	4		펭펭		3.0		Math	3.4
4	5		NaN		  NaN	  CS	  3.9
```

- 모든 데이터 출력한다.