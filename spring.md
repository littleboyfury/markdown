# 一、Spring框架功能整体介绍

![image-20200220164430720](spring.assets/image-20200220164430720.png)

## 1 Spring Core Container

​		模块作用：Core和Beans模块是框架的基础部分，提供IoC（反转控制）和DI（依赖注入）特性。这里的基础概念是BeanFactory，它提供对Factory模式的经典实现来消除对程序性单例模式的需要，并真正地运行你从程序逻辑中分离依赖关系和配置。

### 1.1 Core

​		主要包含 Spring 框架基本的核心工具类， Spring 的其他组件都要用到这个包 里的类， Core模块是其他组件的基本核心

### 1.2 Beans（BeanFactory的作用）

​		它包含访问配直文件、创建和管理 bean 以及进行 Inversion of Control(控制反转IoC)和Dependency Injection(依赖注入DI)操作相关的所有类

### 1.3 Context（处理BeanFactory）

​		模构建于 Core 和 Beans 模块基础之上，提供了一种类似JNDI 注册器的框架式的对象访问方法。Context模块继承了 Beans的特性，为Spring核心提供了大量扩展，添加了对国际化(例如资源绑定)、事件传播、资源加载和对Context的透明创建的支持。Context模块同时也支持J2EE的一些特性，ApplicationContext接口是Context模块的关键。

​		BeanFactory和ApplicationContext的**本质区别**：加载时期不同，使用BeanFacotry的bean是延时加载的，即用到了该Bean，才会加载Bean，ApplicationContext是非延时加载的，即系统启动的时候就回去加载所有的Bean，但是也可以指定为懒加载（lazy-init=true）。

### 1.4 Expression Language

​		模块提供了强大的表达式语言，用于在运行时查询和操纵对象。它是JSP 2.1规范中定义的unifed expression language的扩展。 该语言支持设直/获取属 性的值，属性的分配，方法的调用，访问数 组上下文( accessiong the context of arrays )、 容器和索引器、逻辑和算术运算符、命名变量以及从Spring的 IoC 容器中根据名称检索对象。它也支持 list 投影、选择和一般的 list 聚合。

## 2 Spring Data Access/Integration

### 2.1 JDBC

​		模块提供了一个 JDBC 抽象层，它可以消除冗长的 JDBC 编码和解析数据库厂商特有的错误代码。这个模块包含了 Spring 对 JDBC 数据访问进行封装的所有类。

### 2.2 ORM 模块为流行的对象-关系映射 API

​		如 JPA、 JDO、 Hibernate、 iBatis 等，提供了一个交互层。 利用 ORM 封装包，可以混合使用所有 Spring 提供的特性进行 O/R 映射， 如前边提到的简单声明性事务管理。

### 2.3 OXM 模块提供了一个对 ObjecνXML 映射实现的抽象层

​		Object/XML 映射实现包括 JAXB、 Castor、 XMLBeans、 JiBX 和 XStrearn

### 2.4 JMS ( Java Messaging Service )

​		模块主要包含了一些制造和消费消息的特性。

### 2.5 Transaction

​		支持编程和声明性的事务管理，这些事务类必须实现特定的接口，并且对所有的**POJO**都适用。

## 3 Spring web

​		Web 模块:提供了基础的面向 Web 的集成特性c。例如，多文件上传、使用 servlet listeners 初始化 IoC 容器以及一个面向 Web 的应用上下文。 它还包含 Spring 远程支持中 Web 的相关部分。

## 4 Spring Aop

### 4.1 Aspects 

​		模块提供了对 AspectJ 的集成支持。

### 4.2 Instrumentation 

​		模块提供了 class instrumentation 支持和 classloader 实现，使得可以在特定的应用服务器上使用

## 5 Test

​		Test 模块支持使用 JUnit 和 TestNG 对 Spring 组件进行测试

## 6 Spring 容器继承图

![image-20200220174025356](spring.assets/image-20200220174025356.png)

## 7 控制反转和依赖注入 

### 7.1 什么是控制反转?

​		我觉得有必要先了解软件设计的一个重要思想:依赖倒置原则(Dependency Inversion Principle )

1：什么是依赖倒置原则?

​		假设我们设计一辆汽车:先设计轮子，然后根据轮子大小设计底盘，接着根据底 盘设计车身，最后根据车身设计好整个汽车。这里就出现了一个“依赖”关系:汽车依赖车身，车身依赖 底盘，底盘依赖轮子

![image-20200220174109844](spring.assets/image-20200220174109844.png)

