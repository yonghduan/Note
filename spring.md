## Spring

### 步骤

> 1. 导入Spring开发的基本包坐标
> 2. 编写接口和实现类
> 3. 创建Spring核心配置文件
> 4. 在Spring配置文件中配置接口的实现
> 5. 使用Spring的API获得Bean实例

### Spring配置文件

### Bean标签配置

* 用于配置对象交由spring来创建，默认情况下调用无参构造函数，所以得保证类中含有无参构造函数
* scope属性，默认为singleton，即spring容器中只含有一个对象，当属性为prototype时，springBean容器中含有多个对象

> 当属性为singleton时，bean创建时机是xml配置文件被加载时，它的生命周期是创建容器时，就被创建了，销毁容器时，对象被销毁
>
> 多属性为prototype时，实例化时机是调用getBean方法时，对象运行，则对象一直存在，当对象长时间不用时，被java垃圾回收器回收

* Bean生命周期配置

> init-method,destroy-method可以配置初始化方法和销毁方法

Bean实例化三种方式

### Bean的依赖注入方式

> 假设spring容器中有两个对象，对象1依赖于对象2，而在程序代码中只关注对象1，那么可以在spring配置中将对象2配进对象1之中

依赖注入，它是spring框架的IOC的具体实现。在编写程序时，通过控制反转，把对象的创建权交给spring，但是代码中不可能出现没有依赖的情况。ioc解耦只是降低了他们的依赖关系，但不会消除，例如业务成仍然会调用持久层的方法，这种业务层和持久层的依赖关系，在使用spring之后，就交给spring来维护了。简单的说，就是等框架把持久层对象传入业务层，而不用自己去获取

实现依赖注入方式

> 1. set方法，在spring配置中使用property标签
> 2. 构造方法，在bean标签内部使用<constructor-arg>标签进行配置

注入的数据类型：普通数据类型，引用数据类型，集合数据类型

实际开发中，spring配置文件非常多，可以将部分配置拆解到其他配置文件中，在主配置文件中通过import标签进行加载

### Spring相关api

### Spring配置数据源

配置一些非自定义的bean

数据源（连接池）作用

数据源开发步骤：

> 导入数据源的坐标和数据库驱动坐标
>
> 创建数据源对象
>
> 设置数据源的基本连接数据
>
> 使用数据源获取连接资源和归还资源

### Spring注解开发

Spring原始注解：spring是轻代码而重配置的框架，配置比较繁重，影响开发效率，所以注解开发是一种趋势，注解代替xml配置文件可以简化配置，提高开发效率，spring原始注解主要代替Bean标签配置

> @Component:使用在类上用于实例化Bean
>
> @Controller：在web层实例化用于实例化Bean
>
> @Service：在service层上实例化Bean
>
> @Repository：在dao层上实例化Bean
>
> @Autowired：按照类型从spirng容器中进行匹配
>
> @Qualifier：按照id值进行匹配的，但要与Autowired结合使用
>
> @Resource：相当于Autowired和Qualifier的结合
>
> @Value：给成员赋值
>
> @Scope：配置单例还是多例
>
> @PostConstruct：配置初始化方法
>
> @PreDestroy：配置结束方法

使用注解进行开发时，要进行组件扫描，告诉spring扫描注解,使用标签为<context:component-scan base-package="">

Spring新注解：一些不是自定义类的配置，组件扫描，加载property配置文件，import标签等，无法用注解代替，新注解则是解决这些问题

> @Configuration：用于指定当前类是一个spring配置类，当创建容器时会从该类上加载注解
>
> @ComponentScan：用于指定spring在初始化容器时要扫描的包，作用和xml中的组件扫描标签一样
>
> @Bean：使用在方法上，标注该方法的返回值存储到spring容器中
>
> @PropertySource：用于加载.properties文件中的配置
>
> @Import：用于导入其他配置类

### Spring与web环境集成

### SpringMVC

springmvc是一种基于java实现的请求驱动类型的轻量级web框架，属于SpringFrameWork的后续产品，它通过一套注解，让一个简单的java类成为处理请求的控制器，而无需实现任何借口

开发步骤：

> 1.导入SpringMVC包
>
> 2.配置servlet
>
> 3.编写POJO
>
> 4.将controller使用注解配置到Spring容器中（@Controller）
>
> 5.配置spring-xml文件（配置组件扫描）