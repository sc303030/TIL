# Pandas실습_01

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
```

```python
data = np.loadtxt('./data/ratings.dat', delimiter='::', dtype=np.object)
data
>
array([['1', '1193', '5', '978300760'],
       ['1', '661', '3', '978302109'],
       ['1', '914', '3', '978301968'],
       ...,
       ['6040', '562', '5', '956704746'],
       ['6040', '1096', '4', '956715648'],
       ['6040', '1097', '4', '956715569']], dtype=object)
```

- `::` 로 나누어져있어서 지웠다.

**데이터의 첫 5 행만 확인**

```python
data[0:5,:]
>
array([[        1,      1193,         5, 978300760],
       [        1,       661,         3, 978302109],
       [        1,       914,         3, 978301968],
       [        1,      3408,         4, 978300275],
       [        1,      2355,         5, 978824291]], dtype=int64)
```

**데이터의 형태 확인**

```python
data.shape
>
(1000209, 4)
```

**전체 평균 평점 계산**

```python
data[:,2].mean()
>
3.581564453029317
```

**사용자 아이디 수집**

```python
user_id = list(set(data[:,0]))
user_id
>
[1,
 2,
 1000,
 ...]
```

```python
user_ids = np.unique(data[:,0])
```

- `set` 을 사용해서 중복없이 값을 가져왔다.
- 유티크도 가능하다.

**수집한 사용자 아이디별 평점 확인**

```python
user_movie_rating = [   data[data[:,0] == user_id[i]][:,2] for i in range(len(user_id))]
user_movie_rating 
>
[array([5, 3, 3, 4, 5, 3, 5, 5, 4, 4, 5, 4, 4, 4, 5, 4, 3, 4, 5, 4, 3, 3,
        5, 5, 3, 5, 4, 4, 4, 3, 4, 4, 4, 4, 4, 4, 5, 5, 4, 5, 5, 5, 4, 4,
        4, 5, 5, 4, 5, 4, 4, 4, 4], dtype=int64),
 array([5, 4, 4, 3, 4, 4, 5, 3, 3, 3, 5, 4, 3, 3, 2, 5, 3, 4, 3, 4, 2, 3,
```

- 내가 수집한 사용자 `id` 만큼 루프를 돌려서 만약에 data의 사용자 id와 내가 수집한사용자 id가 일치하면 그 id에 해당하는 모든 평점을 리스트에 저장한다.

**수집한 사용자 아이디별로 평점의 평점 확인**

```python
mean_user_id = [   data[data[:,0] == user_id[i]][:,2].mean() for i in range(len(user_id))]
mean_user_id
>
[4.188679245283019,
 3.7131782945736433,
 3.9019607843137254,...]
```

```python
mean_userids= []
for user in user_id:
    data_for_user = data[user == data[ : , 0], :]
    user_mean = data_for_user[:,2].mean()
    mean_userids.append([user,user_mean])
mean_userids
>
[[1, 4.188679245283019],
 [2, 3.7131782945736433],
 [3, 3.9019607843137254],
```

- 리스트 형태로 저장된다.

**리스트를 Numpy 배열로 변환**

```python
mean_userids_array = np.array(mean_userids, dtype=np.float32)
print(mean_userids_array[:5])
print(mean_userids_array.shape)
print(type(mean_userids_array))
>
[[1.        4.188679 ]
 [2.        3.7131784]
 [3.        3.9019608]
 [4.        4.1904764]
 [5.        3.1464646]]
(6040, 2)
<class 'numpy.ndarray'>
```

**csv 파일로 만들기**

```python
np.savetxt('./data/movies_mean_userids.csv',mean_userids_array,delimiter=',',fmt='%.1f')
```

- 최종으로 csv파일에 저장하였다.

