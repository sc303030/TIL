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

##### 선실등급 별 요금 평균

```python
titanic.groupBy(['Pclass']).avg('Fare').show()
>
+------+------------------+
|Pclass|         avg(Fare)|
+------+------------------+
|     1| 84.15468749999992|
|     3|13.675550101832997|
|     2| 20.66218315217391|
+------+------------------+
```

- 그룹을 묶어서 평균을 구한다.

- 타입이 그룹데이터여서 sort는 못한다.

```python
titanic.groupBy(['Pclass']).avg('Fare').sort('avg(Fare)',ascending=False).show()
>
+------+------------------+
|Pclass|         avg(Fare)|
+------+------------------+
|     1| 84.15468749999992|
|     2| 20.66218315217391|
|     3|13.675550101832997|
+------+------------------+
```

- 이렇게 체인으로 연결하면 가능하다.

### Spark DB연동

- 벤더사에서 제공하는 플러그인 모듈을 세팅해야한다.

```python
conf  = SparkConf().setMaster('local').setAppName('sparkApp').set("spark.driver.extraClassPath", "../data/ojdbc6.jar")
spark = SparkContext(conf=conf)
```

```python
from pyspark.sql import SQLContext
sqlCtx = SQLContext(spark)
spark = sqlCtx.sparkSession
spark
```

```python
sql_url = "localhost"
user = "hr"
password = "hr"
database = "xe"
table = "SPARK_TITANIC"
```

```python
jdbc = spark.read.format("jdbc")\
                .option("driver", "oracle.jdbc.driver.OracleDriver")\
                .option("url", "jdbc:oracle:thin:@{}:1521:{}".format(sql_url, database))\
                .option("user", user)\
                .option("password", password)\
                .option("dbtable", table)\
                .load()
jdbc.show()
```

```python
jdbc.createOrReplaceTempView('titancc') #sql의 뷰처럼 만들어서 이름을 titanic로 부여
sqlCtx.sql('select * from titanic where sex = "female"').show()
```

- 이렇게 연동하면 sql구문을 바로 사용할 수 있다.

---

#### CSV파일로도 가능하다.

```python
titanic.createOrReplaceTempView('titanccView') #sql의 뷰처럼 만들어서 이름을 titanic로 부여
sqlCtx.sql('select * from titanccView where sex = "female"').show()
>
+-----------+--------+------+--------------------+------+----+-----+-----+----------------+--------+-----+--------+
|PassengerId|Survived|Pclass|                Name|   Sex| Age|SibSp|Parch|          Ticket|    Fare|Cabin|Embarked|
+-----------+--------+------+--------------------+------+----+-----+-----+----------------+--------+-----+--------+
|          2|       1|     1|Cumings, Mrs. Joh...|female|38.0|    1|    0|        PC 17599| 71.2833|  C85|       C|
```

- 위에서 만들었던 titanic파일로 뷰를 만들어서 sql구문을 사용하였다.

#### Spark SQL

```python
import pandas as pd
```

```python
data1 = {'PassengerId':{0:1, 1:2, 2:3, 3:4, 4:5},
         'Name' : {0:'Owen', 1:'Florence', 2:'Laina', 3:'Lily', 4:"William"},
         'sex' : {0: 'male', 1: 'female', 2:'female', 3:'female', 4:'male'},
         'Survived': {0:0, 1:1, 2:1, 3:1, 4:0}
        }

data2 = {'PassengerId':{0:1, 1:2, 2:3, 3:4, 4:5},
         'Age' : {0: 22, 1: 38, 2: 33, 3: 35, 4: 35},
         'Fare' : {0: 7.3, 1: 71.3, 2:7.9, 3:53.1, 4:8.0},
         'Pclass': {0:3, 1:1, 2:3, 3:1, 4:3}
        }
```

#### pandas df -> spark df

```python
sample_df01 = pd.DataFrame(data1, columns=data1.keys())
sample_df02 = pd.DataFrame(data2, columns=data2.keys())
```

- 컬럼명을 가져와 합칠 기준을 만든다.

```python
spark_df01 = sqlCtx.createDataFrame(sample_df01)
spark_df02 = sqlCtx.createDataFrame(sample_df02)
>
root
 |-- PassengerId: long (nullable = true)
 |-- Name: string (nullable = true)
 |-- sex: string (nullable = true)
 |-- Survived: long (nullable = true)
```

- 데이터프레임으로 만들어서 합친다.

#### mirroring - view와 비슷한 기능

```python
spark_df01.createOrReplaceTempView('titanic01')
spark_df02.createOrReplaceTempView('titanic02')
```

#### Spark SQL SELECT

```python
sqlCtx.sql('select * from titanic01').show()
>
+-----------+--------+------+--------+
|PassengerId|    Name|   sex|Survived|
+-----------+--------+------+--------+
|          1|    Owen|  male|       0|
|          2|Florence|female|       1|
|          3|   Laina|female|       1|
|          4|    Lily|female|       1|
|          5| William|  male|       0|
+-----------+--------+------+--------+
```

```python
sqlCtx.sql('select * from titanic01 where sex =="male"').show()
>
+-----------+-------+----+--------+
|PassengerId|   Name| sex|Survived|
+-----------+-------+----+--------+
|          1|   Owen|male|       0|
|          5|William|male|       0|
+-----------+-------+----+--------+
```

##### 성별에 따른 생존자 수를 구해본다면?

```python
sqlCtx.sql('''select sex,count(Survived)  as cnt
              from titanic01  
              where Survived = 1
              group by sex''').show()
>
+------+---+
|   sex|cnt|
+------+---+
|female|  3|
+------+---+
```

##### PannengerId를 기준으로 합쳐서 성별과 선실을 기준으로 Fare의 평균

```python
sqlCtx.sql('''select one.sex, two.Pclass, avg(two.Fare)
              from titanic01 one, titanic02 two
              where one.PassengerId == two.PassengerId
              group by one.sex, two.Pclass''').show()
>
+------+------+---------+
|   sex|Pclass|avg(Fare)|
+------+------+---------+
|female|     1|     62.2|
|  male|     3|     7.65|
|female|     3|      7.9|
+------+------+---------+
```