​		上图看上去没有什么毛病?但是 万一轮胎尺寸改了,那么地盘需要改，底盘改了，车身也改了，让后整个汽车构造都改了. 然后汽车公司倒闭了。

​		董事长依赖总经理挣钱，总经理依赖部门经理挣钱，部门经理依赖员工争取，那么员工离职了怎么办

​		反过来，假如汽车公司决定修改轮胎的我们就只需要改动轮子的设计，而不需要动底盘，车身，汽车的设计了。

![image-20200220174245059](spring.assets/image-20200220174245059.png)

IOC容器的最核心思想：ioc的思想最核心的地方在于，<font color=red>**资源不由使用资源的双方管理，而由不使用资源的第三方管理，**</font>这可以带来很多好处。

第一，资源集中管理，实现资源的可配置和易管理。

第二，降低了使用资源双方的依赖程度，也就是我们说的耦合度

# 二 Spring IOC 容器底层注解使用

## 1 xml配置文件的形式 VS 配置类的形式

### 1.1 基于xml的形式定义Bean的信息

```xml
 <?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/sp">
  <!--定义一个Bean的信息-->
	<bean id="car" class="com.tuling.compent.Car"></bean> 
</beans>
```

去容器中读取Bean

```java
public static void main( String[] args ) {
	ClassPathXmlApplicationContext ctx = new ClassPathXmlApplicationContext("beans.xml");
	System.out.println(ctx.getBean("person")); 
}
```

### 1.2 基于读取配置类的形式定义Bean信息

```java
@Configuration
public class MainConfig {
	@Bean
	public Person person(){
		return new Person(); 
  }
}
```

注意: 通过@Bean的形式是使用的话， bean的默认名称是方法名，若@Bean(value="bean的名称") 那么bean的名称是指定的

去容器中读取Bean的信息(传入配置类)

```java
public static void main( String[] args ) {
		AnnotationConfigApplicationContext ctx = new	AnnotationConfigApplicationContext(MainConfig.class);
		System.out.println(ctx.getBean("person")); 
}
```

### 1.3 在配置类上写@CompentScan注解来进行包扫描

```java
@Configuration
@ComponentScan(basePackages = {"com.tuling.testcompentscan"}) 
public class MainConfig {
}
```

* 排除用法 excludeFilters(排除@Controller注解的,和TulingService的)

```java
@Configuration
@ComponentScan(basePackages = {"com.tuling.testcompentscan"},excludeFilters = {@ComponentScan.Filter(type = FilterType.ANNOTATION,value = {Controller.class}),@ComponentScan.Filter(type = FilterType.ASSIGNABLE_TYPE,value = {TulingService.class}) })
public class MainConfig { }
```

* 包含用法 includeFilters ,注意，若使用包含的用法，需要把useDefaultFilters属性设置为false(true表示扫描全部的)

  useDefaultFilters表示是否使用默认的filter，扫描basePackages下的所有bean，生成只包含includeFilters中的bean

```java
@Configuration
@ComponentScan(basePackages = {"com.tuling.testcompentscan"},includeFilters = {@ComponentScan.Filter(type = FilterType.ANNOTATION,value = {Controller.class, Service.class}) },useDefaultFilters = false)
public class MainConfig {
}
```

* @ComponentScan.Filter type的类型

```java
public enum FilterType {
	//注解形式 比如@Controller @Service @Repository @Compent 
  ANNOTATION,
	//指定的类型 
  ASSIGNABLE_TYPE,
	//aspectJ形式的 
  ASPECTJ,
	//正则表达式的 
  REGEX,
	//自定义的 
  CUSTOM;
  private FilterType() {
  }
}
```

a) 注解形式的FilterType.ANNOTATION 比如<font color=red>@Controller @Service @Repository @Compent</font>

```java
@ComponentScan.Filter(type = FilterType.ANNOTATION,value = {Controller.class})
```

b) 指定类型的 FilterType.ASSIGNABLE_TYPE 

```java
@ComponentScan.Filter(type = FilterType.ASSIGNABLE_TYPE,value = {TulingService.class})
```

c) aspectj类型的 FilterType.ASPECTJ(不常用) 

d) 正则表达式的 FilterType.REGEX(不常用) 

a) 自定义的 FilterType.CUSTOM

