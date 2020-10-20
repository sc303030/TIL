# visualization_02

#### 이상치 정제

##### car_mpg 데이터에서 구동방식별 고속도로 연비 평균과 도시연비 평균을 극단치를 제외하고 확인

##### 각 연비별 이상치 확인 boxplot()

```python
outlier_df = data_df.filter(['hwy','cty'])
outlier_df.boxplot()
```

![vi24](./img/vi24.png)

#### quantile()

#### 3사분위 정보를 얻어본다면?

```python
quantile75 = outlier_df.quantile(q=0.75)
quantile75
>
hwy    27.0
cty    19.0
Name: 0.75, dtype: float64
```

#### 1사분위 정보를 얻어본다면?

```python
quantile25 = outlier_df.quantile(q=0.25)
quantile25
>
hwy    18.0
cty    14.0
Name: 0.25, dtype: float64
```

#### iqr ( 3사분위 수 - 1사분위 수의 차)

```python
iqr = quantile75 - quantile25
iqr
>
hwy    9.0
cty    5.0
dtype: float64
```

#### 최저 한계치 (lower fence)

```python
lower_fence = quantile25 - 1.5 * iqr
print('lower_fence', lower_fence)
>
lower_fence hwy    4.5
cty    6.5
dtype: float64
```

#### 최고 한계치(upper fence)

```python
upper_fence = quantile75 - 1.5 * iqr
print('upper_fence', upper_fence)
>
upper_fence hwy    13.5
cty    11.5
dtype: float64
```

- 이상치 제거하기 위해 만들었다. 