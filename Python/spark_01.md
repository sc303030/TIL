# Spark_01

### 특징

- 머신러닝 가능
- 내부적 프로세싱만 다르다.

#### 언어

1. 자바
2. 스칼라(객체 지향이지만 함수기반)
3. 파이썬
   1. 파이썬과 스파트를 이어주는 환경을 만든다.
   2. 분산환경을 지원하기때문에 연산이 빠르다.
   3. 지도학습, 비지도학습도 가능
   4. 기존의 언어들을 스파크에서 사용할 수 있도록 해준다.
   5. RDD를 데이터 프레임이나 데이터셋으로 바꿔주는 작업이 필요하다.

#### 데이터

- RDD -> DataFrame-> Dataset
  - 순차적 처리가 아니라 병렬로 처리
  - 분산되는 데이터 집합
  - 파티션으로 나누어서 그걸 병렬로 처리
- 데이터를 메모리에 바로 저장하지 않고 RDD로 연산하는 과정을 기록
  - 트랜스포메이션
    - **Transformation** -> Lazy Excution 또는 Lazy Loading
    - 트랜스포메이션이 행해지면, RDD가 수행되는 것이 아니라 새로운 RDD를 만들어 내고 그 새로운 RDD에 수행결과를 저장하게 된다
  - 이 결과를 보려면 **Action**을 해야한다.
    - 메서드로 이루어진 실행작업이며 , 트랜스포메이션이 행해지고 나서 이루어지는 Evaluation 작업
    - 함수로 이루어져 있다.
  - 오류가 나지 않는다.

#### 하둡

- 기본적으로 자바언어를 알아야 한다.

#### 라이브러리 설치

```
conda install -c conda-forge pyspark
```

#### import 하기

```python
from pyspark import SparkConf, SparkContext
from pyspark.sql import SQLContext
```

```python
conf = SparkConf().setMaster('local').setAppName('sparkApp')
spark = SparkContext(conf=conf)
spark
>
SparkContext

Spark UI

Version
v2.4.6
Master
local
AppName
sparkApp
```

```python
rdd = spark.textFile('./data/test.txt')
rdd
>
./data/test.txt MapPartitionsRDD[1] at textFile at NativeMethodAccessorImpl.java:0
```

- 파일이 없지만 오류가 나지 않는다. 파티션이 만들어 진것 
  - 트랜스포메이션

```python
lines = rdd.filter(lambda x :'spark' in x)
lines
>
PythonRDD[2] at RDD at PythonRDD.scala:53
```

- filter 이게 액션을 한 것.

- RDD 생성
- 데이터를 직접 만드는 방법(parallelize()), 외부 데이터를 로드 방법으로

```python
sample_rdd = spark.parallelize(['test','this is a test rdd'])
sample_rdd
>
ParallelCollectionRDD[3] at parallelize at PythonRDD.scala:195
```

- 롬에 rdd가 저장되었다.

```python
sample_rdd.collect()
>
['test', 'this is a test rdd']
```

- 롬에 rdd가 저장되어야 불러올 수 있다.

- RDD 자주 쓰는 연산 함수
- collect() : RDD에 트랜스포메이션된 결과를 리턴하는 함수
- map() : 연산을 수행하고 싶을 때 사용하는 함수

```python
numbers = spark.parallelize(list(range(5)))
numbers
>
ParallelCollectionRDD[4] at parallelize at PythonRDD.scala:195
```

```python
s = numbers.map(lambda x: x * x).collect()
s
>
[0, 1, 4, 9, 16]
```

- map을 사용하여 액션을 취함. map은 연산만 해주고 리턴은 못해줌
- 그래서 collect()를 붙여줘야 리턴한다.

- flatmap() : 리스트들의 원소를 하나의 리스트로 flatten해서 리턴하는 함수

```python
strings = spark.parallelize(['hi pengso','hi pengpeng','hi pengha','hi pemgbba'])
unique_string = strings.flatMap(lambda x :x.split(' ')).collect()
unique_string
>
['hi', 'pengso', 'hi', 'pengpeng', 'hi', 'pengha', 'hi', 'pemgbba']
```



