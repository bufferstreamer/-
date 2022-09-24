[TOC]

# jvm整体结构

![image-20220821163928179](D:\笔记\jvm.assets/image-20220821163928179.png)

# jvm生命周期

![image-20220821165736371](D:\笔记\jvm.assets/image-20220821165736371.png)

![image-20220821170307849](D:\笔记\jvm.assets/image-20220821170307849.png)

#  jvm发展

![image-20220822162836724](D:\笔记\jvm.assets/image-20220822162836724.png)

![image-20220822162731174](D:\笔记\jvm.assets/image-20220822162731174.png)

![image-20220822162924929](D:\笔记\jvm.assets/image-20220822162924929.png)

![image-20220822163441605](D:\笔记\jvm.assets/image-20220822163441605.png)

![image-20220822163927938](D:\笔记\jvm.assets/image-20220822163927938.png)

![image-20220822164821700](D:\笔记\jvm.assets/image-20220822164821700.png)

![image-20220822165002414](D:\笔记\jvm.assets/image-20220822165002414.png)

# 类加载子系统

![image-20220824155616761](D:\笔记\jvm.assets/image-20220824155616761.png)

![image-20220824160102322](D:\笔记\jvm.assets/image-20220824160102322.png)

##  加载

![image-20220824161531430](D:\笔记\jvm.assets/image-20220824161531430.png)

## 链接

![image-20220824161917463](D:\笔记\jvm.assets/image-20220824161917463.png)

## 初始化

![image-20220824162919982](D:\笔记\jvm.assets/image-20220824162919982.png)

## 类加载器分类

![image-20220824165225183](D:\笔记\jvm.assets/image-20220824165225183.png)

![image-20220824165555537](D:\笔记\jvm.assets/image-20220824165555537.png)

### 引导类加载器

![image-20220824170532745](D:\笔记\jvm.assets/image-20220824170532745.png)

### 扩展类加载器

![image-20220824170936382](D:\笔记\jvm.assets/image-20220824170936382.png)

### 系统类加载器

![image-20220824171043699](D:\笔记\jvm.assets/image-20220824171043699.png)

### 用户自定义类加载器

![image-20220824171756735](D:\笔记\jvm.assets/image-20220824171756735.png)

![image-20220824181343905](D:\笔记\jvm.assets/image-20220824181343905.png)

![image-20220824181849600](D:\笔记\jvm.assets/image-20220824181849600.png)

## 双亲委派机制

![image-20220824182331762](D:\笔记\jvm.assets/image-20220824182331762.png)

![image-20220824183414809](D:\笔记\jvm.assets/image-20220824183414809.png)

## 沙箱安全机制

![image-20220824184825917](D:\笔记\jvm.assets/image-20220824184825917.png)

## 补充

![image-20220824185009009](D:\笔记\jvm.assets/image-20220824185009009.png)

![image-20220824185124282](D:\笔记\jvm.assets/image-20220824185124282.png)

![image-20220824185234622](D:\笔记\jvm.assets/image-20220824185234622.png)

# 运行时数据区

![image-20220825134935180](D:\笔记\jvm.assets/image-20220825134935180.png)

![image-20220825135709498](D:\笔记\jvm.assets/image-20220825135709498.png)

## 线程

![image-20220825140256641](D:\笔记\jvm.assets/image-20220825140256641.png)

![image-20220825140730098](D:\笔记\jvm.assets/image-20220825140730098.png)

## pc register

![image-20220825141227130](D:\笔记\jvm.assets/image-20220825141227130.png)

![image-20220825141422150](D:\笔记\jvm.assets/image-20220825141422150.png)

![image-20220825141442737](D:\笔记\jvm.assets/image-20220825141442737.png)

![image-20220825141720847](D:\笔记\jvm.assets/image-20220825141720847.png)

![image-20220825144826220](D:\笔记\jvm.assets/image-20220825144826220.png)

![image-20220825150845565](D:\笔记\jvm.assets/image-20220825150845565.png)

## 虚拟机栈

![image-20220825154717846](D:\笔记\jvm.assets/image-20220825154717846.png)

![image-20220825160001732](D:\笔记\jvm.assets/image-20220825160001732.png)

![image-20220825163018390](D:\笔记\jvm.assets/image-20220825163018390.png)

![image-20220825163540100](D:\笔记\jvm.assets/image-20220825163540100.png)

![image-20220825164614283](D:\笔记\jvm.assets/image-20220825164614283.png)

### 栈存储单位

![image-20220825170222635](D:\笔记\jvm.assets/image-20220825170222635.png)

### 栈运行原理

![image-20220826143626001](D:\笔记\jvm.assets/image-20220826143626001.png)

### 栈帧内部结构

![image-20220826143929795](D:\笔记\jvm.assets/image-20220826143929795.png)

#### 局部变量表

![image-20220826144443404](D:\笔记\jvm.assets/image-20220826144443404.png)

#### slot

![image-20220827154017137](D:\笔记\jvm.assets/image-20220827154017137.png)

![image-20220827154234617](D:\笔记\jvm.assets/image-20220827154234617.png)

![image-20220827155333645](D:\笔记\jvm.assets/image-20220827155333645.png)

![image-20220827160045737](D:\笔记\jvm.assets/image-20220827160045737.png)

#### 操作数栈

![image-20220827160506482](D:\笔记\jvm.assets/image-20220827160506482.png)

![image-20220827160943600](D:\笔记\jvm.assets/image-20220827160943600.png)

![image-20220827161358101](D:\笔记\jvm.assets/image-20220827161358101.png)

#### 栈顶缓存技术

![image-20220827163553518](D:\笔记\jvm.assets/image-20220827163553518.png)

#### 动态链接

![image-20220827173801590](D:\笔记\jvm.assets/image-20220827173801590.png)

![image-20220827175416123](D:\笔记\jvm.assets/image-20220827175416123.png)

#### 方法调用

![image-20220827175622479](D:\笔记\jvm.assets/image-20220827175622479.png)

![image-20220827175756680](D:\笔记\jvm.assets/image-20220827175756680.png)

![image-20220827180823968](D:\笔记\jvm.assets/image-20220827180823968.png)

![image-20220827181623905](D:\笔记\jvm.assets/image-20220827181623905.png)

![image-20220827183201140](D:\笔记\jvm.assets/image-20220827183201140.png)

![image-20220827183353144](D:\笔记\jvm.assets/image-20220827183353144.png)

#### 方法重写

![image-20220827184120292](D:\笔记\jvm.assets/image-20220827184120292.png)

![image-20220828135121148](D:\笔记\jvm.assets/image-20220828135121148.png)

#### 方法返回地址

![image-20220828135320173](D:\笔记\jvm.assets/image-20220828135320173.png)

![image-20220828135803192](D:\笔记\jvm.assets/image-20220828135803192.png)

![image-20220828140205562](D:\笔记\jvm.assets/image-20220828140205562.png)

## 本地方法接口

![image-20220829182016142](D:\笔记\jvm.assets/image-20220829182016142.png)

![image-20220829182403284](D:\笔记\jvm.assets/image-20220829182403284.png)

### 为何使用

![image-20220829182453213](D:\笔记\jvm.assets/image-20220829182453213.png)

![image-20220829182933873](D:\笔记\jvm.assets/image-20220829182933873.png)

![image-20220829183914408](D:\笔记\jvm.assets/image-20220829183914408.png)

## 本地方法栈

![image-20220829184105307](D:\笔记\jvm.assets/image-20220829184105307.png)

![image-20220829184417505](D:\笔记\jvm.assets/image-20220829184417505.png)

## 堆

![image-20220831135715357](D:\笔记\jvm.assets/image-20220831135715357.png)

![image-20220831140153835](D:\笔记\jvm.assets/image-20220831140153835.png)

![image-20220831140709693](D:\笔记\jvm.assets/image-20220831140709693.png)

![image-20220831141624439](D:\笔记\jvm.assets/image-20220831141624439.png)

