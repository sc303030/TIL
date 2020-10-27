# ml_03

### 데이터 전처리

- 데이터 클린징
- 결손값 처리
- 데이터 인코딩(레이블, 원-핫 인코딩)
- 데이터 스케일링
- 이상치 제거
- 피처 선택, 추출 및 가공

#### 데이터 전처리가 필요한 이유?

- 학습을 할 때 학습 알고리즘들은 Null값 허용하지 않는다.
- 분류 알고리즘들 : target 값들이 숫자로 이루어져 있다. 
  - 숫자가 아닌 문자열로 되어 있다면 예측, 분류를 할 수 있을까?
  - 기본적으로 label값들을 수치형으로 변경해주는 작업이 필요하다.

##### 데이터 인코딩

> 머신러닝 알고리즘은 문자열 데이터 속성을 입력 받지 않으며 모든 데이터는 숫자형으로 표현되어야 한다. 문자형 카테고리형 속성은 모두 숫자값으로 변환/인코딩 되어야 한다.

- 레이블(Label)  인코딩
  - 원본 : 상품분류(TV, 냉장고, 전자레인지...) 
  - 인코딩 : 상품 분류 (0,1,2...)
- 원-핫(One-Hot) 인코딩

> 피처 값의  유형에 따라 새로운 피처를 추가해 고유 값에 해당하는 컬럼에만 1을 표시하고 나머지 컬럼에는 0을 표시하는 방식

- 원본 : 상품분류(TV, 냉장고, 전자레인지...) 
- 인코딩 : 상품 분류 (0,1,2...) 후 해당 되는 분류에 1/ 나머지 0

##### pd.get_dummies(DataFrame) : 원-핫 인코딩

```python
from sklearn.datasets import load_iris, load_breast_cancer
from sklearn.tree     import DecisionTreeClassifier
from sklearn.model_selection import  GridSearchCV,train_test_split
from sklearn.metrics import accuracy_score

from sklearn.preprocessing import LabelEncoder
import pandas as pd
import numpy as np
```

`from sklearn.preprocessing import LabelEncoder` 추가

- import 대문자는 class -> 객체임

```python
item_label = ['TV', '냉장고', '전자렌지', '컴퓨터', '선풍기' ,'믹서', '믹서']
```

- 영문은 할글보다 우선하고 한글은 가나다라 순으로 매겨진다.

```python
encoder = LabelEncoder()
encoder.fit(item_label)
digit_label = encoder.transform(item_label)
print('encoder 결과', digit_label)
>
encoder 결과 [0 1 4 5 3 2 2]
```

- 문자 순서대로 레이블이 생겼다.

- 이걸 `pd.get_dummies(DataFrame) ` 쓰면 한 번에 된다.

```python
print('decoder 결과', encoder.inverse_transform(digit_label))
>
decoder 결과 ['TV' '냉장고' '전자렌지' '컴퓨터' '선풍기' '믹서' '믹서']
```

- 디코더도 가능하다.

##### OneHotEncoder

```python
from sklearn.preprocessing import OneHotEncoder
item_label = ['TV', '냉장고', '전자렌지', '컴퓨터', '선풍기' ,'믹서', '믹서']
encoder = LabelEncoder()
encoder.fit(item_label)
digit_label = encoder.transform(item_label)
type(digit_label)
>
numpy.ndarray
```

- 학습을 하려면 2차원이여야 한다.

##### 2차원 데이터로 변환

```python
digit_label = digit_label.reshape(-1,1)
print(digit_label)
>
[[0]
 [1]
 [4]
 [5]
 [3]
 [2]
 [2]]
```

- reshape(-1,1) : 행을 데이터 개수에 맞추겠다. 1열로

```python
print(digit_label.shape)
>
(7, 1)
```

- 2차원이다.

##### One-Hot encoding

```python
ont_hot_encoder = OneHotEncoder()
ont_hot_encoder.fit(digit_label)
ont_hot_label = ont_hot_encoder.transform(digit_label)
print(ont_hot_label.toarray())
print(ont_hot_label.shape)
>
[[1. 0. 0. 0. 0. 0.]
 [0. 1. 0. 0. 0. 0.]
 [0. 0. 0. 0. 1. 0.]
 [0. 0. 0. 0. 0. 1.]
 [0. 0. 0. 1. 0. 0.]
 [0. 0. 1. 0. 0. 0.]
 [0. 0. 1. 0. 0. 0.]]
(7, 6)
```

- 값이 들어 있는 곳에는 1이 들어가있고 아닌 곳은 0으로 .

- 7행 6열의 2차원의 배열이다.

1. 레이블 인코딩
2. 2차원 데이터로 변환
3. 원-핫 인코딩

##### pandas get_dummies(df)

```python
ont_hot_df = pd.DataFrame({'item' :  ['TV', '냉장고', '전자렌지', '컴퓨터', '선풍기' ,'믹서', '믹서']})
pd.get_dummies(ont_hot_df)
>
	item_TV		item_냉장고	item_믹서		item_선풍기	item_전자렌지	item_컴퓨터
0		1				0			0				0				0			0
1		0				1			0				0				0			0
2		0				0			0				0				1			0
3		0				0			0				0				0			1
4		0				0			0				1				0			0
5		0				0			1				0				0			0
6		0				0			1				0				0			0
```

- 이렇게 한 번에 해준다.

### 결측값 처리

```python
from io import StringIO
import pandas as pd
import numpy as np 

csv_data = StringIO("""
x1,x2,x3,x4,x5
1,0.1,"1",2019-01-01,A
2,,,2019-01-02,B
3,,"3",2019-01-03,C
,0.4,"4",2019-01-04,A
5,0.5,"5",2019-01-05,B
,,,2019-01-06,C
7,0.7,"7",,A
8,0.8,"8",2019-01-08,B
9,0.9,,2019-01-09,C
""")

df = pd.read_csv(csv_data)
df

```

- `StringIO` : 코드상의 문자열을 데이터로 만들어서 csv로 불러올 수 있다.