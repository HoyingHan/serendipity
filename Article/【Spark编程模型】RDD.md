## 【Spark编程模型】RDD



###  概念

##### 简介

RDD：

分布式对象集合，本质上提供了一种高度受限的共享内存模型，即RDD是只读的记录分区的集合，不能直接修改

弹性分布式数据集，对分布式内存的抽象

Spark API 的所有操作都是基于 RDD 的

目的：为了减少网络及磁盘 IO 开销



##### 概念解读

1. 由一系列partition组成，partition分布在不同的节点上
2. 存在一系列的依赖关系
3. 函数是作用在每一个partition（split）上的
4. 分区器是作用在K,V格式的RDD上
5. 提供一系列最佳的计算位置



##### 特性

高效计算

1. 容错性 - 血缘关系：血缘关系、重新计算丢失分区、无需回滚系统、重
   算过程在不同节点之间并行、只记录粗粒度的操作
2. 避免磁盘读写：中间结果持久化到内存
3. 避免序列化和反序列化：可存放Java对象



##### 典型执行过程

•RDD读入外部数据源进行创建
•RDD经过一系列的转换（Transformation）操作，每一次都会产生不同的
RDD，供给下一个转换操作使用
•最后一个RDD经过“动作”操作进行转换，并输出到外部数据源，这一系列处理称为一个Lineage（血缘关系），即DAG拓扑排序的结果



### 操作类型（算子）



##### 创建

1. 并行化集合创建

   ```scala
   val distData = sc.parallelize(Array(1,2,3,4))
   ```

   

2. 



##### 转换操作（延迟执行）

- 对单条Record
  1. filter

     ```scala
     sc.parallelize(Array(1,2,3,4)).filter(_> 1).collect()
     res1: Array[Int] = Array(2, 3, 4)
     ```

     

  2. map

     ```scala
     sc.parallelize(Array(1,2,3,4)).map(x=>x+1).collect()
     res2: Array[Int] = Array(2, 3, 4, 5)
     ```

  3. flatMap

     ```scala
     sc.parallelize(Array(1,2,3,4)).flatMap(x=> Array(x+1,x+10)).collect()
     res5: Array[Int] = Array(2, 11, 3, 12, 4, 13, 5, 14)
     ```

     

- 对分区shuffle
  1. reduceByKey：按照key分组然后聚集，返回一个新的(K, V)形式的数据集，其中的每个值是将每个key传递到函数func中进行聚合

     ```scala
     sc.parallelize(Array( "a" -> 1,"a" ->2,"a" ->3, "b" -> 4)).reduceByKey((x,y)=> x+y).collect()
     res1: Array[(String, Int)] = Array((a,6), (b,4))
     ```

     

     

  2. groupByKey：针对(K, V)类型，返回(K, Iterable<V>)类型的数据集

     ```scala
     sc.parallelize(Array( "a" -> 1,"a" ->2, "b" -> 3))
     res2: Array[(String, Iterable[Int])] = Array((a,CompactBuffer(1, 2)), (b,CompactBuffer(3)))
     ```

     

  3. sortByKey

  4. join

  5. union

  6. mapPartitions

     ```scala
     sc.parallelize(Array(1,2,3,4)).mapPartitions(x=> x.map(_+1)).collect()
     res1: Array[Int] = Array(2, 3, 4, 5) 
     ```

  7. repartition

  8. repartitionAndSortWithinPartitions



##### 控制（持久化）

1. cache
2. persist
3. checkpoint

##### 行动

1. count
2. take
3. first
4. foreach
5. collect：拉取rdd中所有数据到driver节点，转换成一个数组，如果数据量过大，会造成driver的OOM