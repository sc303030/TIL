# 머신러닝_04

### 분류 성능 평가 지표

- 정확도(Accuracy)
- 오차행렬 (Confusion Matrix)
  - 이진 분류의 예측 오류가 얼마인지와 더불어 어떠한 유형의 예측 오류가 발생하고 있는지 함께 나타내는 지표
  - 행 : 실제 target
    - Negative(0) : 스팸이 아니다.
    - Positive(1) : 스팸이다.
  - 열 : 예측 값
    - Negative(0) : 스팸이 아니다.
    - Positive(1) : 스팸이다.
- 정밀도 (Precision)
- 재현율 (Recall)
- F1 스코어
- ROC AUC

#### 정확도만 가지고 분류 모델을 평가하면 안될까?

짜장 9개 - 짬뽕 1개

레이블 자체가 불균형할 때 성능을 평가하는 건 위험하다.

다른 평가지표도 함께 따져야 한다.

#### 함수 종류

- sklearn의 matrix 서브패키지
- confusion_matrix(answer, prediction)
- accuracy_score()
- precision_score()
- recall_score()
- f1_score()
- classification_report()
- roc_curve()
- auc()

```python
from sklearn.metrics import confusion_matrix

y_true = [2,0,2,2,0,1]
y_pred = [0,0,2,2,0,2]
```

##### 분류 결과표

```python
confusion_matrix(y_true, y_pred)
>
array([[2, 0, 0],
       [0, 0, 1],
       [1, 0, 2]], dtype=int64)
```

[2, 0, 0], : 실제데이터 0 : 실제값이 0인데 예측도 0으로 한게 2개
[0, 0, 1], : 실제데이터 1 : 실제값이 1인데 예측은 2로함
[1, 0, 2]], : 실제데이터 2 : 실제값이 2인데 0으로 예측한게 1개, 2로 예측한게 2개

0   1   2   예측 데이터

#### 이진 분류표

제품을 생산하는 제조공장에서 품질 테스트를 실시하여 불향품을 찾아내고 싶다. 이 불량품을 공장으로 돌려 보내고 싶다. (리콜) 

품질 테스트의 결과가 양성(Positove) -> 불량을 예측한 것이고
             						 음성(Negitive) -> 정상제품이라고 예측한 것이다.

- TP : 불량품을 불량품으로 정확하게 예측

- TN : 양품을 양품으로 예측 

즉 예측을 정확하게 했으면 앞에 T, 예측을 못했으면 F

- FP : 양품을 불량품으로 잘못 예측

- FN : 불량품을 양품으로 잘못 예측

```
  불량예측         정상예측

불량품         TP               FN


정상제품       FP               TN
```

```
# 암 - 양성(p), 암x -> 음성(n)
        예측 암    예측 암x
    
암        TP         FN
암x       FP         TN

# FN : 암인데 암이 아니라고 예측
# FP : 암이 아닌데 암이라고 예측
```

```
# 사기거래를 찾아내는 시스템
         사기예측         사기x예측
    
사기       TP                FN

사기x      FP                TN
```

```python
y_true = [1,0,1,1,0,1]
y_pred = [0,0,1,1,0,1]
confusion_matrix(y_true,y_pred)
>
array([[2, 0],
       [1, 3]], dtype=int64)
```

[[2, 0], : 실제 0인데 0을 예측한게 2개 / 0을 1로 예측 한 건 없음
 [1, 3]], : 실제 1인데 0으로 예측한게 1개/ 1을 1로 예측한게 2개

- Accuracy(맞게 검출한 비율) : T P + T N / (T P + T N + F P + F N) 
  - 전체 데이터에서 음성을 음성으로, 양성을 양성으로 예측한 비율
- Precision(P로 예측한 것 중 실제 P의 비율) : T P / T P + F P 
  - 우리가 찾고자 하는 샘플 중 실제 양성으로 찾아낸 샘플의 비율
