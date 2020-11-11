# Spark_03

## titanic_ml

- 준비

```python
from pyspark import SparkConf, SparkContext
from pyspark.sql import SQLContext

conf = SparkConf().setMaster('local').setAppName('spark_ml')
spark = SparkContext(conf=conf)

sqlCtx = SQLContext(spark)
sqlCtx
```

- 파일 불러오기

```python
orders = sqlCtx.read.csv('./data/spark_titanic_train.csv',header=True,inferSchema=True)
orders.printSchema()
>
root
 |-- PassengerId: integer (nullable = true)
 |-- Survived: integer (nullable = true)
 |-- Pclass: integer (nullable = true)
 |-- Name: string (nullable = true)
 |-- Sex: string (nullable = true)
 |-- Age: double (nullable = true)
 |-- SibSp: integer (nullable = true)
 |-- Parch: integer (nullable = true)
 |-- Ticket: string (nullable = true)
 |-- Fare: double (nullable = true)
 |-- Cabin: string (nullable = true)
 |-- Embarked: string (nullable = true)
```

- 학습은 데이터값이 수치형이어야 한다. 그래서 형식을 확인해서 레이블 인코딩이나 제외할 열을 찾는다.

- 데이터 head확인

```python
orders.show(5)
>
+-----------+--------+------+--------------------+------+----+-----+-----+----------------+-------+-----+--------+
|PassengerId|Survived|Pclass|                Name|   Sex| Age|SibSp|Parch|          Ticket|   Fare|Cabin|Embarked|
+-----------+--------+------+--------------------+------+----+-----+-----+----------------+-------+-----+--------+
|          1|       0|     3|Braund, Mr. Owen ...|  male|22.0|    1|    0|       A/5 21171|   7.25| null|       S|
|          2|       1|     1|Cumings, Mrs. Joh...|female|38.0|    1|    0|        PC 17599|71.2833|  C85|       C|
|          3|       1|     3|Heikkinen, Miss. ...|female|26.0|    0|    0|STON/O2. 3101282|  7.925| null|       S|
|          4|       1|     1|Futrelle, Mrs. Ja...|female|35.0|    1|    0|          113803|   53.1| C123|       S|
|          5|       0|     3|Allen, Mr. Willia...|  male|35.0|    0|    0|          373450|   8.05| null|       S|
+-----------+--------+------+--------------------+------+----+-----+-----+----------------+-------+-----+--------+
only showing top 5 rows
```

- 전처리가 필요하다.

- 특정 열 선택

```python
orders.select(['Survived','Pclass','Embarked']).show()
>
+--------+------+--------+
|Survived|Pclass|Embarked|
+--------+------+--------+
|       0|     3|       S|
|       1|     1|       C|
```

#### EDA

```python
orders.groupBy('Survived').count().show()
>
+--------+-----+
|Survived|count|
+--------+-----+
|       1|  342|
|       0|  549|
+--------+-----+
```

```python
orders.groupBy('Sex','Survived').count().show()
>
+------+--------+-----+
|   Sex|Survived|count|
+------+--------+-----+
|  male|       0|  468|
|female|       1|  233|
|female|       0|   81|
|  male|       1|  109|
+------+--------+-----+
```

```python
orders.groupBy('Pclass','Survived').count().show()
>
+------+--------+-----+
|Pclass|Survived|count|
+------+--------+-----+
|     1|       0|   80|
|     3|       1|  119|
|     1|       1|  136|
|     2|       1|   87|
|     2|       0|   97|
|     3|       0|  372|
+------+--------+-----+
```

- groupBy로 확인해보기

#### 새로운 패키지 사용해보기

```python
from pyspark.sql.functions import mean,col,split, col, regexp_extract, when, lit
```

- Null값 개수 확인

```python
# This function use to print feature with null values and null count 
def null_value_count(df):
  null_columns_counts = []
  numRows = df.count()
  for k in df.columns:
    nullRows = df.where(col(k).isNull()).count()
    if(nullRows > 0):
      temp = k,nullRows
      null_columns_counts.append(temp)
  return(null_columns_counts)
```

- 판다스가 아니기에 isnull().sum()같은 걸 못쓴다.
- where을 안주고 k.isnull하면 오류난다.
  - 위치 지정해주는 함수
  - spark의 문법
  - df[col(k).isNull()].count()로 해도 가능

```python
null_list = null_value_count(orders)
null_list
>
[('Age', 177), ('Cabin', 687), ('Embarked', 2)]
```

- createDataFrame() : 판다스의 df를 sparkd의 df로 만들기

```python
sqlCtx.createDataFrame(null_list, ['column','cnt']).show()
>
+--------+---+
|  column|cnt|
+--------+---+
|     Age|177|
|   Cabin|687|
|Embarked|  2|
+--------+---+
```

##### 나이 평균을 구한다면?

