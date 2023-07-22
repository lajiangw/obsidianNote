
# 1. SSM 整合
# 1.0.1 步骤

①Spring整合上Mybatis

通过Service层Dao层都注入Spring容器中

②引入配置SpringMVC

把Controller层注入SpringMVC容器中

③让web项目启动时自动读取Spring配置文件来创建Spring容器

可以使用 ContextLoaderListener 来实现 Spring 容器的创建。

[[容器图解]]
![[图片/Pasted image 20230113164551.png]]

## 1.0.2 常见疑惑

-   为什么要用两个容器？   
    
    **因为Controller如果不放在MVC容器中会没有效果，无法处理请求。而Service如果不放在Spring容器中，声明式事务也无法使用。**
    
-   SpringMVC容器中的Controller需要依赖Service，能从Spring容器中获取到所依赖的Service对象嘛？
    
    **Spring容器相当于是父容器，MVC容器相当于是子容器。子容器除了可以使用自己容器中的对象外还可以使用父容器中的对象。**
    
-   是如何实现这样父子容器的关系的？
     
    **具体可以看源码解析阶段的视频。但是我们目前可以用代码模拟下**。
    
-   是什么时候让两个容器产生这种父子容器的关系的？
    
    **在ContextLoaderListener中，会在创建好容器后把容器存入servletContext域。这样在DispatcherServlet启动时，创建完SpringMVC容器后会从servletContext域中获取到Spring容器对象，设置为其父容器,这样子容器就能获取到父容器中的bean了。详情请见源码解析视频。**
    

SpringMVC容器中的Controller需要依赖Service，能从Spring容器中获取到所依赖的Service对象嘛？
Spring 容器相当于父容器，MVC 相当于子容器，子容器是可以使用自己父容器中的对象外还可以使用父容器中的对象。

是如何实现这样父子容器的关系的？
我们在父容器中设置了对象，当在子容器中去获取父容器中的那个值得时候，他会判断我们是否设置过子容器的值，如果有，他会直接去取子容器的值，如果没有，他会判断是否设置过父容器相同的值，如果有，他就会取到父容器的值
# 准备工作
根据自己的项目选择依赖，不需要可以剔除

```xml
<dependencies>  
  <!--Spring-context-->  
  <dependency>  
    <groupId>org.springframework</groupId>  
    <artifactId>spring-context</artifactId>  
    <version>5.1.9.RELEASE</version>  
  </dependency>  
  <!--AOP相关依赖-->  
  <dependency>  
    <groupId>org.aspectj</groupId>  
    <artifactId>aspectjweaver</artifactId>  
    <version>1.8.13</version>  
  </dependency>  
  <!-- spring-jdbc -->  
  <dependency>  
    <groupId>org.springframework</groupId>  
    <artifactId>spring-jdbc</artifactId>  
    <version>5.1.9.RELEASE</version>  
  </dependency>  
  <!-- mybatis整合到Spring的整合包 -->  
  <dependency>  
    <groupId>org.mybatis</groupId>  
    <artifactId>mybatis-spring</artifactId>  
    <version>2.0.4</version>  
  </dependency>  
  <!--mybatis依赖-->  
  <dependency>  
    <groupId>org.mybatis</groupId>  
    <artifactId>mybatis</artifactId>  
    <version>3.5.4</version>  
  </dependency>  
  <!--log4j依赖，打印mybatis日志-->  
  <dependency>  
    <groupId>log4j</groupId>  
    <artifactId>log4j</artifactId>  
    <version>1.2.17</version>  
  </dependency>  
  <!--分页查询，pagehelper-->  
  <dependency>  
    <groupId>com.github.pagehelper</groupId>  
    <artifactId>pagehelper</artifactId>  
    <version>4.0.0</version>  
  </dependency>  
  
  <!--mysql驱动-->  
  <dependency>  
    <groupId>mysql</groupId>  
    <artifactId>mysql-connector-java</artifactId>  
    <version>8.0.29</version>  
  </dependency>  
  <!-- druid数据源 -->  
  <dependency>  
    <groupId>com.alibaba</groupId>  
    <artifactId>druid</artifactId>  
    <version>1.1.16</version>  
  </dependency>
```

