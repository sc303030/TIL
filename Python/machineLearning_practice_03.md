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

