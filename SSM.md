netstat -aon|findstr “端口号”

# Spring 

**IOC**

IOC：控制反转

对象的创建控制权由程序转移到外部，由外部提供对象这种思想称为控制反转

![image-20240201101244233](C:\Users\13030\AppData\Roaming\Typora\typora-user-images\image-20240201101244233.png)

![image-20240201101221687](C:\Users\13030\AppData\Roaming\Typora\typora-user-images\image-20240201101221687.png)

因为上面换实现方法，源代码需重新编译、部署、测试，浪费效率

所以创建**IOC容器**充当外部，IOC容器管理大量的对象（Service对象和dao对象）

IOC容器负责对象的创建、初始化等一系列工作，被创建或被管理的对象在IOC容器中统称为**Bean**

**入门**

1.导入依赖

```
<dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>5.2.0.RELEASE</version>
    </dependency>
```

2.定义Spring管理的类（接口）

3.配置xml

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
        <bean id="bookDao" class="dao.Impl.BookDaoImpl"/>
        <bean id="bookService" class="service.Impl.BookServiceImpl">
                <!--7.配置server与dao的关系-->
                <property name="bookDao" ref="bookDao"/>
        </bean>
</beans>
```

4.main:初始化IoC容器，通过容器获取Bean

```
 		//获取IOC容器
        ApplicationContext ctx = new ClassPathXmlApplicationContext("ApplicationContext.xml");
        //获取Bean

        BookDao bookDao = (BookDao)ctx.getBean("bookDao");
        bookDao.save();
        BookService bookService = (BookService)ctx.getBean("bookService");
        bookService.save();
```



**Object getBean(String name)：** 返回容器中id为name的bean实例，所以需要强制类型转换

```
public class BookServiceImpl implements BookService {
    //5.删除业务层使用new的方式创建的dao对象
    private BookDao bookDao;
    @Override
    public void save() {
        System.out.println("book service");
        bookDao.save();
    }
    //6.提供对应的set方法

    public void setBookDao(BookDao bookDao) {
        this.bookDao = bookDao;
    }
}
```

[(35条消息) 什么是Spring IoC容器？_什么是springioc容器_南_茗的博客-CSDN博客](https://blog.csdn.net/weixin_46856842/article/details/106520313)

理解：分为4层：

用户层（表现层）
IOC容器
服务层（业务层）
dao层

用户层调用IOC容器里的对象，再调用对象里的方法

**实例化Bean的两种方式**

1.Spring创建bean调用的是无参构造函数

```
配置文件
<bean id="bookDao" class="dao.Impl.BookDaoImpl"/>-->
```

2.创建BookDaoFactoryBean

```
public class BookDaoFactoryBean implements FactoryBean<BookDao> {

    @Override
    public BookDao getObject() throws Exception {
        return new BookDaoImpl();
    }

    @Override
    public Class<?> getObjectType() {
        return BookDao.class;
    }
}
```

```
配置文件
<bean id="bookDao" class="Factory.BookDaoFactoryBean"/>
```

**依赖注入**

DI(Dependency Injection)依赖注入：在容器中建立bean与bean之间的依赖关系的整个过程

依赖关系类型：

引用类型
简单类型（基本类型与String）

**依赖注入方式：**

1.setter注入

在bean中定义引用类型属性并提供可访问的set方法

```
public class BookServiceImpl implements BookService {
    private BookDao bookDao;
    public void setBookDao(BookDao bookDao) {
        this.bookDao = bookDao;
    }
}
```

配置中使用property标签ref属性注入引用类型对象，value注入值

```
<bean id="bookDao" class="dao.Impl.BookDaoImpl" init-method="init" destroy-method="destory"/>
        <bean id="bookService" class="service.Impl.BookServiceImpl">
                <property name="bookDao" ref="bookDao"/>
                //<property name="···" value=1/>
        </bean>
```

2.构造器注入

```
//BookServiceImpl.java
public BookServiceImpl(BookDao bookDao) { 
        this.bookDao = bookDao;
    }
```

配置

```
<bean id="bookService" class="service.Impl.BookServiceImpl">
                <!--7.配置server与dao的关系-->
                <constructor-arg name="bookDao" ref="bookDao"/>
        </bean>
```

**配置自动注入**：autowire="byType"和autowire="byName "

按种类注入

配置 

```
<bean id="bookDao" class="dao.Impl.BookDaoImpl" init-method="init" destroy-method="destory"/>
<bean id="bookService" class="service.Impl.BookServiceImpl" autowire="byType">
```

按名字注入：bean的id与BookServiceImpl.java里的对象名相同

配置

```
<bean id="bookDao" class="dao.Impl.BookDaoImpl" init-method="init" destroy-method="destory"/>
<bean id="bookService" class="service.Impl.BookServiceImpl" autowire="byName">
```

![image-20230330102511509](C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20230330102511509.png)![image-20230330102512743](C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20230330102512743.png)

**集合(array,list,set,map)注入**

配置 

<property name="···">
<array>
</array>
<list>
</list>
</property>

```
 <bean id="bookDao" class="dao.Impl.BookDaoImpl">
                <property name="array">
                        <array>
                                <value>100</value>
                                <value>200</value>
                                <value>300</value>
                        </array>
                </property>
        </bean>
```

配置文件加载properties文件

![image-20230330113518346](C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20230330113518346.png)

```
//使用context命名空间，加载指定properties文件
<context:property-placeholder location="classpath*:*.properties" system-properties-mode="NEVER"/>
```

## **注解开发**

**注解代替配置**

创建一个内部配置文件

@Configuration：用于设定当前类为配置类
@ComponentScan("dao")：扫描dao下的对象，加载bean
@ComponentScan("service")

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="
        http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd"> 
    <context:component-scan base-package="dao"/>//通过这个注解代替@ComponentScan("dao")，其余的@Configuration代替
</beans>
```

