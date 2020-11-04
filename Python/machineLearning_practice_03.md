# 머신러닝 실습_03

### 고객만족 데이터 세트를 이용한 앙상블

```python
# 학습데이터
customer_train = pd.read_csv('./data/train.csv')
customer_train.head()
```

```python
#학습데이터
customer_test = pd.read_csv('./data/test.csv')
customer_test.head()
```

```python
# 클래스에 대한  비율 확인/ 0은 만족, 1은 불만족
customer_train['TARGET'].value_counts
# 불만족 비율
unsati_cnt = customer_train[customer_train['TARGET'] == 1]['TARGET'].count() / customer_train['TARGET'].count()
print('불만족 비율 : ', unsati_cnt)
>
불만족 비율 :  0.0395685345961589
```

#### 데이터 전처리 없이 XGBM을 이용한 예측

- 성능평가를 조기 중단 파라미터를 설정하고 학습/ 예측/ 평가

```python
from xgboost import XGBClassifier
xgb_clf = XGBClassifier(n_estimators=400,
                        learning_rate=0.1,
                        max_depth=3)
features = customer_train.drop('TARGET',axis=1)
label = customer_train['TARGET']
X_train, X_test, y_train, y_test = train_test_split(features,label,test_size=0.2,random_state=100 )
xgb_clf.fit(X_train, y_train,
           early_stopping_rounds=100,
           eval_metric='logloss',
           eval_set = [(X_test, y_test)],
           verbose= True)
```

- customer_train을 features와 label로 나누어서 데이터를 X와 y로 받는다.
- 그 다음에 학습을 한다.

```python
xgb_pred = xgb_clf.predict(X_test)
```

```python
def classifier_eval(y_test , y_pred) :
    print('오차행렬 : ' , confusion_matrix(y_test, y_pred))
    print('정확도   : ' , accuracy_score(y_test, y_pred))
    print('정밀도   : ' , precision_score(y_test, y_pred))
    print('재현율   : ' , recall_score(y_test, y_pred))
    print('F1       : ' , f1_score(y_test, y_pred))
    print('AUC      : ' , roc_auc_score(y_test, y_pred))
```

```python
classifier_eval(y_test, xgb_pred)
>
오차행렬 :  [[14584     1]
 [  616     3]]
정확도   :  0.9594185740594581
정밀도   :  0.75
재현율   :  0.004846526655896607
F1       :  0.009630818619582664
AUC      :  0.5023889815315822
```

#### 하이퍼 파라미터 튜닝을 해보자 - GridSearchCV - 교차검증

```

```

