### Java Agent、JDK动态代理、CGLIB



Java Agent: 很多工具都是通过它来实现，例如热部署，线上诊断工具等。对于Java Agent最简单的理解方式，是将它运行在 main方法之前的拦截器。









JDK动态代理是基于接口的，JDK 会动态生成实现类的字节码，加载字节码生成相应的类。

CGLI 是基于继承实现代理机制的，所以其不需要接口，CGLIB 底层使用的 ASM 生成一个代理目标类的子类，来实现代理功能

Java Agent 是通过字节码修改实现的，但是Java Agent 没有创建新的Class 而是直接修改了原始Class



三者的不同主要体现在实现方式上：

Java Agent：修改字节码——不创建新类，修改原有类

动态代理：创建新的代理类

- JDK：基于接口——动态生成实现类
- CGLIB：基于继承——动态生成子类





Java Agent+ASM字节码  ---> Java探针



Instrumentaion：为JVM上运行时的程序提供测量手段，一些工具都是通过Instrumentaion对字节码进行修改，达到收集数据的目的。



Instrumentaion Agent 





关于Java Agent，