## 2 相关配置

### ①整合 Spring 和 Mybatis

在 resources 目录下创建 Spring 核心配置文件： **applicationContext. xml** 内容如下
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context" xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd http://www.springframework.org/schema/aop https://www.springframework.org/schema/aop/spring-aop.xsd">

    <!--组件扫描，排除controller-->
    <context:component-scan base-package="com.sangeng">
        <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"></context:exclude-filter>
    </context:component-scan>

    <!--读取properties文件-->
    <context:property-placeholder location="classpath:jdbc.properties"></context:property-placeholder>
    <!--创建连接池注入容器-->
    <bean class="com.alibaba.druid.pool.DruidDataSource" id="dataSource">
        <property name="url" value="${jdbc.url}"></property>
        <property name="username" value="${jdbc.username}"></property>
        <property name="password" value="${jdbc.password}"></property>
        <property name="driverClassName" value="${jdbc.driver}"></property>
    </bean>
    <!--spring整合mybatis后控制的创建获取SqlSessionFactory的对象-->
    <bean class="org.mybatis.spring.SqlSessionFactoryBean" id="sessionFactory">
        <!--配置连接池-->
        <property name="dataSource" ref="dataSource"></property>
        <!--配置mybatis配置文件的路径-->
        <property name="configLocation" value="classpath:mybatis-config.xml"></property>
    </bean>

    <!--mapper扫描配置，扫描到的mapper对象会被注入Spring容器中-->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer" id="mapperScannerConfigurer">
        <property name="basePackage" value="com.sangeng.dao"></property>
    </bean>

    <!--开启aop注解支持-->
    <aop:aspectj-autoproxy></aop:aspectj-autoproxy>

    <!--声明式事务相关配置-->
    <bean class="org.springframework.jdbc.datasource.DataSourceTransactionManager" id="transactionManager">
        <property name="dataSource" ref="dataSource"></property>
    </bean>
    <tx:annotation-driven transaction-manager="transactionManager"></tx:annotation-driven>

</beans>
```

在 resources 目录下创建**jdbc. properties** 文件，内容如下：
```xml
jdbc.url=jdbc:mysql://localhost:3306/db_test?useSSL=false&allowPublicKeyRetrieval=true&serverTimezone=UTC  
jdbc.driver=com.mysql.cj.jdbc.Driver  
jdbc.username=root  
jdbc.password=root
```

在 resources 目录下创建**mybatis-config. xml** ，内容如下：
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <settings>
        <!--指定使用log4j打印Mybatis日志-->
        <setting name="logImpl" value="LOG4J"/>
    </settings>
    <!--配置别名包-->
    <typeAliases>
        <package name="com.sangeng.domain"></package>
    </typeAliases>
    <plugins>
        <!-- 注意：分页助手的插件，配置在通用mapper之前 -->
        <plugin interceptor="com.github.pagehelper.PageHelper">
            <!-- 指定方言 -->
            <property name="dialect" value="mysql"/>
        </plugin>
    </plugins>
</configuration>
```

