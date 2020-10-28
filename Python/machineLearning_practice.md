# 머신러닝  타이타닉 실습

### [탐색적 데이터 분석]

#### 1. 데이터 로드 및 확인

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
%matplotlib inline

titanic_df = pd.read_csv('./data/titanic_train.csv')
titanic_df.head(3)
>
	PassengerId	Survived	Pclass	Name	Sex	Age	SibSp	Parch	Ticket	Fare	Cabin	Embarked
0	1	0	3	Braund, Mr. Owen Harris	male	22.0	1	0	A/5 21171	7.2500	NaN	S
1	2	1	1	Cumings, Mrs. John Bradley (Florence Briggs Th...	female	38.0	1	0	PC 17599	71.2833	C85	C
2	3	1	3	Heikkinen, Miss. Laina	female	26.0	0	0	STON/O2. 3101282	7.9250	NaN	S
```

#### 2. 데이터 정보 확인

```python
titanic_df.info()
>
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 891 entries, 0 to 890
Data columns (total 12 columns):
PassengerId    891 non-null int64
Survived       891 non-null int64
Pclass         891 non-null int64
Name           891 non-null object
Sex            891 non-null object
Age            714 non-null float64
SibSp          891 non-null int64
Parch          891 non-null int64
Ticket         891 non-null object
Fare           891 non-null float64
Cabin          204 non-null object
Embarked       889 non-null object
dtypes: float64(2), int64(5), object(5)
memory usage: 83.6+ KB
```

####  3. missingno 패키지를 이용한 결측값을 가지는 컬럼 확인 및 처리

- age는 평균으로, cabin 'N', embarked 'N' 으로 대체

```python
titanic_df['Age'].mean()
> 29.69911764705882

titanic_age_imputer = SimpleImputer(strategy = 'mean')
titanic_df['Age'] = titanic_age_imputer.fit_transform(titanic_df[['Age']])
msno.matrix(titanic_df)
plt.show()
```

- 우선 Age는 `SimpleImputer` 객체를 만들어 fit 과 transform을 실행하여 결측값을 처리하였다.

![ml13](./img/ml13.png)

- 정상적으로 Age컬럼의 결측값이 평균으로 처리되었다.

```python
titanic_df['Cabin'] = titanic_df['Cabin'].fillna('N')
msno.matrix(titanic_df)
plt.show()
```

- fillna로 간단하게 'N'으로 바꾸었다.

![ml14](./img/ml14.png)

```python
titanic_df['Embarked'] = titanic_df['Embarked'].fillna('N')
msno.matrix(titanic_df)
plt.show()
```

![ml15](./img/ml15.png)

- 모든 결측값이 대체되었다.

#### 4. age , cabin , embarked 빈도확인

```python
pd.DataFrame(titanic_df['Age'].value_counts()).T
```

![ml16](./img/ml16.jpg)

- value_counts()로 빈도를 확인해보았다.

```python
pd.DataFrame(titanic_df['Cabin'].value_counts()).T
```

![ml17](./img/ml17.jpg)

```python
pd.DataFrame(titanic_df['Embarked'].value_counts()).T
>
			S	C	Q	N
Embarked	644	168	77	2
```

#### 성별에 따른 생존여부 확인 및 barplot를 이용한 시각화

```python
fig = plt.figure(figsize=(15,5))

area01 = fig.add_subplot(1,3,1)
area01.set_title('titanic survived - sex')
area02 = fig.add_subplot(1,3,2)
area02.set_title('titanic survived - sex hue')
area03 = fig.add_subplot(1,3,3)
area03.set_title('titanic survived - sex dodge')

# 성별에 따른 생존률 시각화
sns.barplot(x='Sex',y='Survived',data=titanic_df,ax=area01,palette='Set2')
# hue 
sns.barplot(x='Sex',y='Survived',hue='Pclass',data=titanic_df,ax=area02,palette='Set2')
# dodge
sns.barplot(x='Sex',y='Survived',hue='Pclass',dodge=False,data=titanic_df,ax=area03,palette='Set2')

plt.show()
```

- 우선 그래프를 그리기 위해 fig를 생성하고 3개의 그림을 그리기 위해 (1,3,x) 를 지정한다.
- x에는 성별을 y에는 생존률을 주고 hue와 dodge에는 pclass를 줘서 정보를 확인해보았다.

![ml17](./img/ml17.png)

#### 6. Sex , Cabin , Embarked 에 대한 라벨인코딩

**Sex 라벨 인코딩**

```python
Sex_label = titanic_df['Sex']
encoder = LabelEncoder()
encoder.fit(Sex_label)
Sex_digit_label = encoder.transform(Sex_label)
print('encoder 결과', Sex_digit_label)
>
encoder 결과 [1 0 0 0 1 1 1 1 0 0 0 0 1 1 0 0 1 1 0 0 1 1 0 1 0 0 1 1 0 1 1 0 0 1 1 1 1...]
```

- 성별에 따라 1과 0이 부여되었다. male : 1, female:0이다.

**Cabin 라벨 인코딩**

```python
cabin_label = titanic_df['Cabin']
encoder = LabelEncoder()
encoder.fit(cabin_label)
cabin_digit_label = encoder.transform(cabin_label)
print('encoder 결과', cabin_digit_label)
>
encoder 결과 [146  81 146  55 146 146 129 146 146 146 145  49 146 146 146 146 146 146 ...]
```

**Embarked  라벨 인코딩**

```python
Embarked_label = titanic_df['Embarked']
encoder = LabelEncoder()
encoder.fit(Embarked_label)
Embarked_digit_label = encoder.transform(Embarked_label)
print('encoder 결과', Embarked_digit_label)
>
encoder 결과 [3 0 3 3 3 2 3 3 3 0 3 3 3 3 3 3 2 3 3 0 3 3 2 3 3 3 0 3 2 3 0 0 2 3 0 3 0...]
```

### [ML학습]

#### 1. 원본 데이터를 재로딩 하고, feature데이터 셋과 Label 데이터 셋 추출

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
%matplotlib inline

titanic_df = pd.read_csv('./data/titanic_train.csv')
titanic_df.head(3)
```

**결측값 처리**

```python
titanic_df['Cabin'] = titanic_df['Cabin'].fillna('N')
titanic_df['Embarked'] = titanic_df['Embarked'].fillna('N')
titanic_age_imputer = SimpleImputer(strategy = 'mean')
titanic_df['Age'] = titanic_age_imputer.fit_transform(titanic_df[['Age']])
```

**라벨인코딩**

```python
Sex_label = titanic_df['Sex']
encoder = LabelEncoder()
encoder.fit(Sex_label)
Sex_digit_label = encoder.transform(Sex_label)
cabin_label = titanic_df['Cabin']
encoder = LabelEncoder()
encoder.fit(cabin_label)
cabin_digit_label = encoder.transform(cabin_label)
Embarked_label = titanic_df['Embarked']
encoder = LabelEncoder()
encoder.fit(Embarked_label)
Embarked_digit_label = encoder.transform(Embarked_label)
```

```python
titanic_df['Sex'] = Sex_digit_label
titanic_df['Cabin'] = cabin_digit_label
titanic_df['Embarked'] = Embarked_digit_label
```

**feature, label 나누기**

```python
titanic_feature = titanic_df.loc[:, ~titanic_df.columns.isin(['Survived', 'Name','Ticket'])]
```

```python
titanic_label =  titanic_df.loc[:,['Survived']]
```

#### 2. 80:20 으로 데이터 분리(train_test_split)

```python
X_train, X_test, y_train, y_test = train_test_split(titanic_feature,titanic_label, test_size=0.2 ,random_state=100)
```

#### 3. 의사결정트리를 이용한 학습, 예측 및 정확도 확인

```python
titanic_df_dtc = DecisionTreeClassifier(random_state=100)
titanic_df_dtc.fit(X_train, y_train)
predition = titanic_df_dtc.predict(X_test)
print('예측 정확도 : %.2f' % accuracy_score(y_test, predition))
>
예측 정확도 : 0.78
```

- 정확도가 낮다.
- 