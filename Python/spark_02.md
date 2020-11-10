# Spark_02

```python
from pyspark import SparkConf, SparkContext
from pyspark.sql import SQLContext
```

```python
conf = SparkConf().setMaster('local').setAppName('sparkApp')
spark = SparkContext(conf=conf)
```

```python
sqlCtx = SQLContext(spark)
sqlCtx
>
<pyspark.sql.context.SQLContext at 0x22d3d64aba8>
```

### CSV 파일을 이용한 DataFrame 만들기

```python
orders = sqlCtx.read.csv('./data/orders.csv')
orders.printSchema()
>
root
 |-- _c0: string (nullable = true)
 |-- _c1: string (nullable = true)
 |-- _c2: string (nullable = true)
 |-- _c3: string (nullable = true)
 |-- _c4: string (nullable = true)
```

- 지금은 read.csv 할 때 아무 속성을 부여하지 않았다.
  - 스파크는 속성이 없다.

```python
orders = sqlCtx.read.csv('./data/orders.csv',header=True,inferSchema=True)
orders.printSchema()
>
root
 |-- OrderID: integer (nullable = true)
 |-- CustomerID: integer (nullable = true)
 |-- EmployeeID: integer (nullable = true)
 |-- OrderDate: string (nullable = true)
 |-- ShipperID: double (nullable = true)
```

- 지금은 속성을 부여하였다.

```python
type(orders)
>
pyspark.sql.dataframe.DataFrame
```

```python
orders.columns
>
['OrderID', 'CustomerID', 'EmployeeID', 'OrderDate', 'ShipperID']
```

```python
orders.describe().show()
>
+-------+-----------------+------------------+------------------+---------+------------------+
|summary|          OrderID|        CustomerID|        EmployeeID|OrderDate|         ShipperID|
+-------+-----------------+------------------+------------------+---------+------------------+
|  count|              196|               196|               196|      196|               196|
|   mean|          10345.5| 48.64795918367347|4.3520408163265305|     null|2.0714285714285716|
| stddev|56.72448031200168|25.621776513566466|  2.41651283366105|     null|0.7877263614433762|
|    min|            10248|                 2|                 1| 1/1/1997|               1.0|
|    max|            10443|                91|                 9| 9/9/1996|               3.0|
+-------+-----------------+------------------+------------------+---------+------------------+

```

- describe는 트랜스포메이션만 한 것.
- show라는 액션을 취해줘야 함

```python
orders.summary().show()
>
+-------+-----------------+------------------+------------------+---------+------------------+
|summary|          OrderID|        CustomerID|        EmployeeID|OrderDate|         ShipperID|
+-------+-----------------+------------------+------------------+---------+------------------+
|  count|              196|               196|               196|      196|               196|
|   mean|          10345.5| 48.64795918367347|4.3520408163265305|     null|2.0714285714285716|
| stddev|56.72448031200168|25.621776513566466|  2.41651283366105|     null|0.7877263614433762|
|    min|            10248|                 2|                 1| 1/1/1997|               1.0|
|    25%|            10296|                25|                 2|     null|               1.0|
|    50%|            10345|                51|                 4|     null|               2.0|
|    75%|            10394|                69|                 6|     null|               3.0|
|    max|            10443|                91|                 9| 9/9/1996|               3.0|
+-------+-----------------+------------------+------------------+---------+------------------+
```

```python
orders.first()
>
Row(OrderID=10248, CustomerID=90, EmployeeID=5, OrderDate='7/4/1996', ShipperID=3.0)
```

- 첫 번째 행을 알 수 있다.

#### 검색 select(컬럼리스트)

```python
orders.select('OrderID').show()
>
+-------+
|OrderID|
+-------+
|  10248|
```

```python
orders.select(['OrderID','CustomerID']).show()
>
+-------+----------+
|OrderID|CustomerID|
+-------+----------+
|  10248|        90|
```

#### withColumn() : 새로운 컬럼만들거나 원래 있던 컬럼이면 업데이트 된다.

```python
orders.withColumn('newOrderID',orders['OrderID']+2).show()
>
+-------+----------+----------+---------+---------+----------+
|OrderID|CustomerID|EmployeeID|OrderDate|ShipperID|newOrderID|
+-------+----------+----------+---------+---------+----------+
|  10248|        90|         5| 7/4/1996|      3.0|     10250|
```

- 새로운 컬럼이 생성되었다.
- 재할당 하지 않아서 원본에는 업데이트가 안되어있다.

#### withColumnRenamed() : 컬럼 이름 수정

```python
orders.withColumnRenamed('OrderID','renameOrderID').show()
>
+-------------+----------+----------+---------+---------+
|renameOrderID|CustomerID|EmployeeID|OrderDate|ShipperID|
+-------------+----------+----------+---------+---------+
|        10248|        90|         5| 7/4/1996|      3.0|
```

- 컬림명을 변경하였다. 
- 재할당하지 않아서 원본에는 업데이트가 안 되어있다.

#### groupby() - 집계함수