```python
orders.select(mean('Age')).show()
>
+-----------------+
|         avg(Age)|
+-----------------+
|29.69911764705882|
+-----------------+
```

#### regexp_extract : 정규표현식을 extract할 수 있다.

```python
type(col('Name'))
>
pyspark.sql.column.Column

type('Name')
>
str
```

- 컬럼으로 만들어서 함수를 적용해야 한다.
- 그래서 col()로 감싸준다.

```python
orders = orders.withColumn('initial',regexp_extract(col('Name'),'([A-Za-z]+)\.',1))
```

- 정규식을 이용하여 중간 mr, mrs들만 가져왔다.

```python
orders.select(['Pclass','initial']).show()
>
+------+-------+
|Pclass|initial|
+------+-------+
|     3|     Mr|
|     1|    Mrs|
```

- 중복제거해서 출력해보기

```python
orders.select('initial').distinct().show()
>
+--------+
| initial|
+--------+
|     Don|
|    Miss|
|Countess|
```

- 제대로 동작되지 않았다.

**replace(['바꾸고 싶은 값'],['새로운 값'])**

```python
orders = orders.replace(['Don'],['Other'])
orders.select('initial').distinct().show()
>
+--------+
| initial|
+--------+
|    Miss|
|Countess|
|     Col|
|     Rev|
|    Lady|
|   Other|
```

- 정상적으로 바뀌었다.

```python
orders.groupby('initial').avg('age').show()
>
+--------+------------------+
| initial|          avg(age)|
+--------+------------------+
|    Miss|21.773972602739725|
|Countess|              33.0|
|     Col|              58.0|
|     Rev|43.166666666666664|
```

- initial 별로 평균가져올 수 있다.

```python
orders = orders.replace(['Mlle','Mme', 'Ms', 'Dr','Major','Lady','Countess','Jonkheer','Col','Rev','Capt','Sir','Don'],
               ['Miss','Miss','Miss','Mr','Mr',  'Mrs',  'Mrs',  'Other',  'Other','Other','Mr','Mr','Mr'])
orders.groupby('initial').avg('age').show()
>
+-------+------------------+
|initial|          avg(age)|
+-------+------------------+
|   Miss|             21.86|
|  Other|              45.3|
| Master| 4.574166666666667|
|     Mr| 32.72181372549019|
|    Mrs|35.981818181818184|
+-------+------------------+
```

```python
orders.groupby('initial').avg('age').collect()
>
[Row(initial='Miss', avg(age)=21.86),
 Row(initial='Other', avg(age)=45.3),
 Row(initial='Master', avg(age)=4.574166666666667),
 Row(initial='Mr', avg(age)=32.72181372549019),
 Row(initial='Mrs', avg(age)=35.981818181818184)]
```

```python
orders.filter(orders['Age']==48).select('initial').show()
>
+-------+
|initial|
+-------+
|     Mr|
|     Mr|
|    Mrs|
|     Mr|
|     Mr|
|    Mrs|
|    Mrs|
|     Mr|
|    Mrs|
+-------+
```

- 나이제한을 걸어 결과값 확인

#### null 처리 방법

```python
orders.groupby('Embarked').count().show()
>
+--------+-----+
|Embarked|count|
+--------+-----+
|       Q|   77|
|    null|    2|
|       C|  168|
|       S|  644|
+--------+-----+
```

```python
orders = orders.na.fill({'Embarked':'S'})
```

- `fill` 과 `na` 를 이용하여 na값을 'S'로 바꾸었다.

```python
orders.groupby('Embarked').count().show()
>
+--------+-----+
|Embarked|count|
+--------+-----+
|       Q|   77|
|       C|  168|
|       S|  646|
+--------+-----+
```

- 불필요한 컬럼 삭제

```python
orders = orders.drop('Cabin')
orders.columns
>
['PassengerId',
 'Survived',
 'Pclass',
 'Name',
 'Sex',
 'Age',
 'SibSp',
 'Parch',
 'Ticket',
 'Fare',
 'Embarked',
 'initial']
```

- 삭제되었다.
- csv 파일을 불러올 때 inferSchema=True를 줘야 나중에 형변환 안 해도 된다.

**파생 컬럼 만드는 방법**

```python
orders = orders.withColumn('Family_Size',col('SibSp') +col('Parch'))
orders.show()
>
+-----------+--------+------+--------------------+------+----+-----+-----+----------------+-------+--------+-------+-----------+
|PassengerId|Survived|Pclass|                Name|   Sex| Age|SibSp|Parch|          Ticket|   Fare|Embarked|initial|Family_Size|
+-----------+--------+------+--------------------+------+----+-----+-----+----------------+-------+--------+-------+-----------+
|          1|       0|     3|Braund, Mr. Owen ...|  male|22.0|    1|    0|       A/5 21171|   7.25|       S|     Mr|          1
```

