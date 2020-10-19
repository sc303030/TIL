# 시각화_01

### 학습목표
- 시각화 패키지 matplotlib
- 서브 패키지 pyplot
- seaborn
- 분석된 내용을 시각화 
- Pandas 시각화 기능

```python
import pandas as pd
import numpy as np
import matplotlib as mpl
import matplotlib.pyplot as plt
%matplotlib inline
```

- %matplotlib inline : 주피터에서 라인 그릴 때 사용

```python
import matplotlib.pyplot as plt
%matplotlib inline

import platform

from matplotlib import font_manager, rc
# plt.rcParams['axes.unicode_minus'] = False

if platform.system() == 'Darwin':
    rc('font', family='AppleGothic')
elif platform.system() == 'Windows':
    path = "c:/Windows/Fonts/malgun.ttf"
    font_name = font_manager.FontProperties(fname=path).get_name()
    rc('font', family=font_name)
else:
    print('Unknown system... sorry~~~~') 
```

- 한글 깨짐 방지

#### line plot : 데이터가 시간, 순서 등에 따라 어떻게 변화하는지를 보여주는

```python
plt.title('line plot')
plt.plot([10,20,30,40],[1,4,9,16]) #ndarray나 list이여도 상관 없음
plt.xlabel('x 라벨')
plt.ylabel('y 라벨')
plt.show()
```

- [1,4,9,16] :  ndarray나 list이여도 상관 없음
- title : 제목 주는 것

- plt.plot([x축],[y축])

![vi01](./img/vi01.png)

#### option(color, marker, style)

- 약어 -> b(blue), r(red), . o (마커), - or -- or : or -. 등등

```python
plt.title('line plot')
plt.plot([10,20,30,40],[1,4,9,16],'rs--')
plt.xlabel('x 라벨')
plt.ylabel('y 라벨')
plt.show()
```

- 'rs--'
  - 빨간색과 square 마커와 --스타일

![vi02](./img/vi02.png)

#### 라인 플롯에서 자주 사용되는 기타 스타일

![line](./img/line_style.png)

```python
plt.title('스타일 적용')
plt.plot([10,20,30,40],[1,4,9,16],c='b',lw=5,ls='--',marker='o',ms=15,mec='g',mew=5,mfc='r')
plt.xlabel('x 라벨')
plt.ylabel('y 라벨')
plt.show()
```

![vi03](./img/vi03.png)

**x,y 구간 지정**

```python
plt.title('스타일 적용')
plt.plot([10,20,30,40],[1,4,9,16],c='b',lw=5,ls='-.',marker='o',ms=15,mec='g',mew=5,mfc='r') 
plt.xlabel('x 라벨')
plt.ylabel('y 라벨')
plt.xlim(0,50)
plt.ylim(-10,30)
plt.show()
```

![vi04](./img/vi04.png)

#### tick : x,y축에 지정되는 한 지점

- xticks, yticks

```python
X = np.linspace(-np.pi, np.pi,256)
Y = np.cos(X)
plt.plot(X,Y)
plt.show()
```

![vi09](./img/vi09.png)

```python
X = np.linspace(-np.pi, np.pi,256)
Y = np.cos(X)
plt.plot(X,Y)
plt.xticks([-np.pi,-np.pi/2,0,np.pi/2,np.pi])
plt.yticks([-1,0,1])
plt.grid(True)
plt.show()
```

![vi10](./img/vi10.png)

- xticks과 yticks을 넣어서 범위를 지정하였다.
- grid를 추가하여 격자를 주었다.

#### 여러개의 라인 플롯 그리기

```python
data = np.arange(0. , 5.0, 0.2)
data
plt.title('여러개의 라인 플롯')
plt.plot(data,data,'r--',data, data**2, 'g-',data,data**3,'b-.')
plt.show()
```

- plot안에 여러개의 데이터를 넣으면 된다.

![vi05](./img/vi05.png)

#### 라인 겹쳐 그리기

```python
plt.title('라인을 겹쳐 그리기')
plt.plot([1,4,9,16],c='b',lw=5,ls='--',marker='o',ms=15,mec='g',mew=5,mfc='g') 
plt.plot([5,9,3,2],c='k',lw=5,ls='-.',marker='s',ms=15,mec='m',mew=7,mfc='c') 
plt.show() 
```

-  y축을 기준으로 겹쳐 그린다.

![vi03](./img/vi06.png)

#### 범례주기

```python
X = np.linspace(-np.pi, np.pi,256)
Y_C , Y_S = np.cos(X), np.sin(X)
plt.plot(X,Y_C,ls='--',label='cosine')
plt.plot(X,Y_S,ls='--',label='sine')
plt.legend(loc=0)
plt.show()
```

- 범례는  0 ~ 10까지
  - 0은 가장 최적의 위치에 알아서 들어간다.
  - 1 : 오른쪽 탑, 2 : 왼쪽 탑, 3 : 왼쪽 아래, 4 : 오른쪽 아래, 5 : 오른쪽 중앙, 6 : 왼쪽 중앙, 7 : 오른쪽 중앙, 8 : 아래 중간, 9 : 위 중간 , 10 : 정 가운데

![vi07](./img/vi07.png)

#### 여러개 그리기

- matplotlib 그림을 그리는 객체, Figure, Axes, Axis 객체를 포함하고 있다.
- Figure -> 그림이 그려지는 종이 뜻
- Axes -> 플롯
- Axis -> 축
- subplot(2,1,1) 2행 1열 첫번째
- subplot(2,1,2) 2행 1열 두번째

```python
X1 = np.linspace(0.0, 5.0)
Y1 = np.cos(2 * np.pi) * np.exp(-X1)
X2 = np.linspace(0.0, 2.0)
Y2 = np.cos(2 * np.pi * X2)
```

```python
axes01 = plt.subplot(2,1,1)
plt.plot(X1,Y1)
plt.title('subplot 01')
axes02 = plt.subplot(2,1,2)
plt.plot(X2,Y2)
plt.title('subplot 02')
plt.tight_layout() # 자동으로 간격을 맞춰주는 역할
plt.show()
```

![vi11](./img/vi11.png)

```python
axes01 = plt.subplot(1,2,1)
plt.plot(X1,Y1)
plt.title('subplot 01')
axes02 = plt.subplot(1,2,2)
plt.plot(X2,Y2)
plt.title('subplot 02')

plt.tight_layout() # 자동으로 간격을 맞춰주는 역할
plt.show()
```

![vi12](./img/vi12.png)

### 플롯의 유형

- line plot
- scatter plot
- contour plot
- surface plot
- bar plot
- box plot
- histogram plot

#### bar plot

-  x 데이터는 카테코리 값인 경우가 대부분

```python
import matplotlib.pylab as plt
```

- import 한다.

```python
Y = [2,3,1]
X = np.arange(len(Y))

xlabel = ['1등실','2등실','3등실']
plt.title('bar plot')
plt.bar(X,Y)
plt.xticks(X, xlabel)
plt.yticks(sorted(Y))
plt.xlabel('선실 등급')
plt.ylabel('생존자 수')
plt.show()
```

![vi13](./img/vi13.png)

- xticks에 x를 주고 그거의 이름을 xlabel로 준다.

```python
Y = [2,3,1]
X = np.arange(len(Y))

xlabel = ['1등실','2등실','3등실']
plt.title('bar plot')
plt.barh(X,Y)
plt.xticks(sorted(Y))
plt.yticks(X, xlabel)
plt.xlabel('생존자 수')
plt.ylabel('선실 등급')
plt.show()
```

![vi14](./img/vi14.png)