![image-20220831142246330](D:\笔记\jvm.assets/image-20220831142246330.png)

### 设置

![image-20220831142824385](D:\笔记\jvm.assets/image-20220831142824385.png)

![image-20220831151531945](D:\笔记\jvm.assets/image-20220831151531945.png)

![image-20220831151718468](D:\笔记\jvm.assets/image-20220831151718468.png)

### 对象分配过程

![image-20220831152013373](D:\笔记\jvm.assets/image-20220831152013373.png)

![image-20220831153528456](D:\笔记\jvm.assets/image-20220831153528456.png)

![image-20220831153928371](D:\笔记\jvm.assets/image-20220831153928371.png)

### GC简介

![image-20220831175252638](D:\笔记\jvm.assets/image-20220831175252638.png)

![image-20220831180123604](D:\笔记\jvm.assets/image-20220831180123604.png)

![image-20220831180648381](D:\笔记\jvm.assets/image-20220831180648381.png)

![image-20220831180842876](D:\笔记\jvm.assets/image-20220831180842876.png)

### 分代思想

![image-20220831182722346](D:\笔记\jvm.assets/image-20220831182722346.png)

### 总结

![image-20220831193413875](D:\笔记\jvm.assets/image-20220831193413875.png)

![image-20220831193429550](D:\笔记\jvm.assets/image-20220831193429550.png)

### TLAB

![image-20220902192228097](D:\笔记\jvm.assets/image-20220902192228097.png)

![image-20220902192311947](D:\笔记\jvm.assets/image-20220902192311947.png)

![image-20220902192444743](D:\笔记\jvm.assets/image-20220902192444743.png)

![image-20220902192735907](D:\笔记\jvm.assets/image-20220902192735907.png)

### 参数小结

![image-20220902193356473](D:\笔记\jvm.assets/image-20220902193356473.png)

### 逃逸分析

![image-20220902195114023](D:\笔记\jvm.assets/image-20220902195114023.png)

![image-20220902195738974](D:\笔记\jvm.assets/image-20220902195738974.png)

![](D:\笔记\jvm.assets/image-20220903141631946.png)

#### 栈上分配

![image-20220903141042843](D:\笔记\jvm.assets/image-20220903141042843.png)

#### 同步省略

![image-20220903141746761](D:\笔记\jvm.assets/image-20220903141746761.png)

![image-20220903141951275](D:\笔记\jvm.assets/image-20220903141951275.png)

#### 分离对象及标量替换    

![image-20220903142441454](D:\笔记\jvm.assets/image-20220903142441454.png)

![image-20220903142513863](D:\笔记\jvm.assets/image-20220903142513863.png)

![image-20220903142541670](D:\笔记\jvm.assets/image-20220903142541670.png)

![image-20220903142950048](D:\笔记\jvm.assets/image-20220903142950048.png)

#### 小结

![image-20220903143011196](D:\笔记\jvm.assets/image-20220903143011196.png)

### 堆总结

![image-20220903143442083](D:\笔记\jvm.assets/image-20220903143442083.png)

## 方法区/元空间

### 交互

![image-20220903144359431](D:\笔记\jvm.assets/image-20220903144359431.png)

 	

![image-20220903151307836](D:\笔记\jvm.assets/image-20220903151307836.png)

![image-20220903152810753](D:\笔记\jvm.assets/image-20220903152810753.png)

​																																																																																																																																																																																																																																																																																																																												

### HOTSPOT 发展

![image-20220903152721661](D:\笔记\jvm.assets/image-20220903152721661.png)

### 设置方法区大小与OOM

![image-20220903153011392](D:\笔记\jvm.assets/image-20220903153011392.png)

![image-20220903153430978](D:\笔记\jvm.assets/image-20220903153430978.png)

![image-20220903155010235](D:\笔记\jvm.assets/image-20220903155010235.png)

### 内部结构

![image-20220903155245957](D:\笔记\jvm.assets/image-20220903155245957.png)

![image-20220903155413486](D:\笔记\jvm.assets/image-20220903155413486.png)

![image-20220903155707632](D:\笔记\jvm.assets/image-20220903155707632.png)

![image-20220903155806515](D:\笔记\jvm.assets/image-20220903155806515.png)

![image-20220903155903063](D:\笔记\jvm.assets/image-20220903155903063.png)

####  常量池

![image-20220903162119952](D:\笔记\jvm.assets/image-20220903162119952.png)

![image-20220903162653286](D:\笔记\jvm.assets/image-20220903162653286.png)

![image-20220903162812334](D:\笔记\jvm.assets/image-20220903162812334.png)

#### 运行时常量池

![image-20220903163931522](D:\笔记\jvm.assets/image-20220903163931522.png)

### 发展

![image-20220906211009123](D:\笔记\jvm.assets/image-20220906211009123.png)

![image-20220906210931872](D:\笔记\jvm.assets/image-20220906210931872.png)

![image-20220906210941572](D:\笔记\jvm.assets/image-20220906210941572.png)

### 原因

![image-20220906212228121](D:\笔记\jvm.assets/image-20220906212228121.png)

![image-20220906213402988](D:\笔记\jvm.assets/image-20220906213402988.png)

### 垃圾收集

![image-20220906214928148](D:\笔记\jvm.assets/image-20220906214928148.png)

![image-20220906215211284](D:\笔记\jvm.assets/image-20220906215211284.png)

![image-20220906215536166](D:\笔记\jvm.assets/image-20220906215536166.png)

## 总结

![image-20220906215812916](D:\笔记\jvm.assets/image-20220906215812916.png)

# 对象

## 实例化

![image-20220907214814433](D:\笔记\jvm.assets\image-20220907214814433.png)

![image-20220907220022394](D:\笔记\jvm.assets\image-20220907220022394.png)

![image-8](D:\笔记\jvm.assets\image-20220907220233388.png)

![image-20220907220439764](D:\笔记\jvm.assets\image-20220907220439764.png)

![image-20220907220519156](D:\笔记\jvm.assets\image-20220907220519156.png)

![image-20220907220727219](D:\笔记\jvm.assets\image-20220907220727219.png)

![image-20220907220810133](D:\笔记\jvm.assets\image-20220907220810133.png)

![image-20220907221323255](D:\笔记\jvm.assets\image-20220907221323255.png)

![image-20220907221342851](D:\笔记\jvm.assets\image-20220907221342851.png)

## 内存布局

![image-20220907222309874](D:\笔记\jvm.assets\image-20220907222309874.png)

## 对象访问定位

### 句柄访问

![image-20220907223543341](D:\笔记\jvm.assets\image-20220907223543341.png)

![image-20220907223847439](D:\笔记\jvm.assets\image-20220907223847439.png)



### 直接指针（Hotspot采用、效率较高）

![image-20220907223942349](D:\笔记\jvm.assets\image-20220907223942349.png)

## 小结

![image-20220907223020181](D:\笔记\jvm.assets\image-20220907223020181.png)

# 直接内存

![image-20220911122247827](D:\笔记\jvm.assets\image-20220911122247827.png)

![image-20220911122945262](D:\笔记\jvm.assets\image-20220911122945262.png)

![image-20220911123613593](D:\笔记\jvm.assets\image-20220911123613593.png)

![image-20220911131602060](D:\笔记\jvm.assets\image-20220911131602060.png)

![image-20220911132208323](D:\笔记\jvm.assets\image-20220911132208323.png)

# 执行引擎

## 概述

![image-20220911132616625](D:\笔记\jvm.assets\image-20220911132616625.png)

![image-20220911132725089](D:\笔记\jvm.assets\image-20220911132725089.png)

![image-20220911133025359](D:\笔记\jvm.assets\image-20220911133025359.png)

![image-20220911142727650](D:\笔记\jvm.assets\image-20220911142727650.png)

## 工作过程

![image-20220911133710924](D:\笔记\jvm.assets\image-20220911133710924.png)