- Sensitivity = Recal (실제 P를 P로 예측) : T P / T P + F N
  - 실제 양성을 양성으로 판단한 비율/ 실제 사기인데 사기라고 예측한 비율/높으면 높을수록 좋은 학습 모형

- Specificity(실제 N을 N로 예측) : T N / F P + T N
  -  음성을 음성으로 예측한 비율

- Error(오류율) : F N + F P / T P + F N + F P + T N
- F1 score : 2 * (precision * recall) / (precision + recall) 
  - 조합평균(정밀도 높아지면 재현율 낮아지는 trade off가 나타남) 평가지표로 활용할 때 이거 써야 한다.

- FP : 1종 오류
  - 회귀분석에서 제 1종 오류
- FN : 2종 오류
  - 회귀분석에서 제 2종 오류

#### 업무에 따른 재현율과 정밀도의 상대적 중요

- 재현율 (recall_score())
  - 실제 positive 양성인 데이터 예측을 Negitive음성으로 잘못 판단하게 되면 업무상 큰 영향이 발생하는 경우
    - 암진단, 금융사기 판별
- 정밀도 (precision_score())
  - 실제 음성인 데이터 예측을 양성으로 잘못 판단하게 되면 업무상 큰 영향이 발생하는 경우  
    - 스팸메일

```python
from sklearn.preprocessing import LabelEncoder
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score
import pandas as pd
import numpy as np
import warnings
warnings.filterwarnings('ignore')
```

```python
### fit() 메서드는 아무 것도 수행하지 않고, predict()는 Sex 피처가 1이면 0, 그렇지 않으면 1로 예측하는 단순한 분류기 생성
from sklearn.base import BaseEstimator

class MyDummyClassifier(BaseEstimator):
    # fit 메서드는 아무것도 학습하지 않음
    def fit(self, X, y=None):
        pass
    # predict 메서드는 단순히 Sex 피처가 1이면 0, 아니면 1로 예측
    def predict(self, X):
        pred = np.zeros( (X.shape[0],1) )
        for i in range(X.shape[0]):
            if X['Sex'].iloc[i] == 1:
                pred[i] = 0
            else :
                pred[i] = 1 
        return pred
```

```python
titanic.info()
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

- object 중에서 필요없는건 버리고 필요한 것들은 labelencoding한다.
- Label : Survived

```python
titanic_label = titanic['Survived']
# print(titanic_label)
titanic_feature_df = titanic.drop(['Survived'], axis=1)
titanic_feature_df.head()
>
	PassengerId	Pclass	Name	Sex	Age	SibSp	Parch	Ticket	Fare	Cabin	Embarked
