# 1. SpringBoot 简介
## 1.1 为什么要学习 SpringBoot
我们之前的 SSM 还是使用起来不够爽。

-   还需要写很多的配置才能进行正常的使用。
    
-   实现一个功能需要引入很多的依赖，尤其是要自己去维护依赖的版本，特别容易出现依赖冲突等问题。
    

SpringBoot 就能很好的解决上述问题。

## 1.2 SpringBoot 是什么
Spring Boot 是基于 Spring 开发的全新框架，相当于对 Spring 做了又一层封装。仅仅只是封装了 Spring，如果使用 MVC 和 Mybat 还需要引入。

其设计目的是用来简化**Spring**应用的初始搭建以及开发过程。该框架使用了特定的方式来进行配置，从而使开发人员不再需要定义样板化的配置。（自动配置）
并且对第三方依赖的添加也进行了封装简化。（起步依赖）
所以Spring能做的他都能做，并且简化了配置。
并且还提供了一些Spring所没有的比如：
-   内嵌web容器，不再需要部署到web容器中
    提供准备好的特性，如指标、健康检查和外部化配置

最大特点：**自动配置**、**起步依赖**

官网：[https://spring.io/projects/spring-boot](https://spring.io/projects/spring-boot)

# 2. 快速入门
## 2.1 基本环境要求
JDK : 8
Maven ：3.5. x
## 2.2 HelloWorld
①继承父工程

在 pom. xml 中添加一下配置，继承 spring-boot-starter-parent 这个父工程
```xml
<parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.5.0</version>
    </parent>
```

②添加依赖，这里添加依赖不需要制定版本号，因为继承父类工程。
```xml
<dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>
```

③创建启动类
创建一个类在其实加上@SpringBootApplication 注解标识为启动类。
```java
@SpringBootApplication  
public class Test {  
    public static void main(String[] args) {  
        System.out.println("hello");  
    }
```

④定义 Controller
创建 Controller, 主要 Controller 要放在启动类所在包或者其子包下。
```java
@RestController
public class HelloController {
    @RequestMapping("/hello")
    public String hello(){
        return "hello";
    }
}
```

⑤运行测试
直接**运行启动类的 main 方法**即可。

## 2.4 打包运行
我们可以把 springboot 的项目打成 jar 包直接去运行。
需要添加 maven 插件才可以打包
```xml
   <build>  
  <plugins>  
    <!--springboot打包插件-->  
    <plugin>  
      <groupId>org.springframework.boot</groupId>  
      <artifactId>spring-boot-maven-plugin</artifactId>  
      <version>2.5.0</version>  
    </plugin>  
  </plugins>  
</build>
```

打包成 jar 即可运行。
SpringBoot 的打包成 jar 也是可以运行的，比我们之前的项目运行方便很多。

## 2.5 SpringBoot 的快速构建
在 idea 选择 Spring 的快速构建，即可快速创建一个 SpringBoot 项目

## 3. 起步依赖
SpringBoot 依靠父项目中的**版本锁定和 starter 机制**让我们能更轻松的实现对依赖的管理。

## 3.0 依赖冲突及其解决方案
### 3.0.1 依赖冲突
一般程序在运行时发生类似于 java. lang. ClassNotFoundException，Method not found: '……'，或者莫名其妙的异常信息，这种情况一般很大可能就是 jar 包依赖冲突的问题引起的了。

一般在是 A 依赖 C (低版本)，B 也依赖 C (高版本)。都是他们依赖的又是不同版本的 C 的时候会出现。

### 3.0.2 解决方案
如果出现了类似于 java.lang.ClassNotFoundException，Method not found: 这些
异常检查相关的依赖冲突问题，排除掉低版本的依赖，留下高版本的依赖。

## 3.1 版本锁定
我们的 SpringBoot 模块都需要继承一个父工程：**spring-boot-starter-parent**。在 spring-boot-starter-parent 的父工程**spring-boot-dependencies**中对常用的依赖进行了版本锁定。这样我们在添加依赖时，很多时候都不需要添加依赖的版本号了。
我们也可以采用覆盖 properties 配置或者直接指定版本号的方式修改依赖的版本。
例如：
直接指定版本号
```xml
     <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            <version>1.7.2</version>
        </dependency>
```

## 3.2 starter 机制

当我们需要使用某种功能时只需要引入对应的starter即可。一个starter针对一种特定的场景，其内部引入了该场景所需的依赖。这样我们就不需要单独引入多个依赖了。

命名规律
-   **官方starter都是以 `spring-boot-starter`开头后面跟上场景名称。例如：spring-boot-starter-data-jpa**
    
-   **非官方 starter 则是以 `场景名-spring-boot-starter` 的格式，例如：mybatis-spring-boot-starter**


# 4. 自动配置
SpringBoot 中最重要的特性就是自动配置。

Springboot 遵循“**约定优于配置**”的原则，自动进行了默认配置。这样我们就不需要做大量的配置。
比如说我们的拦截器，静态资源访问，SpringBoot 帮我们自动配置好了。

当我们需要使用什么场景时，就会自动配置这个场景相关的配置。

如果他的默认配置不符合我们的需求时修改这部分配置即可。
# 5. YML 配置
## 5.1. 简介
### 5.1.1YML 是什么
YAML (YAML Ain't a Markup Language) YAML 不是一种标记语言，通常以. yml 为后缀的文件，是一种直观的能够被电脑识别的数据序列化格式，并且容易被人类阅读，容易和脚本语言交互的，可以被支持 YAML 库的不同的编程语言程序导入，**一种专门用来写配置文件的语言**。

YAML 试图用一种比 XML 更敏捷的方式，来完成 XML 所完成的任务。
例如：
Yml
```yml
student:
    name: sangeng
    age: 15
```
Xml
```xml
<student>
    <name>sangeng</name>
    <age>15</age>
</student>
```

### 5.1.2YML 优点
1.  YAML易于人们阅读。
2.  更加简洁明了

## 5.2. 语法
### 5.2.1 约定
-   k: v 表示键值对关系，**冒号后面必须有一个空格**
-   使用空格的缩进表示层级关系，空格数目不重要，**只要是左对齐的一列数据，都是同一个层级的**
-   大小写敏感
-   缩进时**不允许使用Tab键，只允许使用空格**。
-   java中对于驼峰命名法，可用原名或使用-代替驼峰，如java中的lastName属性,在yml中使用lastName或 last-name都可正确映射。
-   yml中注释前面要加#

### 5.2.2 键值关系

#### 普通值(字面量)

k: v：字面量直接写；

字符串默认不用加上单引号或者双绰号；

"": 双引号；转意字符能够起作用，而单引号对转义字符无效。

name: "sangeng \n caotang"：输出；sangeng 换行 caotang

''：单引号；会转义特殊字符，特殊字符最终只是一个普通的字符串数据。
```yml
name1: sangeng 
name2: 'sangeng  \n caotang'
name3: "sangeng  \n caotang"
age: 15
flag: true
```

#### 日期
日期需要用/分隔开
```yml
date: 2019/01/01
```

#### 对象 (属性和值)、Map (键值对)
多行写法：
在下一行来写对象的属性和值的关系，注意缩进
```yml
student:
  name: zhangsan
  age: 20
```

行内写法：
```yml
student: {name: zhangsan,age: 20}
```

#### 数组、list、set
用- 值表示数组中的一个元素
多行写法：
```yml
pets:
  - dog
  - pig
  - cat
```

行内写法：
```yml
pets: [dog,pig,cat]
```

#### 对象数组、对象 list、对象 set、
```yml
students:
 - name: zhangsan
   age: 22
 - name: lisi
   age: 20
 - {name: wangwu,age: 18}
```

### 5.2.3 占位符赋值
可以使用 **${key: defaultValue}** 的方式来赋值，若 key 不存在，则会使用 defaultValue 来赋值。
例如
```yml
server:
  port: ${myPort:88}

myPort: 80   
```

## 5.3.SpringBoot读取YML
### 5.3.1 @Value 注解
只需要在赋值的参数上面加上注解格式即可。
注意使用此注解只能获取简单类型的值（8 种基本数据类型及其包装类，String, Date）
```java
@RestController
public class TestController {
    @Value("${student.lastName}")
    private String lastName;
    @RequestMapping("/test")
    public String test(){
        System.out.println(lastName);
        return "hi";
    }
}
```

### 5.3.2 @ConfigurationProperties
yml 配置
```yaml
user:  
  userName: "zhangsan"  
  password: 123
```

在类上添加注解@Component 和@ConfigurationProperties (prefix = "配置前缀")
prefix 的值就是 key 键，否则会无法具体定位具体是哪个值。
**封装类一定要有 Get Set 方法。**
```java
@Component  
@ConfigurationProperties(prefix = "user")  
public class User {  
    private String userName;  
    private String password;  
  
  
    public User(String userName, String password) {  
        this.userName = userName;  
        this.password = password;  
    }  
  
    public User() {  
    }  
  
    public String getUserName() {  
        return userName;  
    }  
  
    public void setUserName(String userName) {  
        this.userName = userName;  
    }  
  
    public String getPassword() {  
        return password;  
    }  
  
    public void setPassword(String password) {  
        this.password = password;  
    }  
      
  
    @Override  
    public String toString() {  
        return "User{" +  
                "userName='" + userName + '\'' +  
                ", password='" + password + '\'' +  
                '}';  
    }  
}
```
从 Spring 容器中获取到这个对象
```java
@Autowired  
private User user;
```

## 5.4. 练习
要求把下面实体类中的各个属性在 yml 文件中进行赋值。然后想办法读取 yml 配置的属性值，进行输出测试
```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class Student {
    private String lastName;
    private Integer age;
    private Boolean boss;

    private Date birthday;
    private Map<String,String> maps;
    private Map<String,String> maps2;
    private List<Dog> list;

    private Dog dog;
    private String[] arr;
    private String[] arr2;

    private Map<String,Dog> dogMap;
}
@Data
@AllArgsConstructor
@NoArgsConstructor
class Dog {
    private String name;
    private Integer age;
}
```

```yml
# 练习
student:
  lastName: sangeng
  age: 15
  boss: true
  birthday: 2006/2/3
  maps:
    name: sangeng
    age: 11
  maps2: {name: caotang,age: 199}
  list:
    - name: 小白
      age: 3
    - name: 小黄
      age: 4
    - {name: 小黑,age: 1}
  dog:
    name: 小红
    age: 5
  arr:
    - sangeng
    - caotang

  arr2: [sangeng,caotang]
  dogMap:
    xb: {name: 小白,age: 9}
    xh:
      name: 小红
      age: 6

```

## 5.5 YML 和 properties 配置的相互转换
我们可以使用一些网站非常方便的实现 YML 和 properties 格式配置的相互转换。

转换网站：[https://www.toyaml.com/index.html](https://www.toyaml.com/index.html)

# 配置提示
如果使用了@ConfigurationProperties 注解，可以增加以下依赖，让我们在书写配置（yml 或者 xml 文件）时有相应的提示。
在 pom.xml 文件中添加依赖
```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-configuration-processor</artifactId>
            <optional>true</optional>
        </dependency>
```
**注意：添加完依赖加完注解后要运行一次程序才会有相应的提示。**

# 热部署
SpringBoot 为我们提供了一个方便我们开发测试的工具 dev-tools。使用后可以实现热部署的效果。当我们运行了程序后对程序进行了修改，程序会自动重启。

原理是使用了两个 ClassLoder, 一个 ClassLoader 加载哪些不会改变的类 (第三方 jar 包), 另一个 ClassLoader 加载会更改的类. 称之为 Restart ClassLoader, 这样在有代码更改的时候, 原来的 Restart Classloader 被丢弃, 重新创建一个 Restart ClassLoader, 由于需要加载的类相比较少, 所以实现了较快的重启。

1. 设置 idea自动编译
![[imgs/Pasted image 20230117184823.png]]

2. 设置允许自启动
![[imgs/Pasted image 20230117184945.png]]

3. 添加依赖
```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <optional>true</optional>
        </dependency>
```

- 触发热部署
	- 当我们在修改完代码或者静态资源后可以切换到其它软件，让 IDEA 自动进行编译，自动编译后就会触发热部署。
	- 或者使用 Ctrl+F9 手动触发重新编译。

# 2. 单元测试

我们可以使用SpringBoot整合Junit进行单元测试。

**Spring Boot 2.2.0 版本开始引入 JUnit 5 作为单元测试默认库**。

Junit5 功能相比于 Junit4 也会更强大。但是本课程是 SpringBoot 的课程，所以主要针对 SpringBoot 如何整合 Junit 进行单元测试做讲解。暂不针对 Junit5 的新功能做介绍。如有需要会针对 Junit5 录制专门的课程进行讲解。

## 使用
1. 添加依赖
```xml
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-test</artifactId>
            </dependency>
```

2. 编写测试类
```java
import com.sangeng.controller.HelloController;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;

//classes属性来指定启动类
@SpringBootTest(classes = HelloApplication.class)
public class ApplicationTest {

    @Autowired
    private HelloController helloController;

    @Test
    public void testJunit(){
        System.out.println(1);
        System.out.println(helloController);
    }
}
```
**注意：测试类所在的包需要和启动类是在同一个包下。否则就要使用如下写法指定启动类。**

## 2.2 兼容老版本
如果是对老项目中的 SpringBoot 进行了版本升级会发现之前的单元测试代码出现了一些问题。

因为Junit5和之前的Junit4有比较大的不同。

先看一张图：
![[imgs/junit5.jpeg]]
从上图可以看出 **JUnit 5 = JUnit Platform + JUnit Jupiter + JUnit Vintage**

-   **JUnit Platform**： 这是 Junit 提供的平台功能模块，通过它，其它的测试引擎也可以接入
    
-   **JUnit JUpiter**：这是JUnit5的核心，是一个基于JUnit Platform的引擎实现，它包含许多丰富的新特性来使得自动化测试更加方便和强大。
    
-   **JUnit Vintage**：这个模块是兼容 JUnit3、JUnit4 版本的测试引擎，使得旧版本的自动化测试也可以在 JUnit5 下正常运行。

虽然 Junit5 包含了**JUnit Vintage**来兼容 JUnit3 和 Junit4，但是**SpringBoot 2.4 以上版本对应的 spring-boot-starter-test 移除了默认对** **Vintage 的依赖。**所以当我们仅仅依赖 spring-boot-starter-test 时会发现之前我们使用的@Test 注解和@RunWith 注解都不能使用了。

我们可以单独在依赖 vintage 来进行兼容。
```xml
        <dependency>
            <groupId>org.junit.vintage</groupId>
            <artifactId>junit-vintage-engine</artifactId>
            <scope>test</scope>
        </dependency>
```

**注意：**

**org.junit.Test对应的是Junit4的版本，就搭配@RunWith注解来使用。**

SpringBoot2.2.0 之前版本的写法
```java
import com.sangeng.controller.HelloController;
//import org.junit.jupiter.api.Test;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.junit4.SpringRunner;

//classes属性来指定启动类
@SpringBootTest
@RunWith(SpringRunner.class)
public class ApplicationTest {

    @Autowired
    private HelloController helloController;

    @Test
    public void testJunit(){
        System.out.println(1);
        System.out.println(helloController);
    }
}
```

# 3. 整合 mybatis
## 3.1 准备工作
①数据准备
创建一个表作为测试数据

②实体类
创建对应实体类

## 整合步骤
github: [MyBatis整合文档版本要求](https://github.com/mybatis/spring-boot-starter/)

1. 添加依赖
```xml
<!--mybatis启动器-->  
<dependency>  
  <groupId>org.mybatis.spring.boot</groupId>  
  <artifactId>mybatis-spring-boot-starter</artifactId>  
  <version>2.2.0</version>  
</dependency>  
<!--mysql驱动-->  
<dependency>  
  <groupId>mysql</groupId>  
  <artifactId>mysql-connector-java</artifactId>  
  <version>8.0.29</version>  
</dependency>
```
2. 配置数据库信息
```yml
# Spring 整合Mybatis配置  
spring:  
  datasource:  
    url: jdbc:mysql://localhost:3306/MyBatis?useSSL=false&allowPublicKeyRetrieval=true&serverTimezone=UTC  
    username: root  
    password: root  
	driver-class-name: com.mysql.cj.jdbc.Driver
```

3. 配置 Mybat 

 classpath: mapper/ * Mapper. xml：表示包括所有结尾为 Mapper 的 xml 文件
```yml
# 配置Mybatis的配置  
mybatis:  
  mapper-locations: classpath:mapper/*Mapper.xml  mapper文件路径
  type-aliases-package: com.zml.bean   配置哪个包下的类有默认的别名
```

4.  编写 Mapper 接口

注意在接口上加上@Mapper 和@Repository 注解
@Repository 的作用和@Controller 类似，就是将类注入对象，idea 检查时不会报错。
```java
@Mapper  
@Repository  
public interface UserMapper {  
  
    List<User> findAllUser();  
}
```

5.  编写mapper接口对应的xml文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>  
<!DOCTYPE mapper  
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"  
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">  
<mapper namespace="com.zml.mapper.UserMapper">  
  
    <select id="findAllUser" resultType="User">  
        select *        from user    </select>  
</mapper>
```

测试即可
```java
@Autowired  
private UserMapper userMapper;  

@RequestMapping("/test2")  
public String test() {  
    List<User> allUser = userMapper.findAllUser();  
    System.out.println(allUser);  
    return allUser.toString();  
}
```

# Web 开发准备
由于 SpringBoot 的项目是打成 jar 包的所以没有之前 web 项目的那些 web 资源目录 (webapps)。
那么我们的静态资源要放到哪里呢？
从 SpringBoot 官方文档中我们可以知道，我们可以把静态资源放到 `resources/static` (或者 `resources/public` 或者 `resources/resources` 或者 `resources/META-INF/resources`) 中即可。
静态资源放完后，
例如我们想访问文件：resources/static/index. html 只需要在访问时资源路径写成/index. html 即可。

例如我们想访问文件：resources/static/pages/login. html 访问的资源路径写成： /pages/login. html

##  4.1.1 修改静态资源访问路径
SpringBoot 默认的静态资源路径匹配为/** 。如果想要修改可以通过 `spring.mvc.static-path-pattern` 这个配置进行修改。

例如想让访问静态资源的url必须前缀有/res。例如/res/index.html 才能访问到static目录中的。我们可以修改如下：

在 application. yml 中
```yml
spring:
  mvc:
    static-path-pattern: /res/** [[修改静态资源访问路径]]
```

## 4.1.2 修改静态资源存放目录
我们可以修改 spring. web. resources. static-locations 这个配置来修改静态资源的存放目录。
格式不可以写错！是一个集合，这两个路径都可以访问到
```yaml
spring:
  web:
    resources:
      static-locations:
        - classpath:/sgstatic/ 
        - classpath:/static/
```

##  响应体响应数据
无论是 RestFul 风格还是我们之前 web 阶段接触过的异步请求，都需要把数据转换成 Json 放入响应体中。

### 4.4.2.1 配置
SpringBoot 项目中使用了 web 的 start 后，不需要进行额外的依赖和配置

### 4.4.2.2 使用
只要把要转换的数据直接作为方法的返回值返回即可。SpringMVC 会帮我们把返回值转换成 json。具体代码请参考范例。

### 4.4.3 范例
#### 范例一
要求定义个RestFul风格的接口，该接口可以用来根据id查询用户。请求路径要求为 /response/user ，请求方式要求为GET。

而请求参数id要写在请求路径上，例如 /response/user/1 这里的1就是id。

要求获取参数 id, 去查询对应 id 的用户信息（模拟查询即可，可以选择直接 new 一个 User 对象），并且转换成 json 响应到响应体中。
实现效果就是页面显示这个 json 对信息。
```java
@Controller
@RequestMapping("/response")
public class ResponseController {

    @RequestMapping("/user/{id}")
    @ResponseBody
    public User findById(@PathVariable("id") Integer id){
        User user = new User(id, "三更草堂", 15, null);
        return user;
    }
}
```

## 4.5 跨域请求
### 4.5.1 什么是跨域
浏览器出于安全的考虑，使用 XMLHttpRequest 对象发起 HTTP 请求时必须遵守同源策略，否则就是跨域的 HTTP 请求，默认情况下是被禁止的。
**同源策略要求源相同才能正常进行通信，即协议、域名、端口号都完全一致。**

### 4.5.2 CORS 解决跨域
CORS 是一个 W3C 标准，全称是”跨域资源共享”（Cross-origin resource sharing），允许浏览器向跨源服务器，发出 XMLHttpRequest 请求，从而克服了 AJAX 只能同源使用的限制。

它通过服务器增加一个特殊的 Header [Access-Control-Allow-Origin] 来告诉客户端跨域的限制，如果浏览器支持 CORS、并且判断 Origin 通过的话，就会允许 XMLHttpRequest 发起跨域请求。
### 4.5.3 SpringBoot 使用 CORS 解决跨域
#### 1. 使用@CrossOrigin
可以在支持跨域的方法上或者是 Controller 上加上@CrossOrigin 注解
```java
@RestController
@RequestMapping("/user")
@CrossOrigin
public class UserController {

    @Autowired
    private UserServcie userServcie;

    @RequestMapping("/findAll")
    public ResponseResult findAll(){
        //调用service查询数据 ，进行返回
        List<User> users = userServcie.findAll();

        return new ResponseResult(200,users);
    }
}
```

#### 2. 使用 WebMvcConfigurer 的 addCorsMappings 方法配置 CorsInterceptor
直接使用这个配置类，来配置跨域请求的一些设置。
```java
@Configuration
public class CorsConfig implements WebMvcConfigurer {

    @Override
    public void addCorsMappings(CorsRegistry registry) {
      // 设置允许跨域的路径
        registry.addMapping("/**")
                // 设置允许跨域请求的域名
                .allowedOriginPatterns("*")
                // 是否允许cookie
                .allowCredentials(true)
                // 设置允许的请求方式
                .allowedMethods("GET", "POST", "DELETE", "PUT")
                // 设置允许的header属性
                .allowedHeaders("*")
                // 跨域允许时间
                .maxAge(3600);
    }
}
```

## 4.6 拦截器
### 4.6.0 登录案例
前后端分离登录效验流程
![[imgs/前后端分离登陆效验流程.png]]

#### 4.6.0.1 思路分析
在前后端分离的场景中，很多时候会采用 token 的方案进行登录校验。

登录成功时，后端会根据一些用户信息生成一个 token 字符串返回给前端。

前端会存储这个token。以后前端发起请求时如果有token就会把token放在请求头中发送给后端。

后端接口就可以获取请求头中的token信息进行解析，如果解析不成功说明token超时了或者不是正确的token，相当于是未登录状态。

如果解析成功，说明前端是已经登录过的。

#### 4.6.0.2 Token 生成方案-JWT
本案例采用目前企业中运用比较多的 JWT 来生成 token。

使用时先引入相关依赖
```xml
        <dependency>
            <groupId>io.jsonwebtoken</groupId>
            <artifactId>jjwt</artifactId>
            <version>0.9.0</version>
        </dependency>
```

然后可以使用下面的工具类来生成和解析 token
```java
import io.jsonwebtoken.Claims;
import io.jsonwebtoken.JwtBuilder;
import io.jsonwebtoken.Jwts;
import io.jsonwebtoken.SignatureAlgorithm;

import javax.crypto.SecretKey;
import javax.crypto.spec.SecretKeySpec;
import java.util.Base64;
import java.util.Date;
import java.util.UUID;

/**
 * JWT工具类
 */
public class JwtUtil {

    //有效期为
    public static final Long JWT_TTL = 60 * 60 *1000L;// 60 * 60 *1000  一个小时
    //设置秘钥明文
    public static final String JWT_KEY = "sangeng";

    /**
     * 创建token
     * @param id
     * @param subject
     * @param ttlMillis
     * @return
     */
    public static String createJWT(String id, String subject, Long ttlMillis) {

        SignatureAlgorithm signatureAlgorithm = SignatureAlgorithm.HS256;
        long nowMillis = System.currentTimeMillis();
        Date now = new Date(nowMillis);
        if(ttlMillis==null){
            ttlMillis=JwtUtil.JWT_TTL;
        }
        long expMillis = nowMillis + ttlMillis;
        Date expDate = new Date(expMillis);
        SecretKey secretKey = generalKey();

        JwtBuilder builder = Jwts.builder()
                .setId(id)              //唯一的ID
                .setSubject(subject)   // 主题  可以是JSON数据
                .setIssuer("sg")     // 签发者
                .setIssuedAt(now)      // 签发时间
                .signWith(signatureAlgorithm, secretKey) //使用HS256对称加密算法签名, 第二个参数为秘钥
                .setExpiration(expDate);// 设置过期时间
        return builder.compact();
    }

    /**
     * 生成加密后的秘钥 secretKey
     * @return
     */
    public static SecretKey generalKey() {
        byte[] encodedKey = Base64.getDecoder().decode(JwtUtil.JWT_KEY);
        SecretKey key = new SecretKeySpec(encodedKey, 0, encodedKey.length, "AES");
        return key;
    }
    
    /**
     * 解析
     *
     * @param jwt
     * @return
     * @throws Exception
     */
    public static Claims parseJWT(String jwt) throws Exception {
        SecretKey secretKey = generalKey();
        return Jwts.parser()
                .setSigningKey(secretKey)
                .parseClaimsJws(jwt)
                .getBody();
    }
}
```

#### 4.6.0.3 登录接口实现

数据准备
```java
DROP TABLE IF EXISTS `sys_user`;
CREATE TABLE `sys_user` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `username` varchar(50) DEFAULT NULL,
  `password` varchar(50) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=3 DEFAULT CHARSET=utf8;

/*Data for the table `sys_user` */

insert  into `sys_user`(`id`,`username`,`password`) values (1,'root','root'),(2,'sangeng','caotang');


```

实体类
```java
@Data
@NoArgsConstructor
@AllArgsConstructor
public class SystemUser {

    private Integer id;
    private String username;
    private String password;
}

```

SystemUserController
```java
import com.sangeng.domain.ResponseResult;
import com.sangeng.domain.SystemUser;
import com.sangeng.service.SystemUserService;
import com.sangeng.utils.JwtUtil;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import java.util.HashMap;
import java.util.Map;
import java.util.UUID;

@RestController
@RequestMapping("/sys_user")
public class SystemUserController {
    @Autowired
    private SystemUserService userService;

    @PostMapping("/login")
    public ResponseResult login(@RequestBody SystemUser user) {
        //校验用户名密码是否正确
        SystemUser loginUser = userService.login(user);
        Map<String, Object> map;
        if (loginUser != null) {
            //如果正确 生成token返回
            map = new HashMap<>();
            String token = JwtUtil.createJWT(UUID.randomUUID().toString(), String.valueOf(loginUser.getId()), null);
            map.put("token", token);
        } else {
            //如果不正确 给出相应的提示
            return new ResponseResult(300, "用户名或密码错误，请重新登录");
        }
        return new ResponseResult(200, "登录成功", map);
    }
}

```

Service
```java
public interface SystemUserService {

    public SystemUser login(SystemUser user);
}
@Service
public class SystemUserServcieImpl implements SystemUserService {
    @Autowired
    private SystemUserMapper systemUserMapper;

    @Override
    public SystemUser login(SystemUser user) {
        SystemUser loginUser = systemUserMapper.login(user);
        return loginUser;
    }
}
```

dao
```java
@Mapper
@Repository
public interface UserMapper {
    List<User> findAll();
}
```

```java
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd" >
<mapper namespace="com.sangeng.mapper.SystemUserMapper">
    <select id="login" resultType="com.sangeng.domain.SystemUser">
        select * from sys_user where username = #{username} and password = #{password}
    </select>
</mapper>
```

登录页面见资料

## 拦截器的概念
如果我们想在多个 Handler 方法执行之前或者之后都进行一些处理，甚至某些情况下需要拦截掉，不让 Handler 方法执行。那么可以使用 SpringMVC 为我们提供的拦截器。


### 4.6.1 SpringBoot 中拦截器使用步骤 
[[../SpringMVC/SpringMVC#拦截器的三个抽象方法|拦截器的相关知识]]
①创建类实现 HandlerInterceptor 接口
```java
public class LoginInterceptor implements HandlerInterceptor {
}
```

实现方法
```java
@Component
public class LoginInterceptor implements HandlerInterceptor {

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        //获取请求头中的token
        String token = request.getHeader("token");
        //判断token是否为空，如果为空也代表未登录 提醒重新登录（401）
        if(!StringUtils.hasText(token)){
            response.sendError(HttpServletResponse.SC_UNAUTHORIZED);
            return false;
        }
        //解析token看看是否成功
        try {
            Claims claims = JwtUtil.parseJWT(token);
            String subject = claims.getSubject();
            System.out.println(subject);
        } catch (Exception e) {
            e.printStackTrace();
            //如果解析过程中没有出现异常说明是登录状态
            //如果出现了异常，说明未登录，提醒重新登录（401）
            response.sendError(HttpServletResponse.SC_UNAUTHORIZED);
            return false;
        }
        return true;
    }
}
```

配置拦截器
创建一个配置类
```java
package com.zml.config;  
  
import com.zml.lnterceptor.LoginInterceptor;  
import org.springframework.beans.factory.annotation.Autowired;  
import org.springframework.context.annotation.Configuration;  
import org.springframework.web.servlet.config.annotation.InterceptorRegistry;  
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;  
  
/**  
 * @author ZHANGMINLEI  
 * @version 1.0  
 * @description: TODO  
 * @date 2023/1/27 15:59  
 */@Configuration  
public class LoginConfig implements WebMvcConfigurer {  
  
    @Autowired  
    private LoginInterceptor loginInterceptor;  
  
    @Override  
    public void addInterceptors(InterceptorRegistry registry) {  
        registry.addInterceptor(loginInterceptor)//注册拦截器  
                .addPathPatterns("/**")//配置拦截路径  
                .excludePathPatterns("/sys_user/login");//排除路径  
    }  
}
```

## 4.7 异常统一处理
[[../SpringMVC/SpringMVC#^805c13]]
①创建类加上@ControllerAdvice 注解进行标识
```java
@ControllerAdvice
public class MyControllerAdvice {
}
```

②定义异常处理方法
定义异常处理方法，**使用@ExceptionHandler**标识可以处理的异常。
```java
@ControllerAdvice
public class MyControllerAdvice {

    @ExceptionHandler(RuntimeException.class)
    @ResponseBody
    public ResponseResult handlerException(Exception e){
        //获取异常信息，存放如ResponseResult的msg属性
        String message = e.getMessage();
        ResponseResult result = new ResponseResult(300,message);
        //把ResponseResult作为返回值返回，要求到时候转换成json存入响应体中
        return result;
    }
}
```

## 4.8 获取 web 原生对象
我们之前在 web 阶段我们经常要使用到 request 对象，response，session 对象等。我们也可以通过 SpringMVC 获取到这些对象。（不过在 MVC 中我们很少获取这些对象，因为有更简便的方式，避免了我们使用这些原生对象相对繁琐的 API。）

我们只需要在方法上添加对应类型的参数即可，但是注意数据类型不要写错了，SpringMVC 会把我们需要的对象传给我们的形参。
```java
@RestController
public class TestController {

    @RequestMapping("/getRequestAndResponse")
    public ResponseResult getRequestAndResponse(HttpServletRequest request, HttpServletResponse response, HttpSession session){
        System.out.println(request);
        return new ResponseResult(200,"成功");
    }
}

```

## 4.9 自定义参数解析
如果我们想实现像获取请求体中的数据那样，在 Handler 方法的参数上增加一个@RepuestBody 注解就可以获取到对应的数据的话。

可以使用 HandlerMethodArgumentResolver 来**实现自定义的参数解析。**

①定义用来标识的注解，**（自定义注解）**
```java
@Target(ElementType.PARAMETER)
@Retention(RetentionPolicy.RUNTIME)
public @interface CurrentUserId {
}
```

②创建类实现 **HandlerMethodArgumentResolver** 接口并重写其中的方法
**注意加上@Component 注解注入 Spring 容器**
```java
@Component
public class UserIdArgumentResolver implements HandlerMethodArgumentResolver {

    //判断方法参数使用能使用当前的参数解析器进行解析
    //返回true表示可以被解析 反之不可以被解析
    @Override
    public boolean supportsParameter(MethodParameter parameter) {
        //如果方法参数有加上CurrentUserId注解，就能把被我们的解析器解析
        return parameter.hasParameterAnnotation(CurrentUserId.class);
    }
    //进行参数解析的方法，可以在方法中获取对应的数据，然后把数据作为返回值返回。方法的返回值就会赋值给对应的方法参数
    @Override
    public Object resolveArgument(MethodParameter parameter, ModelAndViewContainer mavContainer, NativeWebRequest webRequest, WebDataBinderFactory binderFactory) throws Exception {
        //获取请求头中的token
        String token = webRequest.getHeader("token");
        if(StringUtils.hasText(token)){
            //解析token，获取userId
            Claims claims = JwtUtil.parseJWT(token);
            String userId = claims.getSubject();
            //返回结果
            return userId;
        }
        return null;
    }
}
```

因为这算我们 SpringMVC 的一个组件，所以我们需要进行配置。
配置类
```java
@Configuration
public class ArgumentResolverConfig implements WebMvcConfigurer {

    @Autowired	
    private UserIdArgumentResolver userIdArgumentResolver;

    @Override
    public void addArgumentResolvers(List<HandlerMethodArgumentResolver> resolvers) {
    //        将我们的解析器进行注册，填入我们自动注入好的解析器对象
        resolvers.add(userIdArgumentResolver);
    }
}
```

测试
在需要获取 UserId 的方法中增加对应的方法参数然后使用@CurrentUserId 进行标识即可获取到数据，
**解析完成后他会将数据传递给方法形参表示注解的参数。然后直接调用即可。**
```java
@RestController
@RequestMapping("/user")
//@CrossOrigin
public class UserController {

    @Autowired
    private UserServcie userServcie;

    @RequestMapping("/findAll")
    public ResponseResult findAll(@CurrentUserId String userId) throws Exception {
        System.out.println(userId);
        //调用service查询数据 ，进行返回s
        List<User> users = userServcie.findAll();

        return new ResponseResult(200,users);
    }
}
```


## 4.10 声明式事务
声明式事务 SpringBoot 做好了相应配置，我们引入依赖就可以使用。
MyBatis 中携带了声明式事务的依赖，不需要我们再次引入。
直接在需要事务控制的方法上加上对应的注解 **@Transactional**
```java
@Service
public class UserServiceImpl implements UserServcie {

    @Autowired
    private UserMapper userMapper;

    @Override
    public List<User> findAll() {
        return userMapper.findAll();
    }

    @Override
    @Transactional
    public void insertUser() {
        //添加2个用户到数据库
        User user = new User(null,"sg666",15,"上海");
        User user2 = new User(null,"sg777",16,"北京");
        userMapper.insertUser(user);
        System.out.println(1/0);
        userMapper.insertUser(user2);
    }
}
```

## 4.11 AOP
在 SpringBoot 中默认是开启 AOP 功能的。如果不想开启 AOP 功能可以使用如下配置设置为 false
```yml
spring:
  aop:
    auto: false
```

### 使用步骤
1. 添加依赖

```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-aop</artifactId>
        </dependency>
```

2. 自定义注解

```java
@Target(ElementType.METHOD)  
@Retention(RetentionPolicy.RUNTIME)  
public @interface InvokeLog {  
}
```

3. 定义切面类

```java
@Aspect  //标识这是一个切面类
@Component
public class InvokeLogAspect {

    //确定切点
    @Pointcut("@annotation(com.sangeng.aop.InvokeLog)")
    public void pt(){
    }

    @Around("pt()")
    public Object printInvokeLog(ProceedingJoinPoint joinPoint){
        //目标方法调用前
        Object proceed = null;
        MethodSignature signature = (MethodSignature) joinPoint.getSignature();
        String methodName = signature.getMethod().getName();
        System.out.println(methodName+"即将被调用");
        try {
            proceed = joinPoint.proceed();
            //目标方法调用后
            System.out.println(methodName+"被调用完了");
        } catch (Throwable throwable) {
            throwable.printStackTrace();
            //目标方法出现异常了
            System.out.println(methodName+"出现了异常");
        }
        return proceed;
    }
}
```

4. 在需要增强的方法添加注解

```java
@Service
public class UserServiceImpl implements UserServcie {

    @Autowired
    private UserMapper userMapper;

    @Override
    @InvokeLog  //需要被增强方法需要加上对应的注解
    public List<User> findAll() {
        return userMapper.findAll();
    }
}
```

### 4.11.2 切换动态代理
有的时候我们需要修改 AOP 的代理方式。

我们可以使用以下方式修改：

在配置文件中配置 `spring. aop. proxy-target-class 为 false` 这为使用 jdk 动态代理。该配置默认值为 true，代表使用 cglib 动态代理。
```java
@SpringBootApplication
@EnableAspectJAutoProxy(proxyTargetClass = false)//修改代理方式
public class WebApplication {
    public static void main(String[] args) {
        ConfigurableApplicationContext context = SpringApplication.run(WebApplication.class, args);
    }
}
```

如果想生效还需要在配置文件中做如下配置:
```yml
spring:
  aop:
    proxy-target-class: false [[切换动态代理的方式]]
```

## 4.12 模板引擎相关-Thymeleaf
### 4.12.1 快速入门
#### 4.12.1.1 依赖
```xml
       <!--thymeleaf依赖-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
        </dependency>
```

#### 4.12.1.2 定义 Controller
在 controller 中往域中存数据，并且跳转
```java
@Controller
public class ThymeleafController {

    @Autowired
    private UserServcie userServcie;

    @RequestMapping("/thymeleaf/users")
    public String users(Model model){
        //获取数据
        List<User> users = userServcie.findAll();
        //望域中存入数据
        model.addAttribute("users",users);
        model.addAttribute("msg","hello thymeleaf");
        //页面跳转
        return "table-standard";
    }
}
```

#### 4.12.1.3 htmL
在**resources\templates**下存放模板页面。

在html标签中加上 xmlns:th="[http://www.thymeleaf.org](http://www.thymeleaf.org/)"

获取域中的 name 属性的值可以使用： ${name} 注意要在 th 开头的属性中使用
```html
<html lang="en" class="no-ie" xmlns:th="http://www.thymeleaf.org">
 .....
 <div class="panel-heading" th:text="${msg}">Kitchen Sink</div>
```

如果需要引入静态资源，需要使用模板引擎的写法如下写法。
```html
 <link rel="stylesheet" th:href="@{/app/css/bootstrap.css}">
   <!-- Vendor CSS-->
   <link rel="stylesheet" th:href="@{/vendor/fontawesome/css/font-awesome.min.css}">
   <link rel="stylesheet" th:href="@{/vendor/animo/animate+animo.css}">
   <link rel="stylesheet" th:href="@{/vendor/csspinner/csspinner.min.css}">
   <!-- START Page Custom CSS-->
   <!-- END Page Custom CSS-->
   <!-- App CSS-->
   <link rel="stylesheet" th:href="@{/app/css/app.css}">
   <!-- Modernizr JS Script-->
   <script th:src="@{/vendor/modernizr/modernizr.js}" type="application/javascript"></script>
   <!-- FastClick for mobiles-->
   <script th:src="@{/vendor/fastclick/fastclick.js}" type="application/javascript"></script>
```

遍历语法：遍历的语法 th:each="自定义的元素变量名称 : ${集合变量名称}"
```html
<tr th:each="user:${users}">
    <td th:text="${user.id}"></td>
    <td th:text="${user.username}"></td>
    <td th:text="${user.age}"></td>
    <td th:text="${user.address}"></td>
</tr>
```

# 5. 整合 Redis
1. 引入依赖

```xml
        <!--redis-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-redis</artifactId>
        </dependency>
```

2. 配置 Redis 地址和端口号

```yml
spring:
  redis:
    host: 127.0.0.1 [[redis服务器ip地址]]
    port: 6379  [[redis端口号]]
```

3. 注入 Redisplate 使用

```java
    @Autowired
    private StringRedisTemplate redisTemplate;

    @Test
    public void testRedis(){
        redisTemplate.opsForValue().set("name","三更");
    }
```

# 6.环境切换
## 6.1 为什么要使用 profile

在实际开发环境中，我们存在开发环境的配置，部署环境的配置，测试环境的配置等等，里面的配置信息很多时，例如：端口、上下文路径、数据库配置等等，若每次切换环境时，我们都需要进行修改这些配置信息时，会比较麻烦，profile 的出现就是为了解决这个问题。它可以让我们针对不同的环境进行不同的配置，然后可以通过激活、指定参数等方式快速切换环境。

## 6.2 使用
### 6.2.1 创建 profile 配置文件
我们可以用**application-xxx. yml**的命名方式创建配置文件，其中 xxx 可以根据自己的需求来定义。

例如

我们需要一个测试环境的配置文件，则可以命名为：**application-test. yml**
需要一个生产环境的配置文件，可以命名为：**application-prod. yml**

我们可以不同环境下不同的配置放到对应的 profile 文件中进行配置。然后把不同环境下都相同的配置放到 application. yml 文件中配置。

### 6.2.2 激活环境
我们可以在**application. yml**文件中使用**spring. profiles. active**属性来配置激活哪个环境。

也可以使用虚拟机参数来指定激活环境。例如 ： **Dspring. profiles. active=test**

也可以使用命令行参数来激活环境。例如： **--spring. profiles. active =test**

# 7. 日志
开启日志
```yml
debug: true [[开启日志]]
logging:
  level:
    com.zml: debug [[设置日志级别]]
```

# 指标监控
我们在日常开发中需要对程序内部的运行情况进行监控，比如：健康度、运行指标、日志信息、线程情况等等，而 SpringBoot 的监控 Actuator 就可以帮我们解决这些问题。
## 使用
1. 谈价依赖

```xml
<dependency>
 	<groupId>org.springframework.boot</groupId>
 	<artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

2. 访问监控接口

[http://localhost:81/actuator](http://localhost:81/actuator)

3. 配置启用监控端点

```yml
management:
  endpoints:
    enabled-by-default: true [[配置启用所有端点]]
	web:
      exposure:
        include: "*" [[web端暴露所有端点]]
```

## 常用端点
![[imgs/Pasted image 20230131112606.png]]

## 图形化界面 SpringBoot Admin
①创建 SpringBoot Admin Server 应用，（接收信息进行展示的服务。）
需要新建一个模块，来作为 Admin Server 。
要求引入 spring-boot-admin-starter-server 依赖
```xml
     <dependency>
            <groupId>de.codecentric</groupId>
            <artifactId>spring-boot-admin-starter-server</artifactId>
        </dependency>
```
然后在启动类上加上@EnableAdminServer 注解.
访问 Admin Server 的地址即可打开架空页面 http://localhost:8084/

②配置 SpringBoot Admin client 应用 （需要被展示的类）
在需要监控的应用中加上 spring-boot-admin-starter-client 依赖
```xml
  <dependency>
            <groupId>de.codecentric</groupId>
            <artifactId>spring-boot-admin-starter-client</artifactId>
            <version>2.3.1</version>
        </dependency>
```

然后配置 SpringBoot Admin Server 的地址
```yml
spring:
  boot:
    admin:
      client:
        url: http://localhost:8884 [[配置]] Admin Server的地址
```

# 源码部分
## Sping 容器创建 Bean 文件
**Spring 创建 Bean 的过程？**
创建过程：
非懒加载的单实例 bean 会在容器创建的时候创建。容器内部会创建一个 beanFactory，使用 beanFactory 的 doGetBean 方法来进行创建，并且在创建后会把 bean 放入一个单例 bean 的 map 集合（singletonObjects）中存储。key 就是我们配置的 bean 的名称。
所以我们调用容器的 getBean 方法来获取对象的时候，其实他也是调用了 doGetBean 方法。就会从对应的集合中获取到之前创建的对象。

## Spring-BeanDefinition 文件解析
在容器创建时会先去创建一个 beanFactory，然后使用 XmlBeanDefinitionReader 去读取 xml 配置文件，把里面的标签进行解析，然后创建 BeanDefinition 对象来存放 bean 标签中的各个属性的值。所以 BeanDefinition 相当于就是保存了 bean 的定义信息的对象。

BeanDefinition 被创建后会被存入 beanFactoty 的 beanDefinitionMap 集合和 beanDefinitionNames。

## SpringBoot 中 Spring 容器 bean 的创建与刷新

**探究 SpringBoot 项目中的 Spring 容器是在什么时候创建和刷新的?**

在启动类中调用 SpringApplication 的 run 方法时会根据容器的类型创建不同的容器对象，并且调用容器的 refresh 方法。

## SpringBoot 组件扫描
**探究 SpringBoot 项目中我们没有设置组件扫描的包为什么会默认扫描启动项类所在的包？**


在容器刷新时会调用 BeanFactoryPostProcessor (Bean 工厂后置处理器) 进行处理。其中就有一个 ConfigurationClassPostProcessor（配置类处理器）。在这个处理器中使用 ConfigurationClassParser（配置类解析器）的 parse 方法去解析处理我们的配置类，其中就有对 ComponentScan 注解的解析处理。会去使用 ComponentScanAnnotationParser 的 parse 方法进行解析。解析时如果发现没有配置 basePackage，它会去获取我们加了注解的这个类所在的包，作为我们的 basepackage 进行组件扫描。

额外收获：
	ConfigurationClassParser 中除了对组件扫描会进行处理，还对@PropertySource，@Import，@ImportResource，@Bean 等注解进行处理

## @Import 注解的作用
- 定义配置类
- 使用 improt 引入配置类

1. 可以引入其他的配置类，即使我们的配置配不在扫描的包下，只要在注解中传入配置类的. Class 参数，他依然可以生效，作为我们的配置类。


## IprotSelector 注解的作用
1. 可以根据字符串数组（数组元素为类的全类名）来批量加载指定的 bean。

### 使用方法
1. 新建一个 zmlConfig 类实现 ImportSelector。实现里面的方法，其中 selectImports 这个方法里面返回的字符串数组就是我们需要加载的配置文件，字符串的元素需要是全类名。
2. 在启动类上面使用 improt 注解导入 A 类即可


import.Properties 文件
```properties
className=com. A,\  
  com.B
```

ZmlConfig 类
```java
public class ZmlConfig implements ImportSelector {  
    @Override  
    public String[] selectImports(AnnotationMetadata annotationMetadata) {  
        ResourceBundle anImport = ResourceBundle.getBundle("import");  
        String className = anImport.getString("className");  
        String[] split = className.split(",");  
        return split;  
        //读取improt.properties文件的内容，然后返回。
    }  
}
```

启动类
```java
@SpringBootApplication  
//使用improt注解进行导入
@Import(ZmlConfig.class)  
public class WebApplication {  
    public static void main(String[] args) {  
        SpringApplication.run(WebApplication.class, args);  
    }  
}
```

## ImportSelector 排除过滤器
配置类
```java
public class ZmlConfig implements ImportSelector {  
    @Override  
    public String[] selectImports(AnnotationMetadata annotationMetadata) {  
        ResourceBundle anImport = ResourceBundle.getBundle("import");  
        String className = anImport.getString("className");  
        String[] split = className.split(",");  
        return split;  
    }  
  
    @Override  
    public Predicate<String> getExclusionFilter() {  
    //过滤器排除方法实现
        return new Predicate<String>() {  
            @Override  
            public boolean test(String s) {  
                return false;  
//                false 表示通过 true 表示排除  
            }  
        };  
    }  
}
```


## ImportBeanDefinitionRegistrar 类
### 使用场景
如要实现动态 Bean 的装载可以使用 ImportBeanDefinitionRegistrar。尤其是如果想装载动态代理对象的时候。例如 Mybatis 的启动器就是使用了它实现了 Mapper 接口的代理对象装载的。

### 探究@Import 注解引入 ImportBeanDefinitionRegistrar 是如何生效的

遍历调用 ImportBeanDefinitionRegistrar 的方法创建 BeanDefinition 对象注册入容器

导入 ImportBeanDefinitionRegistrar
	ConfigurationClassParser 的 doProcessConfigurationClass 方法在处理启动类的时候会先递归扫描其上所有的@Import 注解，获取其中的的 value 属性值添加到集合中。然后获取遍历这个集合判断他们是 ImportSelector 的子类还是 ImportBeanDefinitionRegistrar 的子类还是配置类。
	如果是 ImportBeanDefinitionRegistrar 就会通过反射创建其对象。
	然后把创建出来的对象添加到集合中保存。


### 进阶使用
#### 使用场景：
我们想去识别加了指定注解的类，把这些类的 BeanDefinition 对象注册到容器中。这个时候就可以使用 **ClassPathBeanDefinitionScanner**

为什么我知道可以用这个类？
想想我们之前看过的组件扫描源码中是否有类似的功能。
#### 使用教程
我们自己定义一个 ZML 注解，期望加了这个注解的类都能被装载到容器中。
1. 自定义注解

```java
@Target (ElementType. TYPE)  
@Retention(RetentionPolicy.RUNTIME)  
public @interface ZML {  
}
```

2. 给需要的类添加注解



3. 创建 Registrar，在其中创建扫描器然后把扫描到的 beanDefinition 转载到容器中

```java
public class ZMLComponentRegistrar implements ImportBeanDefinitionRegistrar {  
    @Override  
    public void registerBeanDefinitions(AnnotationMetadata importingClassMetadata, BeanDefinitionRegistry registry) {  
        ClassPathBeanDefinitionScanner scanner = new ClassPathBeanDefinitionScanner(registry, false);  
        scanner.addIncludeFilter(new TypeFilter() {  
            @Override  
            public boolean match(MetadataReader metadataReader, MetadataReaderFactory metadataReaderFactory) throws IOException {  
//                定制过滤规则  
//                判断这个类上面是否添加有  
                return metadataReader.getAnnotationMetadata().hasAnnotation("com.registrar.ZML");  
            }  
        });  
  
        scanner.scan("com.registrar");  
    }  
}
```

使用的话需要在启动类上使用 improt 注解注入这个 Registrar。

### 究极用法
#### 前置知识 FactoryBean
##### 应用场景
一些对象的创建过程比较复杂可以使用 FactoryBean 来实现对象的创建。可以让 Spring 容器需要创建该对象的时候调用 factoryBean 来实现创建。尤其是一些动态代理对象的创建。

##### 简单使用方法
目标使用工厂类创建一个 Pig 对象

Pig 类
```java
public class Pig {  
     int age;  
  
    public Pig(int age) {  
        this.age = age;  
    }  
}
```

工厂类
```java
public class PigFactoryBean implements FactoryBean<Pig> {  
    @Override  
    public Pig getObject() throws Exception {  
        Pig pig = new Pig(5);  
        return pig;  
    }  
  
    @Override  
    public Class<?> getObjectType() {  
        return Pig.class;  
    }  
}
```

在启动类新建一个方法，打上 Bean 注解，创建一个工厂类返回。
```java
@SpringBootApplication  
@Import(ZMLComponentRegistrar.class)  
public class WebApplication {  
    public static void main(String[] args) {  
        ConfigurableApplicationContext run = SpringApplication.run(WebApplication.class);  
    }  
  
    @Bean  
    public PigFactoryBean getPigFactoryBean() {  
        return new PigFactoryBean();  
    }  
}
```

##### 究极使用方法
接口类
```java
public interface UserMapper {  
    void select();  
}
```

工厂类，使用工厂类来动态创建对象。
```java
public class ZMLMapperFactoryBean implements FactoryBean {  
  
    private String className;  
  
    public ZMLMapperFactoryBean(String className) {  
        this.className = className;  
    }  
  
    @Override  
    public Object getObject() throws Exception {  
        Class<?> aClass = Class.forName(className);  
        Object o = Proxy.newProxyInstance(ZMLMapperFactoryBean.class.getClassLoader(), new Class[]{aClass}, new InvocationHandler() {  
            @Override  
            public Object invoke(Object o, Method method, Object[] objects) throws Throwable {  
//                填写增强方法规则。  
                if (method.getName().equals("select")) {  
                    System.out.println(className + "动态代理对象的select方法被执行");  
                }  
                return null;  
            }  
        });  
        return o;  
    }  
  
    @Override  
    public Class<?> getObjectType() {  
        try {  
            return Class.forName(className);  
        } catch (ClassNotFoundException e) {  
            e.printStackTrace();  
        }  
        return null;  
    }  
}
```

在启动类中使用 Bean 注解导入工厂类
```java
@SpringBootApplication  
@Import(ZMLComponentRegistrar.class)  
public class WebApplication {  
    public static void main(String[] args) {  
        ConfigurableApplicationContext run = SpringApplication.run(WebApplication.class);  
  
    }  
  
  
    @Bean  
    public ZMLMapperFactoryBean getMapperFactoryBean() {  
       return new ZMLMapperFactoryBean("com.registrar.UserMapper");  
    }  
}
```

### 实现目标
我们期望实现类似 Mybaits 的效果，定义接口，接口上使用指定注解进行标识。然后能生成对应的动态代理对象装载到容器中。 

### 实现方法
 自定义注解·
 ```java
   
@Target(ElementType.TYPE)  
@Retention(RetentionPolicy.RUNTIME)  
public @interface ZmlMapper {  
  
}
```
给对应接口加注解
```java
@ZmlMapper  
public interface UserMapper {  
  
    void select();  
}
```
创建 Registrar
	- 要创建扫描器的子类重写 isCandidateComponent
	- 使用扫描器扫描
	- 对接口的 BeanDefinition 进行转换
	- 对接口的 BeanDefinition 进行转换
```java
  public class ZmlMapperRegistrar implements ImportBeanDefinitionRegistrar {  
    @Override  
    public void registerBeanDefinitions(AnnotationMetadata importingClassMetadata, BeanDefinitionRegistry registry) {  
        //创建扫描器  不使用默认过滤器  要创建扫描器的子类 重写 isCandidateComponent        ZmlMapperScanner scanner = new ZmlMapperScanner(registry, false);  
        //设置一个包含过滤器  判断是否有SGMapper注解  
        scanner.addIncludeFilter(new TypeFilter() {  
            @Override  
            public boolean match(MetadataReader metadataReader, MetadataReaderFactory metadataReaderFactory) throws IOException {  
                return metadataReader.getAnnotationMetadata().hasAnnotation("com.registrar.UserMapper");  
            }  
        });  
        //进行扫描  
        Set<BeanDefinition> beanDefinitions = scanner.findCandidateComponents("com.registrar");  
        //扫描到BeanDefinition后应该进行转换  转换SGMapperFactoryBean的BeanDefinition 进行注册  
        for (BeanDefinition beanDefinition : beanDefinitions) {  
            AbstractBeanDefinition factoryBeanDefinition = BeanDefinitionBuilder.genericBeanDefinition(ZMLMapperFactoryBean.class)  
                    .addConstructorArgValue(beanDefinition.getBeanClassName()).getBeanDefinition();  
            registry.registerBeanDefinition(beanDefinition.getBeanClassName(),factoryBeanDefinition);  
        }  
    }  
}
```

使用@Import 注解注入 Registrar
```java
@SpringBootApplication  
@Import(ZmlMapperRegistrar.class)  
public class WebApplication {  
    public static void main(String[] args) {  
        ConfigurableApplicationContext run = SpringApplication.run(WebApplication.class);  
    run.getBean(UserMapper.class).select();  
    }
```

# 定时任务
定时任务的实现方式有很多，比如 XXL-Job 等。但是其实核心功能和概念都是类似的，很多情况下只是调用的 API 不同而已。 

这里就先用 SpringBoot 为我们提供的定时任务的 API 来实现一个简单的定时任务，让大家先对定时任务里面的一些核心概念有个大致的了解。

实现步骤
① 使用@EnableScheduling注解开启定时任务功能

我们可以在配置类上加上@EnableScheduling，当然启动类也是配置类。自己新建一个配置添加也是可以的。
```java
@SpringBootApplication
@MapperScan("com.sangeng.mapper")
@EnableScheduling
public class SanGengBlogApplication {
    public static void main(String[] args) {
        SpringApplication.run(SanGengBlogApplication.class,args);
    }
}
```

② 确定定时任务执行代码，并配置任务执行时间
使用@Scheduled 注解标识需要定时执行的代码。注解的 cron 属性相当于是任务的执行时间。目前可以使用 0/5 * * * * ? 进行测试，代表从 0 秒开始，每隔 5 秒执行一次。 

注意：对应的 bean 要注入容器，否则不会生效。
```java
@Component
public class TestJob {

    @Scheduled(cron = "0/5 * * * * ?")
    public void testJob(){
        //要执行的代码
        System.out.println("定时任务执行了");
    }
}
```

## cron 表达式语法
*cron 表达式是用来设置定时任务执行时间的表达式。*

很多情况下我们可以用 ： [在线Cron表达式生成器](https://www.bejson.com/othertools/cron/) 来帮助我们理解cron表达式和书写cron表达式。

但是我们还是有需要学习对应的Cron语法的，这样可以更有利于我们书写Cron表达式。

如上我们用到的 0/5 * * * * ? * ，cron 表达式由七部分组成，中间由空格分隔，这七部分从左往右依次是：
*秒（0~59），分钟（0~59），小时（0~23），日期（1-月最后一天），月份（1-12），星期几（1-7, 1 表示星期日），年份（一般该项不设置，直接忽略掉，即可为空值）*
SpringBoot 中直接禁用掉了年。

通用特殊字符：,  -  *  /  (可以在任意部分使用)

> *

星号表示任意值，例如：
```
* * * * * ?
```
表示 “ 每年每月每天每时每分每秒 ” 。


> ,

可以用来定义列表，例如 ：
```
1,2,3 * * * * ?
```
表示 “ 每年每月每天每时每分的每个第1秒，第2秒，第3秒 ” 。


> -

定义范围，例如：
```
 1-3 * * * * ?
```
表示 “ 每年每月每天每时每分的第 1 秒至第 3 秒 ”。


> /

每隔多少，例如
```
 5/10 * * * * ?
```
表示 “ 每年每月每天每时每分，从第5秒开始，每10秒一次 ” 。即 “ / ” 的左侧是开始值，右侧是间隔。如果是从 “ 0 ” 开始的话，也可以简写成 “ /10 ”



*日期部分还可允许特殊字符： ? L W
星期部分还可允许的特殊字符: ? L #*

> ?

只可用在日期和星期部分。表示没有具体的值，使用?要注意冲突。日期和星期两个部分如果其中一个部分设置了值，则另一个必须设置为 “ ? ”。
例如：

```
 0\* * * 2 * ?  
```
  和  
```
 0\* * * ? * 2
```

同时使用? 和同时不使用? 都是不对的

例如下面写法就是错的
```
 * * * 2 * 2  
```
  和  
```
 * * * ? * ?  
```
 ​


> W

只能用在日期中，表示当月中最接近某天的工作日
```
 0 0 0 31W * ?
```
表示最接近31号的工作日，如果31号是星期六，则表示30号，即星期五，如果31号是星期天，则表示29号，即星期五。如果31号是星期三，则表示31号本身，即星期三。


> L

表示最后（Last）,只能用在日期和星期中
在日期中表示每月最后一天，在一月份中表示31号，在六月份中表示30号
也可以表示每月倒是第N天。例如： L-2表示每个月的倒数第2天

0 0 0 LW * ? LW
可以连起来用，表示每月最后一个工作日，即每月最后一个星期五

在星期中表示7即星期六

```
 0 0 0 ? * L  
 表示每个星期六  
 0 0 0 ? * 6L  
 若前面有其他值的话，则表示最后一个星期几，即每月的最后一个星期五
```


> #

只表示第几个星期几
```
 0 0 0 ? * 6#3  
```
 表示每个月的第三个星期五。





