![image-20220911134106940](D:\笔记\jvm.assets\image-20220911134106940.png)

## 编译和执行

![image-20220911134253675](D:\笔记\jvm.assets\image-20220911134253675.png)

![image-20220911134546894](D:\笔记\jvm.assets\image-20220911134546894.png)

![image-20220911134722903](D:\笔记\jvm.assets\image-20220911134722903.png)

![image-20220911134819874](D:\笔记\jvm.assets\image-20220911134819874.png)

![image-20220911135210261](D:\笔记\jvm.assets\image-20220911135210261.png)

## 字节码

![image-20220911141404417](D:\笔记\jvm.assets\image-20220911141404417.png)

## 解释器

![image-20220911141615754](D:\笔记\jvm.assets\image-20220911141615754.png)

![image-20220911141952340](D:\笔记\jvm.assets\image-20220911141952340.png)

![image-20220911142307044](D:\笔记\jvm.assets\image-20220911142307044.png)

## JIT

 ![image-20220911144445560](D:\笔记\jvm.assets\image-20220911144445560.png)

![image-20220911144925882](D:\笔记\jvm.assets\image-20220911144925882.png)

![image-20220911145315362](D:\笔记\jvm.assets\image-20220911145315362.png)

### 热点探测

![image-20220911150149877](D:\笔记\jvm.assets\image-20220911150149877.png)

![image-20220911150511946](D:\笔记\jvm.assets\image-20220911150511946.png)

### 方法调用计数器

![image-20220911150600123](D:\笔记\jvm.assets\image-20220911150600123.png)

![image-20220911150915095](D:\笔记\jvm.assets\image-20220911150915095.png)

### 热度衰减

![image-20220911151057342](D:\笔记\jvm.assets\image-20220911151057342.png)

### 回边计数器

![image-20220911151440041](D:\笔记\jvm.assets\image-20220911151440041.png)

![image-20220911151504609](D:\笔记\jvm.assets\image-20220911151504609.png)

### 编译模式选择

![image-20220911152517260](D:\笔记\jvm.assets\image-20220911152517260.png)

### 编译器模式指定

![image-20220911153225851](D:\笔记\jvm.assets\image-20220911153225851.png)

### 优化策略

![image-20220911153759909](D:\笔记\jvm.assets\image-20220911153759909.png)

### 分层编译

![image-20220911153833829](D:\笔记\jvm.assets\image-20220911153833829.png)

### 总结

![image-20220911154022382](D:\笔记\jvm.assets\image-20220911154022382.png)

## 总结

![image-20220911154103907](D:\笔记\jvm.assets\image-20220911154103907.png)

![image-20220911154742103](D:\笔记\jvm.assets\image-20220911154742103.png)

![image-20220911154829464](D:\笔记\jvm.assets\image-20220911154829464.png)

# stringtable

## 基本特性

![image-20220912194719776](D:\笔记\jvm.assets\image-20220912194719776.png)

![image-20220912195849531](D:\笔记\jvm.assets\image-20220912195849531.png)

![image-20220912200002158](D:\笔记\jvm.assets\image-20220912200002158.png)

## 内存分配

![image-20220913141958409](D:\笔记\jvm.assets\image-20220913141958409.png)

## 字符串拼接

![image-20220913144312949](D:\笔记\jvm.assets\image-20220913144312949-16630513953201.png)

## intern

![image-20220914132555972](D:\笔记\jvm.assets\image-20220914132555972.png)

![image-20220914141141610](D:\笔记\jvm.assets\image-20220914141141610.png)

==JDK7以后的intern才会保证堆空间中对于某一个字符串对象唯一**（本质上来说是因为字符串常量池自从JDK7从永久代被移入了堆空间，为了节省空间，intern在堆空间中就只保存一份对象）**，也就是说当堆空间中有该字符串时，在字符串常量池中存放对象地址的引用而不是复制一份对象。但是new string（）来创建字符串对象时不会在字符串常量池中存放引用，而是会在字符串常量池和堆空间中都创建一个对象==

![ ](D:\笔记\jvm.assets\image-20220914141551816.png)

![image-20220914141906471](D:\笔记\jvm.assets\image-20220914141906471.png)

![image-20220914144302714](D:\笔记\jvm.assets\image-20220914144302714.png)

## 去重

![image-20220914145542475](D:\笔记\jvm.assets\image-20220914145542475.png)

![image-20220914145718693](D:\笔记\jvm.assets\image-20220914145718693.png)

![image-20220914145742029](D:\笔记\jvm.assets\image-20220914145742029.png)

# 垃圾回收

![image-20220915152637137](D:\笔记\jvm.assets\image-20220915152637137.png)

![image-20220915153507737](D:\笔记\jvm.assets\image-20220915153507737.png)

## 垃圾标记

![image-20220915155738478](D:\笔记\jvm.assets\image-20220915155738478.png)

### 引用计数算法

![image-20220915160527063](D:\笔记\jvm.assets\image-20220915160527063.png)

![image-20220915160822937](D:\笔记\jvm.assets\image-20220915160822937.png)

![image-20220915161743238](D:\笔记\jvm.assets\image-20220915161743238.png)

### 可达性分析（根搜索算法，追踪性垃圾收集）

![image-20220915162051336](D:\笔记\jvm.assets\image-20220915162051336.png)

![image-20220915162540426](D:\笔记\jvm.assets\image-20220915162540426.png)

![image-20220915162230788](D:\笔记\jvm.assets\image-20220915162230788.png)

![image-20220915163337102](D:\笔记\jvm.assets\image-20220915163337102.png)

### GC Roots

![image-20220915162712145](D:\笔记\jvm.assets\image-20220915162712145.png)

![image-20220915162947897](D:\笔记\jvm.assets\image-20220915162947897.png)

## finalization

![image-20220915173714543](D:\笔记\jvm.assets\image-20220915173714543.png)

![image-20220915174153260](D:\笔记\jvm.assets\image-20220915174153260.png)

![image-20220915174641229](D:\笔记\jvm.assets\image-20220915174641229.png)

![image-20220915175144253](D:\笔记\jvm.assets\image-20220915175144253.png)

## MAT

![image-20220915181213654](D:\笔记\jvm.assets\image-20220915181213654.png)

## 垃圾清除

![image-20220915183544604](D:\笔记\jvm.assets\image-20220915183544604.png)

### 标记清除算法

![image-20220915183703123](D:\笔记\jvm.assets\image-20220915183703123.png)

![image-20220915184233457](D:\笔记\jvm.assets\image-20220915184233457.png)

![image-20220915184342944](D:\笔记\jvm.assets\image-20220915184342944.png)

### 复制算法

![image-20220916220324633](D:\笔记\jvm.assets\image-20220916220324633.png)

![image-20220916220515468](D:\笔记\jvm.assets\image-20220916220515468.png)

  ![image-20220916220845466](D:\笔记\jvm.assets\image-20220916220845466.png)

![image-20220916221616806](D:\笔记\jvm.assets\image-20220916221616806.png)

### 标记压缩算法

![image-20220916221658218](D:\笔记\jvm.assets\image-20220916221658218.png)

![image-20220916221926995](D:\笔记\jvm.assets\image-20220916221926995.png)

![image-20220916221950349](D:\笔记\jvm.assets\image-20220916221950349.png)

![image-20220916222319470](D:\笔记\jvm.assets\image-20220916222319470.png)

![image-20220916222615906](D:\笔记\jvm.assets\image-20220916222615906.png)

### 对比

![image-20220916223228708](D:\笔记\jvm.assets\image-20220916223228708.png)

### 分代收集算法

![image-20220916223720141](D:\笔记\jvm.assets\image-20220916223720141.png)

![image-20220917180711225](D:\笔记\jvm.assets\image-20220917180711225.png)

![image-20220917181010480](D:\笔记\jvm.assets\image-20220917181010480.png)

### 增量收集算法

![image-20220917181408345](D:\笔记\jvm.assets\image-20220917181408345.png)

