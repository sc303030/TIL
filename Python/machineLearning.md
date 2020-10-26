### 머신러닝

#### 지도 학습

- 분류 : 분류형 예측
- 회귀 : 예측

#### 비지도 학습

- 정답이 주어지지 않고 데이터만 있다.

#### 강화 학습

- 데이터도, 정답도 없다. 현장에서 데이터를 수집해 학습해서 예측한다.

#### 단점

- 과적합 되기 쉽다.
- 데이터에 너무 의존적

#### 피처

- 데이터세트의 일반 속성
- 타겟값(레이블)을 제외한 나머지 속성을 모두 피처로 지칭

#### 레이블, 클래스, 타켓(값), 결정(값)

- 데이터의 학습을 위해 주어지는 정답 데이터
- 지도 학습 중 분류의 경우에는 이 결정값을 레이블 또는 클래스로 지칭

#### 학습데이테, 테스트 데이터

- 학습데이터 80% , 테스트 데이터 20% 정도로 설정

- 학습 데이터 : 학습을 위해 주어진 데이터 세트
- 테스트 데이터 : 머신러닝 모델의 예측 성능을 평가하기 위해 별도로 주어진 데이터 세트

- 데이터 세트 분리 -> 모델 학습 -> 예측 수행 -> 평가 

#### 사이킷런

- Estimator

---

```python
from sklearn.datasets import load_iris
from sklearn.tree     import DecisionTreeClassifier
from sklearn.model_selection import train_test_split
import pandas as pd
import numpy as np
```

### 간단한 머신러닝을 구현

```python
import sklearn
print(sklearn.__version__)
```

#### 1. 데이터 로딩

```python
iris = load_iris()
print(type(iris))
> <class 'sklearn.utils.Bunch'>
print(iris.head())
> error
```

- 내장 데이터는 데이터 프레임이 아니라서 head()가 안 된다.
  -  우리가 데이터 프레임으로 만들어야 한다.

```python
keys = iris.keys()
print('dataset keys', keys)
>
dataset keys dict_keys(['data', 'target', 'target_names', 'DESCR', 'feature_names'])
```

- data : 피처

- target : 레이블, 정답이 들어 있다.

- target_names : 레이블의 이름 (세토사, 버지니아, 버지니카)

- DESCR  :설명들

```python
print( 'key data\n', iris.data)
>
key data
 [[5.1 3.5 1.4 0.2]
 [4.9 3.  1.4 0.2]
 [4.7 3.2 1.3 0.2]...
```

```python
print('key target\n', iris.target)
>
key target
 [0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
 0 0 0 0 0 0 0 0 0 0 0 0 0 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1
 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 2 2 2 2 2 2 2 2 2 2 2
 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2
 2 2]
```

- target의 이름을 target_names로 알 수 있다.

```python
print('key target_names\n', iris.target_names)
>
key target_names
 ['setosa' 'versicolor' 'virginica']
```

- 아까 0,1,2 값들의 이름이다.

```python
print('key feature_names\n', iris.feature_names)
>
key feature_names
 ['sepal length (cm)', 'sepal width (cm)', 'petal length (cm)', 'petal width (cm)']
```

- [5.1 3.5 1.4 0.2] 이것들의 이름이다.

```python
print('key DESCRs\n', iris.DESCR)
>
Data Set Characteristics:
    :Number of Instances: 150 (50 in each of three classes)
    :Number of Attributes: 4 numeric, predictive attributes and the class
    :Attribute Information:
        - sepal length in cm
        - sepal width in cm
        - petal length in cm
        - petal width in cm
        - class:
                - Iris-Setosa
                - Iris-Versicolour
                - Iris-Virginica
```

- 이러한 정보들이 나온다.

#### 피처 데이터세트 확인

```python
iris_data = iris.data
iris_data
>
array([[5.1, 3.5, 1.4, 0.2],
       [4.9, 3. , 1.4, 0.2],
       [4.7, 3.2, 1.3, 0.2],
       [4.6, 3.1, 1.5, 0.2],
```

