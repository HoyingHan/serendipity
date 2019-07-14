## 【Spark】如何用Scala写Spark任务

#### 主要类

- **SparkContext：**Spark程序的起点
- **SparkConf：**SparkContext初始化时，需要初始化一个SparkConf对象，包含了Spark集群的各种参数
- **SparkSession：** Spark2.0引入，封装了SparkContext，SparkConf，SQLContext
- **DataFrame：**分布式的Row对象的集合，更重要的特点是提升执行效率
- **DataSet：**Dataset可以认为是DataFrame的一个特例，主要区别是Dataset每一个record存储的是一个强类型值而不是一个Row





#### 一些优化调参

```Scala
    spark.conf.set("spark.executor.memory", "4g")
    spark.conf.set("spark.yarn.executor.memoryOverhead", "40960")
    spark.conf.set("spark.executor.cores", "1")
    //    spark.conf.set("spark.memory.storageFraction", "0.35")
    spark.conf.set("spark.sql.small.file.combine","true")
    //    spark.conf.set("spark.default.parallelism","100")
    spark.conf.set("yarn.nodemanager.vmem-check-enabled","false")
    spark.conf.set("spark.sql.shuffle.partitions","3000")
```





#### Spark架构

1. 集群资源管理器 - Cluster Manage
2. 工作节点 - 运行作业任务 - Worker Node
   - 具体任务的执行进程 - Executor
     - 存储模块 - BlockManager - 存放中间结果
3. 应用的任务控制节点 - Driver