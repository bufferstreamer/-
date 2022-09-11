# IOC

## 简介

![image-20220831211117485](D:\笔记\spring.assets/image-20220831211117485.png)

![image-20220831211134910](D:\笔记\spring.assets/image-20220831211134910.png)

控制反转，一种设计思想。spring的核心内容![](D:\笔记\spring.assets/image-20220831211206050.png)

## bean管理

### 基于xml

![](D:\笔记\spring.assets/image-20220831211943552.png)

![image-20220901131058955](D:\笔记\spring.assets/image-20220901131058955.png)

### 基于注解

![image-20220901145554785](D:\笔记\spring.assets/image-20220901145554785.png)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
<!--
        使用spring来创建对象，在spring中成为bean
        类型   变量名 = new 类型
        Hello hello = new Hello()
        bean = 对象
        id   = 变量名
        class = 类的全路径名
        property 变量值设定

        核心是通过set方法来实现的
        value     基本数据类型
        ref       引用spring创建好的对象 即bean的id
-->
        <bean id="hello" class="dut.ln.pojo.hello">
            <property name="srt" value="spring"/>
        </bean>
</beans>
```

```java
package dut.ln.pojo;

public class hello {
    private String srt;

    public String getSrt() {
        return srt;
    }

    public void setSrt(String srt) {
        this.srt = srt;
    }

    public  void show ( ) {
        System.out.println("hello spring");
    }


}
```

不需要new hello 只需要使用ClassPathApplicationContext获取注册了hello的bean的对应的xml文件，在使用getbean得到对应的bean，就完成了实例化

```java
import dut.ln.pojo.hello;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class myTest {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
        hello h = (hello) context.getBean("hello");
        h.show();
    }
}
```

## IOC创建对象的方式

### 默认为无参构造方式

### 有参构造方式

#### 第一种

下标赋值

```xml
<bean id="hello" class="dut.ln.pojo.hello">
    <constructor-arg index="0" value="有参构造方式"/>
</bean>
```

#### 第二种

```xml
<bean id="hello" class="dut.ln.pojo.hello">
   <constructor-arg type="java.lang.String" value="有参构造"/>
</bean>
```

但是当hello类的有参构造函数有多个形参的时候，就不建议使用第二种方法

#### 第三种

直接赋值给构造函数的形参

```xml
<bean id="hello" class="dut.ln.pojo.hello">
    <constructor-arg name="srt" value="有参构造2"/>
</bean>
```

在配置文件加载的时候，文件内的bean就已经被加载了

# Spring配置

## 别名alias

```xml
<alias name="hello" alias="hello2"/>
```

相当于给实例化的对象取了一个别名

## bean

```xml
<bean id="hello" class="dut.ln.pojo.hello" name="hello3，hello4">
    <constructor-arg name="srt" value="有参构造2"/>
</bean>
```

id   = 变量名
		class = 类的全路径名
		name = 别名  并且可以取多个 通过; 空格 和，分割



## import

将多个配置文件导入合并

# 依赖注入

## 构造器注入



## set注入

### 	依赖：bean对象的创建依赖于容器

### 	注入：bean对象中的所有属性，由容器来注入

```xml
<bean id="user" class="dut.ln.pojo.user">
    <property name="id" value="12"/>
    <property name="name" value="ln"/>
</bean>
<bean id="hello" class="dut.ln.pojo.hello" name="hello3,hello4">
    <property name="use" ref="user"/>
</bean>
```

## 其他方式

### p命名空间

需要导入 xmlns:p="http://www.springframework.org/schema/p"

```xml
<bean id="user" class="dut.ln.pojo.user" p:id="11112" p:name="ln"/>
```

p即property，可以直接注入属性值

### c命名空间

需要导入xmlns:c="http://www.springframework.org/schema/c"

```xml
<bean id="user2" class="dut.ln.pojo.user" c:id="115" c:name="ln"/>
```

c即constructor，即通过构造器注入

总的来说只有通过set注入和通过构造器注入两种方式

# BEAN作用域

## 单例模式

每次从容器中getbean的时候，都产生的是同一个对象，只是命名不同

```xml
<bean id="user" class="dut.ln.pojo.user" p:id="11112" p:name="ln" scope="singleton"/>
```

## 原型模式

每次取得的是不同的对象

```xml
<bean id="user2" class="dut.ln.pojo.user" c:id="115" c:name="ln" scope="prototype"/>
```

# bean的自动装配

spring会在上下文中自动寻找装配

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class hello {
    private String srt;
    private user user;
    private student student;
}
```

