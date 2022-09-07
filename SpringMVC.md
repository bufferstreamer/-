#  	初识SpringMVC

![](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220319221241136.png)

1、spring框架围绕Dispatcherservlet展开，dispatcherservlet本质上还是一个servlet，所有的请求都会经过dispatcherservlet，所以只在web.xml中注册一个servlet。



```xml
 <servlet>
   <servlet-name>springmvc</servlet-name>
   <servlet-class>
       org.springframework.web.servlet.DispatcherServlet
   </servlet-class>
<!--        dispatcher绑定spring的配置文件-->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc-servlet.xml</param-value>
        </init-param>
<!--        启动级别1 随服务器一起启动-->
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
```

其中<url-pattern>/</url-pattern>的/会匹配所有的请求，但是不会匹配jsp页面

而/*则会匹配请求和jsp页面。

比如输入localhost：8080/r/a.jsp

也会匹配进入dispatchservlet，然后走到视图解析器的时候再拼接.jsp后缀，最后变成a.jsp.jsp会导致出错。

classpath是当前类的路径	



2、HandlerMapping是处理器映射，被diapatcherservlet调用。他会根据url查找handler，如下图的<bean id="/hello" class="com.ln.control.HelloController"/>，通过/hello就匹配到了。相当于servlet的mapping。

```xml
<?xml version="1.0" encoding="utf-8" ?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">

    <!--配置annotation类型的处理映射器-->
    <bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"/>
    <!--配置annotation类型的处理器适配器-->
    <bean class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter"/>
    <!--视图解析器-->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="internalResourceViewResolver">
        <!--    前缀-->
        <property name="prefix" value="/WEB-INF/jsp/"/>
        <!--        后缀-->
        <property name="suffix" value=".jsp"/>
    </bean>
    <bean id="/hello" class="com.ln.control.HelloController"/>
</beans>
```

3、执行HandlerExecution，解析控制器后返回给dispatcherservlet。即通过handlemapping所匹配到的bean，进一步执行。

4、handleadaptation会通过mapping接受的handle，跳转到hellocontroller控制器中。

```java
public class HelloController implements Controller {
    @Override
    public ModelAndView handleRequest(HttpServletRequest request, HttpServletResponse response) throws Exception {
        ModelAndView mv = new ModelAndView();
        //业务代码
        String result = "springmvc";
        mv.addObject("dd", result);
        //视图跳转
        mv.setViewName("hello");
        return mv;
    }
}
```

5、controller调用业务层，进行操作，然后将modelandview返回给adapter，然后adapt将请求返回dispatcherservlet

6、dispatch调用视图解析器，viewResolver获得viewandmodel的数据，解析视图的名字，如上式的hello，然后拼接上前缀和后缀，得到完整的视图名字，然后将数据渲染到视图上。

这种实现control接口的方法，一个类只能实现一个方法，而且每一次都要注册bean 所以推荐下面使用注解的方法

# springMVC的配置文件

## pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.example</groupId>
    <artifactId>Spring-MVC</artifactId>
    <packaging>pom</packaging>
    <version>1.0-SNAPSHOT</version>
    <modules>
        <module>springmvc-annotation</module>
    </modules>

    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
    </properties>
    <dependencies>
<!--        spring  -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>5.3.12</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>5.3.12</version>
        </dependency>
<!--        servlet -->
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>servlet-api</artifactId>
            <version>2.5</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet.jsp</groupId>
            <artifactId>jsp-api</artifactId>
            <version>2.2</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>jstl</artifactId>
            <version>1.2</version>
        </dependency>
<!--        mybatis  -->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.7</version>
        </dependency>
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis-spring</artifactId>
            <version>2.0.6</version>
        </dependency>
<!--    JSON    -->
        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-databind</artifactId>
            <version>2.13.0</version>
        </dependency>
<!--    LOMBOK    -->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.18.22</version>
            <scope>provided</scope>
        </dependency>
<!--        json    -->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>fastjson</artifactId>
            <version>1.2.78</version>
        </dependency>
<!--        数据库驱动   -->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.25</version>
        </dependency>
<!--        junit   -->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.13.2</version>
            <scope>test</scope>
        </dependency>

<!--        数据库连接池  -->
        <dependency>
            <groupId>com.mchange</groupId>
            <artifactId>c3p0</artifactId>
            <version>0.9.5.5</version>
        </dependency>
    </dependencies>
   
<build>
    <resources>
        <resource>
            <directory>src/main/java</directory>
            <includes>
                <include>**/*.xml</include>
                <include>**/*.properties</include>
            </includes>
            <filtering>true</filtering>
        </resource>
        <resource>
            <directory>src/main/resources</directory>
            <includes>
                <include>**/*.xml</include>
                <include>**/*.properties</include>
            </includes>
            <filtering>true</filtering>
        </resource>
    </resources>
</build>
</project>
```

## web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>
            org.springframework.web.servlet.DispatcherServlet
        </servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:applicationContext.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
    <filter>
        <filter-name>encoding</filter-name>
        <filter-class>
            org.springframework.web.filter.CharacterEncodingFilter
        </filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>UTF-8</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>encoding</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
</web-app>
```

## spring配置xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                           http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/context
                                                          
                         http://www.springframework.org/schema/context/spring-context.xsd
                            http://www.springframework.org/schema/mvc
                            http://www.springframework.org/schema/mvc/spring-mvc.xsd">
<!--    在该包下的controller就会被识别    -->
<context:component-scan base-package="dlut.ln.contr"/>
<!--    注解驱动    -->
    <!-- mvc 返回乱码处理 -->
    <mvc:annotation-driven>
        <mvc:message-converters register-defaults="true">
            <bean
         class="org.springframework.http.converter.StringHttpMessageConverter">
                <constructor-arg value="utf-8"/>
            </bean>
            <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter">
                <property name="objectMapper">
                    <bean class="org.springframework.http.converter.json.Jackson2ObjectMapperFactoryBean">
                        <property name="failOnEmptyBeans" value="false"/>
                    </bean>
                </property>
            </bean>
        </mvc:message-converters>
    </mvc:annotation-driven>
<!--    过滤静态资源  -->
    <mvc:default-servlet-handler/>
<!--    视图解析器   -->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="internalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/jsp/"/>
        <property name="suffix" value=".jsp"/>
    </bean>

</beans>
```

## mybatis配置xml文件

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

 

# 不同的请求方法

1、GET请求会向数据库发索取数据的请求，从而来获取信息，该请求就像数据库的select操作一样，只是用来查询一下数据，不会修改、增加数据，不会影响资源的内容，即该请求不会产生副作用。无论进行多少次操作，结果都是一样的。

2、与GET不同的是，PUT请求是向服务器端发送数据的，从而改变信息，该请求就像数据库的update操作一样，用来修改数据的内容，但是不会增加数据的种类等，也就是说无论进行多少次PUT操作，其结果并没有不同。

3、POST请求同PUT请求类似，都是向服务器端发送数据的，但是该请求会改变数据的种类等资源，就像数据库的insert操作一样，会创建新的内容。几乎目前所有的提交操作都是用POST请求的。

4、DELETE请求顾名思义，就是用来删除某一个资源的，该请求就像数据库的delete操作。

就像前面所讲的一样，既然PUT和POST操作都是向服务器端发送数据的，那么两者有什么区别呢。。。POST主要作用在一个集合资源之上的（url），而PUT主要作用在一个具体资源之上的（url/xxx），通俗一下讲就是，如URL可以在客户端确定，那么可使用PUT，否则用POST。

综上所述，我们可理解为以下：

1、POST /url 创建 
		2、DELETE /url/xxx 删除 
		3、PUT /url/xxx 更新
		4、GET /url/xxx 查看

总结一下，Get是向服务器发索取数据的一种请求，而Post是向服务器提交数据的一种请求，在FORM（表单）中，Method默认为"GET"，实质上，GET和POST只是发送机制不同，并不是一个取一个发！

#  注解

注解开发

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                            http://www.springframework.org/schema/beans/spring-beans.xsd
                            http://www.springframework.org/schema/context
                            http://www.springframework.org/schema/context/spring-context.xsd
                            http://www.springframework.org/schema/mvc
                            http://www.springframework.org/schema/mvc/spring-mvc.xsd">
<!--    在该包下的controller注解就会被识别    -->
<context:component-scan base-package="dlut.ln"/>
<!--    注解驱动    -->
    <mvc:annotation-driven/>
<!--    过滤静态资源  -->
    <mvc:default-servlet-handler/>
<!--    视图解析器   -->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="internalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/jsp/"/>
        <property name="suffix" value=".jsp"/>
    </bean>

</beans>
```

```java
@Controller//代表会被spring自动接管，被这个注解的类的方法返回值是string，并且有该页面的话就会被解析
public class annotation {
    @RequestMapping("/h1")//映射访问路径
    //需要传数据的时候可以形参设置为model
    public String hello(@org.jetbrains.annotations.NotNull Model model){
        model.addAttribute("msg","hello");
        return "hello";//回到哪个视图
    }
    @RequestMapping("/h2")//映射访问路径
    //需要传数据的时候可以形参设置为model
    public String hello2(@org.jetbrains.annotations.NotNull Model model){
        model.addAttribute("msg","hello22222222");
        return "hello";//回到哪个视图
    }
}
```

可以看到，不同的请求会指向同一个页面，展现不同的信息，所以控制器和视图是弱耦合

## RequestMapping

![image-20220319221725718](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220319221725718.png)

## ResponseBody

![image-20220319223716614](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220319223716614.png)

![image-20220319224501993](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220319224501993.png)

![image-20220319224613281](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220319224613281.png)

## RequestBody

![image-20220319225909871](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220319225909871.png)

## RequestParam

![image-20220319230617610](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220319230617610.png)

![image-20220319230850577](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220319230850577.png)

## RequestHeader

![image-20220319232549179](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220319232549179.png)

![image-20220319232806555](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220319232806555.png)





#  Restful

一种资源操作和资源定位的风格。

原始风格

```java
@Controller
public class Restful {
    @RequestMapping("/r")
    public String res(int a, int b, Model model){
        int res = a + b;
        model.addAttribute("msg","结果是： " + res);
        return "hello";
    }
}
```

想要得到结果需要的URL如下

http://localhost:8080/springmvc/r?a=2&b=1

输出结果为3

Restful风格

```java
@Controller
public class Restful {
    @RequestMapping("/r/{a}/{b}")
    public String res(@PathVariable int a, @PathVariable int b, Model model){
        int res = a + b;
        model.addAttribute("msg","结果是： " + res);
        return "hello";
    }
}
```

通过注解@Pathvariable使得不需要繁琐的url

新的url如下

http://localhost:8080/springmvc/r/2/1

输出结果为3



return +"/"或者+"forward" 表示转发

+"redirect"表示重定向。需要将视图解析器注释掉

##  接受请求参数

```java
@Controller
public class User {
    @GetMapping("/user")
    public String user(String name, Model model){
        System.out.println("name:" + name);
        model.addAttribute("msg",name);
        return "hello";
    }
}
```

![image-20211105215707082](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20211105215707082.png)

URL如上，即name和user的形参格式一致时，可以直接接收到。

如果不一致

```java
@Controller
public class User {
    @GetMapping("/user")
    public String user(@RequestParam("username") String name, Model model){
        System.out.println("name:" + name);
        model.addAttribute("msg",name);
        return "hello";
    }
}
```

@RequestParam可以将括号内的参数识别为name

此时的url

![image-20211105220116795](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20211105220116795.png)

如果前端传递的是一个对象，参数直接使用对象即可，但是表单所提供的属性参数必须和对象的属性相同。

# 类型转换器

![image-20220319231928995](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220319231928995.png)



#  乱码问题

springmvc为我们提供了一个乱码问题的过滤器

```xml
<filter>
    <filter-name>encoding</filter-name>
    <filter-class>
        org.springframework.web.filter.CharacterEncodingFilter
    </filter-class>
    <init-param>
        <param-name>encoding</param-name>
        <param-value>UTF-8</param-value>
    </init-param>
</filter>
<filter-mapping>
    <filter-name>encoding</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

注意url匹配的要是/*过滤掉jsp等文件

# 文件上传



#  JSON

一种数据格式，使前后端可以通过该方式进行数据传输，做到数据的一致性

```java
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data   //提供了get set hashcode equal等方法
@AllArgsConstructor     //有参构造器
@NoArgsConstructor      //无参构造器
public class User {
    private String name;
    private int age;
    private String sex;
}
```

```java
@Controller
public class Json {
    @RequestMapping("j1")
    @ResponseBody   //该注解使函数不经过视图解析器，return返回的就是普通的字符串 同@Restcontroller
    public String json(){

        User user = new User("LN",22,"man");

        return user.toString();
    }
}
```

此时前端的页面响应为

![image-20211106140704372](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20211106140704372.png)

可见，将一个对象转化为字符串形式，这也是json的思想

上文的@Data   //提供了get set hashcode equal等方法
					@AllArgsConstructor     //有参构造器
					@NoArgsConstructor      //无参构造器

需要导入lombok包

```xml
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.12</version>
    <scope>provided</scope>
</dependency>
```

json包

```xml
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.12.3</version>
</dependency>
```

导入json后会出现中文乱码问题，由于不是响应过程，过滤器不起作用。spring提供了专用于json的乱码解决框架

```xml
<mvc:annotation-driven>
    <mvc:message-converters register-defaults="true">
        <bean class="org.springframework.http.converter.StringHttpMessageConverter">
            <constructor-arg value="utf-8"/>
        </bean>
        <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter">
            <property name="objectMapper">
                <bean class="org.springframework.http.converter.json.Jackson2ObjectMapperFactoryBean">
                    <property name="failOnEmptyBeans" value="false"/>
                </bean>
            </property>
        </bean>
    </mvc:message-converters>
</mvc:annotation-driven>
```

# Ajax

无需在刷新整个页面的情况下，更新部分页面的的技术

```
<groupId>org.example</groupId>
<artifactId>Spring-MVC-archetype</artifactId>
    <version>1.0-SNAPSHOT</version>
<packaging>maven-archetype</packaging>
```

# jdbcTemplate

![image-20220320174155592](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220320174155592.png)

![image-20220321202343957](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220321202343957.png)

# 	拦截器

![image-20220322184513003](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220322184513003.png)

![image-20220322190411488](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220322190411488.png)

# 异常处理

# AOP

![image-20220323203523548](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220323203523548.png)

![image-20220326134503146](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220326134503146.png)

## XML方式

![image-20220326173231840](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220326173231840.png)

###  切点表达式写法![image-20220326171457214](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220326171457214.png)

![image-20220326171535009](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220326171535009.png)

### 通知类型

![image-20220326171610145](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220326171610145.png)

其中环绕通知的通知中形参必须有一个proceedingJoinPoint 作为当前的切点 如下：

![image-20220326172324461](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220326172324461.png)

### 切点表达式抽取

![image-20220326173056764](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220326173056764.png)

## 注解方式

![image-20220326182419183](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220326182419183.png)

### 注解通知类型

![image-20220326173953987](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220326173953987.png)

### 切点表达式抽取

![image-20220326182401732](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220326182401732.png)

本来想自己定义一个字符串 结果发现不是单纯的字符串 他不能识别

![image-20220326190702402](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220326190702402.png)

# 事务

## 编程式事务

![image-20220326210514575](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220326210514575.png)

![image-20220326210550018](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220326210550018.png)

![image-20220326210606215](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220326210606215.png)

![image-20220326210629819](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220326210629819.png)

## 声明式事务

![](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220326211331967.png)

![image-20220328134925187](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220328134925187.png)

先声明一个事务管理器 然后进行相应的事务增强配置

![image-20220326213141834](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220326213141834.png)

## 基于注解的事务配置

![image-20220326215923267](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220326215923267.png)



同样需要配置事务管理器 然后开启注解驱动 在相应的类或者方法上面加注解即可

![image-20220328135014326](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220328135014326.png)

![image-20220328135106935](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220328135106935.png)

![image-20220328135151614](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220328135151614.png)



# SSM整合

