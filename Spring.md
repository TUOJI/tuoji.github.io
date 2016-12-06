#Spring笔记：
**一句话概括：通过DI,AOP,样板式代码来简化java开发。**
##DI
###DI原理：
通过应用上下文(Application Context)把Bean装载和组装起来                                                  
###DI方式：
1.构造器注入（constructor injection）；
2.seter方法注入；
###DI目的：（解决组件之间的耦合）
1.松耦合；


###装配(wiring)：
###装配方式：
1.xml；
2.注解


##AOP
###AOP目的：（解决任务之间的耦合）
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
###Spring 模块


###数据库类型
1.传统关系型数据库
2.Nosql数据库，例如：mongoDB,Neo4J







##装配Bean


####装配（wiring）：创建对象之间的关系,是DI的本质
####装配方式：
######1.XML显示方式；
######2.Java Config显示方式；(多用于使用第三方类的时候装配)
######3.组件扫描（Component Scanning） 和 自动装配(autoWiring)  推荐方式；

#####*tips: @Configeration注解 ：表示使用JavaConfig配置的方式进行装配*


####高级装配：
#####profiles：用于不同环境指定不同的Bean装配;
#######设置值：spring.profiles.active;spring.profiles.default;
#######设置方法：(有多个则使用逗号隔开)
>dispatcherservlet初始化参数；
>
>web应用上下文参数
>
>JNDI条目
>
>环境变量
>
>JVM系统参数
>
>测试类上使用@ActiveProfiles

#####条件化Bean : @Conditional注解
#####歧义处理方法：
>定义首选项（primary）
>
>限定符（qolifier）（*更强大*）:每个Bean默认以类名为限定符，如果自定义，建议使用特征性或者描述性的术语，不建议直接名称；任然有冲突时可使用多个限定符；但是Java不支持同时使用多个相同的注解，解决办法是定义自定义的注解；


#####作用域：使用@Scope指定，*tips:其属性proxyMode用于解决不同域的依赖注入*
* sigleton(默认)
* Prototype
* Session
* Request

#####运行时值注入：注入方式:
* 属性占位符（Property placeholder）:
使用方法：@Value注解 + ${}； 原理：属性加载到环境变量，再获取；依赖：需要placeholder:可通过<context:property-placeholder>创建；

* Spring表达式语言（SPEL）：
使用方法：#{} 语法：*待补充*


##Spring AOP
*待定*

##Spring Web
####跳转逻辑：
模型的key会根据返回值的类型得出（例：返回List<User>  ----> 模型名为userList ）
逻辑视图名称可以指定，默认跟请求路径名相同
####入参值校验：
加属性注解：例如：@NotNull;
入参Bean加注解：@Valid;

##Spring视图
####视图解析器可选方案：例如：jsp,tiles,thymeleaf;
##Spring高级技术
####文件上传；
####异常处理：
* 控制器级别异常处理：@ExceptionHandler(作用范围：整个控制器)
* 全局级别异常处理：@ControllerAdvice 即通知(作用范围：所有带有@RequestMapping的控制器)

####重定向值传递：
* String类型传递；（局限：不安全，可能携带SQL）
* 路径变量或者查询参数；（局限：只能传递原始数据类型，不能传递对象类型）
* 放到session中；(局限：需要手动删除)
* flash属性：RedirectAttributes类addFlashAttributes()方法；（原理：先放到回话，再从会话中读取）

##Spring Web Flow

####流程注册：<flow:flow-registry>
##Spring Security
####原理：基于Spring AOP 和 Servlet filter实现；
####配置方式：@EnableWebMvcSecurity 并且实现WebSecurityConfigurerAdapter

##Spring 数据库连接：
####数据源连接方式：
* JNDI
* 连接池
* JDBC
* 测试数据配置：嵌入式数据源：<jdbc:embeded-dataSource>
数据源选择：Profile;

##Spring 对象-关系映射持久化数据（ORM）
##Spring 使用NoSQL数据库
1. Mongo DB;
2. Neo4J；
3. Redis(6379端口);


##Spring cache
* ehcache
* Redis
##Spring Security
* @Secured
* @RolesAllowed
* 表达式注解:@PreAuthorise,@PostAuthorise,@PreFilter,@PostFilter;

##Spring 使用远程服务
使用服务导出器：

* RMI(协议：JRMP；消息格式：二级制；端口：默认1099 ；导出类：RmiServieExporter)  
缺点：1.可使用任意端口，难穿越防火墙，内部网络没限制；2，RMI基于Java，限制客户端和服务端都使用Java；优点：使用Java标准的对象序列化机制，可支持复杂的数据模型；
* Hessian(协议：HTTP;消息格式：二进制;优点：可非Java平台，有带宽优势；缺点：私有的对象序列化机制，不支持复杂的数据模型) 
* Burlab(协议：HTTP;消息格式：xml;优点：可非Java平台，有可读性优势；缺点：私有的对象序列化机制，不支持复杂的数据模型）
* Spring HttpInvoker（综合前两者的优势）
* JAX-WS(协议：soap)

##Spring MVC创建Rest API
区分：
SOA:关注行为和动作
REST:关注资源数据

####rest概念：
* 1.资源协商描述ContentNegotiatingViewResolver:（有缺陷，不建议使用）
工作原理：
**1.决定请求媒体类型（默认）：**a,先查看请求URL扩展名；b,再查看请求头ContentType;c,以/作为请求类型，表示客户端接受任何类型的媒体类型；
**2.决定请求媒体类型（配置）:通过装配ContentNegotiatingManager来指定**

* http信息转换器：（只需要类路径下有相关的转换器包即可，不需要额外的配置，例如Json:MappingJackson2HttpMessageConverter类）
注解：@RequestMapping--->produces/consume属性(指定接受类型)；@ResponseBody（指定响应类型）；@RequestController指定全局数据格式（作用域为单个Controller）; @ResponseStatus指定状态；使用ResponseEntity来封装更多响应内容；使用@ExceptioHandler来捕获详细信息；

####客户端：
######两种请求方式：
* Apache HttpClient
* Rest Template

##Spring 消息

消息的两种目的地：点对点模型（队列）& 发布/订阅模型（主题）

####消息发送/接收的两种方式：
1. JMS(java message serice)
	1.模板JmsTemplate;（确缺点：发送异步，但是接收（receive方法）同步） 2.消息驱动pojo（配置消息监听器）; 3.基于消息的RPC（JmsInvokerServiceExporter用于导出；JmsInvokerProxyFactoryBean用于提供给客户端使用）；

2. AMQP：（优势：1.其线路层协议规范了消息格式，使得可以跨平台或者跨语言使用；2.具有更灵活和透明的消息模型（不局限于队列和订阅），通过Exchange指定）




##使用JMX管理 Spring Bean
作用：

管理，监视，配置应用；

* JMX： （java manage extensions）管理暴露了特定方法的Bean；

##Spring Boot

### Spring Boot Stater
* 原理：将项目所需的所有依赖聚合成一项依赖（利用传递依赖）

### 自动配置
* 原理：通过扫描类路径自动配置相关的Bean
### 命令行接口（CLI）
* 使用：（结合groovy使用）
### Actuator
* 特性：（管理端点）






