![image-20220917181618601](D:\笔记\jvm.assets\image-20220917181618601.png)

### 分区算法

![image-20220917182014700](D:\笔记\jvm.assets\image-20220917182014700.png)

## System.gc()

![image-20220918133811248](D:\笔记\jvm.assets\image-20220918133811248.png)

## 内存溢出

![image-20220918135344512](D:\笔记\jvm.assets\image-20220918135344512.png)

![image-20220918140305397](D:\笔记\jvm.assets\image-20220918140305397.png)

![image-20220918140440208](D:\笔记\jvm.assets\image-20220918140440208.png)

## 内存泄漏

![image-20220918140618255](D:\笔记\jvm.assets\image-20220918140618255.png)

![image-20220918141650951](D:\笔记\jvm.assets\image-20220918141650951.png)

## STW

![image-20220918142332368](D:\笔记\jvm.assets\image-20220918142332368.png)

![image-20220918142612133](D:\笔记\jvm.assets\image-20220918142612133.png)

## 并发

![image-20220918143347406](D:\笔记\jvm.assets\image-20220918143347406.png)

## 并行

![image-20220918143512638](D:\笔记\jvm.assets\image-20220918143512638.png)

## 对比

![image-20220918143607859](D:\笔记\jvm.assets\image-20220918143607859.png)

![image-20220918143911140](D:\笔记\jvm.assets\image-20220918143911140.png)

![image-20220918144039164](D:\笔记\jvm.assets\image-20220918144039164.png)

## 安全点

![image-20220918144145980](D:\笔记\jvm.assets\image-20220918144145980.png)

![image-20220918144612102](D:\笔记\jvm.assets\image-20220918144612102.png)

## 安全区域

![image-20220918144710240](D:\笔记\jvm.assets\image-20220918144710240.png)

![image-20220918144938491](D:\笔记\jvm.assets\image-20220918144938491.png)

## 引用

![image-20220918145538410](D:\笔记\jvm.assets\image-20220918145538410.png)

![image-20220918150019116](D:\笔记\jvm.assets\image-20220918150019116.png)

### 强引用

![image-20220918150100024](D:\笔记\jvm.assets\image-20220918150100024.png)

### 软引用

![image-20220918150756412](D:\笔记\jvm.assets\image-20220918150756412.png)

![image-20220918151232924](D:\笔记\jvm.assets\image-20220918151232924.png)

### 弱引用

![image-20220918151933451](D:\笔记\jvm.assets\image-20220918151933451.png)

### 虚引用

![image-20220918152639829](D:\笔记\jvm.assets\image-20220918152639829.png)

![image-20220918153014025](D:\笔记\jvm.assets\image-20220918153014025.png)

### 终结器引用

![image-20220918154102151](D:\笔记\jvm.assets\image-20220918154102151.png)

# 垃圾回收器

![image-20220919143922662](D:\笔记\jvm.assets\image-20220919143922662.png)

![image-20220919145355525](D:\笔记\jvm.assets\image-20220919145355525.png)

![image-20220919150432604](D:\笔记\jvm.assets\image-20220919150432604.png)

![image-20220919151122458](D:\笔记\jvm.assets\image-20220919151122458.png)

## 吞吐量

![image-20220919151345335](D:\笔记\jvm.assets\image-20220919151345335.png)

## 暂停时间

![image-20220919151059561](D:\笔记\jvm.assets\image-20220919151059561.png)

![image-20220919151503760](D:\笔记\jvm.assets\image-20220919151503760.png)

![image-20220919154244144](D:\笔记\jvm.assets\image-20220919154244144.png)

![image-20220919154544800](D:\笔记\jvm.assets\image-20220919154544800.png)

## 垃圾分代与垃圾回收器

![image-20220919155028849](D:\笔记\jvm.assets\image-20220919155028849.png)

![image-20220919155049723](D:\笔记\jvm.assets\image-20220919155049723.png)

## 查看默认垃圾回收器

![image-20220919160234188](D:\笔记\jvm.assets\image-20220919160234188.png)

## serial

![image-20220919164754564](D:\笔记\jvm.assets\image-20220919164754564.png)

![image-20220919165140532](D:\笔记\jvm.assets\image-20220919165140532.png)

![image-20220919165338645](D:\笔记\jvm.assets\image-20220919165338645.png)

![image-20220919174428252](D:\笔记\jvm.assets\image-20220919174428252.png)

## parnew

![image-20220919174643142](D:\笔记\jvm.assets\image-20220919174643142.png)

![image-20220919174900167](D:\笔记\jvm.assets\image-20220919174900167.png)

## parallel

![image-20220919175707828](D:\笔记\jvm.assets\image-20220919175707828.png)

![image-20220919175949806](D:\笔记\jvm.assets\image-20220919175949806.png)

![image-20220919180550498](D:\笔记\jvm.assets\image-20220919180550498.png)

![image-20220919181302403](D:\笔记\jvm.assets\image-20220919181302403.png)

![image-20220919181800013](D:\笔记\jvm.assets\image-20220919181800013.png)

## cms

![image-20220920131727262](D:\笔记\jvm.assets\image-20220920131727262.png)

![image-20220920132207549](D:\笔记\jvm.assets\image-20220920132207549.png)

![image-20220920132335707](D:\笔记\jvm.assets\image-20220920132335707.png)

![image-20220920132605607](D:\笔记\jvm.assets\image-20220920132605607.png)

![image-20220920132813533](D:\笔记\jvm.assets\image-20220920132813533.png)

![image-20220920133505691](D:\笔记\jvm.assets\image-20220920133505691.png)

![image-20220920133756476](D:\笔记\jvm.assets\image-20220920133756476.png)