0	1	3	Braund, Mr. Owen Harris	male	22.0	1	0	A/5 21171	7.2500	NaN	S
1	2	1	Cumings, Mrs. John Bradley (Florence Briggs Th...	female	38.0	1	0	PC 17599	71.2833	C85	C
2	3	3	Heikkinen, Miss. Laina	female	26.0	0	0	STON/O2. 3101282	7.9250	NaN	S
3	4	1	Futrelle, Mrs. Jacques Heath (Lily May Peel)	female	35.0	1	0	113803	53.1000	C123	S
4	5	3	Allen, Mr. William Henry	male	35.0	0	0	373450	8.0500	NaN	S
```

```python
## Null 처리 함수
def fillna(df):
    df['Age'].fillna(df['Age'].mean(), inplace=True)
    df['Cabin'].fillna('N', inplace=True)
    df['Embarked'].fillna('N', inplace=True)
    df['Fare'].fillna(0, inplace=True)
    return df

## 머신러닝에 불필요한 피처 제거
def drop_features(df):
    df.drop(['PassengerId', 'Name', 'Ticket'], axis=1, inplace=True)
    return df

## Label Encoding 수행
def format_features(df):
    df['Cabin'] = df['Cabin'].str[:1]
    features = ['Cabin', 'Sex', 'Embarked']
    for feature in features:
        le = LabelEncoder()
        le.fit(df[feature])
        df[feature] = le.transform(df[feature])
    return df

## 앞에서 실행한 Data Preprocessing 함수 호출
def transform_features(df):
    df = fillna(df)
    df = drop_features(df)
    df = format_features(df)
    return df

```

```python
titanic_feature_df = transform_features(titanic_feature_df)
titanic_feature_df.head()
>
Pclass	Sex	Age	SibSp	Parch	Fare	Cabin	Embarked
0	3	1	22.0	1	0	7.2500	7	3
1	1	0	38.0	1	0	71.2833	2	0
2	3	0	26.0	0	0	7.9250	7	3
3	1	0	35.0	1	0	53.1000	2	3
4	3	1	35.0	0	0	8.0500	7	3
```

```python
x_train, X_test, y_train, y_test = train_test_split(titanic_feature_df,
                                                   titanic_label,
                                                   test_size=0.2,
                                                   random_state=10)
```

- 함수 적용하고 난 뒤 데이터를 나눈다.

```python
dummy_model = MyDummyClassifier()
dummy_model.fit(x_train, y_train)
```

- 위에 만들어놓은 class를 만든다.
- fit해서 학습한다.

```python
y_pred = dummy_model.predict(X_test)
print('accuracy {}'.format(accuracy_score(y_test,y_pred)))
>
accuracy 0.8212290502793296
```

- 성별이 일치했을 때 0과 1로 반환해주는데 이걸 맹신할 수 없다. 왜?
  - 여성이 더 많다. 그러니 정확성에 문제가 생긴다.

```python
from sklearn.metrics import recall_score, precision_score
```

```python
def display_eval(y_test, y_pred):
    confusion = confusion_matrix(y_test,y_pred)
    accuracy  = accuracy_score(y_test, y_pred)
    presicion = precisin_score(y_test, y_pred)
    recall    = recall_score(y_test, y_pred)
    
    print()
    print(confusion)
    print('*'*50)
    print()
    print('정확도 : {}, 정밀도 : {}, 재현율 : {}'.format(accuracy,presicion, recall))
```

- 정확도, 정밀도, 재현율, 이진분류를 나타내는 함수를 만들었다.

#### 로지스틱 회귀

- 로지스틱 회귀를 분류로 보는 이유는??
  - 대부분 이진 분류 값을 값으로 받는다.

```python
from sklearn.linear_model import LogisticRegression
lr_model = LogisticRegression()
lr_model.fit(x_train, y_train)
prediction = lr_model.predict(X_test)
display_eval(y_test, prediction)



#       FALSE   TRUE
#FALSE   TN      FP 
#TRUE    FN      TP
>
[[101  16]
 [ 15  47]]
**************************************************

정확도 : 0.8268156424581006, 정밀도 : 0.746031746031746, 재현율 : 0.7580645161290323
```

- LogisticRegression을 import한다.
- 학습기 객체를 만든다.
- 우리가 만든 함수를 실행한다.

##### 공식대로 계산해서 맞는지 확인해보기

```python
print('accuracy : ',(101  + 47) / (101 + 16 + 15 + 47))
print('Recall : ', 47 / (47 + 15))
print('Precision : ', 47 / (47 + 16))
>
accuracy :  0.8268156424581006
Recall :  0.7580645161290323
Precision :  0.746031746031746
```

- 공식대로 해보니 값이 동일하다.

#### [실습] - 우방암 관련 데이터 - 정확, 재현율이 중요 (Recall) 실제 P를  N으로 예측 하면 안 된다.

- 재현율은 실제 양성을 양성으로 예측한 비율이므로 높을수록 좋은 성능모형이라 판단할 수 있다.

```python
from sklearn.datasets import load_breast_cancer
from sklearn.ensemble import RandomForestClassifier
from sklearn.preprocessing import LabelEncoder
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score
import pandas as pd
import numpy as np
import warnings
warnings.filterwarnings('ignore')
```

- ensemble : 여러 알고리즘을 합습해서 최적의 결과를 도출해내는 모듈

```python
cancer = load_breast_cancer()
print(type(cancer))
>
<class 'sklearn.utils.Bunch'>
```

- 키와 value로 이루어진 딕셔너리

#### cancer_df 만들기

```python
cancer_df = pd.DataFrame(data=cancer.data, columns=cancer.feature_names)
cancer_df['target'] =cancer.target
cancer_df.head()
>
	mean radius	mean texture	mean perimeter	mean area	mean smoothness	mean compactness	mean concavity	mean concave points	mean symmetry	mean fractal dimension	...	worst texture	worst perimeter	worst area	worst smoothness	worst compactness	worst concavity	worst concave points	worst symmetry	worst fractal dimension	target
0	17.99	10.38	122.80	1001.0	0.11840	0.27760	0.3001	0.14710	0.2419	0.07871	...	17.33	184.60	2019.0	0.1622	0.6656	0.7119	0.2654	0.4601	0.11890	0
```

##### 분류학습기 생성

- 학습 및 평가 ( 교차 검증 )
- 평가 지표들에 대한 평균값을 구해보자
- accuracy, precision, recall

```python
ranfore = RandomForestClassifier(random_state=100)

cancer_feature = cancer_df.drop(['target'],axis=1)
cancer_label   = cancer_df['target']


X_train, X_test, y_train, y_test = train_test_split(cancer_feature, cancer_label, test_size=0.3, random_state=100)
ranfore.fit(X_train, y_train)
can_pred = ranfore.predict(X_test)
display_eval(y_test, can_pred)
>
[[ 62   7]
 [  2 100]]
**************************************************

정확도 : 0.9473684210526315, 정밀도 : 0.9345794392523364, 재현율 : 0.9803921568627451
```

- 학습기만 RandomForestClassifier로 생성하면 뒤에는 다른 검증과 똑같다.

##### KFold 교차검증

```python
from sklearn.model_selection import KFold
kfold = KFold(n_splits=5)
cv_accuracy = []
cv_confusion = []
cv_precision = []
cv_recall = []
```

- kfold를 5개로 만들고 평균을 구하기 위해 리스트도 만든다.

```python
n_iter = 0
for train_idx, test_idx in kfold.split(cancer_feature):
    label_train = cancer_label.iloc[train_idx]
    label_test = cancer_label.iloc[test_idx]
    features_train =  cancer_feature.iloc[train_idx,:-1]
    features_test =  cancer_feature.iloc[test_idx,:-1]
    ranfore.fit(features_train, label_train)
    can_pred = ranfore.predict(features_test)
    
    # 정확도 측정
    n_iter += 1
    confusion = confusion_matrix(label_test,can_pred)
    cv_confusion.append(confusion)
    accuracy  = accuracy_score(label_test,can_pred)
    cv_accuracy.append(accuracy)
    precision = precision_score(label_test,can_pred)
    cv_precision.append(precision)
    recall    = recall_score(label_test,can_pred)
    cv_recall.append(recall)

    
    
print(cv_confusion)
print('\n\n')
print('\n 평균 검증 정확도 : {}, 평균 재현율  : {} , 평균 정밀도 : {}'.format(np.mean(cv_accuracy),np.mean(cv_precision),np.mean(cv_recall)))
>
[array([[59,  9],
       [ 1, 45]], dtype=int64), array([[46,  3],
       [ 4, 61]], dtype=int64), array([[37,  3],
       [ 0, 74]], dtype=int64), array([[28,  1],
       [ 3, 82]], dtype=int64), array([[26,  0],
       [ 2, 85]], dtype=int64)]




 평균 검증 정확도 : 0.9543549138332557, 평균 재현율  : 0.947089820320242 , 평균 정밀도 : 0.9716879569265142
```

- 루프 구문에 위에서 만들었던 재현율과 정밀도를 추가하며 평균을 구한다.