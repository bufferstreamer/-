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
