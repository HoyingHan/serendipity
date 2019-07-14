## 【Spark】运行原理



spark-submit

会启动一个对应的Driver进程

Driver进程：

- 向集群管理器申请资源（资源 = Executor进程）
- 调度和执行代码
  1. 拆分为多个Stage
  2. 为每个Stage创建一批Task
  3. 分配到各个Executor进程



【如何划分Stage】

- 根据shuffle类算子