#### 레이블(결정값, 타켓, 클래스) 데이터를 확인

```python
iris_label = iris.target
iris_label
print(iris.target_names)
>
array([0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
       0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
       0, 0, 0, 0, 0, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,
       1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,
       1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2,
       2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2,
       2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2])
['setosa' 'versicolor' 'virginica']
```

####  데이터 프레임 변환

```python
iris_df = pd.DataFrame(data = iris.data, columns=iris.feature_names)
iris_df
>
	sepal length (cm)	sepal width (cm)	petal length (cm)	petal width (cm)
0				5.1					3.5					1.4					0.2
```

```python
iris_df['target'] = iris_label
>
sepal length (cm)	sepal width (cm)	petal length (cm)	petal width (cm)	target
0			5.1					3.5					1.4					0.2			0
```

#### 학습 데이터, 테스트 데이터 분리

```python
X_train, X_test, y_train, y_test =train_test_split(iris_data, iris_label, test_size = 0.2,  random_state=20)
```

- 언패킹해서 값을 받는다.
- 테스트 데이터 20% , 학습 80% 

- 

```python
print('train data\n' ,X_train )
print('train label\n' ,y_train )
print('*'*50)
print('test data\n' ,X_test )
print('test label\n' ,y_test )
```

- X_train , y_train : 학습한 데이터
- X_test :  예측은 이 값으로 한다.
- y_test : 이거랑 예측 데이터랑 비교해서 정확도 측정

#### 학습을 위한 학습기 - 알고리즘으로 이루어져 있는 객체

```python
iris_dtc = DecisionTreeClassifier(random_state=20)
iris_dtc.fit(X_train, y_train)
>
DecisionTreeClassifier(class_weight=None, criterion='gini', max_depth=None,
            max_features=None, max_leaf_nodes=None,
            min_impurity_decrease=0.0, min_impurity_split=None,
            min_samples_leaf=1, min_samples_split=2,
            min_weight_fraction_leaf=0.0, presort=False, random_state=20,
            splitter='best')
```

```python
iris_dtc = DecisionTreeClassifier(random_state=20, criterion='entropy')
iris_dtc.fit(X_train, y_train)
>
DecisionTreeClassifier(class_weight=None, criterion='entropy', max_depth=None,
            max_features=None, max_leaf_nodes=None,
            min_impurity_decrease=0.0, min_impurity_split=None,
            min_samples_leaf=1, min_samples_split=2,
            min_weight_fraction_leaf=0.0, presort=False, random_state=20,
            splitter='best')
```

- 지니계수나 엔트로피 계수 모두 93%의 예측을 보여준다.

#### 예측(Predict) 수행

```python
predition = iris_dtc.predict(X_test)
print(' y_test\n', y_test)
print(' prediction\n', predition)
```

- y_test : 실제 데이터
- prediction : 예측 데이터

#### 예측 정확도 평가

```python
from sklearn.metrics import accuracy_score
print('예측 정확도 : %.2f' % accuracy_score(y_test, predition))
>
예측 정확도 : 0.93
```

#### 데이터 프레임 형식을 나누는 방법 두 번째

> 테스트와 트렌인, 피처와 레이블이 나누어 있지 않을 때

##### 피처와 레이블 나누기

```python
feature_df = iris_df.iloc[:,:-1]
display(feature_df.head())
>
	sepal length (cm)	sepal width (cm)	petal length (cm)	petal width (cm)
0					5.1				3.5				1.4						0.2
1					4.9				3.0				1.4						0.2
```

```python
label_df   = iris_df.iloc[:,-1]
display(label_df)
>
0      0
1      0
2      0
3      0
4      0
```

