# 基础篇

## 对比

![image-20220330195844581](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220330195844581.png)

##  初体验

<img src="C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220330200136743.png" alt="image-20220330200136743" style="zoom:130%;" />

![image-20220330200150205](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220330200150205.png)

@RestController相当于@Controller+@ResponBody

## 解读

###  POM

![image-20220330200852964](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220330200852964.png)

![image-20220330202102612](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220330202102612.png)

![image-20220330203445554](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220330203445554.png)



### 引导类

![](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220330203759740.png)

### 内嵌tomcat

![image-20220330204404021](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220330204404021.png)

![image-20220330204228214](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220330204228214.png)



starter-web里面调用了starter-tomcat然后调用了starter-embed-core



如果不想用tomcat可以采用exclusion去除 再另行添加别的服务器

![image-20220330204246939](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220330204246939.png)



三种服务器

![image-20220330204337089](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220330204337089.png)

## restful

![image-20220330204922802](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220330204922802.png)

![](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220330205154873.png)

### 案例

![image-20220330210213131](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220330210213131.png)

![image-20220330210258459](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220330210258459.png)

### 简化注解

![image-20220330212308336](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220330212308336.png)

![image-20220330212314898](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220330212314898.png)

## 属性配置

### 修改服务器端口

![image-20220330212943727](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220330212943727.png)

即只需在该配置文件中书写需要修改的属性即可

**举例：**

![image-20220330213635608](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220330213635608.png)

![image-20220330220022886](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220330220022886.png)

![image-20220330220653520](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220330220653520.png)

### YAML

![image-20220330221147109](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220330221147109.png)

#### 语法

![image-20220330221245213](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220330221245213.png)

![image-20220330221904258](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220330221904258.png)

![image-20220330221926278](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220330221926278.png)

#### 读取YAML数据

![image-20220330232528725](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220330232528725.png)



![image-20220330232450220](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220330232450220.png)





![image-20220330232748828](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220330232748828.png)

![image-20220330232733932](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220330232733932.png)



#### 一次读取yaml全部数据

![image-20220330232937068](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220330232937068.png)

##### 封装数据

![image-20220330233536405](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220330233536405.png)

![image-20220330233509308](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220330233509308.png)

## 整合第三方技术

### 整合Junit

![image-20220330234057843](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220330234057843.png)

![image-20220330234050894](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220330234050894.png)



**==注意：==**


    ![image-20220330235014834](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220330235014834.png)

![image-20220330234927593](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220330234927593.png)

### 整合mybatis

![image-20220331000523682](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220331000523682.png)

![image-20220331000444379](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220331000444379.png)

![image-20220331000452991](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220331000452991.png)

![image-20220331000512317](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220331000512317.png)

### 整合mybatisPlus

![image-20220331223101474](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220331223101474.png)

![image-20220331223156346](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220331223156346.png)

### 整合Druid

![image-20220331224238527](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220331224238527.png)

![image-20220331224231874](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220331224231874.png)

## ssmp整合

### dao

![image-20220401150043028](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220401150043028.png)

![image-20220401145950798](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220401145950798.png)

![image-20220401150004726](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220401150004726.png)

![image-20220401150029675](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220401150029675.png)



### service

![image-20220401145813922](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220401145813922.png)

![image-20220401145643171](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220401145643171.png)

![image-20220401145725758](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220401145725758.png)

![image-20220401145747072](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220401145747072.png)

### controller

![image-20220401163103794](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220401163103794.png)

![image-20220401162850761](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220401162850761.png)

返回格式：

![image-20220401163312519](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220401163312519.png)

### 数据格式统一

![](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220401163953694.png)

![image-20220401163600362](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220401163600362.png)

### 前后端联调

![image-20220401164719306](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220401164719306.png)

![image-20220401164710492](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220401164710492.png)

# 运维篇

## 打包

![image-20220401172145767](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220401172145767.png)

![image-20220401172126860](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220401172126860.png)

### 比较

![image-20220401173139280](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220401173139280.png)

关键是下面的文件

![image-20220401173217207](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220401173217207.png)

## 配置

![image-20220401180103874](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220401180103874.png)

### 端口被占用

![image-20220401173412641](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220401173412641.png)

### 临时属性修改

![image-20220401174143250](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220401174143250.png)

![image-20220401174017968](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220401174017968.png)

### idea临时属性

![image-20220401174529012](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220401174529012.png)

#### 通过程序实现

![image-20220401174504325](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220401174504325.png)

#### 通过代码实现

![image-20220401174606707](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220401174606707.png)

