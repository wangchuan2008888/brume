# Spark中的序列化
>  Spark是基于内存的计算框架，但是分布式的系统中，序列化与反序列化是分布式系统中通信必经的过程。本文描述Spark的序列化机制。

## 支持的序列化机制
* java Serilization：这是Spark默认的序列化机制。Java的序列化机制，需要自定义类实现Seriizable接口，如有需要可定义writeObject和readObject类，具体可参考Java的ArrayList之类的序列化实现。
* Kryo：据官方描述，该框架的序列化速度与序列化之后的数据体积比Java本身的序列化框架快10倍（这年头不比它快10倍都不好意思说）。

## Kryo使用
在创建SparkConf时首先改变序列化框架，之后还要把序列化的类注册到Kryo中去。
```java

  conf.set("spark.serializer","org.apache.spark.serializer.KryoSerializer").
  conf.registerKryoClasses(Array(classOf[MyClass1], classOf[MyClass2]))
  val sc = new SparkContext(conf)
```
工作线程切换，以后再补充




http://spark.apache.org/docs/1.2.1/tuning.html