```python
X_train, X_test, y_train, y_test =train_test_split(feature_df, label_df, test_size = 0.2,  random_state=20)
print('train data\n' ,X_train )
print('train label\n' ,y_train )
print('*'*50)
print('test data\n' ,X_test )
print('test label\n' ,y_test )
print(type(X_train),type(y_train),type(X_test),type(y_test))
>
Name: target, dtype: int32
<class 'pandas.core.frame.DataFrame'> <class 'pandas.core.series.Series'> <class 'pandas.core.frame.DataFrame'> <class 'pandas.core.series.Series'>
```

- 데이터 프레임으로 넘어오는 것을 알 수 있다.

#### 학습을 위한 학습기 - 알고리즘으로 이루어져 있는 객체

```python
iris_dtc = DecisionTreeClassifier(random_state=20, criterion='entropy')
iris_dtc.fit(X_train, y_train)
predition = iris_dtc.predict(X_text)
```

#### 예측 정확도 평가

```python
from sklearn.metrics import accuracy_score
print('예측 정확도 : %.2f' % accuracy_score(y_test, predition))
>
예측 정확도 : 0.93
```

- 같은 결과를 도출한다.

random_state : 1번 부터 120번까지 랜덤하게 선택할 때 같은 데이터로 해라.

#### random_state  다르게 해보기

```python
X_train, X_test, y_train, y_test =train_test_split(feature_df, label_df, test_size = 0.2,  random_state=11)
print('train data\n' ,X_train )
print('train label\n' ,y_train )
print('*'*50)
print('test data\n' ,X_test )
print('test label\n' ,y_test )
print(type(X_train),type(y_train),type(X_test),type(y_test))
```

```python
iris_dtc = DecisionTreeClassifier(random_state=20, criterion='entropy')
iris_dtc.fit(X_train, y_train)
predition = iris_dtc.predict(X_test)
from sklearn.metrics import accuracy_score
print('예측 정확도 : %.2f' % accuracy_score(y_test, predition))
>
예측 정확도 : 0.90
```

- 예측 정확도가 달라진다.

#### 학습을 시킬 때 테스트 데이터 세트를 이용하지 않고 학습 데이터 세트로만 학습하고 예측한다면?

```python
bad_iris = load_iris()

# 데이터 새트 분할없이 얻어오기
train_data = iris.data
train_label = iris.target

# 학습을 위한 분류 모델
bad_iris_clf = DecisionTreeClassifier()
bad_iris_clf.fit(train_data, train_label)

# 잘못된 예측
pred = bad_iris_clf.predict(train_data)
print('예측 정확도 : ', accuracy_score(train_label, pred))
>
예측 정확도 :  1.0
```

- 100%가 나오기 때문에 학습과 테스트를 분리해야 한다.

- 예측은 실제 학습 기반 데이터로 하면 안 된다. 그래서 분할이 필요하다.
- test_size : 지정하지 않으면 기본 25%정도

- random_state : 같은 학습/데이터 용 데이터를 생성한다. 지정하지 않으면 다른 학습/데이터 용 데이터를 생성한다.

### 교차 검증 (Cross Validation)

> 모의고사에 비유하면 모의고사를 많이 보자는 뜻

- 학습데이터를 다시 분할하여 학습 데이터와 학습된 모델의 성능을 일차 형가하는 검증 데이터로 나눈다.
- 모든 학습. 검증 과정이 완료된 후 최종적으로 성능을 평가하기 위한 데이터 세트

#### K 폴드 교차 검증

**train set**

- 학습/ 검증을 나눈다.
  - 1,2,3,4 학습하고 5번 검증
  - 1,2,3,5 학습하고 4번 검증...
- 우리가 지정한 횟수만큼 반복한다.
  - 5번 검증한다고 하면 5개의 세트를 만들어서 4개는 학습, 1개는 검증으로 사용

```python
from sklearn.model_selection import KFold
```

- KFold를 import한다.