### 配置文件权限

![image-20220401175410523](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220401175410523.png)

![image-20220401175325023](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220401175325023.png)

### 自定义配置文件

![image-20220401175853910](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220401175853910.png)

![image-20220401175819619](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220401175819619.png)

	![image-20220401175831937](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220401175831937.png)

## 多环境开发

![image-20220401181222887](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220401181222887.png)

![image-20220401180812542](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220401180812542.png)

### YAML版

![image-20220401181126868](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220401181126868.png)

![](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220401181202195.png)

![image-20220401181852764](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220401181852764.png)

解决办法

![image-20220401182040515](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220401182040515.png)

![image-20220401182143218](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220401182143218.png)

![image-20220401182019430](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220401182019430.png)

### properties

![image-20220401182419623](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220401182419623.png)

![image-20220401182411973](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220401182411973.png)

### 功能拆分

![](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220401183338744.png)

![image-20220401183354788](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220401183354788.png)

### 开发控制

![image-20220401184153822](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220401184153822.png)

![image-20220401183924891](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220401183924891.png)

![image-20220401183936050](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220401183936050.png)

## 日志

![image-20220401185146265](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220401185146265.png)

### 基础

![image-20220401184637418](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220401184637418.png)

![image-20220401185123406](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220401185123406.png)

![image-20220401185157282](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220401185157282.png)

![image-20220401185416192](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220401185416192.png)

![image-20220401185842792](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220401185842792.png)

### 输出格式

![image-20220401190046748](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220401190046748.png)

![image-20220401190611452](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220401190611452.png)

### 日志文件

![image-20220401190946958](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220401190946958.png)

# 开发篇

## 热部署



![image-20220402133947389](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220402133947389.png)

![image-20220402133836109](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220402133836109.png)

### 自动热部署

算了 感觉没啥用 手动挺好

### 范围

![image-20220402135125748](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220402135125748.png)

![image-20220402135431969](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220402135431969.png)

## 配置高级

![image-20220402154933507](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220402154933507.png)

![image-20220402155204323](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220402155204323.png)

![image-20220402171237837](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220402171237837.png)

### 宽松绑定

![image-20220404135948831](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220404135948831.png)

![image-20220404140234728](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220404140234728.png)

### 单位

![image-20220404140852731](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220404140852731.png)

### 格式校验

![image-20220404141752090](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220404141752090.png)

![image-20220404141711113](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220404141711113.png)

![image-20220404141737920](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220404141737920.png)





## 测试

### 测试专用属性

![image-20220404150451932](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220404150451932.png)

![image-20220404150503424](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220404150503424.png)

### 测试专用配置

![image-20220404150948897](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220404150948897.png)

### web测试

![image-20220404151403117](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220404151403117.png)

![image-20220404152454864](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220404152454864.png)

#### 请求匹配

![image-20220404154243479](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220404154243479.png)

![image-20220416134003678](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220416134003678.png)

![image-20220416135000420](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220416135000420.png)

### 测试事务回滚

![image-20220416141110555](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220416141110555.png)

### 测试数据自动生成

![image-20220416141719986](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220416141719986.png)

# 数据层

## SQL

![image-20220416151659834](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220416151659834.png)

### 数据源

 ![image-20220416143344555](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220416143344555.png)

### 持久化方案

![image-20220416144527373](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220416144527373.png)

![image-20220416144458726](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220416144458726.png)

### 数据库

![image-20220416151346380](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220416151346380.png)

![image-20220416151411414](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220416151411414.png)

![image-20220416151447496](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220416151447496.png)

## NOSQL

### Redis

![image-20220416160842184](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220416160842184.png)

####   简介

![image-20220416153226863](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220416153226863.png)

#### 整合步骤

![image-20220416160717361](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220416160717361.png)

![image-20220416160728263](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220416160728263.png)

![image-20220416160743303](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220416160743303.png)

#### 测试

![image-20220416160826918](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220416160826918.png)

#### 客户端选择

![image-20220416162159594](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220416162159594.png)

![image-20220416162124209](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220416162124209.png)

![image-20220416162236861](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220416162236861.png)

#### 补充

![image-20220416161447606](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220416161447606.png)

![image-20220416161504606](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220416161504606.png)

也可以redisTemplate指定泛型为String



### MongoDB

![image-20220416162407665](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220416162407665.png)

![image-20220416162838023](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220416162838023.png)

![image-20220416165154711](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220416165154711.png)

#### 整合

![image-20220416170859711](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220416170859711.png)

![image-20220416170814126](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220416170814126.png)

