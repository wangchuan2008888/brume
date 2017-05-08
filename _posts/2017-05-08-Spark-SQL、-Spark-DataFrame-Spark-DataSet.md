# Overview
Spark SQL是用来处理结构化数据的模块，Spark SQL提供了结构化信息，为Spark实现加速提供了可能性。
# SQL
Spark SQL一个重要的应用就是执行SQL查询，当时用SQL查询时，便返回一个DataSet/DataFrame
# DataSet and DataFrames
DataSet是在Spark1.6加入的新特性，提供了情类型的Spark SQL支持，这变为Spark SQL进行加速提供了可能。DataSet可以从map、flatMap、filter等操作创建。
DataFrame是讲DataSet以columns的形式进行组织，其意义等同于关系型数据库里的表，但是可以进行更多的优化。DataFrame可以从更为广泛的来源创建，例如structured data files, tables in Hive, external databases, or existing RDDs。其实是DataSet里存储了一个Row对象。
>  DataFrame与Dataset的根本区别在于DataSet里面保存了每个值域的信息，但是DataFrame里面保存的Row，丢失了类型信息。所以DataSet可以在编译时进行类型检查。

# DataFrame和DataSet相互转化
* 反射方式推断schema
``` scala
  import org.apache.spark.sql.catalyst.encoders.ExpressionEncoder
  import org.apache.spark.sql.Encoder

  // For implicit conversions from RDDs to DataFrames
  import spark.implicits._

  // Create an RDD of Person objects from a text file, convert it to a Dataframe
  val peopleDF = spark.sparkContext
    .textFile("examples/src/main/resources/people.txt")
    .map(_.split(","))
    .map(attributes => Person(attributes(0), attributes(1).trim.toInt))
    .toDF()
  // Register the DataFrame as a temporary view
  peopleDF.createOrReplaceTempView("people")

  // SQL statements can be run by using the sql methods provided by Spark
  val teenagersDF = spark.sql("SELECT name, age FROM people WHERE age BETWEEN 13 AND 19")

  // The columns of a row in the result can be accessed by field index
  teenagersDF.map(teenager => "Name: " + teenager(0)).show()
  // +------------+
  // |       value|
  // +------------+
  // |Name: Justin|
  // +------------+

  // or by field name
  teenagersDF.map(teenager => "Name: " + teenager.getAs[String]("name")).show()
  // +------------+
  // |       value|
  // +------------+
  // |Name: Justin|
  // +------------+

  // No pre-defined encoders for Dataset[Map[K,V]], define explicitly
  implicit val mapEncoder = org.apache.spark.sql.Encoders.kryo[Map[String, Any]]
  // Primitive types and case classes can be also defined as
  // implicit val stringIntMapEncoder: Encoder[Map[String, Any]] = ExpressionEncoder()

  // row.getValuesMap[T] retrieves multiple columns at once into a Map[String, T]
  teenagersDF.map(teenager => teenager.getValuesMap[Any](List("name", "age"))).collect()
  // Array(Map("name" -> "Justin", "age" -> 19))
```
* 通过编写胆码的方式创建schema
``` scala
  import org.apache.spark.sql.types._

  // Create an RDD
  val peopleRDD = spark.sparkContext.textFile("examples/src/main/resources/people.txt")

  // The schema is encoded in a string
  val schemaString = "name age"

  // Generate the schema based on the string of schema
  val fields = schemaString.split(" ")
    .map(fieldName => StructField(fieldName, StringType, nullable = true))
  val schema = StructType(fields)

  // Convert records of the RDD (people) to Rows
  val rowRDD = peopleRDD
    .map(_.split(","))
    .map(attributes => Row(attributes(0), attributes(1).trim))

  // Apply the schema to the RDD
  val peopleDF = spark.createDataFrame(rowRDD, schema)

  // Creates a temporary view using the DataFrame
  peopleDF.createOrReplaceTempView("people")

  // SQL can be run over a temporary view created using DataFrames
  val results = spark.sql("SELECT name FROM people")

  // The results of SQL queries are DataFrames and support all the normal RDD operations
  // The columns of a row in the result can be accessed by field index or by field name
  results.map(attributes => "Name: " + attributes(0)).show()
  // +-------------+
  // |        value|
  // +-------------+
  // |Name: Michael|
  // |   Name: Andy|
  // | Name: Justin|
  // +-------------+
```
http://www.jianshu.com/p/c0181667daa0