```python
fold_iris = load_iris()
features = fold_iris.data
print(features)
label = fold_iris.target
print(label)
fold_df_clf = DecisionTreeClassifier()
>
[[5.1 3.5 1.4 0.2]
 [4.9 3.  1.4 0.2]
 [4.7 3.2 1.3 0.2]
[0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
 0 0 0 0 0 0 0 0 0 0 0 0 0 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1
 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 2 2 2 2 2 2 2 2 2 2 2
 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2
 2 2]
```

#### 5개의 폴드 세트를 분리하여 각 폴드 세트별 정확도를 담을 리스트를 생성

```python
kfold = KFold(n_splits=5)
cv_accuracy = []
print('iris shape', features.shape[0])
>
iris shape 150
```

```python
n_iter = 0
for train_idx, test_idx in kfold.split(features):
    print(train_idx, test_idx)
>
[ 30  31  32  33  34  35  36  37  38  39  40  41  42  43  44  45  46  47
  48  49  50  51  52  53  54  55  56  57  58  59  60  61  62  63  64  65
  66  67  68  69  70  71  72  73  74  75  76  77  78  79  80  81  82  83
  84  85  86  87  88  89  90  91  92  93  94  95  96  97  98  99 100 101
 102 103 104 105 106 107 108 109 110 111 112 113 114 115 116 117 118 119
 120 121 122 123 124 125 126 127 128 129 130 131 132 133 134 135 136 137
 138 139 140 141 142 143 144 145 146 147 148 149] [ 0  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23
 24 25 26 27 28 29]
[  0   1   2   3   4   5   6   7   8   9  10  11  12  13  14  15  16  17
  18  19  20  21  22  23  24  25  26  27  28  29  60  61  62  63  64  65
  66  67  68  69  70  71  72  73  74  75  76  77  78  79  80  81  82  83
  84  85  86  87  88  89  90  91  92  93  94  95  96  97  98  99 100 101
 102 103 104 105 106 107 108 109 110 111 112 113 114 115 116 117 118 119
 120 121 122 123 124 125 126 127 128 129 130 131 132 133 134 135 136 137
 138 139 140 141 142 143 144 145 146 147 148 149] [30 31 32 33 34 35 36 37 38 39 40 41 42 43 44 45 46 47 48 49 50 51 52 53
 54 55 56 57 58 59]
[  0   1   2   3   4   5   6   7   8   9  10  11  12  13  14  15  16  17
  18  19  20  21  22  23  24  25  26  27  28  29  30  31  32  33  34  35
  36  37  38  39  40  41  42  43  44  45  46  47  48  49  50  51  52  53
  54  55  56  57  58  59  90  91  92  93  94  95  96  97  98  99 100 101
 102 103 104 105 106 107 108 109 110 111 112 113 114 115 116 117 118 119
 120 121 122 123 124 125 126 127 128 129 130 131 132 133 134 135 136 137
 138 139 140 141 142 143 144 145 146 147 148 149] [60 61 62 63 64 65 66 67 68 69 70 71 72 73 74 75 76 77 78 79 80 81 82 83
 84 85 86 87 88 89]
[  0   1   2   3   4   5   6   7   8   9  10  11  12  13  14  15  16  17
  18  19  20  21  22  23  24  25  26  27  28  29  30  31  32  33  34  35
  36  37  38  39  40  41  42  43  44  45  46  47  48  49  50  51  52  53
  54  55  56  57  58  59  60  61  62  63  64  65  66  67  68  69  70  71
  72  73  74  75  76  77  78  79  80  81  82  83  84  85  86  87  88  89
 120 121 122 123 124 125 126 127 128 129 130 131 132 133 134 135 136 137
 138 139 140 141 142 143 144 145 146 147 148 149] [ 90  91  92  93  94  95  96  97  98  99 100 101 102 103 104 105 106 107
 108 109 110 111 112 113 114 115 116 117 118 119]
[  0   1   2   3   4   5   6   7   8   9  10  11  12  13  14  15  16  17
  18  19  20  21  22  23  24  25  26  27  28  29  30  31  32  33  34  35
  36  37  38  39  40  41  42  43  44  45  46  47  48  49  50  51  52  53
  54  55  56  57  58  59  60  61  62  63  64  65  66  67  68  69  70  71
  72  73  74  75  76  77  78  79  80  81  82  83  84  85  86  87  88  89
  90  91  92  93  94  95  96  97  98  99 100 101 102 103 104 105 106 107
 108 109 110 111 112 113 114 115 116 117 118 119] [120 121 122 123 124 125 126 127 128 129 130 131 132 133 134 135 136 137
 138 139 140 141 142 143 144 145 146 147 148 149]
```