```java
/**
自定义过滤器类，需要实现TypeFilter接口
*/
public class TulingFilterType implements TypeFilter {
    @Override
    public boolean match(MetadataReader metadataReader, MetadataReaderFactory metadataReaderFactory) throws IOException {
    //获取当前类的注解源信息
    AnnotationMetadata annotationMetadata = metadataReader.getAnnotationMetadata();
    //获取当前类的class的源信息
    ClassMetadata classMetadata = metadataReader.getClassMetadata(); //获取当前类的资源信息
    Resource resource = metadataReader.getResource();
    if(classMetadata.getClassName().contains("dao")) { 
      return true;
    }
		return false; 
}
```

使用

```java
@ComponentScan(basePackages = {"com.tuling.testcompentscan"},includeFilters = { @ComponentScan.Filter(type = FilterType.CUSTOM,value = TulingFilterType.class)},useDefaultFilters = false) 
public class MainConfig {
}
```

### 1.4 配置Bean的作用域对象

* 在不指定@Scope的情况下，所有的bean都是单实例的bean，而且是饿汉加载(容器启动实例就创建好了)

```java
@Bean
public Person person() {
		return new Person(); 
}
```

* 指定@Scope为 prototype 表示为多实例的，而且还是懒汉模式加载(IOC容器启动的时候，并不会创建对象，而是在第一次使用的时候才会创建)

```java
@Bean
@Scope(value = "prototype") 
public Person person() {
		return new Person(); 
}
```

* @Scope指定的作用域方法取值

a) singleton 单实例的(**默认**)

b) prototype 多实例的

c) request 同一次请求

d) session 同一个会话级别

**singleton和prototype比较常用**

### 1.5 Bean的懒加载@Lazy

主要针对**单实例**的bean 容器启动的时候，不会创建对象，而是在第一次使用的时候才会创建该对象

```java
@Bean
@Lazy
public Person person() {
		return new Person(); 
}
```

### 1.6 @Conditional进行条件判断等

场景,有二个组件TulingAspect 和TulingLog ，我的TulingLog组件是依赖于TulingAspect的组件，初始化TulingLog组件，就必须先初始化TulingAspect组件。**应用:**自己创建一个TulingCondition的类实现Condition接口

```java
public class TulingCondition implements Condition { 
  /**
  *
  * @param context
  * @param metadata * @return
  */
  @Override
  public boolean matches(ConditionContext context, AnnotatedTypeMetadata metadata) {
  //判断容器中是否有tulingAspect的组件 
    if(context.getBeanFactory().containsBean("tulingAspect")) {
 				return true; 
    }
  	return false; 
  }
}
```

Conditional配置类，只有TulingAspect初始化后，才会初始化tulingLog

```java
public class MainConfig {
  @Bean
  public TulingAspect tulingAspect() {
  	return new TulingAspect(); 
  }
  //当切 容器中有tulingAspect的组件，那么tulingLog才会被实例化. 
  @Bean
  @Conditional(value = TulingCondition.class)
  public TulingLog tulingLog() {
  	return new TulingLog(); 
  }
}
```

### 1.7 往IOC 容器中添加组件的方式

* 通过@CompentScan +@Controller @Service @Respository @compent

适用场景: 针对我们自己写的组件可以通过该方式来进行加载到容器中。 

* 通过@Bean的方式来导入组件(适用于导入第三方组件的类) 

* 通过@Import来导入组件 (导入组件的id为全类名路径)

```java
@Configuration
@Import(value = {Person.class, Car.class}) 
public class MainConfig {
}
```

通过@Import 的ImportSeletor类实现组件的导入 (导入组件的id为全类名路径)

```java
public class TulingImportSelector implements ImportSelector { 
    //可以获取导入类的注解信息
    @Override
    public String[] selectImports(AnnotationMetadata importingClassMetadata) {
    		return new String[]{"com.tuling.testimport.compent.Dog"}; 
    }
} 
```

配置类，通过import导入组件

```java
@Configuration
@Import(value = {Person.class, Car.class, TulingImportSelector.class}) 
public class MainConfig {}
```

通过@Import的 ImportBeanDefinitionRegister导入组件 (可以指定bean的名称)

```java
public class TulingBeanDefinitionRegister implements ImportBeanDefinitionRegistrar { 
  @Override
	public void registerBeanDefinitions(AnnotationMetadata importingClassMetadata, 		BeanDefinitionRegistry registry) { 
    //创建一个bean定义对象
		RootBeanDefinition rootBeanDefinition = new RootBeanDefinition(Cat.class);
    //把bean定义对象导入到容器中
    registry.registerBeanDefinition("cat",rootBeanDefinition); 
  }
}
```

导入Bean

```java
@Configuration
@Import(value = {Person.class, Car.class, TulingImportSelector.class, TulingBeanDefinitionRegister.class}) 
public class MainConfig {}
```

