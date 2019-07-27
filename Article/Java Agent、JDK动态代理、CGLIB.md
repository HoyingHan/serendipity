### Java Agent、JDK动态代理、CGLIB



#### Java Agent

之所以想把这几种技术做个总结，是因为很多框架，工具的底层实现都是通过他们，例如Java Agent用来热部署（JRebel），线上的监控和诊断工具（BTrace）等；Spring AOP，Dubbo的实现也都用到了动态代理。



Java Agent: 最简单的理解方式，是将它运行在 main方法之前的拦截器（premain()）。

优点：独立于应用程序构建

应用：

1. 监测运行在JVM上的程序
2. 动态修改和替换类











#### 接下来要理清几个概念的关系

Java Agent

Javassist

Java探针

Instrumentation





##### Java Agent 与 Instrumentation

大部分情况，将两者结合在一起应用：Instrumentation为JVM上运行时的程序提供测量手段；Java Agent作为单独jar包部署，做到代码无侵入。



##### Java Agent+ASM字节码  ---> Java探针



Instrumentaion：为JVM上运行时的程序提供测量手段，一些工具都是通过Instrumentaion对字节码进行修改，达到收集数据的目的。



Instrumentation 的最大作用，就是类定义动态改变和操作。



Instrumentaion Agent 

有两种加载方式，

1. premain(启动前加载):-javaagent:myagent.jar的方式来间接加载 instrument agent
2. agentmain（启动后加载）：依赖 JVM 的 attach 机制



























#### 三者比较

JDK动态代理是基于接口的，JDK 会动态生成实现类的字节码，加载字节码生成相应的类。

CGLI 是基于继承实现代理机制的，所以其不需要接口，CGLIB 底层使用的 ASM 生成一个代理目标类的子类，来实现代理功能

Java Agent 是通过字节码修改实现的，但是Java Agent 没有创建新的Class 而是直接修改了原始Class



三者的不同主要体现在实现方式上：

Java Agent：修改字节码——不创建新类，修改原有类

动态代理：创建新的代理类

- JDK：基于接口——动态生成实现类
- CGLIB：基于继承——动态生成子类







------

javassist 

 ASM