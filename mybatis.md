# 	持久化

数据持久化

	持久化就是将程序的数据在持久状态和瞬时状态转化的过程

	内存：断电即失，但是为了保存数据，所以需要数据持久化

# 持久层

完成持久化工作的代码块

![image-20220326223855971](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220326223855971.png)

## 创建mybatis配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
       <property name="url" value="jdbc:mysql://localhost:3306/storage?useSSL=true&amp;userUnicode=true&amp;characterEncoding=UTF-8&amp;serverTimezone=GMT"/>
                <property name="username" value="root"/>
                <property name="password" value="123456"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <mapper resource="Dao/CommodityMapper.xml"/>
    </mappers>
</configuration>
```

## 创建mybatis工具类

```java
package Utils;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.IOException;
import java.io.InputStream;

public class MybatisUtils {
    public static SqlSessionFactory sqlSessionFactory;
    //创建一个sqlSession工厂
    static {
        String resources = "mybatis-config.xml";
        InputStream inputStream = null;
        try {
            inputStream = Resources.getResourceAsStream(resources);
        } catch (IOException e) {
            e.printStackTrace();	
        }
         sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
    }
    //获得一个openSession,即SqlSession的实例，其中包含了所有sql的方法
    public static SqlSession getSqlSession(){
        return sqlSessionFactory.openSession();
    }
}
```

## 创建一个接口类并创建相应的xml

```java
package Dao;

import pojo.Commodity;

import java.util.List;

public interface CommodityDao {
    List<Commodity> getCommodityList();
}
```

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--绑定一个对应的mapper接口-->
<mapper namespace="Dao.CommodityDao">

    <select id="CommodityDao" resultType="Dao.CommodityDao.Commodity">
        select * from storage.commodity
    </select>
</mapper>
```



## 测试

```java
public class CommodityDaoTest {
    @Test
    public void test(){
        //获得sqlSession对象
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        CommodityDao commodityDao = sqlSession.getMapper(CommodityDao.class);
        List<Commodity> commodityList = commodityDao.getCommodityList();
        for (Commodity commodity : commodityList) {
            System.out.println(commodity);
        }
        sqlSession.close();
    }
}
```

```java
Commodity(id=1, cateId=null, name=ln, subtitle=null, main_image=, sub_images=null, detail=, price=15.0, quantity=100, location=sss, status=0)

```

结果如上





# CRUD

步骤：

在DAO层编写抽象方法

DAO对应的mapper.xml实现相应的sql语句

测试

## select

```java
public interface CommodityDao {
    //查询全部商品
    List<Commodity> getCommodityList();
    //根据ID查询商品
    Commodity getCommodityByID(int id);
    //插入新的商品
    void addCommodity(Commodity commodity);
    //修改商品信息
    void updateCommodity(Commodity commodity);
    //删除商品信息
    void deleteCommodity(int id);
}
```

```xml
<mapper namespace="Dao.CommodityDao">
    <select id="getCommodityList" resultType="pojo.Commodity" parameterType="">
        select * from storage.commodity
    </select>
</mapper>
```

id为namespace空间对应的类中的方法名

resultType为返回值类型

parameterType为参数类型

```java
@Test
public void test(){
    //获得sqlSession对象
    SqlSession sqlSession = MybatisUtils.getSqlSession();
    CommodityDao commodityDao = sqlSession.getMapper(CommodityDao.class);
    List<Commodity> commodityList = commodityDao.getCommodityList();
    for (Commodity commodity : commodityList) {
        System.out.println(commodity);
    }
    sqlSession.close();
}
@Test
public void testId(){
    SqlSession sqlSession = MybatisUtils.getSqlSession();
    CommodityDao mapper = sqlSession.getMapper(CommodityDao.class);
    System.out.println(mapper.getCommodityByID(1));
    sqlSession.close();
}
```

## insert

```xml
<insert id="addCommodity" parameterType="pojo.Commodity">
    insert into storage.commodity(id, CateId, name, subtitle, main_image, sub_images, detail, price, quantity, location, status) VALUES (#{id},#{CateId},#{name},#{subtitle},#{main_image},#{sub_images},#{detail},#{price},#{quantity},#{location},#{status})
</insert># 
```

```java
@Test
public void testAdd(){
    SqlSession sqlSession = MybatisUtils.getSqlSession();
    CommodityDao sqlSessionMapper = sqlSession.getMapper(CommodityDao.class);
    sqlSessionMapper.addCommodity(new Commodity("6","dm","dd","dd","dd","dd","dd",20.0,1,"dd",0));
    System.out.println("插入成功");
    sqlSession.commit();
    sqlSession.close();
}
```