- 테스트 데이터 인덱스가 달라지는 걸 볼 수 있다.

- 트레인 데이터도 인덱스가 달라진다.

```python
n_iter = 0
for train_idx, test_idx in kfold.split(features):
#     print(train_idx, test_idx)
    X_train, X_test = features[train_idx],features[test_idx]
#     print('X_train\n',X_train)
#     print('X_test\n', X_test)
    y_train, y_test = label[train_idx], label[test_idx]
#     print('y_train', y_train)
#     print('y_test', y_test)
    # 학습을 진행하겠다면?
    fold_df_clf.fit(X_train, y_train)
    # 예측
    fold_pred = fold_df_clf.predict(X_test)
>
y_test [0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0]
y_test [0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 1 1 1 1 1 1 1 1 1 1]
y_test [1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1]
y_test [1 1 1 1 1 1 1 1 1 1 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2]
y_test [2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2]
```

- X_train : 학습데이터 / y_train : 정답 데이터

- 셔플이 되어있지 않다. 분포가 다양하지 않다.

```python
n_iter = 0
for train_idx, test_idx in kfold.split(features):
#     print(train_idx, test_idx)
    X_train, X_test = features[train_idx],features[test_idx]
#     print('X_train\n',X_train)
#     print('X_test\n', X_test)
    y_train, y_test = label[train_idx], label[test_idx]
#     print('y_train', y_train)
#     print('y_test', y_test)
    # 학습을 진행하겠다면?
    fold_df_clf.fit(X_train, y_train)
    # 예측
    fold_pred = fold_df_clf.predict(X_test)
    
    # 정확도 측정
    n_iter += 1
    accuracy = np.round(accuracy_score(y_test, fold_pred),4)
    print('\n{}교차검증 정확도 :  {}, 학습 데이터 크기 : {}, 검증 데이터 크기 : {}'.format(n_iter, accuracy,X_train.shape[0], X_test.shape[0]))
    cv_accuracy.append(accuracy)

print('\n\n')
print('\n 평균 검증 정확도 : ', np.mean(cv_accuracy))
>

1교차검증 정확도 :  1.0, 학습 데이터 크기 : 120, 검증 데이터 크기 : 30

2교차검증 정확도 :  0.9667, 학습 데이터 크기 : 120, 검증 데이터 크기 : 30

3교차검증 정확도 :  0.8333, 학습 데이터 크기 : 120, 검증 데이터 크기 : 30

4교차검증 정확도 :  0.9333, 학습 데이터 크기 : 120, 검증 데이터 크기 : 30

5교차검증 정확도 :  0.8, 학습 데이터 크기 : 120, 검증 데이터 크기 : 30




 평균 검증 정확도 :  0.9099933333333333
```

- 교차 검증 정확도는 할 때마다 달라진다. 
  - random_state 를 지정하지 않아서 그렇다.

### Stratified KFold : 불균형한 분포도를 가진 레이블 데이터 집합을 위한 KFold 방식

#### 레이블의 분포를 먼저 고려한 뒤 이 분포와 동일하게 학습과 검증 데이터 세트로 분할

#### 분류만 쓸 수 있고, 회귀에서는 사용할 수 없다.

#### 회귀는 연속된 숫자값이기 때문에 지원하지 않는다.