在 resources 目录下创建**log4j. properties** ，内容如下：
```xml
### direct log messages to stdout ###
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.Target=System.out
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%d{ABSOLUTE} %5p %c{1}:%L - %m%n

### direct messages to file mylog.log ###
log4j.appender.file=org.apache.log4j.FileAppender
log4j.appender.file.File=c:/mylog.log
log4j.appender.file.layout=org.apache.log4j.PatternLayout
log4j.appender.file.layout.ConversionPattern=%d{ABSOLUTE} %5p %c{1}:%L - %m%n

### set log levels - for more verbose logging change 'info' to 'debug' ###

log4j.rootLogger=debug, stdout
```
### ②SpringMVC 引入
在 resources 目录下创建**spring-mvc. xml** ，内容如下：
```xml

<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd">
    <!--
         SpringMVC只扫描controller包即可
     -->
    <context:component-scan base-package="com.sangeng.controller"/>
    <!-- 解决静态资源访问问题，如果不加mvc:annotation-driven会导致无法访问handler-->
    <mvc:default-servlet-handler/>
    <!--解决响应乱码-->
    <mvc:annotation-driven>
        <mvc:message-converters>
            <bean class="org.springframework.http.converter.StringHttpMessageConverter">
                <constructor-arg value="utf-8"/>
            </bean>
        </mvc:message-converters>
    </mvc:annotation-driven>


    <!--配置视图解析器  前后端不分离项目使用-->
<!--    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="viewResolver">
        &lt;!&ndash;要求拼接的前缀&ndash;&gt;
        <property name="prefix" value="/WEB-INF/page/"></property>
        &lt;!&ndash;要拼接的后缀&ndash;&gt;
        <property name="suffix" value=".jsp"></property>
    </bean>-->

    <!--配置拦截器-->
<!--    <mvc:interceptors>

        <mvc:interceptor>
            &lt;!&ndash;
            &ndash;&gt;
            <mvc:mapping path="/**"/>
            &lt;!&ndash;配置排除拦截的路径&ndash;&gt;
            <mvc:exclude-mapping path="/"/>
            &lt;!&ndash;配置拦截器对象注入容器&ndash;&gt;
            <bean class=""></bean>
        </mvc:interceptor>
    </mvc:interceptors>-->

    <!--
          文件上传解析器
          注意：id 必须为 multipartResolver
          如果需要上传文件时可以放开相应配置
      -->
    <!--<bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">-->
        <!--&lt;!&ndash; 设置默认字符编码 &ndash;&gt;-->
        <!--<property name="defaultEncoding" value="utf-8"/>-->
        <!--&lt;!&ndash; 一次请求上传的文件的总大小的最大值，单位是字节&ndash;&gt;-->
        <!--<property name="maxUploadSize" value="#{1024*1024*100}"/>-->
        <!--&lt;!&ndash; 每个上传文件大小的最大值，单位是字节&ndash;&gt;-->
        <!--<property name="maxUploadSizePerFile" value="#{1024*1024*50}"/>-->
    <!--</bean>-->
</beans>
```

修改 web. xml 文件
```xml
 <servlet>
        <servlet-name>DispatcherServlet</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!--
            为DispatcherServlet提供初始化参数的
            设置springmvc配置文件的路径
                name是固定的，必须是contextConfigLocation
                value指的是SpringMVC配置文件的位置
         -->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:spring-mvc.xml</param-value>
        </init-param>
        <!--
            指定项目启动就初始化DispatcherServlet
         -->
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>DispatcherServlet</servlet-name>
        <!--
             /           表示当前servlet映射除jsp之外的所有请求（包含静态资源）
             *.do        表示.do结尾的请求路径才能被SpringMVC处理(老项目会出现)
             /*          表示当前servlet映射所有请求（包含静态资源,jsp），不应该使用其配置DispatcherServlet
         -->
        <url-pattern>/</url-pattern>
    </servlet-mapping>


    <!--乱码处理过滤器，由SpringMVC提供-->
    <!-- 处理post请求乱码 -->
    <filter>
        <filter-name>CharacterEncodingFilter</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <!-- name固定不变，value值根据需要设置 -->
            <param-name>encoding</param-name>
            <param-value>UTF-8</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>CharacterEncodingFilter</filter-name>
        <!-- 所有请求都设置utf-8的编码 -->
        <url-pattern>/*</url-pattern>
    </filter-mapping>
```

### ③Spring 整合入 web 项目
让 web 项目启动的时候就能够创建 Spring 容器。可以使用 Spring 提供的监听器 ContextLoaderListener，所以我们需要再 web. xml 中配置这个监听器, 并且配置上 Spring 配置文件的路径。
```xml

    <!--配置spring的配置文件路径-->
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:applicationContext.xml</param-value>
    </context-param>
    <!--配置监听器，可以再应用被部署的时候创建spring容器-->
    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>
```

# 1.3 编写 Controller, Service，Dao

我们来编写根据 id 查询用户的接口来进行测试