然后在对应的类里加上注解

```
@Component("bookDao")
```

如果不加名字，可以在main函数里通过类型来获取

```
@Component
```

main

```
BookDao bookDao = (BookDao)ctx.getBean(BookDao.class);
```

@Component三个衍生注解：

@Controller：用于表现层bean定义

@Service：用于业务层bean定义

@Repository：用于数据层bean定义

双例模式：@Scope("prototype")
bean生命周期：@PostConstruct

**依赖注入**

@Autowired：连setter都不用写

```
public class BookServiceImpl implements BookService {
    @Autowired
    private BookDao bookDao;
    @Override
    public void save() {
        System.out.println("book service");
        bookDao.save();
    }
}
```

@Value：实现简单类型注入

**外部配置文件注入**

在内部配置文件添加@PropertySource("外部配置文件")

```
@PropertySource("classpath:jdbc.properties")
```

通过变量注入

```
@Value("${name}")
private String name;
```

**注解开发获取第三方bean**

内部配置文件

```
@Configuration
@ComponentScan("dao")//为注入引用对象做准备
@Import(jdbcConfig.class)
public class SpringConfig2 {
	//注入简单数据类型步骤
	//类中提供四个属性并@Value注入  
	@Value("com.mysql.jdbc.Driver")
    private String driver;
    @Value("jdbc:mysql://localhost:3306/spring_db")
    private String url;
    @Value("root")
    private String userName;
    @Value("root")
    private String password;
	
	//1.定义一个方法获得要管理的对象
    //2.添加@Bean，表示当前方法的返回值为Bean
    @Bean
    //形参为注入引用对象（自动装配）
    public DataSource dataSource(BookDao bookDao){
        System.out.println(bookDao);
        DruidDataSource ds = new DruidDataSource();
        ds.setDriverClassName(driver);
        ds.setUrl(url);
        ds.setUsername(userName);
        ds.setPassword(password);
        return ds;
    }
}

```

main

```
public class AppForAnnotation2 {
    public static void main(String[] args) {
        ApplicationContext ctx=new AnnotationConfigApplicationContext(SpringConfig2.class);
        DataSource dataSource=ctx.getBean(DataSource.class);
        System.out.println(dataSource);
    }
}
```

![image-20230330181745197](C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20230330181745197.png)

[大白话讲解Spring的@bean注解 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/99870991)

**spring整合mybatis**

依赖配置

```
<dependencies>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>5.2.10.RELEASE</version>
    </dependency>
    <dependency>
      <groupId>com.alibaba</groupId>
      <artifactId>druid</artifactId>
      <version>1.1.16</version>
    </dependency>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.12</version>
      <scope>test</scope>
    </dependency>
    <dependency>
      <groupId>com.alibaba</groupId>
      <artifactId>druid</artifactId>
      <version>1.1.16</version>
    </dependency>
    <dependency>
      <groupId>c3p0</groupId>
      <artifactId>c3p0</artifactId>
      <version>0.9.1.2</version>
    </dependency>
    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis</artifactId>
      <version>3.5.5</version>
    </dependency>
    <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>5.1.46</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-jdbc</artifactId>
      <version>5.2.10.RELEASE</version>
    </dependency>
    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis-spring</artifactId>
      <version>1.3.0</version>
    </dependency>
  </dependencies>
```

