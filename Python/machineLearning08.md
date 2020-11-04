# 머신러닝_08

### XGBoost (eXtra Gradient Boost)

- 뛰어난 예측 성능
- GBM 대비 빠른 수행 시간
  - CPU 병렬 처리, GPU 지원
- 다양한 성능 향상 기능
  - 규제 기능 탑재
  - Tree Pruning
- 다양한 편의 기능
  - 조기 중단
  - 자체 내장된 교차 검증
  - 결손값 자체 처리

#### 파이썬 구현

- 파이썬 Wrapper와  사이킷런 Wrapper가 있다.
- 사이킷런 Wrapper
  - 학습과 예측을 다른 사이킷런 언어와 동일하게 fit()과 predict로 수행

| 항목                          | 파이썬 Wrapper                                               | 사이킷런 Wrapper                             |
| ----------------------------- | ------------------------------------------------------------ | -------------------------------------------- |
| 사용 모듈                     | from xgboost as xgb                                          | from xgboost import XGBoostClassifier        |
| 학습용과 테스트용 데이터 세트 | DMatrix 객체를 별도 생성<br/>train = xgb.DMatrix(data=X_train, label=y_train) | 넘파이와 판다스를 이용                       |
| 학습 API                      | Xgb_model = xgb.train()<br/>Xgb_model은 학습된 객체를 반환 받음 | XGBClassifier.fit()                          |
| 예측 API                      | xgb.train()으로 학습된 객체에서 predict() 호출, 즉 Xgb_model.predict()<br/>이때 반환 결과는 예측 결과가 아니라 예측 결과를 추정하는 확률값 반환 | XGBClassifier.predict()<br/>예측 결과값 반환 |
| 피처 중요도 시각화            | plot_importance() 함수이용                                   | plot_importance() 함수이용                   |

- XGBoost 파이썬 래퍼와 사이킷런 래퍼 하이퍼 파라미터 비교

#### XGBoost 조기 중단 가능(Early Stooping)

- 특정 반복 횟수 만큼 더 이상 비용함수가 감소하지 않으면 지정된 반복횟수를 다 완료하지 않고 수행을 종료할 수 있음
- 학습을 위한 시간을 단축 시킬 수 있음. 특히 최적화 튜닝 단계에서 적절하게 사용 가능
- 너무 반복 횟수를 단축할 경우 예측 성능 최적화가 안된 상태에서 학습이 종료 될 수 있으므로 유의  필요
- 조기 중단 설정을 위한 주요 파라미터
  - early_stopping_rounds : 더 이상 비용 평가 지표가 감소하지 않는 최대 반복횟수
  - eval_metric : 반복 수행 시 사용하는 비용 평가 지표
  - eval_set : 평가를 수행하는 별도의 검증 데이터 세트. 일반적으로 검증 데이터 세트에서 반복적으로 비용 감소 성능 평가

- conda install은 의존 관계에 있는 것 까지 설치해준다.

```python
dataset = load_breast_cancer()
features = dataset.data
label = dataset.target
cancer_df = pd.DataFrame(data=features, columns=dataset.feature_names)
cancer_df['target'] = label
cancer_df['target'].value_counts()
>
1    357
0    212
Name: target, dtype: int64
```