## update

```xml
<update id="updateCommodity" parameterType="pojo.Commodity">
    update storage.commodity
        set name = #{name}
    where id = #{id};
</update>
```

```java
@Test
public void testUpdate(){
    SqlSession sqlSession = MybatisUtils.getSqlSession();
    CommodityDao sqlSessionMapper = sqlSession.getMapper(CommodityDao.class);
    sqlSessionMapper.updateCommodity(new Commodity("4","dd","ss","dd","dd","dd","dd",20.0,1,"dd",0));
    sqlSession.commit();
    sqlSession.close();
}
```

## delete

```xml
<delete id="deleteCommodity" parameterType="int">
    delete from storage.commodity WHERE id = #{id}
</delete>
```

```java
@Test
public void testDelete(){
    SqlSession sqlSession = MybatisUtils.getSqlSession();
    CommodityDao mapper = sqlSession.getMapper(CommodityDao.class);
    mapper.deleteCommodity(4);
    sqlSession.commit();
    sqlSession.close();
}
```

## 模糊查询

使用通配符%

```java
public void testLike(){
    SqlSession sqlSession = MybatisUtils.getSqlSession();
    CommodityDao mapper = sqlSession.getMapper(CommodityDao.class);
    List<Commodity> commodityLike = mapper.getCommodityLike("%l%");
    for (Commodity commodity : commodityLike) {
        System.out.println(commodity);
    }
    sqlSession.close();
```

# 配置解析

## 核心配置文件 mybatis-config.xml



- configuration（配置）
	- properties（属性）
	- settings（设置）
	- typeAliases（类型别名）
	- typeHandlers（类型处理器）
	- objectFactory（对象工厂）
	- plugins（插件）
	- environments（环境配置）
		- environment（环境变量）
			- transactionManager（事务管理器）
			- dataSource（数据源）
	- databaseIdProvider（数据库厂商标识）
	- mappers（映射器）

### environments

![image-20220326230835594](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220326230835594.png)

```xml
<environments default="development">
    <environment id="development">
        <transactionManager type="JDBC"/>
        <dataSource type="POOLED">
            <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
            <property name="url" value="jdbc:mysql://localhost:3306/storage?useSSL=true&amp;userUnicode=true&amp;characterEncoding=UTF-8&amp;serverTimezone=GMT"/>
            <property name="username" value="root"/>
            <property name="password" value="123456"/>
        </dataSource>
    </environment>
</environments>
```

### **事务管理器（transactionManager）**

在 MyBatis 中有两种类型的事务管理器（也就是 type="[JDBC|MANAGED]"）：

- JDBC – 这个配置直接使用了 JDBC 的提交和回滚设施，它依赖从数据源获得的连接来管理事务作用域。

- MANAGED – 这个配置几乎没做什么。它从不提交或回滚一个连接，而是让容器来管理事务的整个生命周期（比如 JEE 应用服务器的上下文）。 默认情况下它会关闭连接。然而一些容器并不希望连接被关闭，因此需要将 closeConnection 属性设置为 false 来阻止默认的关闭行为

	**提示** 如果你正在使用 Spring + MyBatis，则没有必要配置事务管理器，因为 Spring 模块会使用自带的管理器来覆盖前面的配置。

### **数据源（dataSource）**

dataSource 元素使用标准的 JDBC 数据源接口来配置 JDBC 连接对象的资源。

**UNPOOLED**– 这个数据源的实现会每次请求时打开和关闭连接。虽然有点慢，但对那些数据库连接可用性要求不高的简单应用程序来说，是一个很好的选择。 性能表现则依赖于使用的数据库，对某些数据库来说，使用连接池并不重要，这个配置就很适合这种情形。UNPOOLED 类型的数据源仅仅需要配置以下 5 种属性：

- `driver` – 这是 JDBC 驱动的 Java 类全限定名（并不是 JDBC 驱动中可能包含的数据源类）。
- `url` – 这是数据库的 JDBC URL 地址。
- `username` – 登录数据库的用户名。
- `password` – 登录数据库的密码。
- `defaultTransactionIsolationLevel` – 默认的连接事务隔离级别。
- `defaultNetworkTimeout` – 等待数据库操作完成的默认网络超时时间（单位：毫秒）。查看 `java.sql.Connection#setNetworkTimeout()` 的 API 文档以获取更多信息。

作为可选项，你也可以传递属性给数据库驱动。只需在属性名加上“driver.”前缀即可，例如：

- `driver.encoding=UTF8`

这将通过 DriverManager.getConnection(url, driverProperties) 方法传递值为 `UTF8` 的 `encoding` 属性给数据库驱动。