多了mybatis-spring：[Mybatis](https://so.csdn.net/so/search?q=Mybatis&spm=1001.2101.3001.7020)社区自己开发了一个Mybatis-Spring用来满足Mybatis用户整合Spring的需求，使用 MyBatis-Spring 之后, 会使用SqlSessionFactoryBean来代替SqlSessionFactoryBuilder创建

 spring.config

```
@Configuration
@ComponentScan({"service","dao"})
@PropertySource("jdbc.properties")
@Import({JdbcConfig.class,MybatisConfig.class})
public class SpringConfig {
}
```

jdbc.properties

```
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/spring_db?useSSL=false
jdbc.username=root
jdbc.password=root
```

JdbcConfig 

```
public class JdbcConfig {
    @Value("${jdbc.driver}")
    private String driver;
    @Value("${jdbc.url}")
    private String url;
    @Value("${jdbc.username}")
    private String userName;
    @Value("${jdbc.password}")
    private String password;

    @Bean
    public DataSource dataSource(){
        DruidDataSource ds = new DruidDataSource();
        ds.setDriverClassName(driver);
        ds.setUrl(url);
        ds.setUsername(userName);
        ds.setPassword(password);
        return ds;
    }
}
```

MybatisConfig(两个bean：SqlSessionFactoryBean，MapperScannerConfigurer需要添加)

```
public class MybatisConfig {
    @Bean
    public SqlSessionFactoryBean sqlSessionFactoryBean(DataSource dataSource){
        SqlSessionFactoryBean ssfb=new SqlSessionFactoryBean();
        ssfb.setTypeAliasesPackage("com.itheima.domain");
        ssfb.setDataSource(dataSource);
        return  ssfb;
    }
    @Bean
    public MapperScannerConfigurer mapperScannerConfigurer(){
        MapperScannerConfigurer msc= new MapperScannerConfigurer();
        msc.setBasePackage("dao");
        return msc;
    }
}
```

总结：与之前java web的区别

1.java web的mybatis-config.xml转化为spring里的配置文件jdbc.properties和jdbcConfig.java和MybatisConfig.java

![image-20230331163735277](C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20230331163735277.png)

![image-20230331163835714](C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20230331163835714.png)

## AOP

[(37条消息) 细说Spring——AOP详解（AOP概览）_spring aop看哪个_Jivan2233的博客-CSDN博客](https://blog.csdn.net/q982151756/article/details/80513340)

AOP：在不惊动原始设计的基础上为其进行功能增强

![image-20230331171628288](C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20230331171628288.png)

连接点：所以方法
切入点：对连接点追加左边的通知（功能）
切面：连接切入点和通知，使它们对应上

入门

BookDaoImpl：定义方法，在原有方法不动前提下，给update()加上System.currentTimeMillis()

```
public class BookDaoImpl implements BookDao {
    @Value("${name}")
    private String name;

    @Override
    public void save() {
        System.out.println(System.currentTimeMillis());
        System.out.println("BookDao save "+name);
    }
    @Override
    public void update() {
        System.out.println("update");
    }
}
```

SpringConfig

```
@Configuration
@ComponentScan({"dao","aop"})
@EnableAspectJAutoProxy  //开启spring对AOP注解驱动支持
public class SpringConfig {
}
```

aop

```
@Component
@Aspect//
@Aspect:
public class MyAdvice {
	//建立切入点
    @Pointcut("execution(void dao.BookDao.update())")
    private void pt(){

    }
    //切面（连接）
    @Before("pt()")
    //通知
    public void method(){
        System.out.println(System.currentTimeMillis());
    }
}
```

切入点定义依托一个不具有实际意义的方法进行，即无参数，无返回值，方法体无实际逻辑

- @Before: 前置通知, 在方法执行之前执行
- @After: 后置通知, 在方法执行之后执行 

**AOP切入点表达式**

格式：动作关键字（ 返回值 包名.类/接口名.方法名）

**AOP通知类型**

@Before

```
@Before("pt()")
    public void method(){
        System.out.println("Before");
    }
```

@After

```
@After("pt()")
    public void method(){
        System.out.println("After");
    } 
```

@Around：需要加上原始操作

```
 @Around("pt()")
    public void method(ProceedingJoinPoint pjp) throws Throwable {
        System.out.println("Before");
        //表示对原始操作的调用
        pjp.proceed();
        System.out.println("After");
    }
```

**可以改变执行目标方法的参数值，也可以改变执行目标方法之后的返回值； 当需要改变目标方法的返回值时，只能使用Around方法；**

 通知标准格式：

```
@Around("pt()")
    public Object around(ProceedingJoinPoint pjp) throws Throwable {
        System.out.println("around before advice ...");
        //表示对原始操作的调用
        Object ret = pjp.proceed();
        System.out.println("around after advice ...");
        //ret = 200;其中ret可以改变目标方法的返回值
        return ret;        
    }
```

 pjp.proceed()为：目标方法执行，返回的是目标方法的返回值，然后提取给ret，可以对ret修改

[(38条消息) aop切面中 joinPoint.proceed()的一个小认识_Yangshiwei....的博客-CSDN博客](https://blog.csdn.net/yyswit/article/details/127409168)

注：

获取接口、方法：Signature

```
Signature signature = pjp.getSignature();
//获取接口
String className=signature.getDeclaringTypeName();
//获取方法
String methodName=signature.getName();
```

获取参数：pjp.getArgs();

```
//原本查询id为1的，在通知里修改参数，id=2，查询id为2
@Around("servicePt()")
    public Object runSpeed(ProceedingJoinPoint pjp) throws Throwable {
        Object[] args = pjp.getArgs();
        System.out.println(Arrays.toString(args));
        args[0]=2;
        Object proceed = pjp.proceed(args);     
        return proceed;
    }
```

**事务**

保证取款存款一致性

```
写在实现类或实现类的方法上
@Transactional
accountDao.outMoney(out,money); 
int i = 1/0; 
accountDao.inMoney(in,money); 
```

@Transactional：保证事务的一致性，建议写在实现类或实现类的方法上

```
SpringConfig
//开启注解式事务驱动
@EnableTransactionManagement
```

在已有事务中新建事务：@Transactional(propagation = Propagation.REQUIRES_NEW)![image-20230411111541886](C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20230411111541886.png)



# SpringMVC

SpringMVC相当于Servlet

入门案例

坑：需把webapp里的内容都删去

![image-20230425101650274](C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20230425101650274.png)

1.创建SpringMVC控制器类（等同于Servlet功能）

```
@Controller
public class UserController {

    //设置映射路径为/save，即外部访问路径
    @RequestMapping("/save")
    //设置当前操作返回结果为指定json数据（本质上是一个字符串信息）
    @ResponseBody
    public String save(){
        System.out.println("user save ...");
        return "{'info':'springmvc'}";
    }

    //设置映射路径为/delete，即外部访问路径
    @RequestMapping("/delete")
    @ResponseBody
    public String delete(){
        System.out.println("user save ...");
        return "{'info':'springmvc'}";
    }
}
```

2.配置SpringMVC环境

```
@Configuration
@ComponentScan("com.itheima.controller")
public class SpringMvcConfig {
}
```

3.加载SpringMVC环境，配置SpringMVC请求

```
//web容器配置类
public class ServletContainersInitConfig extends AbstractDispatcherServletInitializer {
    //加载springmvc配置类，产生springmvc容器（本质还是spring容器）
    protected WebApplicationContext createServletApplicationContext() {
        //初始化WebApplicationContext对象
        AnnotationConfigWebApplicationContext ctx = new AnnotationConfigWebApplicationContext();
        //加载指定配置类
        ctx.register(SpringMvcConfig.class);
        return ctx;
    }

    //设置由springmvc控制器处理的请求映射路径，即所有请求都通过SpringMVC
    protected String[] getServletMappings() {
        return new String[]{"/"};
    }

    //加载spring配置类
    protected WebApplicationContext createRootApplicationContext() {
        return null;
    }
}
```

简化方式：

```
public class ServletContainersInitConfig extends AbstractAnnotationConfigDispatcherServletInitializer {

    protected Class<?>[] getRootConfigClasses() {
        return new Class[]{SpringConfig.class};
    }

    protected Class<?>[] getServletConfigClasses() {
        return new Class[]{SpringMvcConfig.class};
    }

    protected String[] getServletMappings() {
        return new String[]{"/"};
    }
}
```

启动服务器初始化过程
1．服务器启动，执行ServletcontainersInitConfig类，初始化web容器
2．执行createServletApplicationContext方法，创建了webApplicationcontext对象
3．加载SpringMvcConfig
4．执行@Componentscan加载对应的bean
5．加载UserController，每个@RequestMapping的名称对应一个具体的方法
6．执行getServletMappings方法，定义所有的请求都通过SpringMVC



Spring控制的bean

> 业务bean（Service）
>
> 功能bean（DateSource等）

SpringMVC相关bean加载控制

> SpringMVC加载的bean对应的包均在com.itheima.controller包内

将SpringMVC的bean去除于Spring控制下

> 方式一：Spring加载的bean设定扫描范围为com.itheima，排除controller包内的bean
>
> 方式二：Spring加载的bean设定扫描范围为精准扫描，例如service包、dao包等
>
> 方式三：不区分Spring与SpringMVC的环境，加载到同一个环境中

方式一：

SpringConfig

```
@Configuration
//@ComponentScan({"com.itheima.service","com.itheima.dao"})
//设置spring配置类加载bean时的过滤规则，当前要求排除掉表现层对应的bean
//excludeFilters属性：设置扫描加载bean时，排除的过滤规则
//type属性：设置排除规则，当前使用按照bean定义时的注解类型进行排除
//classes属性：设置排除的具体注解类，当前设置排除@Controller定义的bean
@ComponentScan(value="com.itheima",
    excludeFilters = @ComponentScan.Filter(
        type = FilterType.ANNOTATION,
        classes = Controller.class
    )
)
```

## get/post请求

ServletContainersInitConfig

```
public class ServletContainersInitConfig extends AbstractAnnotationConfigDispatcherServletInitializer {
    protected Class<?>[] getRootConfigClasses() {
        return new Class[0];
    }

    protected Class<?>[] getServletConfigClasses() {
        return new Class[]{SpringMvcConfig.class};
    }

    protected String[] getServletMappings() {
        return new String[]{"/"};
    }

    //乱码处理
    @Override
    protected Filter[] getServletFilters() {
        CharacterEncodingFilter filter = new CharacterEncodingFilter();
        filter.setEncoding("UTF-8");
        return new Filter[]{filter};
    }
}
```

UserController(相当于Servlet)

get和post请求：@RequestMapping(路径)+@ResponseBody

```
@Controller
public class UserController {
//普通参数：请求参数与形参名称对应即可完成参数传递
    @RequestMapping("/commonParam")
    @ResponseBody
    public String commonParam(String name ,int age){
        System.out.println("普通参数传递 name ==> "+name);
        System.out.println("普通参数传递 age ==> "+age);
        return "{'module':'common param'}";
    }
}
```

 public String commonParam(@RequestParam("name")String username ,int age){}

参数名与数据库名不一样：@RequestParam

**JSON数据**

Json格式
    1.开启json数据格式的自动转换，在配置类中开启@EnableWebMvc
    2.使用@RequestBody注解将外部传递的json数组数据映射到形参的集合对象中作为数据

```
@Configuration
@ComponentScan("com.itheima.controller")
//开启json数据类型自动转换
@EnableWebMvc
public class SpringMvcConfig {
}
```

```
	@Controller
	public class UserController {
	@RequestMapping("/listParamForJson")
    @ResponseBody
    public String listParamForJson(@RequestBody List<String> likes){
        System.out.println("list common(json)参数传递 list ==> "+likes);
        return "{'module':'list common for json param'}";
    }
    }
```

@RequestBody主要用来接收前端传递给后端的json字符串(传过来一个对象常用） 中的数据的(请求体中的数据的)

比较：RequestBody 接收的是请求体里面的数据；而RequestParam接收的是key-value里面的参数即：如果参数时放在请求体中，application/json传入后台的话，那么后台要用@RequestBody才能接收到； 如果不是放在请求体中的话，那么后台接收前台传过来的参数时，要用@RequestParam来接收，或则形参前 什么也不写也能接收。

[(47条消息) @RequestBody的使用_justry_deng的博客-CSDN博客](https://blog.csdn.net/justry_deng/article/details/80972817)

## 响应

1.响应页面：直接返回值为要跳转的网页名称（String类型）即可

```
//返回值为String类型，设置返回值为页面名称，即可实现页面跳转
    @RequestMapping("/toJumpPage")
    public String toJumpPage(){
        System.out.println("跳转页面");
        return "page.jsp";
    }
```

2.响应文本数据：@ResponseBody(把返回值作为响应体)，如果没有@ResponseBody，就默认返回页面

```
//返回值为String类型，设置返回值为任意字符串信息，即可实现返回指定字符串信息，需要依赖@ResponseBody注解
    @RequestMapping("/toText")
    @ResponseBody
    public String toText(){
        System.out.println("返回纯文本数据");
        return "response text";
    }
```

```
//响应POJO对象
    //返回值为实体类对象，设置返回值为实体类类型，即可实现返回对应对象的json数据，需要依赖@ResponseBody注解和@EnableWebMvc注解
    @RequestMapping("/toJsonPOJO")
    @ResponseBody
    public User toJsonPOJO(){
        System.out.println("返回json对象数据");
        User user = new User();
        user.setName("itcast");
        user.setAge(15);
        return user;
    }
```

总结：如果是响应页面，直接返回页面名称，不是页面的话，统统加上@ResponseBody

## Rest

<img src="C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20230426194114746.png" alt="image-20230426194114746" style="zoom:80%;" />

Rest风格：

GET:查询
POST:新增
PUT:修改
DELETE:删除

@RequestMapping(value = "/users",method = RequestMethod.XXX)

http://localhost/users/1（1省略了id=1）

如何取到1：@PathVariable：获取路径变量   @RequestMapping(value = "/users/{id}",method = RequestMethod.XXX)：补充{id}路径变量

```
//原始版：
@Controller
public class UserController {

    //设置当前请求方法为POST，表示REST风格中的添加操作
    @RequestMapping(value = "/users",method = RequestMethod.POST)
    @ResponseBody
    public String save(){
        System.out.println("user save...");
        return "{'module':'user save'}";
    }

    //设置当前请求方法为DELETE，表示REST风格中的删除操作
    //@PathVariable注解用于设置路径变量（路径参数），要求路径上设置对应的占位符，并且占位符名称与方法形参名称相同
    @RequestMapping(value = "/users/{id}",method = RequestMethod.DELETE)
    @ResponseBody
    public String delete(@PathVariable Integer id){
        System.out.println("user delete..." + id);
        return "{'module':'user delete'}";
    }

    //设置当前请求方法为PUT，表示REST风格中的修改操作
    @RequestMapping(value = "/users",method = RequestMethod.PUT)
    @ResponseBody
    public String update(@RequestBody User user){
        System.out.println("user update..."+user);
        return "{'module':'user update'}";
    }

    //设置当前请求方法为GET，表示REST风格中的查询操作
    //@PathVariable注解用于设置路径变量（路径参数），要求路径上设置对应的占位符，并且占位符名称与方法形参名称相同
    @RequestMapping(value = "/users/{id}" ,method = RequestMethod.GET)
    @ResponseBody
    public String getById(@PathVariable Integer id){
        System.out.println("user getById..."+id);
        return "{'module':'user getById'}";
    }

    //设置当前请求方法为GET，表示REST风格中的查询操作
    @RequestMapping(value = "/users",method = RequestMethod.GET)
    @ResponseBody
    public String getAll(){
        System.out.println("user getAll...");
        return "{'module':'user getAll'}";
    }

}
```

```
//提炼版：
@RestController
@RequestMapping("/users")
public class UserController {
    //设置当前请求方法为POST，表示REST风格中的添加操作
    @PostMapping
    public String save(){
        System.out.println("user save...");
        return "{'module':'user save'}";
    }
    //设置当前请求方法为DELETE，表示REST风格中的删除操作
    //@PathVariable注解用于设置路径变量（路径参数），要求路径上设置对应的占位符，并且占位符名称与方法形参名称相同
    @DeleteMapping("/{id}")
    public String delete(@PathVariable Integer id){
        System.out.println("user delete..." + id);
        return "{'module':'user delete'}";
    }
    //设置当前请求方法为PUT，表示REST风格中的修改操作
    @PutMapping
    public String update(@RequestBody User user){
        System.out.println("user update..."+user);
        return "{'module':'user update'}";
    }
    //设置当前请求方法为GET，表示REST风格中的查询操作
    //@PathVariable注解用于设置路径变量（路径参数），要求路径上设置对应的占位符，并且占位符名称与方法形参名称相同
    @GetMapping("/{id}")
    public String getById(@PathVariable Integer id){
        System.out.println("user getById..."+id);
        return "{'module':'user getById'}";
    }
}
```

注：

1.@Controller+@ResponseBody每个Controller都写，干脆提炼出一个@RestController

2.提炼出路径的@RequestMapping("/users")

3.@RequestMapping(method = RequestMethod.POST)提炼为

**放行页面**（把页面路径交给tomcat处理，而不是SpringMvc）

```
public class SpringMvcSupport extends WebMvcConfigurationSupport {
    protected void addResourceHandlers(ResourceHandlerRegistry registry){
        //当访问/pages/???的时候，交给tomcat处理
        registry.addResourceHandler("/pages/**").addResourceLocations("/pages/");
        registry.addResourceHandler("/js/**").addResourceLocations("/js/");
        registry.addResourceHandler("/css/**").addResourceLocations("/css/");
        registry.addResourceHandler("/plugins/**").addResourceLocations("/plugins/");
    }
}
```

# 案例实战

4_27

其中测试环节：分为两部分

1.业务层测试（通过test模块测试）

2.表现层测试（通过postman测试）

注：构造domain的对象时，无需创建构造函数

<img src="C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20230427192241533.png" alt="image-20230427192241533" style="zoom:80%;" />

**统一前端数据格式**![image-20230427194944984](C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20230427194944984.png)<img src="C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20230427200102581.png" alt="image-20230427200102581" style="zoom:80%;" />

code:区分增删改查（最后一位1为操作成功，0为操作失败）
data:数据
msg:查询失败时的信息

定义一个Result类

```
public class Result {
    private Object data;
    private Integer code;
    private String msg;
}
```

定义Code编码类

```
public class Code {
    public static final Integer SAVE_OK =20011;
    public static final Integer DELETE_OK =20021;
    public static final Integer UPDATE_OK =20031;
    public static final Integer GET_OK =20041;

    public static final Integer SAVE_ERR =20010;
    public static final Integer DELETE_ERR =20020;
    public static final Integer UPDATE_ERR =20030;
    public static final Integer GET_ERR =20040;
}
```

相应地改变

```
@RestController
@RequestMapping("/books")
public class BookController {

    @Autowired
    private BookService bookService;
    @PostMapping
    public Result save(@RequestBody Book book) {
        boolean save = bookService.save(book);
        return new Result(save?Code.SAVE_OK:Code.SAVE_ERR,save);
    }
    @PutMapping
    public Result update(@RequestBody Book book) {
        boolean update = bookService.update(book);
        return new Result(update?Code.UPDATE_OK:Code.UPDATE_ERR,update);
    }
    @DeleteMapping("/{id}")
    public Result delete(@PathVariable Integer id) {
        boolean delete = bookService.delete(id);
        return new Result(delete?Code.DELETE_OK:Code.DELETE_ERR,delete);
    }
    @GetMapping("/{id}")
    public Result getById(@PathVariable Integer id) {
        Book book = bookService.getById(id);
        Integer code=book!=null?Code.GET_OK:Code.GET_ERR;
        String msg= book != null ? "":"数据查询失败";
        return new Result(code,book,msg);
    }
    @GetMapping
    public Result getAll() {
        List<Book> booklist = bookService.getAll();
        Integer code=booklist!=null?Code.GET_OK:Code.GET_ERR;
        String msg= booklist != null ? "":"数据查询失败";
        return new Result(code,booklist,msg);
    }
}
```

**异常处理器**

```
@RestControllerAdvice
public class ProjectExceptionAdvice {
    @ExceptionHandler(SystemException.class)
    //系统异常
    public Result doSystemException(SystemException ex){
        //记录日志
        //发送消息给运维
        //发送邮件给开发人员

        return new Result(ex.getCode(),null,ex.getMessage());
    }
    //业务异常
    @ExceptionHandler(BusinessException.class)
    public Result doBusinessException(BusinessException ex){

        return new Result(ex.getCode(),null,ex.getMessage());
    }
    //其他异常
    @ExceptionHandler(Exception.class)
    public Result doException(Exception ex){
        System.out.println("exception...");
        return new Result(Code.SYSTEM_UNKNOW_ERR,null,"系统繁忙，请稍后等待");
    }
}
```

定义两种异常

```
public class BusinessException extends RuntimeException{
    private Integer code;
    public BusinessException(String message, Integer code) {
        super(message);
        this.code = code;
    }
    public BusinessException(String message, Throwable cause, Integer code) {
        super(message, cause);
        this.code = code;
    }
    public Integer getCode() {
        return code;
    }
    public void setCode(Integer code) {
        this.code = code;
    }
}
---------------------------
public class SystemException extends RuntimeException{
  private Integer code;
  public SystemException(String message, Integer code) {
        super(message);
        this.code = code;
    }
    public SystemException(String message, Throwable cause, Integer code) {
        super(message, cause);
        this.code = code;
    }
    public Integer getCode() {
        return code;
    }
    public void setCode(Integer code) {
        this.code = code;
    }
}

```

# Springboot

**修改端口号**

application.properties

```
server.port=8080 //1.
server:
 port: 8080 //2.
```

读取数据

```
//yml数据
lesson: 2918

server:
  port: 8080

enterprise:
  name: itcast
  age: 16
  subject:
    - java
    - c
```



```
@RestController
@RequestMapping("/books")
public class BookController {

    @Autowired
    private Environment environment;
    @GetMapping("/{id}")
    public String getById(@PathVariable Integer id){
        System.out.println("id:"+id);
        System.out.println(environment.getProperty("lesson"));
        System.out.println(environment.getProperty("port"));
        System.out.println(environment.getProperty("enterprise.subject[0]"));
        return "hello,springboot";
    }
}
```

多环境开发

```
#设置启用环境
spring:
  profiles:
    active: dev

#开发
spring:
  profiles: dev
server:
  port: 8080

---
#生产
spring:
  profiles: pro
server:
  port: 8081

---
#测试
spring:
  profiles: test
server:
  port: 8082
```



## 整合第三方技术

### junit

junit：单元测试

@SpringBootTest

![image-20230717194632620](C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20230717194632620.png)

### mybatis

1.application.yml配置数据库内容

```
spring:
  datasource:
    driver-class-name: com.mysql.jdbc.Driver
    url: jdbc:mysql://localhost:3306/ssm_db
    username: root
    password: root
```

2.BookDao里加一个注解

```
@Mapper
```

### mybatisplus

```
<dependency>
			<groupId>com.baomidou</groupId>
			<artifactId>mybatis-plus-boot-starter</artifactId>
			<version>3.4.3</version>
		</dependency>
```

```
public interface bookdao extends BaseMapper<Book>{

}
```

druid

```
<!-- https://mvnrepository.com/artifact/com.alibaba/druid-spring-boot-starter -->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid-spring-boot-starter</artifactId>
    <version>1.2.6</version>
</dependency>
```

```
spring:
  datasource:
    druid:
      driver-class-name: com.mysql.jdbc.Driver
      url: jdbc:mysql://localhost:3306/ssm_db
      username: root
      password: root
```

分页

创建一个mybatisplus拦截器

```
@Configuration
public class MPConfig {
    @Bean
    public  MybatisPlusInterceptor mybatisPlusInterceptor(){
        MybatisPlusInterceptor Interceptor = new MybatisPlusInterceptor();
        Interceptor.addInnerInterceptor(new PaginationInnerInterceptor());
        return Interceptor;
    }
}

```

需要创建一个Ipage对象

```
void contextLoads() {
		IPage page = new Page(1,1);
		bookdao.selectPage(page,null);
	}
```

条件查询

先封装成queryWrapper或者LambdaQueryWrapper（可以使用对象::getName）

```
@Autowired
	private bookdao bookdao;
	@Test
	void contextLoads() {
		QueryWrapper<Book> queryWrapper = new QueryWrapper<>();
		queryWrapper.like("name","无敌");
		bookdao.selectList(queryWrapper);
	}
```

```
class ApplicationTests {
	@Autowired
	private bookdao bookdao;
	@Test
	void contextLoads() {
		String name = "无敌";
		LambdaQueryWrapper<Book> lqw =new LambdaQueryWrapper<>();
		lqw.like(name!=null,Book::getName,name);
		bookdao.selectList(lqw);
	}
}
```

## 快速开发业务层、数据层

数据层

```
@Mapper
public interface bookdao extends BaseMapper<Book> {

}
```

业务层

```
public interface IBookService extends IService<Book> {

}
```

Impl

```
@Service
public class BookServiceImpl2 extends ServiceImpl<bookdao,Book> implements IBookService {
}
```

## 前后端传输json数据

含有flag和data两个数据

<img src="C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20230720201110705.png" alt="image-20230720201110705" style="zoom:67%;" />

```
@Data
public class R {
    private Boolean flag;
    private Object data;

    public R(){

    }
    public R(Boolean flag){
        this.flag=flag;
    }
    public R(Boolean flag,Object data){
        this.flag=flag;
        this.data=data;
    }
```

表现层

```
@RestController
@RequestMapping("/books")
public class BookController2 {
    @Autowired
    private IBookService bookService;
    @GetMapping
    public R getAll(){
        return new R(true,bookService.list());
    }
    @PostMapping
    public R save(@RequestBody Book book){
        return new R(bookService.save(book));
    }
    @PutMapping
    public R update(@RequestBody Book book){
        return new R(bookService.modify(book));
    }
    @DeleteMapping("{id}")
    public R delete(@PathVariable Integer id){
        return new R(bookService.delete(id));
    }
    @GetMapping("{id}")
    public R getId(@PathVariable Integer id){
        return new R(true,bookService.getById(id));
    }
    @GetMapping("{currentPage}/{Pagesize}")
    public R page(@PathVariable int currentPage,@PathVariable int Pagesize){
        return new R(true,bookService.getPage(currentPage, Pagesize));
    }
}
```

## 前端响应请求

axios：就是发送请求和完成请求后的动作

方法名（）{

axios.请求方式（"/请求路径"）.then((res)=>{

​	请求路径完返回的数据需要做什么?

});

}

```
 data:{
            dataList: [],//当前页要展示的列表数据
            dialogFormVisible: false,//添加表单是否可见
            dialogFormVisible4Edit:false,//编辑表单是否可见
            formData: {},//表单数据
      }
methods: {
           //分页查询
            getAll() {
                axios.get("/books/"+this.pagination.currentPage+"/"+this.pagination.pageSize).then((res)=>{
                    //更新组件数据，一共有多少条数据只用后台知道，所以传一共有多少条
                    this.pagination.pageSize=res.data.data.size;
                    this.pagination.currentPage=res.data.data.current;
                    this.pagination.total=res.data.data.total;
                    this.dataList=res.data.data.records;
                });
            },

            //弹出添加窗口
            handleCreate() {
                this.dialogFormVisible = true;
                this.resetForm();
            },

            //重置表单
            resetForm() {
                this.formData={};
            },

            //添加
            handleAdd () {
                axios.post("/books",this.formData).then((res)=>{
                    if(res.data.flag){
                        //1.关闭弹层
                        this.dialogFormVisible = false;
                        this.$message.success("添加成功");
                    }
                    else{
                        this.$message.success("添加失败");
                    }
                    //2.显示新数据
                    this.getAll();
                });
            },

            //取消
            cancel(){
                this.dialogFormVisible = false;
                this.dialogFormVisible4Edit=false;
            },
            
            // 删除
            handleDelete(row) {
                axios.delete("/books/"+row.id).then((res)=>{
                    if(res.data.flag){
                        this.$message.success("删除成功");
                    }
                    else{
                        this.$message.success("删除失败");
                    }
                    this.getAll();
                });
            },
            
            //弹出编辑窗口
            handleUpdate(row) {
                axios.get("/books/"+row.id).then((res)=>{
                    if(res.data.flag&&res.data.data!=null){
                        this.dialogFormVisible4Edit=true;
                        this.formData=res.data.data;
                    }
                    else{
                        this.$message.error("数据同步失败");
                    }
                    this.getAll();
                });
            },
            
            //修改
            handleEdit() {
                axios.put("/books",this.formData).then((res)=>{
                    if(res.data.flag){
                        //1.关闭弹层
                        this.dialogFormVisible4Edit = false;
                        this.$message.success("修改成功");
                    }
                    else{
                        this.$message.success("修改失败");
                    }
                    //2.显示新数据
                    this.getAll();
                });
            },
```

res:![image-20230720212826935](C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20230720212826935.png)

其中返回值res里的data里的data需要在dataList中显示出来

注：

```
//钩子函数，VUE对象初始化完成后自动执行
created() {
    this.getAll();
},
```

<img src="C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20230722090524272.png" alt="image-20230722090524272" style="zoom:67%;" />

<img src="C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20230722090536818.png" alt="image-20230722090536818" style="zoom:67%;" />

# Spring Security

身份验证：

外部使用者通过将身份验证的必要信息，比如用户名和密码封装一个Authentication传递、调用AuthenticationMananger的authenticate方法。如果没有返回异常和null值，验证服务便是完成。完成身份验证的Authentication不仅包含用户的身份验证信息，比如用户名，还会将该用户身份下所有对应的权限列表也一并封装返回。

```
public ResponseResult login(User user) {
        UsernamePasswordAuthenticationToken authenticationToken = new UsernamePasswordAuthenticationToken(user.getUserName(), user.getPassword());
        //AuthenticationManger authenticate进行用户认证
        Authentication authenticate = authenticationManager.authenticate(authenticationToken);
        //如果认证没通过，给出对应的提示
        if(Objects.isNull(authenticate)){
            throw new RuntimeException("登录失败");
        }
        //如果认证通过了，使用userid生成一个jwt，
        LoginUser loginUser = (LoginUser)authenticate.getPrincipal();
        String userid = loginUser.getUser().getId().toString();
        String jwt= JwtUtil.createJWT(userid);
        Map<String,String> map=new HashMap<>();
        map.put("token",jwt);
        //并且把完整的消息用户存入redis
        redisCache.setCacheObject("login:"+userid,loginUser);
return new ResponseResult(200,"登陆成功",map);
    }
```

**Authentication**:身份验证（身份信息交互的纽带，职责有两个：第一个是封装验证请求的参数，第二个便是封装用户的权限信息）

principal用于存放用户的身份标识信息，比如用户名，credentials用于存放用户的验证凭证比如密码，authorities用于存放用户的权限列表。而details则存放除了用户名和密码其他可能会被用于身份验证的信息，比如应用限定用户的使用ip范围场景下，ip信息可能便会被存放在details做辅助的验证信息使用。

**AuthenticationMananger**[AuthenticationManager - 溪水静幽 - 博客园 (cnblogs.com)](https://www.cnblogs.com/loveLands/articles/16535805.html)

[(54条消息) AuthenticationManager验证原理分析_authentication的成员变量_wh柒八九的博客-CSDN博客](https://blog.csdn.net/qq_31960623/article/details/120864983)

数据验证？：

接口**UserDetailsService**和实现类**UserDetailServiceImpl**：完成对数据库数据的搜索

该接口有UserDetails loadUserByUsername(String username)方法：返回的是LoginUser（继承了UserDetails）

依赖

```
<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

<img src="C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20230511212505299.png" alt="image-20230511212505299" style="zoom:80%;" />

<img src="C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20230511215935815.png" alt="image-20230511215935815" style="zoom:80%;" />

思路

登录

1.自定义登录接口 调用ProviderManager的方法进行验证，如果认证通过，生成jwt，并把用户信息存入redis中

2.自定义UserDetailsService 在这个实现类中查询数据库

校验

1.定义jwt认证过滤器：获取token，解析token，从redis中获取用户信息

**核心代码实现**

创建一个类实现UserDetailsService接口，重写其中的方法。更加用户名从数据库中查询用户信息

~~~~java
/**
 * @Author 三更  B站： https://space.bilibili.com/663528522
 */
@Service
public class UserDetailsServiceImpl implements UserDetailsService {

    @Autowired
    private UserMapper userMapper;

    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        //根据用户名查询用户信息
        LambdaQueryWrapper<User> wrapper = new LambdaQueryWrapper<>();
        wrapper.eq(User::getUserName,username);
        User user = userMapper.selectOne(wrapper);
        //如果查询不到数据就通过抛出异常来给出提示
        if(Objects.isNull(user)){
            throw new RuntimeException("用户名或密码错误");
        }
        //TODO 根据用户查询权限信息 添加到LoginUser中
        
        //封装成UserDetails对象返回 
        return new LoginUser(user);
    }
}
~~~~

因为UserDetailsService方法的返回值是UserDetails类型，所以需要定义一个类，实现该接口，把用户信息封装在其中。

Spring Security 会将前端填写的username 传给 UserDetailService.loadByUserName 方法。我们只需要从数据库中根据用户名查找到用户信息然后封装为UserDetails的实现类返回给SpringSecurity 即可，咱们自己不需要进行密码的比对工作，密码比对交由SpringSecurity 处理。


```java
/**
 * @Author 三更  B站： https://space.bilibili.com/663528522
 */
@Data
@NoArgsConstructor
@AllArgsConstructor
public class LoginUser implements UserDetails {

    private User user;


    @Override
    public Collection<? extends GrantedAuthority> getAuthorities() {
        return null;
    }

    @Override
    public String getPassword() {
        return user.getPassword();
    }

    @Override
    public String getUsername() {
        return user.getUserName();
    }

    @Override
    public boolean isAccountNonExpired() {
        return true;
    }

    @Override
    public boolean isAccountNonLocked() {
        return true;
    }

    @Override
    public boolean isCredentialsNonExpired() {
        return true;
    }

    @Override
    public boolean isEnabled() {
        return true;
    }
}
```



**密码加密存储**

​	实际项目中我们不会把密码明文存储在数据库中。

​	默认使用的PasswordEncoder要求数据库中的密码格式为：{id}password 。它会根据id去判断密码的加密方式。但是我们一般不会采用这种方式。所以就需要替换PasswordEncoder。

​	我们一般使用SpringSecurity为我们提供的BCryptPasswordEncoder。

​	我们只需要使用把BCryptPasswordEncoder对象注入Spring容器中，SpringSecurity就会使用该PasswordEncoder来进行密码校验。

​	我们可以定义一个SpringSecurity的配置类，SpringSecurity要求这个配置类要继承WebSecurityConfigurerAdapter。

~~~~java
/**
 * @Author 三更  B站： https://space.bilibili.com/663528522
 */
@Configuration
public class SecurityConfig extends WebSecurityConfigurerAdapter {


    @Bean
    public PasswordEncoder passwordEncoder(){
        return new BCryptPasswordEncoder();
    }

}
~~~~

![img](https://img-blog.csdnimg.cn/f87e388f68ad4ba88c73f7b3527d838c.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBASVRLYXZlbg==,size_20,color_FFFFFF,t_70,g_se,x_16)

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.5.0</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>
	<groupId>com.itheima</groupId>
	<artifactId>springboot_09_ssm</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<name>springboot_09_ssm</name>
	<description>Demo project for Spring Boot</description>
	<properties>
		<java.version>1.8</java.version>
	</properties>
	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>
		<dependency>
			<groupId>org.mybatis.spring.boot</groupId>
			<artifactId>mybatis-spring-boot-starter</artifactId>
			<version>2.2.0</version>
		</dependency>

		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
			<scope>runtime</scope>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
		<!--TODO 添加必要的依赖坐标-->
		<dependency>
			<groupId>com.alibaba</groupId>
			<artifactId>druid</artifactId>
			<version>1.1.16</version>
		</dependency>
	</dependencies>

	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>

</project>

```

![image-20240205161123113](C:\Users\13030\AppData\Roaming\Typora\typora-user-images\image-20240205161123113.png)
