## 【Spark编程模型】RDD



###  概念

##### 简介

RDD：弹性分布式数据集



##### 特性 

1. 由一系列partition组成，partition分布在不同的节点上
2. 存在一系列的依赖关系
3. 函数是作用在每一个partition（split）上的
4. 分区器是作用在K,V格式的RDD上
5. 提供一系列最佳的计算位置







### 操作类型（算子）



##### 创建





##### 转换（延迟执行）

- 基本操作
  1. filter
  2. map
  3. flatMap
- 键值对（Pair RDD）
  1. reduceByKey
  2. groupByKey
  3. sortByKey
  4. join
  5. union
  6. mapPartitions



##### 控制（持久化）

1. cache
2. persist
3. checkpoint

##### 行动

1. count
2. take
3. first
4. foreach
5. collect