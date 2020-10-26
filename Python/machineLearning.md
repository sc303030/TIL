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