##### 기존 KFold의 문제점 다시 한번 확인

```python
kfold_iris_data = load_iris()
kfold_iris_data_df = pd.DataFrame(data=kfold_iris_data.data, columns=kfold_iris_data.feature_names)
kfold_iris_data_df['target'] = kfold_iris_data.target
print(' value_counts : \n', kfold_iris_data_df['target'].value_counts())
>
 value_counts : 
 2    50
1    50
0    50
Name: target, dtype: int64
```

```python
kfold_iris = KFold(n_splits=3)
cnt_iter = 0
for train_idx, test_idx in kfold_iris.split(kfold_iris_data_df):
#     print(train_idx, test_idx)
    cnt_iter += 1
    label_train = kfold_iris_data_df['target'].iloc[train_idx]
    label_test = kfold_iris_data_df['target'].iloc[test_idx]
#     print('label_train\n', label_train)
    print('교차검증 : {}'.format(cnt_iter))
    print('학습 레이블 데이터 분포 : \n', label_train.value_counts())
    print('검증 레이블 데이터 분포 : \n', label_test.value_counts())
>
교차검증 : 1
학습 레이블 데이터 분포 : 
 2    50
1    50
Name: target, dtype: int64
검증 레이블 데이터 분포 : 
 0    50
Name: target, dtype: int64
교차검증 : 2
학습 레이블 데이터 분포 : 
 2    50
0    50
Name: target, dtype: int64
검증 레이블 데이터 분포 : 
 1    50
Name: target, dtype: int64
교차검증 : 3
학습 레이블 데이터 분포 : 
 1    50
0    50
Name: target, dtype: int64
검증 레이블 데이터 분포 : 
 2    50
Name: target, dtype: int64
```

- 데이터 분포가 어떻게 되는지 중간점검으로 확인해본다.

- 레이블이 50개씩 들어있다. kfold로 나눠보면 2번과1번이 50개, 0이 50개다. 학습한건 2번과 1번인데 실제 데이터는 0밖에 없다. 정확도가 0% 나온다.
- 각각 3번씩 돌렸는데 보면 각각의 학습과 테스트 데이터들의 레이블이 다르다.정확도 0%가 나온다.
- 전체 레이블의 분포의 값을 반영하지 못하는 문제가 발생한다. 그래서 **Stratifird KFold**를 써야한다.

#### 레이블 값의 분포를 반영해 주지 못하는 문제를 해결하기 위해서 StratifiedKFold를 이용

```python
from sklearn.model_selection import StratifiedKFold
skf_iris = StratifiedKFold(n_splits=3)
cnt_iter = 0
for train_idx, test_idx in skf_iris.split(kfold_iris_data_df, kfold_iris_data_df['target']):
#     print(train_idx, test_idx)
    cnt_iter += 1
    label_train = kfold_iris_data_df['target'].iloc[train_idx]
    label_test = kfold_iris_data_df['target'].iloc[test_idx]
#     print('label_train\n', label_train)
    print('교차검증 : {}'.format(cnt_iter))
    print('학습 레이블 데이터 분포 : \n', label_train.value_counts())
    print('검증 레이블 데이터 분포 : \n', label_test.value_counts())
>
교차검증 : 1
학습 레이블 데이터 분포 : 
 2    33
1    33
0    33
Name: target, dtype: int64
검증 레이블 데이터 분포 : 
 2    17
1    17
0    17
Name: target, dtype: int64
교차검증 : 2
학습 레이블 데이터 분포 : 
 2    33
1    33
0    33
Name: target, dtype: int64
검증 레이블 데이터 분포 : 
 2    17
1    17
0    17
Name: target, dtype: int64
교차검증 : 3
학습 레이블 데이터 분포 : 
 2    34
1    34
0    34
Name: target, dtype: int64
검증 레이블 데이터 분포 : 
 2    16
1    16
0    16
Name: target, dtype: int64
```