* 通过实现FacotryBean接口来实现注册 组件

```java
public class CarFactoryBean implements FactoryBean<Car> {
  //返回bean的对象
  @Override
  public Car getObject() throws Exception {
  	return new Car(); 
  }
  //返回bean的类型
  @Override
  public Class<?> getObjectType() {
  	return Car.class; 
  }
  //是否为单例
  @Override
  public boolean isSingleton() {
    return true; 
  }
}
```

### 1.8 Bean的初始化方法和销毁方法

1. 什么是bean的生命周期?

bean的创建----->初始化----->销毁方法 

由容器管理Bean的生命周期，我们可以通过自己指定bean的初始化方法和bean的销毁方法

```java
@Configuration
public class MainConfig {
   //指定了bean的生命周期的初始化方法和销毁方法.
   @Bean(initMethod = "init",destroyMethod = "destroy") 
  public Car car() {
    return new Car(); 
  }
}
```

针对单实例bean的话，容器启动的时候，bean的对象就创建了，而且容器销毁的时候，也会调用Bean的销毁方法

针对多实例bean的话,容器启动的时候，bean是不会被创建的而是在获取bean的时候被创建，而且bean的销毁不受 IOC容器的管理.

2. 通过 InitializingBean和DisposableBean 的二个接口实现bean的初始化以及销毁方法

```java
@Component
public class Person implements InitializingBean,DisposableBean {
	public Person() { 
    System.out.println("Person的构造方法");
  }

  @Override
  public void destroy() throws Exception {
    System.out.println("DisposableBean的destroy()方法 "); 
  }

  @Override
  public void afterPropertiesSet() throws Exception {
    System.out.println("InitializingBean的 afterPropertiesSet方法"); 
  }
}
```

3. 通过JSR250规范 提供的注解@PostConstruct 和@ProDestory标注的方法

```java
@Component 
public class Book {
  public Book() {
  	System.out.println("book 的构造方法");
  }
  
  @PostConstruct 
  public void init() {
  	System.out.println("book 的PostConstruct标志的方法"); 
  }
  
  @PreDestroy
  public void destory() {
  	System.out.println("book 的PreDestory标注的方法"); 
  }
}
```

4. 通过Spring的BeanPostProcessor的 bean的后置处理器会拦截所有bean创建过程 

   postProcessBeforeInitialization 在init方法之前调用 

   postProcessAfterInitialization 在init方法之后调用

```java
@Component
public class TulingBeanPostProcessor implements BeanPostProcessor {
  @Override
  public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
  	System.out.println("TulingBeanPostProcessor...postProcessBeforeInitialization:"+beanName);
  	return bean; 
  }
  
  @Override
  public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
  	System.out.println("TulingBeanPostProcessor...postProcessAfterInitialization:"+beanName);
  	return bean; 
  }
}
```

### 1.9 通过@Value+@PropertySource来给组件赋值

```java
public class Person {
  //通过普通的方式 
  @Value("司马")
  private String firstName;
  //spel方式来赋值 
  @Value("#{28-8}")
  private Integer age; 
  //通过读取外部配置文件的值 
  @Value("${person.lastName}")
  private String lastName;
  
  @Configuration
  @PropertySource(value = {"classpath:person.properties"}) //指定外部文件的位置 
  public class MainConfig {
    @Bean
    public Person person() {
    	return new Person(); 
    }
  }
}
```

### 1.10 自动装配 @AutoWired的使用

自动注入将dao注入到server中：

```java
//一个Dao
@Repository
public interface TulingDao { 
		public String findById(Integer id);
}

```

Service

```java
@Service
public class TulingService {
  @Autowired
  private TulingDao tulingDao;
}
```

结论:

1. 自动装配首先时<font color=red>**按照类型**</font>进行装配，若在IOC容器中发现了多个**相同类型**的组件，那么就按照<font color=red>**属性名称**</font>来进行装配

```java
@Autowired
private TulingDao tulingDao;
```

比如，我容器中有两个TulingDao类型的组件 一个叫tulingDao 一个叫tulingDao2 那么我们通过@AutoWired 来修饰的属性名称时tulingDao，那么拿就加载容器的tulingDao组件，若属性名称为tulignDao2 那么他就加载的时tulingDao2组件 

2. 假设我们需要指定特定的组件来进行装配，我们可以通过使用@Qualifier("tulingDao")来指定装配的组件或者在配置类上的@Bean加上@Primary注解

   @Primary 就是当Spring容器扫描到某个接口的多个 bean 时，如果某个bean上加了@Primary 注解 ，则这个bean会被优先选用

