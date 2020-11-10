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