```xml
<bean id="user" class="dut.ln.pojo.user"/>
<bean id="student" class="dut.ln.pojo.student"/>


<bean id="hello" class="dut.ln.pojo.hello" autowire="byName">
    <property name="srt" value="ln"/>
</bean>
```

autowire=byname会根据上文的beanid 自动将其装配到hello的bean中，要求id值与class中对应的属性值相同，即user而不能是user1111.

同时autowire="byType"会根据bean的class来自动装配

但是两种方法都需要id或者class唯一

# 注解实现自动装配

需要导入

```xml
xmlns:context="http://www.springframework.org/schema/context"
http://www.springframework.org/schema/context
http://www.springframework.org/schema/context/spring-context.xsd
```

使用注解支持

```xml
  <context:annotation-config/>
        <bean id="user" class="dut.ln.pojo.user"/>
        <bean id="student" class="dut.ln.pojo.student"/>
        <bean id="hello" class="dut.ln.pojo.hello"/>
```

在需要装配的属性上加@Autowired

@autowired先bytype后byname

@resource先byname

# 注解开发

原始注解

@Component注解

作用：用于把当前类的对象存入到spring容器中

属性：value：指定bean的id，不写的时候默认是当前的类名，且首字母小写。当指定属性的值的时候就要用该值

其他controller等同理

@Autowired： 根据类型注入          

@Resource ：默认根据名字注入，其次按照类型搜索

 既不指定name属性，也不指定type属性，则自动按byName方式进行查找。如果没有找到符合的bean，则回 退为一个原始类型进行进行查找，如果找到就注入。
          只是指定了@Resource注解的name，则按name后的名字去bean元素里查找有与之相等的name属性bean。
           只指定@Resource注解的type属性，则从上下文中找到类型匹配的唯一bean进行装配，找不到或者找到多个，都会抛出异常。

@Autowired ：@Qualifie("dogServiceImpl") 两个结合起来可以根据名字和类型注入



![image-20220309113015217](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220309113015217.png)

新注解

![image-20220309142716150](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220309142716150.png)

## bean注册

需要导入context

```xml
<context:component-scan base-package="dut.ln.pojo"/>
```

该包下的注解会被扫描

然后在相应的类中使用@component

```java
@Component
public class hello {
    private String srt;
    @Autowired
    private user user;
    @Autowired
    private student student;
}
```

## 属性注入

通过@value

```java
@Value("LN")
private String srt;
```

## 衍生注解

在MVC三层架构中

Dao层：@repository

service层：@service

controller层：@controller

同@component作用相同，都是将某个类注入到spring容器中。

# 使用java的方式配置spring

@configuration代表这是一个配置类，作用与xml文件相同

## @bean

注册一个bean，使用在方法上，相当于之前的bean标签，该方法的名字相当于bean中的id属性，方法的返回值就是返回的注入到bean中的对象。

## @import（xxx.class）

引入另一个@configuration

# Spring集成junit

![image-20220326225431172](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220326225431172.png)

# AOP

![image-20220901162932300](D:\笔记\spring.assets/image-20220901162932300.png)

![image-20220901162505606](D:\笔记\spring.assets/image-20220901162505606.png)

![image-20220901162955982](D:\笔记\spring.assets/image-20220901162955982.png)

## jdk动态代理

```java
UserDao proxyInstance = (UserDao)Proxy.newProxyInstance(MyProxy.class.getClassLoader(), new Class[]{UserDao.class}, (proxy, method, args1) -> {
    System.out.println("加强");
    method.invoke(new UserImpl(), args1);
    System.out.println("加强后");
    return null;
});
proxyInstance.add();
```

## aop术语

![](D:\笔记\spring.assets/image-20220902130923428.png)

## 操作

![image-20220902131500614](D:\笔记\spring.assets/image-20220902131500614.png)

![image-20220902132447168](D:\笔记\spring.assets/image-20220902132447168.png)

![image-20220902132536387](D:\笔记\spring.assets/image-20220902132536387.png)

# webflux

![image-20220902152740005](D:\笔记\spring.assets/image-20220902152740005.png)
