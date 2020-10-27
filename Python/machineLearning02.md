# 머신러닝_02

- 오차행렬을 이용해서 재현율과 정밀도, F1스코어를 사용해서 평가한다.

### feature : 학습을 위한 데이터, label  : 정답

### cross_val_score() : 교차 검증을 간단하게 도와주는 함수

- 폴더 세트 설정, 반복을 통한 학습 및 테스트 인덱스 추출, 학습과 예측수행 반환
- cross_val_score(**estimater**) : `estimater` 어떤 학습방식을 선택할래?

- cross_val_score(**estimater**, **X**(feature), **y**(label), scoring)  : `scoring`  평가 지표

- cross_val_score(**estimater**, **X**(feature), **y**(label), **scoring**, **cv**)  : `cv`  폴더 개수

```python
from sklearn.datasets import load_iris
from sklearn.tree     import DecisionTreeClassifier
from sklearn.model_selection import cross_val_score, cross_validate
import pandas as pd
import numpy as np
```

- `import cross_val_score, cross_validate` 추가

```python
cvs_iris = load_iris()
cvs_iris_feature = cvs_iris.data
cvs_iris_label = cvs_iris.target
cvs_iris_dtc = DecisionTreeClassifier(random_state = 200)
```

- cvs_iris_feature = cvs_iris.data : 학습을 위한 데이터

- cvs_iris_label = cvs_iris.target : 정답 데이터

```python
scoring = cross_val_score(cvs_iris_dtc, cvs_iris_feature,cvs_iris_label,scoring='accuracy',cv=3 )
print('교차 검증별 정확도 :',scoring)
print('평균 검증 정확도 :',np.mean(scoring))
>
교차 검증별 정확도 : [0.98039216 0.92156863 0.97916667]
평균 검증 정확도 : 0.960375816993464
```

- cvs_iris_dtc : 자동으로 StratifiedKFold를 수행한다.

- 3개의 폴더 교차 검증 정확도를 넘겨준다.

```python
scoring2 = cross_validate(cvs_iris_dtc, cvs_iris_feature,cvs_iris_label,scoring='accuracy',cv=3 )
print('교차 검증 정보 :',scoring2)
print('교차 검증별 정확도 :',scoring2['test_score'])
print('교차 검증 시간 :',scoring2['fit_time'])
print('평균 검증 정확도 :',np.mean(scoring2['test_score']))
>
교차 검증 정보 : {'fit_time': array([0.00100064, 0.00099778, 0.00099397]), 'score_time': array([0., 0., 0.]), 'test_score': array([0.98039216, 0.92156863, 0.97916667]), 'train_score': array([1., 1., 1.])}
교차 검증별 정확도 : [0.98039216 0.92156863 0.97916667]
교차 검증 시간 : [0.00100064 0.00099778 0.00099397]
평균 검증 정확도 : 0.960375816993464
```

- fit_time와  score_time도 같이 넘어온다.

##### cv=10

```python
scoring2 = cross_validate(cvs_iris_dtc, cvs_iris_feature,cvs_iris_label,scoring='accuracy',cv=10)
print('교차 검증 정보 :',scoring2)
print('교차 검증 시간 :',scoring2['fit_time'])
print('*'*100)
print('교차 검증별 정확도 :',scoring2['test_score'])
print('평균 검증 정확도 :',np.round(np.mean(scoring2['test_score']),2))
>
교차 검증 정보 : {'fit_time': array([0.00099897, 0.00099897, 0.        , 0.00200081, 0.        ,
       0.00099683, 0.        , 0.        , 0.0009923 , 0.        ]), 'score_time': array([0.        , 0.00099587, 0.        , 0.        , 0.        ,
       0.        , 0.        , 0.00098324, 0.        , 0.00099611]), 'test_score': array([1.        , 0.93333333, 1.        , 0.93333333, 0.93333333,
       0.86666667, 0.93333333, 0.93333333, 1.        , 1.        ]), 'train_score': array([1., 1., 1., 1., 1., 1., 1., 1., 1., 1.])}
교차 검증 시간 : [0.00099897 0.00099897 0.         0.00200081 0.         0.00099683
 0.         0.         0.0009923  0.        ]
****************************************************************************************************
교차 검증별 정확도 : [1.         0.93333333 1.         0.93333333 0.93333333 0.86666667
 0.93333333 0.93333333 1.         1.        ]
평균 검증 정확도 : 0.95
```

### Hyper Parameter Tuning (하이퍼 파마리터 튜닝)

- 학습 시키는 모델들의 파라미터를 우리가 튜닝할 수 있다.
- 과적합 발생시 과적합 방지하거나 scoring를 올릴 수 있다.

```python
DecisionTreeClassifier(class_weight=None, criterion='gini', max_depth=None,
            max_features=None, max_leaf_nodes=None,
            min_impurity_decrease=0.0, min_impurity_split=None,
            min_samples_leaf=1, min_samples_split=2,
            min_weight_fraction_leaf=0.0, presort=False, random_state=None,
            splitter='best')
```

- 디폴트로 설정되어 있는 걸 우리가 조정할 수 있다.

### GridSearchCV :교차 검증과 튜닝을 한번에 할 수 있다.