- 비율을 맞춰서 검증할 수 있게 데이터가 설정되어 있다.

### 붓꽃 데이터 세트에서 StratifiedKFold를 이용해서 교차검증을(3,5) 진행하고 평균정확도를 확인

#### randim_state = 100

```python
cnt = []
from sklearn.model_selection import StratifiedKFold
skf_iris = StratifiedKFold(n_splits=3)
cnt_iter = 0
for train_idx, test_idx in skf_iris.split(kfold_iris_data_df, kfold_iris_data_df['target']):
    cnt_iter += 1
    label_train = kfold_iris_data_df['target'].iloc[train_idx]
    label_test = kfold_iris_data_df['target'].iloc[test_idx]
    features_train =  kfold_iris_data_df.iloc[train_idx,:-1]
    features_test =  kfold_iris_data_df.iloc[test_idx,:-1]
    
     # 학습을 진행하겠다면?
    iris_dtc = DecisionTreeClassifier(random_state=100, criterion='gini')
    iris_dtc.fit(features_train, label_train)
    # 예측
    fold_pred = iris_dtc.predict(features_test)
    
    accuracy = np.round(accuracy_score(label_test, fold_pred),4)
    print('\n{}교차검증 정확도 :  {}, 학습 데이터 크기 : {}, 검증 데이터 크기 : {}'.format(cnt_iter, accuracy,X_train.shape[0], X_test.shape[0]))
    cnt.append(accuracy)

print('\n\n')
print('\n 평균 검증 정확도 : ', np.mean(cnt))
>
1교차검증 정확도 :  0.9804, 학습 데이터 크기 : 102, 검증 데이터 크기 : 48

2교차검증 정확도 :  0.9216, 학습 데이터 크기 : 102, 검증 데이터 크기 : 48

3교차검증 정확도 :  0.9792, 학습 데이터 크기 : 102, 검증 데이터 크기 : 48

 평균 검증 정확도 :  0.9604
```

- 이렇게 해도 되고

```python
cnt = []
from sklearn.model_selection import StratifiedKFold
skf_iris = StratifiedKFold(n_splits=5)
cnt_iter = 0
for train_idx, test_idx in skf_iris.split(kfold_iris_data_df, kfold_iris_data_df['target']):
    cnt_iter += 1
    label_train = kfold_iris_data_df['target'].iloc[train_idx]
    label_test = kfold_iris_data_df['target'].iloc[test_idx]
    features_train =  kfold_iris_data_df.iloc[train_idx,:-1]
    features_test =  kfold_iris_data_df.iloc[test_idx,:-1]
    
     # 학습을 진행하겠다면?
    iris_dtc = DecisionTreeClassifier(random_state=100, criterion='gini')
    iris_dtc.fit(features_train, label_train)
    # 예측
    fold_pred = iris_dtc.predict(features_test)
    
    accuracy = np.round(accuracy_score(label_test, fold_pred),4)
    print('\n{}교차검증 정확도 :  {}, 학습 데이터 크기 : {}, 검증 데이터 크기 : {}'.format(cnt_iter, accuracy,X_train.shape[0], X_test.shape[0]))
    cnt.append(accuracy)

print('\n\n')
print('\n 평균 검증 정확도 : ', np.mean(cnt))
>
1교차검증 정확도 :  0.9667, 학습 데이터 크기 : 102, 검증 데이터 크기 : 48

2교차검증 정확도 :  0.9667, 학습 데이터 크기 : 102, 검증 데이터 크기 : 48

3교차검증 정확도 :  0.9, 학습 데이터 크기 : 102, 검증 데이터 크기 : 48

4교차검증 정확도 :  0.9333, 학습 데이터 크기 : 102, 검증 데이터 크기 : 48

5교차검증 정확도 :  1.0, 학습 데이터 크기 : 102, 검증 데이터 크기 : 48


 평균 검증 정확도 :  0.9533400000000001
```