<font size=5>**[关于浮动垃圾的解释](https://www.cnblogs.com/henuliulei/p/16390937.html)**</font>

### 可以设置的参数

![image-20220920134625327](D:\笔记\jvm.assets\image-20220920134625327.png)

![image-20220920135447507](D:\笔记\jvm.assets\image-20220920135447507.png)

### 小结

![image-20220920135511458](D:\笔记\jvm.assets\image-20220920135511458.png)

![image-20220920135740063](D:\笔记\jvm.assets\image-20220920135740063.png)

## G1

![image-20220920135921125](D:\笔记\jvm.assets\image-20220920135921125.png)

![image-20220920140650936](D:\笔记\jvm.assets\image-20220920140650936.png)

![image-20220920141322920](D:\笔记\jvm.assets\image-20220920141322920.png)

![image-20220920141407962](D:\笔记\jvm.assets\image-20220920141407962.png)

![image-20220920141907594](D:\笔记\jvm.assets\image-20220920141907594.png)

![image-20220920142240392](D:\笔记\jvm.assets\image-20220920142240392.png)

![image-20220920142258808](D:\笔记\jvm.assets\image-20220920142258808.png)

![image-20220920142933711](D:\笔记\jvm.assets\image-20220920142933711.png)

### 参数设置

![image-20220920143406467](D:\笔记\jvm.assets\image-20220920143406467.png)

### region

![image-20220920172051574](D:\笔记\jvm.assets\image-20220920172051574.png)

![image-20220920172358186](D:\笔记\jvm.assets\image-20220920172358186.png)

![image-20220920172737344](D:\笔记\jvm.assets\image-20220920172737344.png)

### 回收过程

![image-20220920173217764](D:\笔记\jvm.assets\image-20220920173217764.png)

![image-20220920173615504](D:\笔记\jvm.assets\image-20220920173615504.png)

![image-20220920174039237](D:\笔记\jvm.assets\image-20220920174039237.png)

### remembered set

![image-20220920174839854](D:\笔记\jvm.assets\image-20220920174839854.png)

![image-20220920174906676](D:\笔记\jvm.assets\image-20220920174906676.png)

### 年轻代GC

![image-20220920182756575](D:\笔记\jvm.assets\image-20220920182756575.png)

![image-20220920183425153](D:\笔记\jvm.assets\image-20220920183425153.png)

![image-20220920183611217](D:\笔记\jvm.assets\image-20220920183611217.png)

### 并发标记

![image-20220920184240135](D:\笔记\jvm.assets\image-20220920184240135.png)

### 混合回收

![image-20220920184724602](D:\笔记\jvm.assets\image-20220920184724602.png)

![image-20220920185141415](D:\笔记\jvm.assets\image-20220920185141415.png)

### 总结

![image-20220920190012930](D:\笔记\jvm.assets\image-20220920190012930.png)

# GC日志分析

## 参数设置

![image-20220921132619106](D:\笔记\jvm.assets\image-20220921132619106.png)

![image-20220921131726767](D:\笔记\jvm.assets\image-20220921131726767.png)

​                                      ![image-20220921132321895](D:\笔记\jvm.assets\image-20220921132321895.png)

## 补充说明

![image-20220921132842144](D:\笔记\jvm.assets\image-20220921132842144.png)

![image-20220921133005957](D:\笔记\jvm.assets\image-20220921133005957.png)

## 日志解释

![image-20220921133110648](D:\笔记\jvm.assets\image-20220921133110648.png)

![image-20220921133255715](D:\笔记\jvm.assets\image-20220921133255715.png)

## 日志工具

![image-20220921134849444](D:\笔记\jvm.assets\image-20220921134849444.png)

# 新GC

![image-20220921140008014](D:\笔记\jvm.assets\image-20220921140008014.png)

![image-20220921140839080](D:\笔记\jvm.assets\image-20220921140839080.png)

# class文件

## 字节码看运行细节

![](D:\笔记\jvm.assets\image-20220922165038354.png)

成员变量赋值过程：①默认初始化（即零值）②调用init方法后显示初始化③构造器初始化④在对象中的赋值

上图在new son对象时会先初始化父类——father 先默认初始化父类的x为0然后调用父类的构造器执行this.print()，但是栈帧中this指向的是子类对象，所以调用的实际是子类的print方法，然后打印的是子类中的x，此时子类还没有调用init初始化，所以x只有默认初始化的值——0.打印完毕后将父类的x先赋值10后赋值20，回到子类的init方法，此时打印x值为子类显示初始化的值——30，然后将子类的x赋值40，最后回到main方法中打印f.x，因为多态只针对方法，对于变量不会改变，所以f.x是父类中的x值，是最后的20

## 字节码

![image-20220922165956043](D:\笔记\jvm.assets\image-20220922165956043.png)

## 概述

###  Class类的本质

任何一个Class文件都对应着唯一一个类或接口的定义信息，但反过来说，Class文件实际上它并不一定以磁盘文件的形式存在。Class文件是一组以8位字节为基础单位的二进制流。

### . Class文件格式

Class的结构不像XML等描述语言，由于它没有任何分隔符号。所以在其中的数据项，无论是字节顺序还是数量，都是被严格限定的，哪个字节代表什么含义，长度是多少，先后顺序如何，都不允许改变。

![image-20220922172242829](D:\笔记\jvm.assets\image-20220922172242829.png)

## Class文件结构

 ![image-20220922173355263](D:\笔记\jvm.assets\image-20220922173355263.png)

![image-20220922173417933](D:\笔记\jvm.assets\image-20220922173417933.png)

### 魔数

![image-20220923103756007](D:\笔记\jvm.assets\image-20220923103756007.png)

### Class文件版本号

![image-20220923104423156](D:\笔记\jvm.assets\image-20220923104423156.png)

![image-20220923105158004](D:\笔记\jvm.assets\image-20220923105158004.png)

### 常量池

![image-20220923105511808](D:\笔记\jvm.assets\image-20220923105511808.png)

![image-20220923105749095](D:\笔记\jvm.assets\image-20220923105749095.png)

#### 常量池计数器

![image-20220923105827063](D:\笔记\jvm.assets\image-20220923105827063-16639019075241.png)

#### 常量池表

![image-20220923110300354](D:\笔记\jvm.assets\image-20220923110300354.png)

![image-20220923110546306](D:\笔记\jvm.assets\image-20220923110546306.png)

![image-20220923110950573](D:\笔记\jvm.assets\image-20220923110950573.png)![image-20220923111138681](D:\笔记\jvm.assets\image-20220923111138681.png)

![image-20220923111412892](D:\笔记\jvm.assets\image-20220923111412892.png)

#### 常量的类型和结构

  

![img](D:\笔记\jvm.assets\watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3NzAxNjI4,size_16,color_FFFFFF,t_70.png)

![img](D:\笔记\jvm.assets\watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3NzAxNjI4,size_16,color_FFFFFF,t_70-16639140652255.png)

![image-20220923142218701](D:\笔记\jvm.assets\image-20220923142218701.png)

### 访问标识

![image-20220923142439107](D:\笔记\jvm.assets\image-20220923142439107.png)

![image-20220923143145349](D:\笔记\jvm.assets\image-20220923143145349.png)

### 索引

![image-20220923143320277](D:\笔记\jvm.assets\image-20220923143320277.png)

![image-20220923144033582](D:\笔记\jvm.assets\image-20220923144033582.png)

![image-20220923144046120](D:\笔记\jvm.assets\image-20220923144046120.png)

### 字段表集合

![image-20220923144302581](D:\笔记\jvm.assets\image-20220923144302581.png)

![image-20220923144937239](D:\笔记\jvm.assets\image-20220923144937239.png)

#### 字段访问标识

![image-20220923145219624](D:\笔记\jvm.assets\image-20220923145219624.png)

#### 字段名索引

根据字段名索引，查询常量池得到字段的名称即可。

#### 字段描述符索引

![image-20220923150515861](D:\笔记\jvm.assets\image-20220923150515861.png)

![image-20220923150528311](D:\笔记\jvm.assets\image-20220923150528311.png)

#### 字段属性表

![image-20220923150045611](D:\笔记\jvm.assets\image-20220923150045611.png)

### 方法表集合

![image-20220923150710961](D:\笔记\jvm.assets\image-20220923150710961.png)

![image-20220923151402319](D:\笔记\jvm.assets\image-20220923151402319.png)

#### 方法访问标识



#### 方法名索引



#### 方法描述符索引



#### 方法属性



### 属性表集合

![image-20220923152625376](D:\笔记\jvm.assets\image-20220923152625376.png)

![img](D:\笔记\jvm.assets\1838403-20211201193241072-1400228627.png)

![img](D:\笔记\jvm.assets\1838403-20211201193310218-1819092658.png)



#### 通用格式

![image-20220923153001483](D:\笔记\jvm.assets\image-20220923153001483.png)

#### 预定义属性Code

##### 简介

Java程序方法体里面的代码经过Javac编译器处理之后，最终变为字节码指令存储在Code属性内。Code属性出现在方法表的属性集合之中，但并非所有的方法表都必须存在这个属性，譬如接口或者抽象类中的方法就不存在Code属性

 

##### Code的结构

 ![img](https://img2020.cnblogs.com/blog/1838403/202112/1838403-20211201193736402-806911883.png)

**1）attribute_name_index**

　　是一项指向CONSTANT_Utf8_info型常量的索引，此常量值固定为“Code”，它代表了该属性的属性名称

**2）attribute_length**

　　指示了属性值的长度，由于属性名称索引(attribute_name_index)与属性长度(attribute_length)一共为6个字节，所以属性值的长度固定为整个属性表长度减去6个字节。

**3）max_stack：**

　　代表了操作数栈（Operand Stack）深度的最大值。在方法执行的任意时刻，操作数栈都不会超过这个深度。虚拟机运行的时候需要根据这个值来分配栈帧（Stack Frame）中的操作栈深度。

**4)max_locals**

　　代表了局部变量表所需的存储空间。在这里，max_locals的单位是变量槽（Slot），变量槽是虚拟机为局部变量分配内存所使用的最小单位。对于byte、char、float、int、short、boolean和returnAddress等长度不超过32位的数据类型，每个局部变量占用一个变量槽，而double和long这两种64位的数据类型则需要两个变量槽来存放。方法参数（包括实例方法中的隐藏参数“this”）、显式异常处理程序的参数（Exception Handler Parameter，就是try-catch语句中catch块中所定义的异常）、方法体中定义的局部变量都需要依赖局部变量表来存放。

　　注意，并不是在方法中用了多少个局部变量，就把这些局部变量所占变量槽数量之和作为max_locals的值，操作数栈和局部变量表直接决定一个该方法的栈帧所耗费的内存，不必要的操作数栈深度和变量槽数量会造成内存的浪费。Java虚拟机的做法是将局部变量表中的变量槽进行重用，当代码执行超出一个局部变量的作用域时，这个局部变量所占的变量槽可以被其他局部变量所使用，Javac编译器会根据变量的作用域来分配变量槽给各个变量使用，根据同时生存的最大局部变量数量和类型计算出max_locals的大小。

**5）code_length和code**

　　用来存储Java源程序编译后生成的字节码指令。code_length代表字节码长度，code是用于存储字节码指令的一系列字节流。既然叫字节码指令，那顾名思义每个指令就是一个u1类型的单字节，当虚拟机读取到code中的一个字节码时，就可以对应找出这个字节码代表的是什么指令，并且可以知道这条指令后面是否需要跟随参数，以及后续的参数应当如何解析。我们知道一个u1数据类型的取值范围为0x00～0xFF，对应十进制的0～255，也就是一共可以表达256条指令。目前，《Java虚拟机规范》已经定义了其中约200条编码值对应的指令含义，编码与指令之间的对应关系可查阅“虚拟机字节码指表”。

　　关于code_length，有一件值得注意的事情，虽然它是一个u4类型的长度值，理论上最大值可以达到2的32次幂，但是《Java虚拟机规范》中明确限制了一个方法不允许超过65535条字节码指令，即它实际只使用了u2的长度，如果超过这个限制，Javac编译器就会拒绝编译。一般来讲，编写Java代码时只要不是刻意去编写一个超级长的方法来为难编译器，是不太可能超过这个最大值的限制的。

 

**6)exception_table_length、exception_table**

