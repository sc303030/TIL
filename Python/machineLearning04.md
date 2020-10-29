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

- FP : 양품인데 불량품으로 예측

- FN : 불량품인데 양품으로 예측