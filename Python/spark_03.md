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