**POOLED**– 这种数据源的实现利用“池”的概念将 JDBC 连接对象组织起来，避免了创建新的连接实例时所必需的初始化和认证时间。 这种处理方式很流行，能使并发 Web 应用快速响应请求。

除了上述提到 UNPOOLED 下的属性外，还有更多属性用来配置 POOLED 的数据源：

- `poolMaximumActiveConnections` – 在任意时间可存在的活动（正在使用）连接数量，默认值：10
- `poolMaximumIdleConnections` – 任意时间可能存在的空闲连接数。
- `poolMaximumCheckoutTime` – 在被强制返回之前，池中连接被检出（checked out）时间，默认值：20000 毫秒（即 20 秒）
- `poolTimeToWait` – 这是一个底层设置，如果获取连接花费了相当长的时间，连接池会打印状态日志并重新尝试获取一个连接（避免在误配置的情况下一直失败且不打印日志），默认值：20000 毫秒（即 20 秒）。
- `poolMaximumLocalBadConnectionTolerance` – 这是一个关于坏连接容忍度的底层设置， 作用于每一个尝试从缓存池获取连接的线程。 如果这个线程获取到的是一个坏的连接，那么这个数据源允许这个线程尝试重新获取一个新的连接，但是这个重新尝试的次数不应该超过 `poolMaximumIdleConnections` 与 `poolMaximumLocalBadConnectionTolerance` 之和。 默认值：3（新增于 3.4.5）
- `poolPingQuery` – 发送到数据库的侦测查询，用来检验连接是否正常工作并准备接受请求。默认是“NO PING QUERY SET”，这会导致多数数据库驱动出错时返回恰当的错误消息。
- `poolPingEnabled` – 是否启用侦测查询。若开启，需要设置 `poolPingQuery` 属性为一个可执行的 SQL 语句（最好是一个速度非常快的 SQL 语句），默认值：false。
- `poolPingConnectionsNotUsedFor` – 配置 poolPingQuery 的频率。可以被设置为和数据库连接超时时间一样，来避免不必要的侦测，默认值：0（即所有连接每一时刻都被侦测 — 当然仅当 poolPingEnabled 为 true 时适用）。

**JNDI** – 这个数据源实现是为了能在如 EJB 或应用服务器这类容器中使用，容器可以集中或在外部配置数据源，然后放置一个 JNDI 上下文的数据源引用。这种数据源配置只需要两个属性：

- `initial_context` – 这个属性用来在 InitialContext 中寻找上下文（即，initialContext.lookup(initial_context)）。这是个可选属性，如果忽略，那么将会直接从 InitialContext 中寻找 data_source 属性。
- `data_source` – 这是引用数据源实例位置的上下文路径。提供了 initial_context 配置时会在其返回的上下文中进行查找，没有提供时则直接在 InitialContext 中查找。

和其他数据源配置类似，可以通过添加前缀“env.”直接把属性传递给 InitialContext。比如：

- `env.encoding=UTF8`

这就会在 InitialContext 实例化时往它的构造方法传递值为 `UTF8` 的 `encoding` 属性。

### 属性（properties）

这些属性可以在外部进行配置，并可以进行动态替换。你既可以在典型的 Java 属性文件中配置这些属性，也可以在 properties 元素的子元素中设置。例如：

```xml
					《引用外部配置文件》
<properties resource="database.properties">
  <property name="username" value="dev_user"/>
  <property name="password" value="F2Fa3!33TYyg"/>
</properties>
					或
 <properties resource="database.properties"/>
```

设置好的属性可以在整个配置文件中用来替换需要动态配置的属性值。比如:

```xml
				《内部引用，当没有外部引用时，value如properties方式写》
<dataSource type="POOLED">
  <property name="driver" value="${driver}"/>
  <property name="url" value="${url}"/>
  <property name="username" value="${username}"/>
  <property name="password" value="${password}"/>
</dataSource>
```

这个例子中的 username 和 password 将会由 properties 元素中设置的相应值来替换。 driver 和 url 属性将会由 config.properties 文件中对应的值来替换。这样就为配置提供了诸多灵活选择。



```properties
						《database.properties》
driver=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/storage?useSSL=true&userUnicode=true&characterEncoding=UTF-8&serverTimezone=GMT
username=root
password=123456
```

同时使用外部和内部文件引用时，以外部为主

![image-20220326231157422](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220326231157422.png)

### 类型别名（typeAliases）

类型别名可为 Java 类型设置一个缩写名字。 它仅用于 XML 配置，意在降低冗余的全限定类名书写。例如：