```python
orders.groupby('Family_Size').count().show()
>
+-----------+-----+
|Family_Size|count|
+-----------+-----+
|          1|  161|
|          6|   12|
|          3|   29|
|          5|   22|
|          4|   15|
|          7|    6|
|         10|    7|
|          2|  102|
|          0|  537|
+-----------+-----+
```

- 사이즈가 0인 것들을 Alone에 0으로 표시해보자.

```python
orders = orders.withColumn('Alone',lit(0))
```

- 0을 컬럼으로 인식해버려서 lit()를 하여 숫자로 인식한다.

```python
orders.select('Alone').show()
>
+-----+
|Alone|
+-----+
|    0|
|    0|
```

```python
orders = orders.withColumn('Alone',when(orders['Family_Size'] == 0, 1).otherwise(orders['Alone']))
orders.select('Alone').show()
orders = orders.na.fill({'Age':20})
>
+-----+
|Alone|
+-----+
|    0|
|    0|
|    1|
|    0|
```

- 정상적으로 실행되었다.

- `when(조건,조건만족 값).otherwise(아닐경우 or 아닐경우 값)` : if 조건이랑 비슷하다.

```python
from pyspark.ml.feature import StringIndexer
from pyspark.ml import Pipeline
```

```python
indexers = [StringIndexer(inputCol=column, outputCol=column+"_index").fit(orders) for column in ["Sex","Embarked","initial"]]
pipeline = Pipeline(stages=indexers)
titanic = pipeline.fit(orders).transform(orders)
```

```python
titanic.printSchema()
>
|-- Embarked_index: double (nullable = false)
 |-- initial_index: double (nullable = false)
```

- 인덱서가 추가됨 

```python
titanic = titanic.drop('PassengerId','Name','Sex','Embarked','Ticket','initial')
titanic.printSchema()
>
root
 |-- Survived: integer (nullable = true)
 |-- Pclass: integer (nullable = true)
 |-- Age: double (nullable = true)
 |-- SibSp: integer (nullable = true)
 |-- Parch: integer (nullable = true)
 |-- Fare: double (nullable = true)
 |-- Family_Size: integer (nullable = true)
 |-- Alone: integer (nullable = false)
 |-- Sex_index: double (nullable = false)
 |-- Embarked_index: double (nullable = false)
 |-- initial_index: double (nullable = false)
```

```python
from pyspark.ml.feature import VectorAssembler
```

```python
feature = VectorAssembler(inputCols = titanic.columns[1:], outputCol = 'features')
feature_vector = feature.transform(titanic)
```

- feature는 레이블을 제외한 여러개의 피처들을 결합해 만든다.

```python
feature_vector.printSchema()
>
root
 |-- Survived: integer (nullable = true)
 |-- Pclass: integer (nullable = true)
 |-- Age: double (nullable = true)
 |-- SibSp: integer (nullable = true)
 |-- Parch: integer (nullable = true)
 |-- Fare: double (nullable = true)
 |-- Family_Size: integer (nullable = true)
 |-- Alone: integer (nullable = false)
 |-- Sex_index: double (nullable = false)
 |-- Embarked_index: double (nullable = false)
 |-- initial_index: double (nullable = false)
 |-- features: vector (nullable = true)
```

- 트레인, 테스트 데이터 나누기

```python
trainData, testData = feature_vector.randomSplit([.8,.2],seed=100)
```

- 모델링
- Spark ML(DTC, LR, RFC, GDTC, NB, SVM)

### LogisticRegression

#### 데이터의 범주가 0,1 사이의 값으로 예측하는 분류 알고리즘

```python
from pyspark.ml.classification import LogisticRegression
```

```python
lr = LogisticRegression(labelCol='Survived', featuresCol='features')
lr_model = lr.fit(trainData)
lr_pred = lr_model.transform(testData)

```

- 알고리즘 분류기 객체를 생성하고 학습하였다. 예측도 하자

```python
lr_pred.printSchema()
>
|-- probability: vector (nullable = true)
 |-- prediction: double (nullable = false)
```

- 예측값 컬럼이 생성되었다.

```python
lr_pred.select('prediction','Survived','features').show()
>
+----------+--------+--------------------+
|prediction|Survived|            features|
+----------+--------+--------------------+
|       0.0|       0|(10,[0,1,6],[1.0,...|
|       0.0|       0|(10,[0,1,4,6],[1....|
|       0.0|       0|(10,[0,1,4,6],[1....|
```

- 예측값과 실제값을 비교해보았다.

#### 평가를 해보자

```python
from pyspark.ml.evaluation import MulticlassClassificationEvaluator
```

```python
evaluator = MulticlassClassificationEvaluator(labelCol='Survived',
                                             predictionCol = 'prediction',
                                             metricName='accuracy')
```

```python
acc = evaluator.evaluate(lr_pred)
print('acc : ', acc)
print('err : ',1.0 - acc)
>
acc :  0.8166666666666667
err :  0.18333333333333335
```

- 예측 정확도를 구했다.