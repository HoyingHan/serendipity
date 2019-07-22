## Spark SQL

### 简介

- 处理结构化数据的Spark组件
- 增加了SchemaRDD，数据既可以来自RDD，也可以是Hive、HDFS、Cassandra等外部数据源，还可以是JSON格式的数据
- 

### DataFrame

 - 好处
   1. 精简代码
   2. 提升执行效率 - 重用对象
   3. 减少数据读取
 - 



### SQLConsole

RDD -> DataFrame :  

1. **Case Class方式:** 利用反射推断Schema
2. **createDataFrame方式：**
   1. 从源RDD创建rowRDD
   2. 创建于源RDD匹配的Schema
   3. 通过createDataFrame应用到rowRDD





读取数据

1. JSON格式
2. 