```xml
<typeAliases>
  <typeAlias alias="Author" type="domain.blog.Author"/>
  <typeAlias alias="Blog" type="domain.blog.Blog"/>
  <typeAlias alias="Comment" type="domain.blog.Comment"/>
  <typeAlias alias="Post" type="domain.blog.Post"/>
  <typeAlias alias="Section" type="domain.blog.Section"/>
  <typeAlias alias="Tag" type="domain.blog.Tag"/>
</typeAliases>
```

当这样配置时，`Blog` 可以用在任何使用 `domain.blog.Blog` 的地方。

也可以指定一个包名，MyBatis 会在包名下面搜索需要的 Java Bean，比如：

```xml
<typeAliases>
  <package name="domain.blog"/>
</typeAliases>
```

每一个在包 `domain.blog` 中的 Java Bean，在没有注解的情况下，会使用 Bean 的首字母小写的非限定类名来作为它的别名。即指定的该包下的所有类，都以其类名首字母小写来作为别名 比如 `domain.blog.Author` 的别名为 `author`；若有注解，则别名为其注解值。见下面的例子：

```java
@Alias("author")
public class Author {
    ...
}
```

### 设置（settings）

| 设置名             | 描述                                                         | 有效值        | 默认值 |
| :----------------- | :----------------------------------------------------------- | :------------ | :----: |
| cacheEnabled       | 全局性地开启或关闭所有映射器配置文件中已配置的任何缓存。     | true \| false |  true  |
| lazyLoadingEnabled | 延迟加载的全局开关。当开启时，所有关联对象都会延迟加载。 特定关联关系中可通过设置 `fetchType` 属性来覆盖该项的开关状态。 | true \| false | false  |

| logImpl | 指定 MyBatis 所用日志的具体实现，未指定时将自动查找。 | SLF4J \| LOG4J \| LOG4J2 \| JDK_LOGGING \| COMMONS_LOGGING \| STDOUT_LOGGING \| NO_LOGGING | 未设置 |
| ------- | ----------------------------------------------------- | ------------------------------------------------------------ | ------ |
|         |                                                       |                                                              |        |

### 映射器（mappers）

既然 MyBatis 的行为已经由上述元素配置完了，我们现在就要来定义 SQL 映射语句了。 但首先，我们需要告诉 MyBatis 到哪里去找到这些语句。 在自动查找资源方面，Java 并没有提供一个很好的解决方案，所以最好的办法是直接告诉 MyBatis 到哪里去找映射文件。 你可以使用相对于类路径的资源引用，或完全限定资源定位符（包括 `file:///` 形式的 URL），或类名和包名等。例如：

```xml
<!-- 使用相对于类路径的资源引用 -->
<mappers>
  <mapper resource="org/mybatis/builder/AuthorMapper.xml"/>
  <mapper resource="org/mybatis/builder/BlogMapper.xml"/>
  <mapper resource="org/mybatis/builder/PostMapper.xml"/>
</mappers>
<!-- 使用完全限定资源定位符（URL） -->
<mappers>
  <mapper url="file:///var/mappers/AuthorMapper.xml"/>
  <mapper url="file:///var/mappers/BlogMapper.xml"/>
  <mapper url="file:///var/mappers/PostMapper.xml"/>
</mappers>
<!-- 使用映射器接口实现类的完全限定类名 -->
<mappers>
  <mapper class="org.mybatis.builder.AuthorMapper"/>
  <mapper class="org.mybatis.builder.BlogMapper"/>
  <mapper class="org.mybatis.builder.PostMapper"/>
</mappers>
<!-- 将包内的映射器接口实现全部注册为映射器 -->
<mappers>
  <package name="org.mybatis.builder"/>
</mappers>
```

这些配置会告诉 MyBatis 去哪里找映射文件，剩下的细节就应该是每个 SQL 映射文件了，也就是接下来我们要讨论的。

## Mapper.xml文件

![image-20220326224507904](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220326224507904.png)

# 常用API

![image-20220326231833706](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220326231833706.png)

![image-20220326231840557](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220326231840557.png)

![image-20220326231927201](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220326231927201.png)

# DAO实现

## 代理开发

![image-20220327204404552](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220327204404552.png)

![image-20220327204429188](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220327204429188.png).

# 动态SQL

![image-20220327205428069](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220327205428069.png)

![image-20220327205819337](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220327205819337.png)

![image-20220327214141284](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220327214141284.png)

## 自定义类型转换器

![image-20220327215551357](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220327215551357.png)

方法的参数有的时候再看

# 注解开发

![image-20220328143235530](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220328143235530.png)



# 一些规则

![image-20220312223253440](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220312223253440.png)

![image-20220312223340965](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220312223340965.png)