## 响应统一格式
我们要保证一个项目中所有接口返回的数据格式的统一。这样无论是前端还是移动端开发获取到我们的数据后都能更方便的进行统一处理。
我们将所有的返回信息都封装到这个类里面：
所以我们定义以下结果封装类：
```java
@JsonInclude(JsonInclude.Include.NON_NULL)
//当某个属性不为null的时候，才会把它转换为json
public class ResponseResult<T> {
    /**
     * 状态码
     */
    private Integer code;
    /**
     * 提示信息，如果有错误时，前端可以获取该字段进行提示
     */
    private String msg;
    /**
     * 查询到的结果数据，
     */
    private T data;

    public ResponseResult(Integer code, String msg) {
        this.code = code;
        this.msg = msg;
    }

    public ResponseResult(Integer code, T data) {
        this.code = code;
        this.data = data;
    }

    public Integer getCode() {
        return code;
    }

    public void setCode(Integer code) {
        this.code = code;
    }

    public String getMsg() {
        return msg;
    }

    public void setMsg(String msg) {
        this.msg = msg;
    }

    public T getData() {
        return data;
    }

    public void setData(T data) {
        this.data = data;
    }

    public ResponseResult(Integer code, String msg, T data) {
        this.code = code;
        this.msg = msg;
        this.data = data;
    }
}
```
## 查询所有用户
Controller 层
```java
@GetMapping("/user")  
public ResponseResult findAll() {  
    List<User> user = userService.findAll();  
    if (user == null) {  
        return new ResponseResult<>(404, "未找到");  
    }    return new ResponseResult<>(200, "操作成功！", user);  
}
```

Service 层接口
```java
List<User> findAll();
```

Service 层接口实现类
```java
@Override  
public List<User> findAll() {  
    return userDao.findAll();  
}
```

mapper 层接口
```java
List<User> findAll();
```

Mapper xml 文件
```xml
<select id="findAll" resultType="user">  
    select *  
    from user;  
</select>
```

## 分页查询
分页查询的结果除了要包含查到的用户数据外还要有当前页数，每页条数，总记录数这些分页数据。
我们需要在封装一个类来存放这些数据：
```java

public class PageResult<T> {

    private Integer currentPage;

    private Integer pageSize;

    private Integer total;
    
    private List<T> data;

    public PageResult(Integer currentPage, Integer pageSize, Integer total, List<T> data) {
        this.currentPage = currentPage;
        this.pageSize = pageSize;
        this.total = total;
        this.data = data;
    }

    public Integer getCurrentPage() {
        return currentPage;
    }

    public void setCurrentPage(Integer currentPage) {
        this.currentPage = currentPage;
    }

    public Integer getPageSize() {
        return pageSize;
    }

    public void setPageSize(Integer pageSize) {
        this.pageSize = pageSize;
    }

    public Integer getTotal() {
        return total;
    }

    public void setTotal(Integer total) {
        this.total = total;
    }

    public List<T> getData() {
        return data;
    }

    public void setData(List<T> data) {
        this.data = data;
    }
}

```

Controller 层
```java
@GetMapping("/user/{pageSize}/{pageNub}")  
public ResponseResult findByPage(@PathVariable("pageSize") Integer pageSize, @PathVariable("pageNub") Integer pageNub) {  
    PageResult pageResult = userService.findBuPage(pageSize, pageNub);  
    return new ResponseResult(200, "操作成功！", pageResult);  
}
```

Service 层接口
```java
PageResult findBuPage(Integer pageSize, Integer pageNub);
```

Service 层接口实现类
```java
@Override  
public PageResult findBuPage(Integer pageSize, Integer pageNub) {  
    PageHelper.startPage(pageNub, pageSize);  
    List<User> user = userDao.findAll();  
    PageInfo<User> pageInfo = new PageInfo<>(user);  
    return new PageResult(pageInfo.getFirstPage(), pageInfo.getPageSize(), (int) pageInfo.getTotal(), user);  
}
```

mapper 层接口
```java
List<User> findAll();
```

Mapper xml 文件
```xml
<select id="findAll" resultType="user">  
    select *  
    from user  
</select>
```