　　显式异常处理表（下文简称“异常表”）集合。那它的格式应如下图所示，包含四个字段，这些字段的含义为：如果当字节码从第start_pc行到第end_pc行之间（不含第end_pc行）出现了类型为catch_type或者其子类的异常（catch_type为指向一个CONSTANT_Class_info型常量的索引），则转到第handler_pc行继续处理。当catch_type的值为0时，代表任意异常情况都需要转到handler_pc处进行处理

![img](https://img2020.cnblogs.com/blog/1838403/202112/1838403-20211203191149322-1782129338.png)

 ![img](https://img2020.cnblogs.com/blog/1838403/202112/1838403-20211203191404040-841107552.png)

##### Code的重要性

　　Code属性是Class文件中最重要的一个属性，如果把一个Java程序中的信息分为代码（Code，方法体里面的Java代码）和元数据（Metadata，包括类、字段、方法定义及其他信息）两部分，那么在整个Class文件里，Code属性用于描述代码，所有的其他数据项目都用于描述元数据。了解Code属性是学习关于字节码执行引擎内容的必要基础，能直接阅读字节码也是工作中分析Java代码语义问题的必要工具和基本技能

 

#####  示例

**![img](https://img2020.cnblogs.com/blog/1838403/202112/1838403-20211201200552366-417842441.png)**

这里接着上一篇3.7.2的方法表

方法的属性表个数：00 01 1个属性表

后面的内容紧接着就是属性表集合---也就是  **方法表.属性表集合**

属性表的类型(attribute_name_index)：00 0D 指向第13个常量 Code，说明这个属性表是Code---**方法表.属性表集合.Code**，接下来就是Code的内容了

![img](https://img2020.cnblogs.com/blog/1838403/202112/1838403-20211201200821348-1959354852.png)

attribute_length：00 00 00 31 属性表长度为49

max_stack：00 02操作数栈的最大深度为2

max_locals:00 01 局部变量表所述的空间(单位是变量槽)：1

code_length：00 00 00 07 字节码长度为7

code：2A B4 00 0204 64 AC 存储字节码指令的字节流

exception_table_length：00 00

exception_table：没有

attribute_count：00 02 Code里面的属性表的个数

接下来紧跟的是Code里面的属性表集合---也就是方法表**.**属性表集合**.**Code**.**属性表集合

属性表的类型(attribute_name_index)：00 0E指向第14个常数 LineNumberTable 也就是**方法表.属性表集合.Code.属性表集合.LineNumberTable** 

![img](https://img2020.cnblogs.com/blog/1838403/202112/1838403-20211201201713601-399098721.png)

![img](https://img2020.cnblogs.com/blog/1838403/202112/1838403-20211203190409642-955193257.png)

 stack是2 locals是1 args_size(参数个数)是1

inc()方法明明没有常数，为什么args_size是1呢？

　　在Javac编译器编译的时候把对this关键字的访问转变为对一个普通方法参数的访问，然后在虚拟机调用实例方法时自动传入此参数而已。因此在实例方法的局部变量表中至少会存在一个指向当前对象实例的局部变量，局部变量表中也会预留出第一个变量槽位来存放对象实例的引用，所以实例方法参数值从1开始计算。这个处理只对实例方法有效，如果代码清单6-1中的inc()方法被声明为static，那Args_size就不会等于1而是等于0了

#### exceptions

##### 简介

　　这里的Exceptions属性是在方法表中与Code属性平级的一项属性，读者不要与前面刚刚讲解完的异常表产生混淆。Exceptions属性的作用是列举出方法中可能抛出的受查异常（Checked Excepitons），也就是方法描述时在throws关键字后面列举的异常。

 

##### 结构

![img](https://img2020.cnblogs.com/blog/1838403/202112/1838403-20211203191708838-940465510.png)

　　此属性中的number_of_exceptions项表示方法可能抛出number_of_exceptions种受查异常，每一种受查异常使用一个exception_index_table项表示；exception_index_table是一个指向常量池中CONSTANT_Class_info型常量的索引，代表了该受查异常的类型

 

####  LineNumberTable

##### 简介

　　LineNumberTable属性用于描述Java源码行号与字节码行号（字节码的偏移量）之间的对应关系。它并不是运行时必需的属性，但默认会生成到Class文件之中。如果选择不生成LineNumberTable属性，对程序运行产生的最主要影响就是当抛出异常时，堆栈中将不会显示出错的行号，并且在调试程序的时候，也无法按照源码行来设置断点。

#####  结构

***\**\*![img](https://img2020.cnblogs.com/blog/1838403/202112/1838403-20211203192108397-1342123422.png)\*\**\***

　　line_number_table是一个数量为line_number_table_length、类型为line_number_info的集合，line_number_info表包含start_pc和line_number两个u2类型的数据项，前者是字节码行号，后者是Java源码行号

#### LocalVariableTable及LocalVariableTypeTable属性

##### 简介

LocalVariableTable属性用于描述栈帧中局部变量表的变量与Java源码中定义的变量之间的关系，它也不是运行时必需的属性，但默认会生成到Class文件之中。如果没有生成这项属性，最大的影响就是当其他人引用这个方法时，所有的参数名称都将会丢失，会对代码编写带来较大不便，而且在调试期间无法根据参数名称从上下文中获得参数值

　　LocalVariableTypeTable与LocalVariableTable非常相似，仅仅是把记录的字段描述符的descriptor_index替换成了字段的特征签名（Signature）。对于非泛型类型来说，描述符和特征签名能描述的信息是能吻合一致的，但是泛型引入之后，由于描述符中泛型的参数化类型被擦除掉[3]，描述符就不能准确描述泛型类型了。因此出现了LocalVariableTypeTable属性，使用字段的特征签名来完成泛型的描述

 

##### 结构

***\**\*![img](https://img2020.cnblogs.com/blog/1838403/202112/1838403-20211203192919020-537168483.png)\*\**\***

其中local_variable_info项目代表了一个栈帧与源码中的局部变量的关联

![img](https://img2020.cnblogs.com/blog/1838403/202112/1838403-20211203192948496-210527760.png)

　　start_pc和length属性分别代表了这个局部变量的生命周期开始的字节码偏移量及其作用范围覆盖的长度，两者结合起来就是这个局部变量在字节码之中的作用域范围。

　　name_index和descriptor_index都是指向常量池中CONSTANT_Utf8_info型常量的索引，分别代表了局部变量的名称以及这个局部变量的描述符。

　　index是这个局部变量在栈帧的局部变量表中变量槽的位置。当这个变量数据类型是64位类型时（double和long），它占用的变量槽为index和index+1两个。

 

#### SourceFile及SourceDebugExtension属性

##### 简介

　　SourceFile属性用于记录生成这个Class文件的源码文件名称。在Java中，对于大多数的类来说，类名和文件名是一致的，但是有一些特殊情况（如内部类）例外。如果不生成这项属性，当抛出异常时，堆栈中将不会显示出错代码所属的文件名。这个属性是一个定长的属性。

 

##### 结构

***\**\*![img](https://img2020.cnblogs.com/blog/1838403/202112/1838403-20211203194225396-2134875130.png)\*\**\***

　　sourcefile_index数据项是指向常量池中CONSTANT_Utf8_info型常量的索引，常量值是源码文件的文件名

 

##### 简介

　　SourceDebugExtension属性用于存储额外的代码调试信息

 

##### 结构

　　***\**\*![img](https://img2020.cnblogs.com/blog/1838403/202112/1838403-20211203194342650-1477902136.png)\*\**\***

　　其中debug_extension存储的就是额外的调试信息，是一组通过变长UTF-8格式来表示的字符串。一个类中最多只允许存在一个SourceDebugExtension属性

 

#### ConstantValue

#####  简介

　　ConstantValue属性的作用是通知虚拟机自动为静态变量赋值。只有被static关键字修饰的变量（类变量）才可以使用这项属性。

　　对非static类型的变量（也就是实例变量）的赋值是在实例构造器<init>()方法中进行的；

　　而对于类变量，则有两种方式可以选择：在类构造器<clinit>()方法中或者使用ConstantValue属性。

　　目前Oracle公司实现的Javac编译器的选择是，如果同时使用final和static来修饰一个变量（按照习惯，这里称“常量”更贴切），并且这个变量的数据类型是基本类型或者java.lang.String的话，就将会生成ConstantValue属性来进行初始化；如果这个变量没有被final修饰，或者并非基本类型及字符串，则将会选择在<clinit>()方法中进行初始化。

　　ConstantValue的属性值只能限于基本类型和String这点，因为此属性的属性值只是一个常量池的索引号，由于**Class文件格式的常量类型中只有与基本属性和字符串相对应的字面量**，所以就ConstantValue属性想支持别的类型也无能为力

 

##### 结构

##### ![img](https://img2020.cnblogs.com/blog/1838403/202112/1838403-20211203194652827-614228513.png)

从数据结构中可以看出ConstantValue属性是一个定长属性，它的attribute_length数据项值必须固定为2

####  InnerClasses

##### 简介

　　InnerClasses属性用于记录内部类与宿主类之间的关联。如果一个类中定义了内部类，那编译器将会为它以及它所包含的内部类生成InnerClasses属性

 

##### 结构

![img](https://img2020.cnblogs.com/blog/1838403/202112/1838403-20211203194844285-952710583.png)

　　数据项number_of_classes代表需要记录多少个内部类信息，每一个内部类的信息都由一个inner_classes_info表进行描述

![img](https://img2020.cnblogs.com/blog/1838403/202112/1838403-20211203194918302-845806802.png)

　　inner_class_info_index和outer_class_info_index都是指向常量池中CONSTANT_Class_info型常量的索引，分别代表了内部类和宿主类的符号引用。

　　inner_name_index是指向常量池中CONSTANT_Utf8_info型常量的索引，代表这个内部类的名称，

　　如果是匿名内部类，这项值为0。

　　inner_class_access_flags是内部类的访问标志，类似于类的access_flagsv，它的取值范围如下图

![img](https://img2020.cnblogs.com/blog/1838403/202112/1838403-20211203195006048-1627205859.png)

#### Deprecated及Synthetic属性

##### 简介

　　Deprecated和Synthetic两个属性都属于标志类型的布尔属性，只存在有和没有的区别，没有属性值的概念。

　　Deprecated属性用于表示某个类、字段或者方法，已经被程序作者定为不再推荐使用，它可以通过代码中使用“@deprecated”注解进行设置。

　　Synthetic属性代表此字段或者方法并不是由Java源码直接产生的，而是由编译器自行添加的，在JDK 5之后，标识一个类、字段或者方法是编译器自动产生的，也可以设置它们访问标志中的ACC_SYNTHETIC标志位。编译器通过生成一些在源代码中不存在的Synthetic方法、字段甚至是整个类的方式，实现了越权访问（越过private修饰器）或其他绕开了语言限制的功能，这可以算是一种早期优化的技巧，其中最典型的例子就是枚举类中自动生成的枚举元素数组和嵌套类的桥接方法（Bridge Method）。所有由不属于用户代码产生的类、方法及字段都应当至少设置Synthetic属性或者ACC_SYNTHETIC标志位中的一项，唯一的例外是实例构造器“<init>()”方法和类构造器“<clinit>()”方法。

 

##### 结构

![img](https://img2020.cnblogs.com/blog/1838403/202112/1838403-20211204180715533-505196128.png)

####  StackMapTable属性

##### 简介

　　StackMapTable属性在JDK 6增加到Class文件规范之中，它是一个相当复杂的变长属性，位于Code属性的属性表中。这个属性会在虚拟机类加载的字节码验证阶段被新类型检查验证器（TypeChecker）使用，目的在于代替以前比较消耗性能的基于数据流分析的类型推导验证器。新的验证器在同样能保证Class文件合法性的前提下，省略了在运行期通过数据流分析去确认字节码的行为逻辑合法性的步骤，而在编译阶段将一系列的验证类型（Verification Type）直接记录在Class文件之中，通过检查这些验证类型代替了类型推导过程，从而大幅提升了字节码验证的性能。这个验证器在JDK 6中首次提供，并在JDK 7中强制代替原本基于类型推断的字节码验证器。

　　StackMapTable属性中包含零至多个栈映射帧（Stack Map Frame），每个栈映射帧都显式或隐式地代表了一个字节码偏移量，用于表示执行到该字节码时局部变量表和操作数栈的验证类型。类型检查验证器会通过检查目标方法的局部变量和操作数栈所需要的类型来确定一段字节码指令是否符合逻辑约束

#####  结构

![img](https://img2020.cnblogs.com/blog/1838403/202112/1838403-20211204181104369-865331672.png)

　　在Java SE 7版之后的《Java虚拟机规范》中，明确规定对于版本号大于或等于50.0的Class文件，如果方法的Code属性中没有附带StackMapTable属性，那就意味着它带有一个隐式的StackMap属性，这个StackMap属性的作用等同于number_of_entries值为0的StackMapTable属性。一个方法的Code属性最多只能有一个StackMapTable属性，否则将抛出ClassFormatError异常

 

#### Signature属性

##### 简介

　　Signature属性在JDK 5增加到Class文件规范之中，它是一个可选的定长属性，可以出现于类、字段表和方法表结构的属性表中。在JDK 5里面大幅增强了Java语言的语法，在此之后，任何类、接口、初始化方法或成员的泛型签名如果包含了类型变量（Type Variable）或参数化类型（ParameterizedType），则Signature属性会为它记录泛型签名信息。之所以要专门使用这样一个属性去记录泛型类型，是因为Java语言的泛型采用的是擦除法实现的伪泛型，字节码（Code属性）中所有的泛型信息编译（类型变量、参数化类型）在编译之后都通通被擦除掉。运行期就无法像C#等有真泛型支持的语言那样，将泛型类型与用户定义的普通类型同等对待，例如运行期做反射时无法获得泛型信息。Signature属性就是为了弥补这个缺陷而增设的，现在Java的反射API能够获取的泛型类型，最终的数据来源也是这个属性。关于Java泛型、Signature属性和类型擦除

 

##### 结构 

![img](https://img2020.cnblogs.com/blog/1838403/202112/1838403-20211204181449513-956664966.png)

　　其中signature_index项的值必须是一个对常量池的有效索引。常量池在该索引处的项必须是CONSTANT_Utf8_info结构，表示类签名或方法类型签名或字段类型签名。如果当前的Signature属性是类文件的属性，则这个结构表示类签名，如果当前的Signature属性是方法表的属性，则这个结构表示方法类型签名，如果当前Signature属性是字段表的属性，则这个结构表示字段类型签名

 

#### BootstrapMethods属性

#####  简介

　　BootstrapMethods属性在JDK 7时增加到Class文件规范之中，它是一个复杂的变长属性，位于类文件的属性表中。这个属性用于保存invokedynamic指令引用的引导方法限定符

　　根据《Java虚拟机规范》（从Java SE 7版起）的规定，如果某个类文件结构的常量池中曾经出现过CONSTANT_InvokeDynamic_info类型的常量，那么这个类文件的属性表中必须存在一个明确的BootstrapMethods属性，另外，即使CONSTANT_InvokeDynamic_info类型的常量在常量池中出现过多次，类文件的属性表中最多也只能有一个BootstrapMethods属性。

　　虽然JDK 7中已经提供了InovkeDynamic指令，但这个版本的Javac编译器还暂时无法支持InvokeDynamic指令和生成BootstrapMethods属性，必须通过一些非常规的手段才能使用它们。直到JDK 8中Lambda表达式和接口默认方法的出现，InvokeDynamic指令才算在Java语言生成的Class文件中有了用武之地

 

##### 结构

![img](https://img2020.cnblogs.com/blog/1838403/202112/1838403-20211204181805599-1293511870.png)

![img](https://img2020.cnblogs.com/blog/1838403/202112/1838403-20211204181813580-1722472808.png)

![img](https://img2020.cnblogs.com/blog/1838403/202112/1838403-20211204181821193-1439395358.png)

　　BootstrapMethods属性里，num_bootstrap_methods项的值给出了bootstrap_methods[]数组中的引导方法限定符的数量。而bootstrap_methods[]数组的每个成员包含了一个指向常量池

CONSTANT_MethodHandle结构的索引值，它代表了一个引导方法。还包含了这个引导方法静态参数的序列（可能为空）。bootstrap_methods[]数组的每个成员必须包含以下三项内容：

　　bootstrap_method_ref：bootstrap_method_ref项的值必须是一个对常量池的有效索引。常量池在该索引处的值必须是一个CONSTANT_MethodHandle_info结构。

　　num_bootstrap_arguments：num_bootstrap_arguments项的值给出了bootstrap_argu-ments[]数组成员的数量。

　　bootstrap_arguments[]：bootstrap_arguments[]数组的每个成员必须是一个对常量池的有效索引。

　　常量池在该索引出必须是下列结构之一：

　　CONSTANT_String_info、CONSTANT_Class_info、CONSTANT_Integer_info、CONSTANT_Long_info、CONSTANT_Float_info、CONSTANT_Double_info、CONSTANT_MethodHandle_info或CONSTANT_MethodType_info

 

####  MethodParameters

##### 简介

　　MethodParameters是在JDK 8时新加入到Class文件格式中的，它是一个用在方法表中的变长属性。MethodParameters的作用是记录方法的各个形参名称和信息。

　　最初，基于存储空间的考虑，Class文件默认是不储存方法参数名称的，因为给参数起什么名字对计算机执行程序来说是没有任何区别的，所以只要在源码中妥当命名就可以了。随着Java的流行，这点确实为程序的传播和二次复用带来了诸多不便，由于Class文件中没有参数的名称，如果只有单独的程序包而不附加上JavaDoc的话，在IDE中编辑使用包里面的方法时是无法获得方法调用的智能提示的，这就阻碍了JAR包的传播。后来，“-g：var”就成为了Javac以及许多IDE编译Class时采用的默认值，这样会将方法参数的名称生成到LocalVariableTable属性之中。不过此时问题仍然没有全部解决，LocalVariableTable属性是Code属性的子属性——没有方法体存在，自然就不会有局部变量表，但是对于其他情况，譬如抽象方法和接口方法，是理所当然地可以不存在方法体的，对于方法签名来说，还是没有找到一个统一完整的保留方法参数名称的地方。所以JDK 8中新增的这个属性，使得编译器可以（编译时加上-parameters参数）将方法名称也写进Class文件中，而且MethodParameters是方法表的属性，与Code属性平级的，可以运行时通过反射API获取

 

##### 结构

![img](https://img2020.cnblogs.com/blog/1838403/202112/1838403-20211204182308314-1873868988.png)

![img](https://img2020.cnblogs.com/blog/1838403/202112/1838403-20211204182314899-694740111.png)

　　其中，name_index是一个指向常量池CONSTANT_Utf8_info常量的索引值，代表了该参数的名称。而access_flags是参数的状态指示器，它可以包含以下三种状态中的一种或多种：

　　0x0010（ACC_FINAL）：表示该参数被final修饰。

　　0x1000（ACC_SYNTHETIC）：表示该参数并未出现在源文件中，是编译器自动生成的。

　　0x8000（ACC_MANDATED）：表示该参数是在源文件中隐式定义的。Java语言中的典型场景是this关键字

 

#### 运行时注解相关属性

##### 简介

　　早在JDK 5时期，Java语言的语法进行了多项增强，其中之一是提供了对注解（Annotation）的支持。为了存储源码中注解信息，Class文件同步增加了RuntimeVisibleAnnotations、RuntimeInvisibleAnnotations、RuntimeVisibleParameterAnnotations和RuntimeInvisibleParameter-Annotations四个属性。到了JDK 8时期，进一步加强了Java语言的注解使用范围，又新增类型注解（JSR 308），所以Class文件中也同步增加了RuntimeVisibleTypeAnnotations和RuntimeInvisibleTypeAnnotations两个属性。由于这六个属性不论结构还是功能都比较雷同，因此我们把它们合并到一起，以RuntimeVisibleAnnotations为代表进行介绍。

　　RuntimeVisibleAnnotations是一个变长属性，它记录了类、字段或方法的声明上记录运行时可见注解，当我们使用反射API来获取类、字段或方法上的注解时，返回值就是通过这个属性来取到的

 

##### 结构

![img](https://img2020.cnblogs.com/blog/1838403/202112/1838403-20211204182512063-515347064.png)

　　num_annotations是annotations数组的计数器，annotations中每个元素都代表了一个运行时可见的注解，注解在Class文件中以annotation结构来存储

![img](https://img2020.cnblogs.com/blog/1838403/202112/1838403-20211204182541227-1217479946.png)

　　type_index是一个指向常量池CONSTANT_Utf8_info常量的索引值，该常量应以字段描述符的形式表示一个注解。

　　num_element_value_pairs是element_value_pairs数组的计数器，element_value_pairs中每个元素都是一个键值对，代表该注解的参数和值

## javac 

![image-20220924170549123](D:\笔记\jvm.assets\image-20220924170549123.png)

## javap

![image-20220924171655521](D:\笔记\jvm.assets\image-20220924171655521.png)

![image-20220924175701763](D:\笔记\jvm.assets\image-20220924175701763.png)

[java字节码指令官方文档](https://docs.oracle.com/javase/specs/jvms/se7/html/jvms-6.html)



