# 并发编程 

## 进程与线程

![image-20220510222932824](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220510222932824.png)

![image-20220510223250056](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220510223250056.png)

## 并行与并发

![image-20220510223644025](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220510223644025.png)

![image-20220510223828343](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220510223828343.png)

![image-20220510223852680](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220510223852680.png)

## 应用

![image-20220510224413100](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220510224413100.png)

![image-20220510224700328](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220510224700328.png)

# java线程

## 创建线程

### Thread

![image-20220510225715458](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220510225715458.png)

### Runnable

![image-20220510225807252](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220510225807252.png)

![image-20220510230310708](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220510230310708.png)

### FutureTask

 

![image-20220510230805127](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220510230805127.png)

## 原理

![image-20220510231809849](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220510231809849.png)

![image-20220510233956448](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220510233956448.png)

![image-20220510234256666](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220510234256666.png)

## 常见方法

![image-20220510234708233](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220510234708233.png)

![image-20220510234857336](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220510234857336.png)

### start run

![image-20220510235510880](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220510235510880.png)

### sleep yield

![image-20220510235556461](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220510235556461.png)

![image-20220513122839095](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220513122839095.png)

### join

![image-20220513124007143](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220513124007143.png)

### interrupt

![image-20220513124044994](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220513124044994.png)

![image-20221008162854884](D:\笔记\JUC.assets\image-20221008162854884.png)

### 两阶段终止模式

![image-20220513130153281](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220513130153281.png)

![image-20220513130628462](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220513130628462.png)

## 守护线程

 

![image-20220513131420146](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220513131420146.png)

## 线程状态

![image-20220513131715670](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220513131715670.png)

![image-20220513131703038](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220513131703038.png)

![image-20220513131803251](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220513131803251.png)

![image-20221008164457154](D:\笔记\JUC.assets\image-20221008164457154.png)

# 线程安全

![image-20220513161617819](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220513161617819.png)

## synchronize

![image-20220513161720653](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220513161720653.png)

![image-20220513161820526](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220513161820526.png)

![image-20220513162718499](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220513162718499.png)

## 变量分析

![image-20220513164918927](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220513164918927.png)

![image-20220513165043535](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220513165043535.png)

## 线程安全类

![image-20220513170405476](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220513170405476.png)

# monitor

![image-20220514132645354](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220514132645354.png)

![image-20220514133113301](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220514133113301.png)

## 轻量级锁

![image-20220514171229444](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220514171229444.png)

![image-20220514171507066](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220514171507066.png)

![image-20220514171522218](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220514171522218.png)

![image-20220514171624630](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220514171624630.png)

![image-20220514171833425](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220514171833425.png)

![image-20220514172009402](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220514172009402.png)

![image-20220514172103970](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220514172103970.png)

## 自旋优化

![image-20220514173235357](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220514173235357.png)

![image-20220514173425648](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220514173425648.png)

![image-20220514173451223](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220514173451223.png)

## 偏向锁

![image-20220514173830962](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220514173830962.png)

![image-20220514173944344](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220514173944344.png)

### 偏向状态

![image-20220514174024797](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220514174024797.png)

![image-20220514175409278](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220514175409278.png)

![image-20220514175900358](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220514175900358.png)

![image-20220514175912142](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220514175912142.png)

### 批量重偏向

![image-20220514180010599](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220514180010599.png)

![image-20220514180036043](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220514180036043.png)

### 批量撤销

![image-20220514180655376](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220514180655376.png)

![image-20220514180748347](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220514180748347.png)

![image-20220514180814779](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220514180814779.png)

### 锁消除

JVM中存在JIT即时优化器，运行前会优化代码，如运行之前检查加锁是否生效，如果不生效就会优化掉锁直接执行代码（加锁后效率可能降低十倍），可以手动控制优化机制是否生效

![image-20220514182157771](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220514182157771.png)

# wait notify

![image-20220514191603551](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220514191603551.png)

![image-20220514191825044](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220514191825044.png)

![image-20220514193739691](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220514193739691.png)

# 保护性暂停

![image-20220517205602295](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220517205602295.png)

![image-20220517210914569](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220517210914569.png)

![image-20220517212346543](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220517212346543.png)

# join原理

![image-20220517212200445](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220517212200445.png)
