# 머신러닝 실습_04

### KMeans [실습]

```python
from sklearn.cluster import KMeans
import pandas as pd
import numpy as np

import matplotlib.pyplot as plt
import seaborn as sns

%matplotlib inline
```

```python
uci_path = 'https://archive.ics.uci.edu/ml/machine-learning-databases/00292/Wholesale%20customers%20data.csv'
sample_df = pd.read_csv(uci_path)
sample_df.head()
```

- 사용할 데이터를 받아온다.

```python
# 데이터 자료형
sample_df.info()
```

```python
sample_df.describe()
```

```python
#분석에 사용할 피처를 카피
copy_df = sample_df.iloc[:,:]
copy_df.head()
```

- 자료를 카피해 원본 손실을 방지한다.

```python
from sklearn.preprocessing import StandardScaler
# 표준화 진행
std_df = StandardScaler().fit_transform(copy_df)
```

- StandardScaler로 표준화를 진행한다.

```python
# 군집모형 학습 및 예측 , 예측 된 결과를 DF에 추가
std_kmeans = KMeans(n_clusters=6, init='k-means++',max_iter=300,n_init=10)
std_kmeans.fit(std_df)
```

- 군집화를 하여 학습한다.

```python
copy_df['clu_id'] = std_kmeans.labels_
>
Channel	Region	Fresh	Milk	Grocery	Frozen	Detergents_Paper	Delicassen	clu_id
0	2		3	12669	9656		7561	214				2674			1338	0
1	2		3	7057	9810		9568	1762			3293			1776	0
2	2		3	6353	8808		7684	2405			3516			7844	0
3	1		3	13265	1196		4221	6404			507				1788	2
4	2		3	22615	5410		7198	3915			1777			5185	0
```

- 군집화에서 얻은 라벨을 원본에 추가해서 군집화를 위한 분류를 추가한다.

```python
plt.scatter(x=copy_df['Grocery'],y=copy_df['Frozen'],marker='o',c=copy_df['clu_id'])
plt.show()
```

![cl09](./img/cl09.png)

```python
plt.scatter(x=copy_df['Milk'],y=copy_df['Delicassen'],marker='o',c=copy_df['clu_id'])
plt.show()
```

![cl10](./img/cl10.png)

```python
copy_df.plot(kind='scatter', x='Grocery',y='Frozen',c='clu_id',colorbar=True,cmap='Set2')
plt.show()
```

![cl11](./img/cl11.png)

- 저기 떨어진 값들은 노이즈, 즉 이상치들이다. 그래서 제거해서 분석하는것도 좋다.

```python
copy_df.plot(kind='scatter', x='Milk',y='Delicassen',c='clu_id',colorbar=True,cmap='Set3')
plt.show()
```

![cl12](./img/cl12.png)

#### PCA차원축소 2개, 군집화하고 시각화해보자.

```python
from sklearn.decomposition import PCA
sample_df_pca = PCA(n_components=2)
sample_df_pca_trans = sample_df_pca.fit_transform(std_df)
copy_df['pca_x'] = sample_df_pca_trans[:,0]
copy_df['pca_y'] = sample_df_pca_trans[:,1]
```

```python
std_kmeans.fit(copy_df.iloc[:,-2:])
copy_df.plot(kind='scatter', x='pca_x', y='pca_y',c='clu_id',colorbar=True,cmap='Set2')
plt.show()
```

![cl13](./img/cl13.png)