```java
@Autowired 
@Qualifier("tulingDao") 
private TulingDao tulingDao2;
```

3. 假设我们容器中即没有tulingDao 和tulingDao2,那么在装配的时候就会抛出异常 

No qualifying bean of type 'com.tuling.testautowired.TulingDao' available 

若我们想不抛异常 ，我们需要指定 required为false的时候可以了，意思就是找不到该Bean，就不要找，默认为null。

```java
@Autowired(required = false) 
@Qualifier("tulingDao") 
private TulingDao tulingDao2;
```

4. @Resource(JSR250规范) 

功能和@AutoWired的功能差不多一样，但是不支持**@Primary**和**@Qualifier**

5. @InJect(JSR330规范)
    需要导入jar包依赖，功能和支持@Primary功能 ,但是没有Require=false的功能

```xml
 <dependency> 
   <groupId>javax.inject</groupId> 
   <artifactId>javax.inject</artifactId> 
   <version>1</version>
</dependency>
```

6. 使用autowired 可以标注在方法上

标注在set方法上

```java
@Autowired
public void setTulingLog(TulingLog tulingLog) {
	this.tulingLog = tulingLog; 
}
```

标注在构造方法上

```java
@Autowired
public TulingAspect(TulingLog tulingLog) {
	this.tulingLog = tulingLog; 
}
```


标注在配置类上的入参中(可以不写)

```java
@Bean
public TulingAspect tulingAspect(@Autowired TulingLog tulingLog) {
	TulingAspect tulingAspect = new TulingAspect(tulingLog);
	return tulingAspect; 
}
```

# 三 使用Spring IOC底层组件

 我们自己的组件 需要使用spring ioc的底层组件的时候，比如 ApplicationContext等，我们可以通过实现XXXAware接口来实现

```java
@Component
public class TulingCompent implements ApplicationContextAware,BeanNameAware {
  
  private ApplicationContext applicationContext;
  @Override
  public void setBeanName(String name) {
    System.out.println("current bean name is :【"+name+"】"); 
  }
  
  @Override
  public void setApplicationContext(ApplicationContext applicationContext) throws BeansException {
    this.applicationContext = applicationContext; 
  }
}
```

# 四 @Profile注解 

来根据环境来激活标识不同的Bean 

1. @Profile标识在类上，那么只有当前环境匹配，整个配置类才会生效 

2. @Profile标识在Bean上 ，那么只有当前环境的Bean才会被激活

3. 没有标志为@Profile的bean 不管在什么环境都可以被激活

```java
 @Configuration
@PropertySource(value = {"classpath:ds.properties"})
public class MainConfig implements EmbeddedValueResolverAware {
  @Value("${ds.username}") 
  private String userName;
  
  @Value("${ds.password}") 
  private String password;
  
  private String jdbcUrl; 
  
  private String classDriver;
  
  @Override
  public void setEmbeddedValueResolver(StringValueResolver resolver) {
    this.jdbcUrl = resolver.resolveStringValue("${ds.jdbcUrl}");
    this.classDriver = resolver.resolveStringValue("${ds.classDriver}"); 
  }
  //标识为测试环境才会被装配 
  @Bean
  @Profile(value = "test") 
  public DataSource testDs() {
    return buliderDataSource(new DruidDataSource()); 
  }
  //标识开发环境才会被激活 
  @Bean
  @Profile(value = "dev") 
  public DataSource devDs() {
    return buliderDataSource(new DruidDataSource()); 
  }
  //标识生产环境才会被激活 
  @Bean
  @Profile(value = "prod") 
  public DataSource prodDs() {
    return buliderDataSource(new DruidDataSource()); 
  }
  
  private DataSource buliderDataSource(DruidDataSource dataSource) { 
    dataSource.setUsername(userName); dataSource.setPassword(password); 
    dataSource.setDriverClassName(classDriver); dataSource.setUrl(jdbcUrl);
  	return dataSource; 
  }
}
```

激活切换环境的方法，实际上参数只能有一种，开发（dev）、测试（test）和生成（prod）
 方法一:通过运行时jvm参数来切换 

```
-Dspring.profiles.active=test|dev|prod 
```

方法二:通过代码的方式来激活

```java
 public static void main(String[] args) {
   AnnotationConfigApplicationContext ctx = new AnnotationConfigApplicationContext();
   ctx.getEnvironment().setActiveProfiles("test","dev");
   ctx.register(MainConfig.class); ctx.refresh(); 
   printBeanName(ctx);
}
```