##  插入用户

Controller 层
```java
@PostMapping("/user")  
public ResponseResult insertUser(@RequestBody User user) {  
    userService.insertUser(user);  
    return new ResponseResult(200, "操作成功！");
```

Service 层接口
```java
void insertUser(User user);
```

Service 层接口实现类
```java
@Override  
public void insertUser(User user) {  
    userDao.insertUser(user);  
}
```

mapper 层接口
```java
void insertUser(User user);
```

Mapper xml 文件
```xml
<insert id="insertUser">  
    insert into user  
    values (null, #{userName}, #{age}, #{address})  
</insert>
```

## 删除用户

Controller 层
```java
@DeleteMapping("/user/{id}")  
public ResponseResult deleteUser(@PathVariable("id") Integer id) {  
    userService.deleteUser(id);  
    return new ResponseResult(200, "操作成功");  
}
```

Service 层接口
```java
void deleteUser(Integer id);
```

Service 层接口实现类
```java
@Override  
public void deleteUser(Integer id) {  
    userDao.deleteUser(id);  
}
```

mapper 层接口
```java
void deleteUser(@Param("id") Integer id);
```

Mapper xml 文件
```xml
<delete id="deleteUser">  
    delete  
    from user  
    where id = #{id}  
</delete>
```

## 更新用户

Controller 层
```java
@PutMapping("/user/{id}")  
public ResponseResult updateUser(User user) {  
    userService.updateUser(user);  
    return new ResponseResult(200, "操作成功");  
}
```

Service 层接口
```java
void updateUser(User user);
```

Service 层接口实现类
```java
@Override  
public void updateUser(User user) {  
    userDao.updateUser(user);  
}
```

mapper 层接口
```java
void updateUser(User user);
```

Mapper xml 文件
```xml
<update id="updateUser">  
 update user set user_name = #{userName},age = #{age},address = #{address} where id = #{id}  
</update>
```

# 异常统一处理
我们可以使用@ControllerAdvice 实现对异常的统一处理。让异常出现时也能返回响应一个 JSON。
代码如下：
```java
@ControllerAdvice
//将此类标示为异常处理
public class zmlControllerAdvice {  
  
    @ExceptionHandler(Exception.class)  
    //将需要处理的异常class文件放入注解参数
    @ResponseBody  
    public ResponseResult handleException(Exception exception) {  
        return new ResponseResult(500, exception.getMessage());  
    }}
```

# 拦截器
实现 HandlerInterceptor 的实现类，并在 SpringMVC 配置文件中配置拦截器

```java
{  
    @Override  
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {  
        return true;  
    }  
    @Override  
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {  
        System.out.println("postHandle");  
    }  
    @Override  
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {  
        System.out.println("afterCompletion");  
    }}
```

```xml
<mvc:interceptors>  
  
       <mvc:interceptor>  
  
           <mvc:mapping path="/**"/>  
           <mvc:exclude-mapping path="/"/>  
           <bean class="com.zml.interceptor.ZmlInterceptor"></bean>  
       </mvc:interceptor>  
   </mvc:interceptors>
```

# 声明式事务
已经做好了相应的配置，只要在 service 方法上加上注解即可；
```java
@Override  
@Transactional  
public void test() {  
    insertUser(new User(null, "zml", 19, "河北"));  
    System.out.println(1/0);  
    insertUser(new User(null, "www", 19, "河北"));  
}
```

# AOP
注意，自己去使用 AOP 进行增强时，应该是对 Service 进行增强。不能对 Controller 进行增强，因为切面类会被放入父容器，而我们的 Controller 是在子容器中的。父容器不能访问子容器。
![[图片/Pasted image 20230113164533.png]]
**并且我们如果需要对 Controller 进行增强，使用拦截器也会更加的好用。**
```java
  
@Aspect  
@Component  
public class Zmlaspect {  
  
  
    @Pointcut("execution(* com.zml.service..*.*(..))")  
    public void pt(){  
  
    }  
    @Before("pt()")  
    public void before(){  
        System.out.println("befort");  
    }
    }
```