![image-20220416170823317](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220416170823317.png)

![image-20220416170836907](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220416170836907.png)

### ES

![image-20220416210347950](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220416210347950.png)

#### 索引

![image-20220416222627483](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220416222627483.png)

其中books即为索引名字

**创建索引规则：**

------

在正常put索引时在body添加mapping条件

![image-20220416223335451](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220416223335451.png)

![image-20220416222650343](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220416222650343.png)

#### 文档

![image-20220416224848843](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220416224848843.png)

![image-20220416224856786](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220416224856786.png)

![image-20220416225657341](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220416225657341.png)

#### 整合

##### 低版本

![image-20220416232150138](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220416232150138.png)

##### 高版本

![image-20220416232217538](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220416232217538.png)

![image-20220416232232937](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220416232232937.png)

![image-20220416232347988](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220416232347988.png)

# 整合第三方

## 缓存

![](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220418211627166.png)

### springboot缓存

<img src="C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220418212430016.png" alt="image-20220418212430016"  />

![image-20220418212344347](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220418212344347.png)

![image-20220418212353368](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220418212353368.png)

![image-20220418212407277](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220418212407277.png)

![image-20220418212605406](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220418212605406.png)

### 缓存商变更

![image-20220422222636052](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220422222636052.png)

![image-20220422222649028](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220422222649028.png)

![image-20220422222704991](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220422222704991.png)

# Thymeleaf

代替jsp，不用tomcat服务器就可以访问，将HTML赋予动态效果	

![image-20220726140156671](SpringBoot.assets/image-20220726140156671.png)

![image-20220726140233400](SpringBoot.assets/image-20220726140233400.png)

![image-20220726140357790](SpringBoot.assets/image-20220726140357790.png)

![image-20220728150903760](SpringBoot.assets/image-20220728150903760.png)

![image-20220728151024344](SpringBoot.assets/image-20220728151024344.png)

​	

# @EnableAutoConfiguration

![image-20220903203359451](SpringBoot.assets/image-20220903203359451.png)

![image-20220903204335472](SpringBoot.assets/image-20220903204335472.png)

![image-20220903204523602](SpringBoot.assets/image-20220903204523602.png)

![image-20220903204532047](SpringBoot.assets/image-20220903204532047.png)

# 自定义配置类yml文件代码提示

自定义的类和配置文件绑定一般没有提示。添加后使得有提示

```xml
    <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-configuration-processor</artifactId>
           <optional>true</optional>
       </dependency>
 
 
<build>
       <plugins>
           <plugin>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-maven-plugin</artifactId>
               <configuration>
                   <excludes>
                       <exclude>
                           <groupId>org.springframework.boot</groupId>
                           <artifactId>spring-boot-configuration-processor</artifactId>
                       </exclude>
                   </excludes>
               </configuration>
           </plugin>
       </plugins>
   </build>
```

# 静态资源访问

 ![image-20220904154356114](SpringBoot.assets/image-20220904154356114.png)

```yml
  spring:
    web:
     resources:
      static-locations: 
```

 改变静态资源的位置默认为

```java
  {
"classpath:/METAINF/resources/",
"classpath:/resources/","classpath:/static/", "classpath:/public/" 
  }
```

![image-20220904154629401](SpringBoot.assets/image-20220904154629401.png)

## 欢迎页

![image-20220904191737204](SpringBoot.assets/image-20220904191737204.png)

![image-20220904195621944](SpringBoot.assets/image-20220904195621944.png)

## 静态资源配置原理

![image-20220906152047821](SpringBoot.assets/image-20220906152047821.png)

# 表单提rest风格请求

原生表单只能提交get和post请求，put和delete请求会被转为post

springboot提供了一个拦截器来实现多请求兼容

![image-20220906161349637](SpringBoot.assets/image-20220906161349637.png)

```java
@Bean
@Conditional0nMissingBean(HiddenHttpMethodFilter.class)
ConditionalOnProperty(prefix = "spring.mvc.hiddenmethod.filter ", name = "enabled", matchIfMissing = false)
    public OrderedHiddenHttpMethodFilter hiddenHttpMethodFilter() {
return new OrderedHiddenHttpMethodFilter();
```

![image-20220906161042583](SpringBoot.assets/image-20220906161042583.png)

![image-20220906161152112](SpringBoot.assets/image-20220906161152112.png)

![image-20220906161232743](SpringBoot.assets/image-20220906161232743.png)

#  异常处理

![image-20220907205656448](D:\笔记\SpringBoot.assets\image-20220907205656448.png)