- 5개로 나누었을 때 값이다.

```python
result_iris = load_iris()
result_features = result_iris.data
result_label = result_iris.target

result_clf = DecisionTreeClassifier(random_state=100)
result_skfold = StratifiedKFold(n_splits=3)
idx_iter = 0
cv_accur = []
```

```python
for train_idx, test_idx in result_skfold.split(features, label):
    X_train , X_test = features[train_idx], features[test_idx]
    y_train, y_test  = label[train_idx], label[test_idx]
    
     # 학습을 진행하겠다면?
    result_clf.fit(X_train, y_train)
    pred = result_clf.predict(X_test)
    idx_iter += 1
    accuracy = np.round(accuracy_score(y_test, pred),4)
    train_size = X_train.shape[0]
    test_size = X_test.shape[0]
    print('\n#{0}교차검증 정확도 :  {1}, 학습 데이터 크기 : {2}, 검증 데이터 크기 : {3}'
          .format(idx_iter, accuracy, train_size, test_size))
    print('#{0} 검증 세트 인덱스 : {1}'.format(idx_iter, test_size))
    cv_accur.append(accuracy)
print('\n\n')
print('\n 평균 검증 정확도 : ', np.mean(cv_accur))
>
#1교차검증 정확도 :  0.9804, 학습 데이터 크기 : 99, 검증 데이터 크기 : 51
#1 검증 세트 인덱스 : 51

#2교차검증 정확도 :  0.9216, 학습 데이터 크기 : 99, 검증 데이터 크기 : 51
#2 검증 세트 인덱스 : 51

#3교차검증 정확도 :  0.9792, 학습 데이터 크기 : 102, 검증 데이터 크기 : 48
#3 검증 세트 인덱스 : 48


 평균 검증 정확도 :  0.9604
```

- 위랑 같은 값이다.

```python
result_iris = load_iris()
result_features = result_iris.data
result_label = result_iris.target

result_clf = DecisionTreeClassifier(random_state=100)
result_skfold = StratifiedKFold(n_splits=5)
idx_iter = 0
cv_accur = []
```

```python
for train_idx, test_idx in result_skfold.split(features, label):
    X_train , X_test = features[train_idx], features[test_idx]
    y_train, y_test  = label[train_idx], label[test_idx]
    
     # 학습을 진행하겠다면?
    result_clf.fit(X_train, y_train)
    pred = result_clf.predict(X_test)
    idx_iter += 1
    accuracy = np.round(accuracy_score(y_test, pred),4)
    train_size = X_train.shape[0]
    test_size = X_test.shape[0]
    print('\n#{0}교차검증 정확도 :  {1}, 학습 데이터 크기 : {2}, 검증 데이터 크기 : {3}'
          .format(idx_iter, accuracy, train_size, test_size))
    print('#{0} 검증 세트 인덱스 : {1}'.format(idx_iter, test_size))
    cv_accur.append(accuracy)
print('\n\n')
print('\n 평균 검증 정확도 : ', np.mean(cv_accur))
>
#1교차검증 정확도 :  0.9667, 학습 데이터 크기 : 120, 검증 데이터 크기 : 30
#1 검증 세트 인덱스 : 30

#2교차검증 정확도 :  0.9667, 학습 데이터 크기 : 120, 검증 데이터 크기 : 30
#2 검증 세트 인덱스 : 30

#3교차검증 정확도 :  0.9, 학습 데이터 크기 : 120, 검증 데이터 크기 : 30
#3 검증 세트 인덱스 : 30

#4교차검증 정확도 :  0.9333, 학습 데이터 크기 : 120, 검증 데이터 크기 : 30
#4 검증 세트 인덱스 : 30

#5교차검증 정확도 :  1.0, 학습 데이터 크기 : 120, 검증 데이터 크기 : 30
#5 검증 세트 인덱스 : 30


 평균 검증 정확도 :  0.9533400000000001
```

- 같은 값이 나온다.