```python
orders.groupBy('EmployeeID').count().show()
>
+----------+-----+
|EmployeeID|count|
+----------+-----+
|         1|   29|
|         6|   18|
|         3|   31|
|         5|   11|
|         9|    6|
|         4|   40|
|         8|   27|
|         7|   14|
|         2|   20|
+----------+-----+
```

---

```python
orders = sqlCtx.read.csv('./data/cospi.csv',header=True,inferSchema=True)
orders.printSchema()
```

- 새로운 파일을 불러온다.

#### filter(조건식)

##### 날짜가 2월인 데이터만 필터링한다면?

```python
orders.filter(orders['Date'] >= '2016-02-01').show()
>
+-------------------+-------+-------+-------+-------+------+
|               Date|   Open|   High|    Low|  Close|Volume|
+-------------------+-------+-------+-------+-------+------+
|2016-02-26 00:00:00|1180000|1187000|1172000|1172000|176906|
|2016-02-25 00:00:00|1172000|1187000|1172000|1179000|128321|
```

- 필터안에 조건식을 줄 수 있다.

##### 종가가 1,200,000이상인 데이터만 필터링 한다면?

```python
orders.filter(orders['Close'] > 1200000).show()
>
+-------------------+-------+-------+-------+-------+------+
|               Date|   Open|   High|    Low|  Close|Volume|
+-------------------+-------+-------+-------+-------+------+
|2016-01-05 00:00:00|1202000|1218000|1186000|1208000|207947|
```

```python
orders.filter(orders['Close'] > 1200000).select(['Date','Open','Close']).show()
>
+-------------------+-------+-------+
|               Date|   Open|  Close|
+-------------------+-------+-------+
|2016-01-05 00:00:00|1202000|1208000|
```

- select로 조건을 줄 수 있다.

```python
orders.filter((orders['Open'] > 1200000) & (orders['Open'] < 1250000)).show()
>
+-------------------+-------+-------+-------+-------+------+
|               Date|   Open|   High|    Low|  Close|Volume|
+-------------------+-------+-------+-------+-------+------+
|2016-02-18 00:00:00|1203000|1203000|1178000|1187000|211795|
|2016-01-06 00:00:00|1208000|1208000|1168000|1175000|359895|
```

- 여러개 조건도 가능하다.

```python
orders.filter((orders['Open'] > 1200000) | (orders['Open'] < 1250000)).show()
>
+-------------------+-------+-------+-------+-------+------+
|               Date|   Open|   High|    Low|  Close|Volume|
+-------------------+-------+-------+-------+-------+------+
|2016-02-26 00:00:00|1180000|1187000|1172000|1172000|176906|
```

- 물론 or도 가능하다.

##### Volumn이 300,000이하인것들만 가져온다면?

```python
orders.filter(orders['Volume'] <= 300000).show()
>
+-------------------+-------+-------+-------+-------+------+
|               Date|   Open|   High|    Low|  Close|Volume|
+-------------------+-------+-------+-------+-------+------+
|2016-02-26 00:00:00|1180000|1187000|1172000|1172000|176906|
```

##### 날짜가 2월 26일 것만 가져온다면?

```python
orders.filter(orders['Date']=='2016-02-26').show()
>
+-------------------+-------+-------+-------+-------+------+
|               Date|   Open|   High|    Low|  Close|Volume|
+-------------------+-------+-------+-------+-------+------+
|2016-02-26 00:00:00|1180000|1187000|1172000|1172000|176906|
+-------------------+-------+-------+-------+-------+------+
```

### titanic_train [실습]

```python
titanic = sqlCtx.read.csv('./data/titanic_train.csv',header=True,inferSchema=True)
titanic.printSchema()
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

- 타이타닉 데이터를 불러온다.

##### count()

```python
titanic.count()
>
891
```

- 행의 개수를 확인

##### select 변수 선택 : Passengerid, Name

```python
titanic.select(['PassengerId','Name']).show()
>
+-----------+--------------------+
|PassengerId|                Name|
+-----------+--------------------+
|          1|Braund, Mr. Owen ...|
```

##### 성별에서 여성인 PassengerId, Name, Sex, Survived 출력

```python
titanic.filter(titanic['Sex']=='female').select(['PassengerId','Name','Sex','Survived']).show()
>
+-----------+--------------------+------+--------+
|PassengerId|                Name|   Sex|Survived|
+-----------+--------------------+------+--------+
|          2|Cumings, Mrs. Joh...|female|       1|
```

##### 성별에서 여성이면서 살아있는 사람의 PassengerId, Name, Sex, Survived 출력

```python
titanic.filter((titanic['Sex']=='female') & (titanic['Survived']==1)).select(['PassengerId','Name','Sex','Survived']).show()
>
+-----------+--------------------+------+--------+
|PassengerId|                Name|   Sex|Survived|
+-----------+--------------------+------+--------+
|          2|Cumings, Mrs. Joh...|female|       1|
```

- 이렇게 조건을 여러개 줄 수 있다.
- filter(lambda)에서 filter는 조건식을 담을 수 있기에 lambda는 함수이므로 적용이 안 된다.