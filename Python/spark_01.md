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

- filter() : 조건으로 필터링하는 연산자

```python
num = spark.parallelize(list(range(1,30,3)))
num
>
ParallelCollectionRDD[11] at parallelize at PythonRDD.scala:195
```

```python
result = num.filter(lambda x : x % 2 == 0).collect()
result
>
[4, 10, 16, 22, 28]
```

- 액션을 취하면 값을 얻을 수 있다.

#### PairRDD

- pair rdd란 key-vlaue 쌍으로 이루어진 RDD
- python tuple을 의미

```python
pairRDD = spark.parallelize([(1,3),(1,5),(2,4),(3,3)])
pairRDD
>
ParallelCollectionRDD[13] at parallelize at PythonRDD.scala:195
```

- reduceByKey()

```python
{
    i:j
    for i,j in pairRDD.reduceByKey(lambda x,y : x*y).collect()
    
}
>
{1: 15, 2: 4, 3: 3}
```

- 동일 키에 대한 값들을 곱해버렸다.

```python
{
    i:j
    for i,j in pairRDD.mapValues(lambda x : x**2).collect()
    
}
>
{1: 25, 2: 16, 3: 9}
```

- map의 키는 중복되면 안 되기 때문에 중복되면 처음 값을 무시하고 그 다음거로 연산을 수행한다.

```python
pairRDD.groupByKey().collect()
>
[(1, <pyspark.resultiterable.ResultIterable at 0x24c4167c048>),
 (2, <pyspark.resultiterable.ResultIterable at 0x24c4167c0b8>),
 (3, <pyspark.resultiterable.ResultIterable at 0x24c4167c128>)]
```

```python
pairRDD.values().collect()
>
[3, 5, 4, 3]
```

```python
pairRDD.sortByKey().collect()
>
[(1, 3), (1, 5), (2, 4), (3, 3)]
```

#### 외부 데이터를 로드해서 RDD 생성하는 방법

```python
customerRDD = spark.textFile('./data/spark-rdd-name-customers.csv')
customerRDD
>
./data/spark-rdd-name-customers.csv MapPartitionsRDD[28] at textFile at NativeMethodAccessorImpl.java:0
```

```python
customerRDD.first()
>
'Alfreds Futterkiste,Germany'
```

- first() :문서의 첫 번째 값을 리턴해주는 값
- ,(콤마)앞이 키 뒤가 벨류

#### map연산자를 이용해서 콤마로 split하고 튜플로 리턴하는 구문을 작성해보자

```python
cusPairs = customerRDD.map(lambda x : (x.split(',')[0],x.split(',')[1]))
cusPairs
>
PythonRDD[30] at RDD at PythonRDD.scala:53
```

- 트랜스포메이션 하기

```python
cusPairs.collect()
>
('Alfreds Futterkiste', 'Germany'),
 ('Ana Trujillo Emparedados y helados', 'Mexico'),
```

- 액션 하기
- 데이터를 살펴보니 뒤에 있는 문자가 나라임->이거를 키로 쓰자

```python
cusPairs = customerRDD.map(lambda x : (x.split(',')[1],x.split(',')[0]))
cusPairs
```

- 다시 키와 벨류를 바꾸었다.

#### groupByKey() : 키값을 리스트 형태로 리턴 함수

```python
cusPairs.groupByKey().collect()
>
('Alfreds Futterkiste',
  <pyspark.resultiterable.ResultIterable at 0x24c41680630>),
 ('Ana Trujillo Emparedados y helados',
  <pyspark.resultiterable.ResultIterable at 0x24c416806d8>),
```

- 유일한 키값들을 리턴시켜준다.

#### UK에 사는 고객 이름만 출력한다면? (dict형식으로 만들어서 )

```python
groupKey = cusPairs.groupByKey().collect()
groupKey
```

- value가 아니라 주소로 반환한다. 이것을 딕셔너리 형식으로 만들어서 value를 꺼내보자.

```python
customerDict = {
    country : [name for name in names] for country, names in groupKey    
}
customerDict['UK']
>
['Around the Horn',
 "B's Beverages",
 'Consolidated Holdings',
 'Eastern Connection',
 'Island Trading',
 'North/South',
 'Seven Seas Imports']
```

- 리스트컴프리헨션으로 for구문 돌린 값을 다시 받아서 만들었다.

#### sortByKey : Key를 오름차순으로 정렬해보고 상위 10개만 뽑는다면?

```python
cusPairs.sortByKey().keys().collect()[:10]
>
['Argentina',
 'Argentina',
 'Argentina',
 'Austria',
 'Austria',
 'Belgium',
 'Belgium',
 'Brazil',
 'Brazil',
 'Brazil']
```

- 키를 오름차순으로 정렬하고 키값만 10개 뽑았다.

#### 나라별 고객이 몇명인지를 카운트 해 본다면?

```python
mapR = cusPairs.mapValues(lambda x: 1 ).reduceByKey(lambda x,y : x+y)
{
    i:j
    for i,j in mapR.collect()   
}
>
PythonRDD[103] at RDD at PythonRDD.scala:53
>
{'Germany': 11,
 'Mexico': 5,
 'UK': 7,
 'Sweden': 2,
 'France': 11,
```

- 스파크는  `{}` 안에서 해줘야 다중 값을 리턴받는다.

- 딕셔너리 형식으로 리턴받을 수 있다.

- mapValues(lambda x: 1 ).reduceByKey(lambda x,y : x+y)
  - x는 키, 그래서 reduce를 하면서 키를 제거하고 그 키가 가지고 있던 값을 더해주면서 키의 중복을 제거해간다.