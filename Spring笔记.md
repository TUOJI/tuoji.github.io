#Spring笔记：
**一句话概括：通过DI,AOP,样板式代码来简化java开发。**
##DI
###DI原理：
通过应用上下文(Application Context)把Bean装载和组装起来                                                  
###DI方式：
1.构造器注入（constructor injection）：
###DI目的：
1.松耦合；


###装配(wiring)：
###装配方式：
1.xml；
2.注解


##AOP
###AOP目的：
1.关注点的分离，实现可重用（例如：日志，事务，安全etc）,这类组件称为横切关注点，通常跨越系统多个组件；
2.使用声明的方式应用到服务，而不是常规的调用，使得业务与业务核心逻辑分离；
###AOP语法：
AspectJ

##Spring容器
###作用：
1.负责创建，装配，配置，并且管理生命周期（new ----> finalize）
###容器类型：
1.Bean工厂：BeanFactory类；（一般不使用）
2.应用上下文：ApplicationContext类；
###容器中Bean的生命周期
（待补充）




































