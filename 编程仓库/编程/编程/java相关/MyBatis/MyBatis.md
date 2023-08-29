[(23条消息) MyBatis笔记_苍茗的博客-CSDN博客](https://blog.csdn.net/qq_19387933/article/details/123256034)
# MyBatis 历史
MyBatis 最初是 Apache 的一个开源项目 iBatis, 2010 年 6 月这个项目由 Apache Software Foundation 迁移到了 Google Code。随着开发团队转投 Google Code 旗下，iBatis3. x 正式更名为 MyBatis。代码于 2013 年 11 月迁移到 Github。
iBatis 一词来源于“internet”和“abatis”的组合，是一个基于 Java 的持久层框架。iBatis 提供的持久层框架包括 SQL Maps 和 Data Access Objects（DAO）。
# MyBatis 特性
1. MyBatis 是支持定制化 SQL、存储过程以及高级映射的优秀的持久层框架

2. 1.  MyBatis 避免了几乎所有的 JDBC 代码和手动设置参数以及获取结果集

 3.   MyBatis 可以使用简单的 XML 或注解用于配置和原始映射，将接口和 Java 的 POJO（Plain Old Java Objects，普通的 Java 对象）映射成数据库中的记录。

4.   MyBatis 是一个半自动的 ORM（Object Relation Mapping）框架。ORM 也就是将实体类对象和关系中数据库中的数据创建映射关系。
# MyBatis 下载
-   [MyBatis下载地址](https://github.com/mybatis/mybatis-3)

# MyBatis 和其他持久层技术对比
JDBC
	SQL 夹杂在 Java 代码中耦合度高，导致硬编码内伤
	维护不易且实际开发需求中 SQL 有变化，频繁修改的情况多见
	代码冗长，开发效率低
Hibernate 和 JPA
	操作简便，开发效率高
	程序中的长难复杂 SQL 需要绕过框架
	内部自动生产的 SQL，不容易做特殊优化
	基于全映射的全自动框架，大量字段的 POJO 进行部分映射时比较困难。
	反射操作太多，导致数据库性能下降
MyBatis
	轻量级，性能出色
	SQL 和 Java 编码分开，功能边界清晰。Java 代码专注业务、SQL 语句专注数据
	开发效率稍逊于 HIbernate，但是完全能够接受
# 搭建 Mybatis
## 1.创建 MyBatis 的核心配置文件
>习惯上命名为 mybatis-config. xml，这个文件名仅仅只是建议，并非强制要求。将来整Spring 之后，这个配置文件可以省略，所以大家操作时可以直接复制、粘贴。
核心配置文件主要用于配置连接数据库的环境以及 MyBatis 的全局配置信息
核心配置文件存放的位置是 src/main/resources 目录下

Mybatis-config. Xml
```xml
<?xml version="1.0" encoding="UTF-8" ?>  
<!DOCTYPE configuration  
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"  
        "http://mybatis.org/dtd/mybatis-3-config.dtd">  
<configuration>  
    <!--设置连接数据库的环境-->  
    <environments default="development">  
        <environment id="development">  
            <transactionManager type="JDBC"/>  
<!--            事务管理类型-->  
            <dataSource type="POOLED">  
<!--                数据源连接池-->  
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>  
                <property name="url" value="jdbc:mysql://localhost:3306/db_test?useSSL=false&allowPublicKeyRetrieval=true&serverTimezone=UTC"/>  
                <property name="username" value="root"/>  
                <property name="password" value="root"/>  
            </dataSource>  
        </environment>  
    </environments>  
    <!--引入映射文件-->  
    <mappers>  
        <mapper resource="mappers/UserMapper.xml"/>  
    </mappers>  
</configuration>
```
## 创建表对应实体类
- 需要给到无参构造，有参构造，get 和 set 方法，tostring 方法。

```java
package com.zml.bean;  
  
/**  
 * @author 张民磊  
 * @version 1.0  
 */public class User {  
    private Integer id;  
    private String username;  
    private String password;  
    private Integer age;  
    private String sex;  
    private String email;  
  
    public User(Integer id, String username, String password, Integer age, String sex, String email) {  
        this.id = id;  
        this.username = username;  
        this.password = password;  
        this.age = age;  
        this.sex = sex;  
        this.email = email;  
    }  
    public User(){}  
  
    @Override  
    public String toString() {  
        return "User{" +  
                "id=" + id +  
                ", username='" + username + '\'' +  
                ", password='" + password + '\'' +  
                ", age=" + age +  
                ", sex='" + sex + '\'' +  
                ", email='" + email + '\'' +  
                '}';  
    }  
    public Integer getId() {  
        return id;  
    }  
    public void setId(Integer id) {  
        this.id = id;  
    }  
    public String getUsername() {  
        return username;  
    }  
    public void setUsername(String username) {  
        this.username = username;  
    }  
    public String getPassword() {  
        return password;  
    }  
    public void setPassword(String password) {  
        this.password = password;  
    }  
    public Integer getAge() {  
        return age;  
    }  
    public void setAge(Integer age) {  
        this.age = age;  
    }  
    public String getSex() {  
        return sex;  
    }  
    public void setSex(String sex) {  
        this.sex = sex;  
    }  
    public String getEmail() {  
        return email;  
    }  
    public void setEmail(String email) {  
        this.email = email;  
    }
    }
```

## 2. 创建 mapper 接口
>MyBatis 中的 mapper 接口相当于以前的 dao。但是区别在于，mapper 仅仅是接口，我们不需要提供实现类，来代替 DAO 层。

```java
public interface UserMapper {  
    
    }
```
## 创建 Mybaitis 的映射文件
-   相关概念：ORM（Object Relationship Mapping）对象关系映射。
    -   对象：Java的实体类对象
    -   关系：关系型数据库
    -   映射：二者之间的对应关系

![[图片/Pasted image 20230103122452.png]]
映射文件的命名规则
	**表所对应的实体类的类名+Mapper. xml**
	例如：表 t_user，映射的实体类为 User，所对应的映射文件为 UserMapper. xml
	因此一个映射文件对应一个实体类，对应一张表的操作
	MyBatis 映射文件用于编写 SQL，访问以及操作表中的数据
	MyBatis 映射文件存放的位置是 src/main/resources/mappers 目录下
**MyBatis 中可以面向接口操作数据，要保证两个一致**
	mapper 接口的全类名和映射文件的命名空间（namespace）保持一致
	mapper 接口中方法的方法名和映射文件中编写 SQL 的标签的 id 属性保持一致
	**一张表的映射关系：表-->实体类-->mapper-->映射文件（xml）**
```xml
<?xml version="1.0" encoding="UTF-8" ?>  
<!DOCTYPE mapper  
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"  
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">  
<mapper namespace="com.atguigu.mybatis.mapper.UserMapper">  
  
    <insert id="insertUser">  
        <!--        id要和接口方法一致-->  
        insert into t_user values (null,'zml','123',23,'男','1234@qq.com')  
    </insert>  

</mapper>
```

## 通过 junit 测试功能
SqlSession：代表 Java 程序和数据库之间的会话。（HttpSession 是 Java 程序和浏览器之间的会话）
SqlSessionFactory：是“生产”SqlSession 的“工厂”
工厂模式：如果创建某一个对象，使用的过程基本固定，那么我们就可以把创建这个对象的相关代码封装到一个“工厂类”中，以后都使用这个工厂类来“生产”我们需要的对象。
-   此时需要手动提交事务，如果要自动提交事务，则在获取 sqlSession 对象时，使用 `SqlSession sqlSession = sqlSessionFactory.openSession(true);`，传入一个 Boolean 类型的参数，值为 true，这样就可以自动提交

```java
public class UserMapperTest {
    @Test
    public void testInsertUser() throws IOException {
        //读取MyBatis的核心配置文件
        InputStream is = Resources.getResourceAsStream("mybatis-config.xml");
        //获取SqlSessionFactoryBuilder对象
        SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
        //通过核心配置文件所对应的字节输入流创建工厂类SqlSessionFactory，生产SqlSession对象
        SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuilder.build(is);
        //获取sqlSession，此时通过SqlSession对象所操作的sql都必须手动提交或回滚事务
        //SqlSession sqlSession = sqlSessionFactory.openSession();
	    //创建SqlSession对象，此时通过SqlSession对象所操作的sql都会自动提交  
		SqlSession sqlSession = sqlSessionFactory.openSession(true);
        //通过代理模式创建UserMapper接口的代理实现类对象
        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
        //调用UserMapper接口中的方法，就可以根据UserMapper的全类名匹配元素文件，通过调用的方法名匹配映射文件中的SQL标签，并执行标签中的SQL语句
        int result = userMapper.insertUser();
        //提交事务
        //sqlSession.commit();
        //因为使用默认的JDBC事务，所以我们需要提交之后才会进行真的插入。
        System.out.println("result:" + result);
    }
}
```

## 加入 log4j 日志功能
1. 添加依赖
```xml
<!-- log4j日志 -->
<dependency>
<groupId>log4j</groupId>
<artifactId>log4j</artifactId>
<version>1.2.17</version>
</dependency>
```
2. 加入 log4j 的配置文件
	-   log4j 的配置文件名为 log4j. xml，存放的位置是 src/main/resources 目录下
	-   日志的级别：FATAL(致命)>ERROR(错误)>WARN(警告)>INFO(信息)>DEBUG(调试) 从左到右打印的内容越来越详细

```xml
<?xml version="1.0" encoding="UTF-8" ?>  
<!DOCTYPE log4j:configuration SYSTEM "log4j.dtd">  
<log4j:configuration debug="true">  
    <appender name="STDOUT" class="org.apache.log4j.ConsoleAppender">  
        <param name="Encoding" value="UTF-8"/>  
        <layout class="org.apache.log4j.PatternLayout">  
            <param name="ConversionPattern" value="%-5p %d{MM-dd HH:mm:ss,SSS} %m (%F:%L) \n"/>  
        </layout>  
    </appender>  
    <logger name="java.sql">  
        <level value="debug"/>  
    </logger>  
    <logger name="org.apache.ibatis">  
        <level value="info"/>  
    </logger>  
    <root>  
        <level value="debug"/>  
        <appender-ref ref="STDOUT"/>  
    </root>  
</log4j:configuration>

```
## 增删改查方法
注意：
	查询的标签 select 必须设置属性 resultType 或 resultMap，用于设置实体类和数据库表的映射关系
	resultType：自动映射，用于属性名和表中字段名一致的情况
	resultMap：自定义映射，用于一对多或多对一或字段名和属性名不一致的情况
	当查询的数据为多条时，不能使用实体类作为返回值，只能使用集合，否则会抛出异常 TooManyResultsException；但是若查询的数据只有一条，可以使用实体类或集合作为返回值

**增删改方法用法相似**
```xml
<insert id="insertUser">  
    <!--        id要和接口方法一致-->  
    insert into t_user values (null,'zml','123',23,'男','1234@qq.com')  
</insert>  
  
<update id="updateUser">  
update t_user set username = '张三' where id = 1  
</update>  
  
<delete id="deleteUser">  
    delete from t_user where id = 4 ;  
</delete>
```
查询方法
```xml
<select id="seleteUser" resultType="com.zml.bean.User">  
    select * from t_user where id = 2; 
    查询单行 
</select>  
<select id="seleteAllUser" resultType="com.zml.bean.User">  
    select * from t_user;  
    查询多行
</select>
<select id="seleteUserId" resultType="com.zml.bean.User">  
    select username from t_user where id = 1;  
    查询单列
</select>
```

# 核心配置文件详解
核心配置文件中的标签必须按照固定的顺序 (有的标签可以不写，但顺序一定不能乱)：properties、settings、typeAliases、typeHandlers、objectFactory、objectWrapperFactory、reflectorFactory、plugins、environments、databaseIdProvider、mappers
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//MyBatis.org//DTD Config 3.0//EN"
        "http://MyBatis.org/dtd/MyBatis-3-config.dtd">
<configuration>
    <!--引入properties文件，此时就可以${属性名}的方式访问属性值-->
    <properties resource="jdbc.properties"></properties>
    <settings>
        <!--将表中字段的下划线自动转换为驼峰-->
        <setting name="mapUnderscoreToCamelCase" value="true"/>
        <!--开启延迟加载-->
        <setting name="lazyLoadingEnabled" value="true"/>
    </settings>
    <typeAliases>
        <!--
        typeAlias：设置某个具体的类型的别名
        这个对应的是 pojo
        属性：
        type：需要设置别名的类型的全类名
        alias：设置此类型的别名，且别名不区分大小写。若不设置此属性，该类型拥有默认的别名，即类名，不区分大小写。
        -->
        <!--<typeAlias type="com.atguigu.mybatis.bean.User"></typeAlias>-->
        <!--<typeAlias type="com.atguigu.mybatis.bean.User" alias="user">
        </typeAlias>-->
        <!--以包为单位，设置改包下所有的类型都拥有默认的别名，即类名且不区分大小写-->
        <package name="com.atguigu.mybatis.bean"/>
    </typeAliases>
    <!--
    environments：设置多个连接数据库的环境
    属性：
	    default：设置默认使用的环境的id
    -->
    <environments default="mysql_test">
        <!--
        environment：设置具体的连接数据库的环境信息
        属性：
	        id：设置环境的唯一标识，不可以重复
	        可通过environments标签中的default设置某一个环境的id，表示默认使用的环境
        -->
        <environment id="mysql_test">
            <!--
            transactionManager：设置事务管理方式
            属性：
	            type：设置事务管理方式，type="JDBC|MANAGED"
	            type="JDBC"：设置当前环境的事务管理都必须手动处理，使用原生的事务
	            type="MANAGED"：设置事务被管理，例如spring中的AOP
            -->
            <transactionManager type="JDBC"/>
            <!--
            dataSource：设置数据源
            属性：
	            type：设置数据源的类型，type="POOLED|UNPOOLED|JNDI"
	            type="POOLED"：使用数据库连接池，即会将创建的连接进行缓存，下次使用可以从缓存中直接获取，不需要重新创建
	            type="UNPOOLED"：不使用数据库连接池，即每次使用连接都需要重新创建
	            type="JNDI"：调用上下文中的数据源
	            首先引入properties，然后即可使用下面的获取方式
            -->
            <dataSource type="POOLED">
                <!--设置驱动类的全类名-->
                <property name="driver" value="${jdbc.driver}"/>
                <!--设置连接数据库的连接地址-->
                <property name="url" value="${jdbc.url}"/>
                <!--设置连接数据库的用户名-->
                <property name="username" value="${jdbc.username}"/>
                <!--设置连接数据库的密码-->
                <property name="password" value="${jdbc.password}"/>
            </dataSource>
        </environment>
    </environments>
    <!--引入映射文件-->
    <mappers>
        <!-- <mapper resource="UserMapper.xml"/> -->
        <!--
        以包为单位，将包下所有的映射文件引入核心配置文件
        注意：
			1. 此方式必须保证mapper接口和mapper映射文件必须在相同的包下
			2. mapper接口要和mapper映射文件的名字一致
			3. 这个对应的是mapper的xml文件
        -->
        <package name="com.atguigu.mybatis.mapper"/>
    </mappers>
</configuration>

```

# Mybatis 获取参数值得两种方式（重点）
MyBatis 获取参数值的两种方式：${}和#{}
${}的本质就是字符串拼接
#{}的本质就是占位符赋值，尽量使用#{ }, 
可以使用任意的参数名字代替，尽量见名知义。
${ }使用字符串拼接的方式拼接 sql，若为字符串类型或日期类型的字段进行赋值时，需要手动加单引号。
但是#{}使用占位符赋值的方式拼接 sql，此时为字符串类型或日期类型的字段进行赋值时，可以自动添加单引号。帮助我们自动添加单引号
## MyBatis 获取参数值的各种情况 1，单个字面量
```xml
<select id="getUsername" resultType="user2">  
    select *  
    from t_user  
    where username = #{username};  
</select>
```

```java
@org.junit.Test  
public void testGetUsername(){  
    SqlSession sqlSession = SqlsessionUtils.getSqlSession();  
    MapperUser2 mapper = sqlSession.getMapper(MapperUser2.class);  
    List<User2> username = mapper.getUsername("zml");  
    for (User2 user2 :username) {  
        System.out.println(user2);  
    }
```

## MyBatis 获取参数值的各种情况 2，多个参数
MyBatis 底层是 map 集合存储参数，已经帮我规定好了键
1.  以arg0，arg1... 为键，以参数为值
2. 以 param1，param2... 为键，以参数为值

-   因此只需要通过 ${}和 #{}访问 map 集合的键就可以获取想对应的值，注意 ${} 需要手动加单引号。
-   使用 arg 或者 param 都行，要注意的是，arg 是从 arg0 开始的，param 是从 param1 开始的.
```xml
<select id="Login" resultType="com.zml.mybatis.pojo.User2">  
    select id  
    from t_user  
    where username = #{arg0}  
      and password = #{arg1};  
</select>
```

```java
@org.junit.Test  
public void testLogin() {  
    SqlSession sqlSession = SqlsessionUtils.getSqlSession();  
    MapperUser2 mapper = sqlSession.getMapper(MapperUser2.class);  
    List<User2> username = mapper.Login("zml","123");  
    for (User2 user2 : username) {  
        System.out.println(user2);  
    }
    }
```

## MyBatis 获取参数值的各种情况 3，map 集合类型
若我们存参数的是 map 集合，只需要通过#{}和${}以键的方式访问值即可，但是需要注意 ${}引号的问题。
```xml
<select id="Login2" resultType="com.zml.mybatis.pojo.User2">  
    select *  
    from t_user  
    where username =#{username}  
      and password =#{password};  
</select>
```

```java
@org.junit.Test  
public void testLogin2() {  
    SqlSession sqlSession = SqlsessionUtils.getSqlSession();  
    MapperUser2 mapper = sqlSession.getMapper(MapperUser2.class);  
    Map<String, Object> hashmap = new HashMap<>();  
    hashmap.put("username", "zml");  
    hashmap.put("password", "1234");  
    List<User2> username = mapper.Login2(hashmap);  
    for (User2 user2 : username) {  
        System.out.println(user2);  
    }
    }
```

## MyBatis 获取参数值的各种情况 4，实体类类型参数
 只需要通过#{}和${}访问属性值即可，同时也要注意 ${} 的单引号问题。
 ```xml
 <insert id="inserUser">  
    insert into t_user  
    values (null, #{username}, #{password}, #{age}, #{sex}, #{email});  
</insert>
```

```java
int inserUser(User user);
```

## MyBatis 获取参数值的各种情况 5，使用@param 注解
  使用方法，在接口方法形参中为添加@param 注解，框架就会为我们存储以这个值为键，以形参为值存储。
  他是以两种方式存储，
  使用@param 的 value 属性为键
  或者 param1 param2... 为键。都可以获得值。
  接口方法
  ```java
  User2 Login3(@Param("username") String username,@Param("password") String password);
```
Xml 文件
```xml
<select id="Login3" resultType="com.zml.mybatis.pojo.User2">  
    select id  
    from t_user  
    where username = #{username}  
      and password = #{password};  
</select>
```
方法实现
```java
@org.junit.Test  
public void testLogin3() {  
    SqlSession sqlSession = SqlsessionUtils.getSqlSession();  
    MapperUser2 mapper = sqlSession.getMapper(MapperUser2.class);  
    List<User2> list = mapper.Login3("zml", "123");  
    for (User2 user : list) {  
        System.out.println(user);  
    }
```

## 总结
分为两种情况使用：
	@Param 注解获取。
	实体类获取。
虽然方法没有固定规则，但是我门要自己给自己注定规则，尽量使用统一的规则。

# 默认类型别名对照表
![[图片/Pasted image 20230103203905.png]]
![[图片/Pasted image 20230103203856.png]]
# MyBatis 的各种查询功能
**- 若查询结果只有一条：**
如果查询出的数据只有一条，可以通过
	实体类对象接收
	List 集合接收
	Map 集合接收，结果{password=123456, sex=男, id=1, age=23, username=admin}
	
**如果查询出的数据有多条，一定不能用实体类对象接收，会抛异常 TooManyResultsException，可以通过**
	实体类类型的 LIst 集合接收
	Map 类型的 LIst 集合接收
	在 mapper 接口的方法上添加@MapKey 注解
	@Mapkey 注解：此时可以将每条数据转换为 map 集合中的值，以某个字段的值作为键，存放在 map 集合中。这样可以防止数据相同被覆盖。
## 查询单条数据，映射为实体类
```xml
<select id="selteUserByID" resultType="user2">  
    select *  
    from t_user where  id = #{id}  
</select>
```

```java
@Test  
public void testSeleteIdUser2() {  
    SqlSession sqlSession = SqlsessionUtils.getSqlSession();  
    SeleteUser2 mapper = sqlSession.getMapper(SeleteUser2.class);  
    User2 user2 = mapper.selteUserByID(1);  
   System.out.println(user2);  
}
```

## 查询单行单列数据
 数据结果集类型是什么 resultType 属性就写什么：
 ```xml
 <select id="seleteUsercount" resultType="int">  
    select count(*)  
    from t_user  
</select>
```

```java
int seleteUsercount();
```

## 查询单条数据结果为 map 集合
```xml
<select id="selectmap" resultType="map">  
    select *  
    from t_user  
    where id = ${id}  
</select>
```

```java
Map<String,Object> selectmap(@Param("id") Integer id);
```

## 查询多条数据结果为 Map 集合
添加注解方式
```xml
@MapKey("id")  
Map<String,Object> slelctAllUser();
```

```java
<select id="slelctAllUser" resultType="map">  
    select *  
    from t_user  
</select>
```
List 集合方式
```xml
<select id="selectAllUser2" resultType="map">  
    select *  
    from t_user  
</select>
```

```java
List<Map<String,Object>> selectAllUser2();
```

# 特殊 Sql 的执行
## 模糊查询
有三种方式拼接。第三种最常用。

```xml
<select id="selectmh" resultType="user2">  
    <!--   select * from t_user where username like '%${username}%';--> 
     
    <!--      select * from t_user where username like concat('%',#{username},'%')-->  
    
      select * from t_user where username like "%"#{username}"%";  
</select>
```
## 批量删除
处理批量删除的 sql 语句我们只能使用 ${} 进行操作, 不可以使用 #{}  。因为 #{} 会自动帮我们添加单引号，这是不正确的。
```xml
<delete id="deleteUser">  
    delete from  t_user where  id in (${ids})  
</delete>
```

```java
int deleteUser(@Param("ids") String ids);
```

## 动态设置表名
-   只能使用${}，因为表名不能加单引号

```xml
<select id="selectUser" resultType="user2">  
    select * from ${table}  
</select>
```

```java
List<User2> selectUser(@Param("table") String table);
```

## 添加功能获取自增的主键
使用场景
	t_clazz (clazz_id, clazz_name)
	t_student (student_id, student_name, clazz_id)
	添加班级信息
	获取新添加的班级的 id
	为班级分配学生，即将某学的班级 id 修改为新添加的班级的 id
在 mapper. xml 中设置两个属性
**useGeneratedKeys：设置使用自增的主键
keyProperty：因为增删改有统一的返回值是受影响的行数，因此只能将获取的自增的主键赋值给传输映射文件中的参数的某个属性**，也就是 user 的 id 中。
```xml
<insert id="insertintoUser2" useGeneratedKeys="true" keyProperty="id">  
    insert into t_user  
    values (null, #{username}, #{password}, #{age}, #{sex}, #{email});  
</insert>
```

```java
void insertintoUser2(User2 user2 );
```

```java
@Test  
public void test8() {  
    SqlSession sqlSession = SqlsessionUtils.getSqlSession();  
    String key = "";  
    SeleteUser2 mapper = sqlSession.getMapper(SeleteUser2.class);  
    User2 user = new User2(null, "ton", "123", 23, "男", "123@321.com");  
    mapper.insertintoUser2(user);  
}
```

# 自定义映射 resultMap
## ResultMap 处理字段和属性的映射关系
当我们字段名和属性名不一致时，我们有三种处理方式：
	1. 为字段设置别名，保持和属性名相同即可
	2. 使用全局配置，在 MyBatis 的配置文件中添加配置 mapUnderscoreToCamelCase 为 true 即可。映射规则为 emp_name : empName。他的作用不能是自动将属性名映射为驼峰命名。
	3. 使用 resultmap 自定义映射关系（经常处理一对多，多对一）
## 全局配置 mapUnderscoreToCamelCase 
```xml
<settings>  
    <setting name="mapUnderscoreToCamelCase" value="true"/>  
</settings>
```

## 使用 resultmap 自定义映射
Resultmap 的属性详解
	Id：唯一标示，不能重复
	Type：设置映射关系中的实体类类型。
	子标签
	Id：设置主键元素的映射关系
	Result：设置普通字段的映射关系
	属性
	Property：设置映射关系中的属性名，必须是 type 属性所设置的实体类类型中的属性名
	Colum：设置映射关系中的字段名，必须 sql 语句查询出的字段名
```xml
<resultMap id="empresult" type="emp">  
    <id property="eid" column="eid"/>  
    <result property="empName" column="emp_name"/>  
    <result property="age" column="age"/>  
    <result property="sex" column="sex"/>  
    <result property="email" column="email"/>  
</resultMap>  
  
<select id="getAllEmp" resultMap="empresult">  
    select * from t_emp  
</select>
```

```java
List<Emp> getAllEmp();
```

## 多对一映射处理
有三种处理方式；
### 级联属性赋值 
使用 resultMap 来设置，使用左外链接查询表。首先实体类中的 dept 的类型是对应表类型。
```xml
<resultMap id="jilian" type="emp">  
    <id property="eid" column="eid"/>  
    <result property="empName" column="emp_name"/>  
    <result property="age" column="age"/>  
    <result property="sex" column="sex"/>  
    <result property="email" column="email"/>  
    <result property="dept.did" column="did"/>  
    <result property="dept.deptName" column="dept_name"/>  
</resultMap>  
  
<select id="getById" resultMap="jilian">  
    select *  
    from t_emp  
             left join t_dept on t_emp.did = t_dept.did  
    where t_emp.eid = #{eid}  
</select>
```

### 使用 association 处理映射关系
-   association：处理多对一的映射关系
-   property：需要处理多对的映射关系的属性名
-   javaType：该属性的类型

```xml
<resultMap id="assoc" type="emp">  
    <id property="eid" column="eid"/>  
    <result property="empName" column="emp_name"/>  
    <result property="age" column="age"/>  
    <result property="sex" column="sex"/>  
    <result property="email" column="email"/>  
    <association property="dept" javaType="Dept">  
        <id property="did" column="did"/>  
        <result property="deptName" column="dept_name"/>  
    </association>  
</resultMap>  
  
  
<select id="getById" resultMap="assoc">  
    select *  
    from t_emp  
             left join t_dept on t_emp.did = t_dept.did  
    where t_emp.eid = #{eid}  
</select>
```

### 使用分布查询解决多对一
-   select：设置分布查询的 sql 的唯一标识（namespace. SQLId 或 mapper 接口的全类名. 方法名）
-   column：设置分步查询的条件

```java
//EmpMapper里的方法
/**
 * 通过分步查询，员工及所对应的部门信息
 * 分步查询第一步：查询员工信息
 * @param  
 * @return com.atguigu.mybatis.pojo.Emp
 * @date 2022/2/27 20:17
 */
Emp getEmpAndDeptByStepOne(@Param("eid") Integer eid);
```

```xml
<resultMap id="empAndDeptByStepResultMap" type="Emp">
	<id property="eid" column="eid"></id>
	<result property="empName" column="emp_name"></result>
	<result property="age" column="age"></result>
	<result property="sex" column="sex"></result>
	<result property="email" column="email"></result>
	<association property="dept"				 select="com.atguigu.mybatis.mapper.DeptMapper.getEmpAndDeptByStepTwo"
				 column="did">
				 这里的column的值是第一条sql查询出的值来当做第二条sql的条件        
				 </association>
</resultMap>
<!--Emp getEmpAndDeptByStepOne(@Param("eid") Integer eid);-->
<select id="getEmpAndDeptByStepOne" resultMap="empAndDeptByStepResultMap">
	select * from t_emp where eid = #{eid}
</select>

```
查询部门信息
```java
//DeptMapper里的方法
/**
 * 通过分步查询，员工及所对应的部门信息
 * 分步查询第二步：通过did查询员工对应的部门信息
 * @param
 * @return com.atguigu.mybatis.pojo.Emp
 * @date 2022/2/27 20:23
 */
Dept getEmpAndDeptByStepTwo(@Param("did") Integer did);

```

```xml
<!--此处的resultMap仅是处理字段和属性的映射关系-->
<resultMap id="EmpAndDeptByStepTwoResultMap" type="Dept">
	<id property="did" column="did"></id>
	<result property="deptName" column="dept_name"></result>
</resultMap>
<!--Dept getEmpAndDeptByStepTwo(@Param("did") Integer did);-->
<select id="getEmpAndDeptByStepTwo" resultMap="EmpAndDeptByStepTwoResultMap">
	select * from t_dept where did = #{did}
</select>
```

### 一对多使用-   collection
**collection：用来处理一对多的映射关系
	property：一对多对应的属性
	ofType：表示该属性对饮的集合中存储的数据的类型**
```xml
<resultMap id="DeptAndEmpResultMap" type="Dept">
	<id property="did" column="did"></id>
	<result property="deptName" column="dept_name"></result>
	<collection property="emps" ofType="Emp">
		<id property="eid" column="eid"></id>
		<result property="empName" column="emp_name"></result>
		<result property="age" column="age"></result>
		<result property="sex" column="sex"></result>
		<result property="email" column="email"></result>
	</collection>
</resultMap>
<!--Dept getDeptAndEmp(@Param("did") Integer did);-->
<select id="getDeptAndEmp" resultMap="DeptAndEmpResultMap">
	select * from t_dept left join t_emp on t_dept.did = t_emp.did where t_dept.did = #{did}
</select>
```

```java
public class Dept {
    private Integer did;
    private String deptName;
    private List<Emp> emps;
	//...构造器、get、set方法等
}

```
### 分布查询处理一对多
一对多我们使用的属性是 collection，里面给到的属性也是 select 和 column
Select 是关联的查询 sql 的全类名 
Column ：关联的 sql 语句的条件
主表：
```xml
<resultMap id="one2" type="Dept">  
    <id property="did" column="did"/>  
    <result property="deptName" column="dept_name"/>  
    <collection property="emps" select="com.zml.mybatis.mapper.EmpMapper.select" column="did">  
    </collection>  
</resultMap> 

<select id="selectById" resultMap="one2">  
    select *  
    from t_dept  
    where did = #{did}  
</select>
```

```java
Dept selectById(@Param("did") int did);
```

附表：
```xml
<resultMap id="two2" type="Emp">  
    <result property="empName" column="emp_name"/>  
    <result property="age" column="age"/>  
    <result property="sex" column="sex"/>  
    <result property="email" column="email"/>  
</resultMap>  
<select id="select" resultMap="two2">  
    select * from t_emp where did = #{did}  
</select>
```

```java
List<Emp> select(@Param("did") int did);
```
### 延迟加载
分步查询的优点：可以实现延迟加载，但是必须在核心配置文件中设置全局配置信息：
	lazyLoadingEnabled：延迟加载的全局开关。当开启时，所有关联对象都会延迟加载
	aggressiveLazyLoading：当开启时，任何方法的调用都会加载该对象的所有属性。否则，每个属性会按需加载
此时就可以实现按需加载，获取的数据是什么，就只会执行相应的 sql。此时可通过 association 和 collection 中的 fetchType 属性设置当前的分步查询是否使用延迟加载，fetchType=“lazy (延迟加载)|eager (立即加载)”
```xml
<settings>
	<!--开启延迟加载-->
	<setting name="lazyLoadingEnabled" value="true"/>
</settings>
```

**fetchType：当开启了全局的延迟加载之后，可以通过该属性手动控制延迟加载的效果，fetchType=“lazy(延迟加载)|eager(立即加载)”**
```xml
<resultMap id="empAndDeptByStepResultMap" type="Emp">
	<id property="eid" column="eid"></id>
	<result property="empName" column="emp_name"></result>
	<result property="age" column="age"></result>
	<result property="sex" column="sex"></result>
	<result property="email" column="email"></result>
	<association property="dept"	 select="com.atguigu.mybatis.mapper.DeptMapper.getEmpAndDeptByStepTwo"
				 column="did"
				 fetchType="lazy"></association>
</resultMap>
```

# 动态 SQL
-   Mybatis 框架的动态 SQL 技术是一种根据特定条件动态拼装 SQL 语句的功能，它存在的意义是为了解决拼接 SQL 语句字符串时的痛点问题

## If 关键字
-   if 标签可通过 test 属性（即传递过来的数据）的表达式进行判断，若表达式的结果为 true，则标签中的内容会执行；反之标签中的内容不会执行

在 where 后面添加一个恒成立条件 1=1
	这个恒成立条件并不会影响查询的结果
	*这个 1=1 可以用来拼接 and 语句，例如：当 empName 为 null 时
	如果不加上恒成立条件，则 SQL 语句为 select * from t_emp where and age = ? and sex = ? and email = ?，此时 where 会与 and 连用，SQL 语句会报错
	如果加上一个恒成立条件，则 SQL 语句为 select * from t_emp where 1= 1 and age = ? and sex = ? and email = ?，此时不报错*
```xml
<select id="select" resultType="emp">  
    select *  
    from t_emp  
    where 1 = 1  
    <if test="empName != null and empName != ''">  
        and emp_name = #{empName}  
    </if>  
    <if test="age != null and age !=''">  
        and age = #{age}  
    </if>  
    <if test="sex != null and sex != ''">  
        and sex = #{sex}  
    </if>  
</select>
```

## Where 关键字
-   where 和 if 一般结合使用：
    若 where 标签中的 if 条件都不满足，则 where 标签没有任何功能，即不会添加 where 关键字
    若 where 标签中的 if 条件满足，则 where 标签会自动添加 where 关键字，并将条件最前方多余的 and/or 去掉，**where 标签只能去掉前方的 and，不能去除后边的 and**

```xml
<select id="select" resultType="emp">  
    select *  
    from t_emp  
    <where>  
        <if test="empName != null and empName != ''">  
            and emp_name = #{empName}  
        </if>  
        <if test="age != null and age !=''">  
            and age = #{age}  
        </if>  
        <if test="sex != null and sex != ''">  
            and sex = #{sex}  
        </if>  
    </where>  
</select>

```
## Trim 关键字
rim 用于去掉或添加标签中的内容
常用属性
prefix：在 trim 标签中的内容的**前面**添加某些内容
suffix：在 trim 标签中的内容的**后面**添加某些内容
prefixOverrides：在 trim 标签中的内容的前面**去掉**某些内容
suffixOverrides：在 trim 标签中的内容的后面**去掉**某些内容
若 trim 中的标签都不满足条件，**则 trim 标签没有任何效果，也就是只剩下 select * from t_emp**
```xml
<select id="select" resultType="emp">  
    select *  
    from t_emp  
    <trim prefix="where" suffixOverrides="and|or">  
        <if test="empName != null and empName != ''">  
             emp_name = #{empName} and  
        </if>  
        <if test="age != null and age !=''">  
             age = #{age} or  
        </if>  
        <if test="sex != null and sex != ''">  
             sex = #{sex}  
        </if>  
    </trim>  
</select>
```

## choose、when、otherwise 关键字
choose、when、otherwise `相当于` if... elseif.. else 
	Chose 为父标签
	When 代表 if elseif
	Otherwise 代表 else
when 至少要有一个，otherwise 最多只有一个
 相当于`if a else if b else if c else d`，只会执行其中一个
```xml
  
<select id="select" resultType="emp">  
    select *  
    from t_emp  
    <choose>  
        <when test="empName != null and empName != ''">  
            where emp_name = #{empName}  
        </when>  
        <when test="sex != '' and sex != null">  
            where sex = #{sex}  
        </when>  
        <otherwise>  
            where did = 1  
        </otherwise>  
    </choose>  
</select>
```

## Foreach 关键字
属性：
	collection：设置要循环的数组或集合
	item：表示集合或数组中的每一个数据
	separator：设置循环体之间的分隔符，分隔符前后默认有一个空格，如`,`
	open：设置 foreach 标签中的内容的开始符
	close：设置foreach标签中的内容的结束符

### 批量删除
```xml
<delete id="delete">  
    delete  
    from t_emp  
    where eid in  
    <foreach collection="eids" item="eid" open="(" close=")" separator=",">  
    #{eid}  
    </foreach>  
</delete>
```

```java
int delete(@Param("eids") int[] eids);
```

### 批量添加
批量添加我们需要用到 List 数组来存放添加的实体类
```xml
<insert id="insert">  
    insert into t_emp  
    values  
    <foreach collection="eids" item="eid" separator=",">  
        (null,#{eid.empName},#{eid.age},#{eid.sex},#{eid.email},null)  
    </foreach>  
</insert>
```

```java
int insert(@Param("eids") List<Emp> eids);
```

## SQL 片段
-   sql 片段，可以记录一段公共 sql 片段，在使用的地方通过 include 标签进行引入
-   声明 sql 片段：`<sql>` 标签

```xml
<sql id="empColumns">eid,emp_name,age,sex,email</sql>
```
-   引用 sql 片段：`<include>` 标签

```xml
<!--List<Emp> getEmpByCondition(Emp emp);-->
<select id="getEmpByCondition" resultType="Emp">
	select <include refid="empColumns"></include> from t_emp
</select>
```
# MyBatis 的缓存
## MyBatis 的一级缓存
一级缓存是 SqlSession 级别的，通过同一个 SqlSession 查询的数据会被缓存，下次查询相同的数据，就会从缓存中直接获取，不会从数据库重新访问
使一级缓存失效的四种情况：
	不同的 SqlSession 对应不同的一级缓存
	同一个 SqlSession 但是查询条件不同
	同一个 SqlSession 两次查询期间执行了任何一次增删改操作
	同一个 SqlSession 两次查询期间手动清空了缓存
		**通过接口实现类. ClearCache（）方法，清空缓存。**

## MyBatis 的二级缓存
二级缓存是 SqlSessionFactory （生产 SqlSession 的工厂类）级别，通过同一个 SqlSessionFactory 创建的 SqlSession 查询的结果会被缓存；此后若再次执行相同的查询语句，结果就会从缓存中获取
二级缓存开启的条件
	在核心配置文件中，设置全局配置属性 cacheEnabled=“true”，默认为 true，不需要设置
	**在映射文件中设置标签
	二级缓存必须在 SqlSession 关闭或提交之后有效，也就是实现 Serializable 
	查询的数据所转换的实体类类型必须实现序列化的接口**
使二级缓存失效的情况：
	两次查询之间执行了任意的增删改，会使一级和二级缓存同时失效
## 二级缓存的相关设置
在 mapper 配置文件中添加的 cache 标签可以设置一些属性
eviction 属性：缓存回收策略
LRU（Least Recently Used） – 最近最少使用的：移除最长时间不被使用的对象。
FIFO（First in First out） – 先进先出：按对象进入缓存的顺序来移除它们。
SOFT – 软引用：移除基于垃圾回收器状态和软引用规则的对象。
WEAK – 弱引用：更积极地移除基于垃圾收集器状态和弱引用规则的对象。
默认的是 LRU
flushInterval 属性：刷新间隔，单位毫秒，也就是清空缓存
默认情况是不设置，也就是没有刷新间隔，缓存仅仅调用语句（增删改）时刷新
size 属性：引用数目，正整数
代表缓存最多可以存储多少个对象，太大容易导致内存溢出
readOnly 属性：只读，true/false
true：只读缓存；会给所有调用者返回缓存对象的相同实例。因此这些对象不能被修改。这提供了很重要的性能优势。
false：读写缓存；会返回缓存对象的拷贝（通过序列化）。这会慢一些，但是安全，因此默认是 false

## MyBatis 缓存查询的顺序
-   先查询二级缓存，因为二级缓存中可能会有其他程序已经查出来的数据，可以拿来直接使用
-   如果二级缓存没有命中，再查询一级缓存
-   如果一级缓存也没有命中，则查询数据库
-   SqlSession关闭之后，一级缓存中的数据会写入二级缓存

**第三方缓存只能替代二级缓存，一级缓存是无法被代替的。**
## 整合第三方缓存 EHCache（了解）
### 添加依赖
```xml
<!-- Mybatis EHCache整合包 -->
<dependency>
	<groupId>org.mybatis.caches</groupId>
	<artifactId>mybatis-ehcache</artifactId>
	<version>1.2.1</version>
</dependency>
<!-- slf4j日志门面的一个具体实现 -->
<dependency>
	<groupId>ch.qos.logback</groupId>
	<artifactId>logback-classic</artifactId>
	<version>1.2.3</version>
</dependency>

```
### 各个 jar 包功能
![[图片/Pasted image 20230107190015.png]]
### 创建 EHCache 的配置文件 ehcaache. Xml
```xml
<?xml version="1.0" encoding="utf-8" ?>
<ehcache xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:noNamespaceSchemaLocation="../config/ehcache.xsd">
    <!-- 磁盘保存路径 -->
    <diskStore path="D:\atguigu\ehcache"/>
    <defaultCache
            maxElementsInMemory="1000"
            maxElementsOnDisk="10000000"
            eternal="false"
            overflowToDisk="true"
            timeToIdleSeconds="120"
            timeToLiveSeconds="120"
            diskExpiryThreadIntervalSeconds="120"
            memoryStoreEvictionPolicy="LRU">
    </defaultCache>
</ehcache>
```

### 设置二级缓存的类型
-   在 xxxMapper. xml 文件中设置二级缓存类型

```xml
<cache type="org.mybatis.caches.ehcache.EhcacheCache"/>
```
###  加入 logback 日志
-   存在 SLF4J 时，作为简易日志的 log4j 将失效，此时我们需要借助 SLF4J 的具体实现 logback 来打印日志。创建 logback 的配置文件 `logback.xml`，名字固定，不可改变

```xml

<?xml version="1.0" encoding="UTF-8"?>
<configuration debug="true">
    <!-- 指定日志输出的位置 -->
    <appender name="STDOUT"
              class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
            <!-- 日志输出的格式 -->
            <!-- 按照顺序分别是：时间、日志级别、线程名称、打印日志的类、日志主体内容、换行 -->
            <pattern>[%d{HH:mm:ss.SSS}] [%-5level] [%thread] [%logger] [%msg]%n</pattern>
        </encoder>
    </appender>
    <!-- 设置全局日志级别。日志级别按顺序分别是：DEBUG、INFO、WARN、ERROR -->
    <!-- 指定任何一个日志级别都只打印当前级别和后面级别的日志。 -->
    <root level="DEBUG">
        <!-- 指定打印日志的appender，这里通过“STDOUT”引用了前面配置的appender -->
        <appender-ref ref="STDOUT" />
    </root>
    <!-- 根据特殊需求指定局部日志级别 -->
    <logger name="com.atguigu.crowd.mapper" level="DEBUG"/>
</configuration>

```
### EHCache 配置文件说明
![[图片/Pasted image 20230107190313.png]]
# MyBatis 的逆向工程
-   正向工程：先创建 Java 实体类，由框架负责根据实体类生成数据库表。Hibernate 是支持正向工程的
-   逆向工程：**先创建数据库表，由框架负责根据数据库表，反向生成如下资源：**
    -   Java实体类
    -   Mapper接口
    -   Mapper映射文件

## 创建逆向工程步骤
### 添加依赖和插件
```xml
<dependencies>
	<!-- MyBatis核心依赖包 -->
	<dependency>
		<groupId>org.mybatis</groupId>
		<artifactId>mybatis</artifactId>
		<version>3.5.9</version>
	</dependency>
	<!-- junit测试 -->
	<dependency>
		<groupId>junit</groupId>
		<artifactId>junit</artifactId>
		<version>4.13.2</version>
		<scope>test</scope>
	</dependency>
	<!-- MySQL驱动 -->
	<dependency>
		<groupId>mysql</groupId>
		<artifactId>mysql-connector-java</artifactId>
		<version>8.0.27</version>
	</dependency>
	<!-- log4j日志 -->
	<dependency>
		<groupId>log4j</groupId>
		<artifactId>log4j</artifactId>
		<version>1.2.17</version>
	</dependency>
</dependencies>
<!-- 控制Maven在构建过程中相关配置 -->
<build>
	<!-- 构建过程中用到的插件 -->
	<plugins>
		<!-- 具体插件，逆向工程的操作是以构建过程中插件形式出现的 -->
		<plugin>
			<groupId>org.mybatis.generator</groupId>
			<artifactId>mybatis-generator-maven-plugin</artifactId>
			<version>1.3.0</version>
			<!-- 插件的依赖 -->
			<dependencies>
				<!-- 逆向工程的核心依赖 -->
				<dependency>
					<groupId>org.mybatis.generator</groupId>
					<artifactId>mybatis-generator-core</artifactId>
					<version>1.3.2</version>
				</dependency>
				<!-- 数据库连接池 -->
				<dependency>
					<groupId>com.mchange</groupId>
					<artifactId>c3p0</artifactId>
					<version>0.9.2</version>
				</dependency>
				<!-- MySQL驱动 -->
				<dependency>
					<groupId>mysql</groupId>
					<artifactId>mysql-connector-java</artifactId>
					<version>8.0.27</version>
				</dependency>
			</dependencies>
		</plugin>
	</plugins>
</build>
```

### 创建 MyBatis 的核心配置文件
```xml
<? xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <properties resource="jdbc.properties"/>
    <typeAliases>
        <package name=""/>
    </typeAliases>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${jdbc.driver}"/>
                <property name="url" value="${jdbc.url}"/>
                <property name="username" value="${jdbc.username}"/>
                <property name="password" value="${jdbc.password}"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <package name=""/>
    </mappers>
</configuration>

```

### 创建逆向工程的配置文件
-   文件名必须是：`generatorConfig. xml`

```xml
<? xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
<generatorConfiguration>
    <!--
    targetRuntime: 执行生成的逆向工程的版本
    MyBatis3Simple: 生成基本的CRUD（清新简洁版）
    MyBatis3: 生成带条件的CRUD（奢华尊享版）
    -->
    <context id="DB2Tables" targetRuntime="MyBatis3Simple">
        <!-- 数据库的连接信息 -->
        <jdbcConnection driverClass="com.mysql.cj.jdbc.Driver"
                        connectionURL="jdbc:mysql://localhost:3306/mybatis"
                        userId="root"
                        password="123456">
        </jdbcConnection>
        <!-- javaBean的生成策略-->
        <javaModelGenerator targetPackage="com.atguigu.mybatis.pojo" targetProject=".\src\main\java">
            <property name="enableSubPackages" value="true" />
            <property name="trimStrings" value="true" />
        </javaModelGenerator>
        <!-- SQL映射文件的生成策略 -->
        <sqlMapGenerator targetPackage="com.atguigu.mybatis.mapper"
                         targetProject=".\src\main\resources">
            <property name="enableSubPackages" value="true" />
        </sqlMapGenerator>
        <!-- Mapper接口的生成策略 -->
        <javaClientGenerator type="XMLMAPPER"
                             targetPackage="com.atguigu.mybatis.mapper" targetProject=".\src\main\java">
            <property name="enableSubPackages" value="true" />
        </javaClientGenerator>
        <!-- 逆向分析的表 -->
        <!-- tableName设置为*号，可以对应所有表，此时不写domainObjectName -->
        <!-- domainObjectName属性指定生成出来的实体类的类名 -->
        <table tableName="t_emp" domainObjectName="Emp"/>
        <table tableName="t_dept" domainObjectName="Dept"/>
    </context>
</generatorConfiguration>

```

### 执行 MBG 插件的 generate 目标
![[图片/Pasted image 20230108123536.png]]

如果出现报错：`Exception getting JDBC Driver`，可能是 pom. xml 中，数据库驱动配置错误
dependency 中的驱动
![[图片/Pasted image 20230108123602.png]]

[mybatis-generator](https://so.csdn.net/so/search?q=mybatis-generator&spm=1001.2101.3001.7020) -maven-plugin 插件中的驱动
![[图片/Pasted image 20230108123620.png]]
-   两者的驱动版本应该相同

执行结果
![[图片/Pasted image 20230108123644.png]]

## QBC
selectByExample：按条件查询，需要传入一个 example 对象或者 null；如果传入一个 null，则表示没有条件，也就是查询所有数据
example. createCriteria (). xxx：创建条件对象，通过 andXXX 方法为 SQL 添加查询添加，每个条件之间是 and 关系
example. or (). xxx：将之前添加的条件通过 or 拼接其他条件
![[图片/Pasted image 20230108123714.png]]

```java
@Test public void testMBG () throws IOException {
	InputStream is = Resources.getResourceAsStream("mybatis-config.xml");
	SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
	SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuilder.build(is);
	SqlSession sqlSession = sqlSessionFactory.openSession(true);
	EmpMapper mapper = sqlSession.getMapper(EmpMapper.class);
	EmpExample example = new EmpExample();
	//名字为张三，且年龄大于等于20
	example.createCriteria().andEmpNameEqualTo("张三").andAgeGreaterThanOrEqualTo(20);
	//或者did不为空
	example.or().andDidIsNotNull();
	List<Emp> emps = mapper.selectByExample(example);
	emps.forEach(System.out::println);
}
```

### 增改
updateByPrimaryKey：通过主键进行数据修改，如果某一个值为 null，也会将对应的字段改为 null

```java
mapper. updateByPrimaryKey (new Emp (1,"admin", 22, null," 456@qq.com ", 3));
```
![[图片/Pasted image 20230108123823.png]]
updateByPrimaryKeySelective ()：通过主键进行选择性数据修改，如果某个值为 null，则不修改这个字段
```java
mapper. updateByPrimaryKeySelective (new Emp (2,"admin2", 22, null," 456@qq.com ", 3));
```
![[图片/Pasted image 20230108123839.png]]

# 分页插件
## 使用步骤
### 添加依赖
```xml
<!-- https://mvnrepository.com/artifact/com.github.pagehelper/pagehelper -->
<dependency>
	<groupId>com.github.pagehelper</groupId>
	<artifactId>pagehelper</artifactId>
	<version>5.2.0</version>
</dependency>

```

### 配置分页插件
-   在 MyBatis 的核心配置文件（mybatis-config. xml）中配置插件

```xml
<plugins>
	<!--设置分页插件-->
	<plugin interceptor="com.github.pagehelper.PageInterceptor"></plugin>
</plugins>
```

## 分页插件的使用
-   在查询功能之前使用 `PageHelper. startPage (int pageNum, int pageSize)` 开启分页功能
    -   pageNum：当前页的页码
    -   pageSize：每页显示的条数
---
```java
 @org. junit. Test  
    public void test8() {  
        SqlSession sqlSession = SqlsessionUtils.getSqlSession();  
        EmpMapper mapper = sqlSession.getMapper(EmpMapper.class);  
        PageHelper.startPage(1, 2);  
//        在查询数据前开启分页插件  
        List<Emp> emps = mapper.selectEmp();  
	      PageInfo pageInfo = new PageInfo (emps, 5);  
System.out.println(pageInfo);
	 }
```

分页插件相关数据
```
PageInfo{pageNum=1, pageSize=2, size=2, startRow=1, endRow=2, total=10, pages=5, list=Page{count=true, pageNum=1, pageSize=2, startRow=0, endRow=2, total=10, pages=5, reasonable=false, pageSizeZero=false}[Emp{eid=3, empName='赵红', age=13, sex='女', email='123@qq.com', dept=null}, Emp{eid=4, empName='a', age=1, sex='男', email='123@qq.com', dept=null}], prePage=0, nextPage=2, isFirstPage=true, isLastPage=false, hasPreviousPage=false, hasNextPage=true, navigatePages=5, navigateFirstPage=1, navigateLastPage=5, navigatepageNums=[1, 2, 3, 4, 5]}
```
### 常用数据
常用数据：
	pageNum：当前页的页码
	pageSize：每页显示的条数
	size：当前页显示的真实条数
	total：总记录数
	pages：总页数
	prePage：上一页的页码
	nextPage：下一页的页码
	isFirstPage/isLastPage：是否为第一页/最后一页
	hasPreviousPage/hasNextPage：是否存在上一页/下一页
	navigatePages：导航分页的页码数
	navigatepageNums：导航分页的页码，[1, 2, 3, 4, 5]



