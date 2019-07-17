## 【Spark编程模型】RDD



###  概念

##### 简介

RDD：弹性分布式数据集，对分布式内存的抽象

Spark API 的所有操作都是基于 RDD 的

目的：为了减少网络及磁盘 IO 开销



##### 特性 

1. 由一系列partition组成，partition分布在不同的节点上
2. 存在一系列的依赖关系
3. 函数是作用在每一个partition（split）上的
4. 分区器是作用在K,V格式的RDD上
5. 提供一系列最佳的计算位置







### 操作类型（算子）



##### 创建

1. 并行化集合创建

   `val distData = sc.parallelize(Array(1,2,3,4))`

2. 



##### 转换操作（延迟执行）

- 对单条Record
  1. filter
  
     `sc.parallelize(Array(1,2,3,4)).filter(_ > 1).collect()`
  
     `res1: Array[Int] = Array(2, 3, 4)`
  
  2. map
  
     `sc.parallelize(Array(1,2,3,4)).map(x=>x+1).collect()`
  
     `res2: Array[Int] = Array(2, 3, 4, 5)`
  
  3. flatMap
  
     `sc.parallelize(Array(1,2,3,4)).flatMap(x=> Array(x+1,x+10)).collect()`
  
     `res5: Array[Int] = Array(2, 11, 3, 12, 4, 13, 5, 14)`



- 对分区shuffle
  1. reduceByKey：按照key分组然后聚集
  
     `sc.parallelize(Array( "a" -> 1,"a" ->2,"a" ->3, "b" -> 4)).reduceByKey((x,y)=> x+y).collect`
  
     `res1: Array[(String, Int)] = Array((a,6), (b,4))`
  
     
  
  2. groupByKey：针对(K, V)类型，返回(K, Iterable<V>)类型的数据集
  
     `sc.parallelize(Array( "a" -> 1,"a" ->2, "b" -> 3))`
  
     `res2: Array[(String, Iterable[Int])] = Array((a,CompactBuffer(1, 2)), (b,CompactBuffer(3)))`
  
  3. sortByKey
  
  4. join
  
  5. union
  
  6. mapPartitions
  
     `sc.parallelize(Array(1,2,3,4)).mapPartitions(x=> x.map(_+1)).collect()`
  
     `res1: Array[Int] = Array(2, 3, 4, 5)  `
  
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