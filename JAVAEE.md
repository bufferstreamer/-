#  final

修饰类 表示该类不能被继承

  修饰方法 表示该方法不能被重写

  修饰变量 表示该变量不能被改变 其中 修饰局部变量表示该值不能改变 修饰引用变量表示该地址值不能改变

# static  （所修饰的成员属于类本身，否则属于对象）

    被类中所有的对象所共享，推荐使用类名调用

    非静态成员方法可以访问所有变量和方法

    静态成员方法只能访问静态变量和方法 因为main函数是静态方法 所以新定义的方法都用静态方法方便main访问

static 方法不能被重写

# 权限修饰符

## private 

只能被本类中的方法所访问

## public  

可以被任意访问

## protected 

可以被同一个包中的子类或者无关类以及不同包中的子类（不同包中的无关类不可以访问）所访问

## default  

可以被同一个包中的子类或者无关类访问 （不同包中的子类和无关类都不可以访问）

# super 

可以理解为指向父类的一个指针   

super.成员变量 访问父类成员变量  

super.（）访问父类成员方法 

super（） 访问父类构造方法 必须在构造器第一行

# this  

可以理解为指向本类的一个指针  

this.成员变量 访问本类成员变量  

this.（）访问本类成员方法 

this（） 访问本类构造方法 必须在构造器第一行 所以super和this不能同时出现

# 变量

全局变量（成员变量） 与方法同级

局部变量 方法内部的和方法的参数的变量

静态变量（类变量）类的所有实例所公用的变量

实例变量 实例的变量

# 重写与继承

重写  在子类中用父类的方法名 方法体可以重新定义
     子类不能重写父类的静态方法和私有方法

继承  子类拥有父类中的全部属性和方法 但是子类不能继承父类的构造方法 会默认调用父类的无参构造器 想要调用父类的带参构造方法，必须在子类用super调用而且必须写在第一句
      子类也不能继承父类的静态方法和私有方法

# 代码块

静态代码块static {...}   静态代码块在类加载时执行, 并且只执行一次.静态代码块可以有多个

实例代码块{...}  main方法的执行不会触发实例代码块在 new对象的时候才触发实例代码块执行, 且在对象产生之前就执行了     每new一个对象都会触发执行一次

# 多态

多态 前提：1、有继承或者实现的关系

    				 2、有方法的重写

    				 3、有父类引用指向子类对象 例： 动物 小白 = new 猫（）;

特点：对于成员变量 编译看左边  运行看左边

       即编译格式需要与父类相同，假如父类中没有age 则小白.age无法通过编译 运行结果也与父类相同 假如动物.age= 10 猫.age= 5 则运行小白.age= 10

     对于成员方法 编译看左边  运行看右边

       即编译格式需要与父类相同，假如父类中没有getage（） 则小白.getage（）无法通过编译 运行结果与子类相同 假如动物.getage（）= 10 猫.getage（）= 5 则运行小白.getage（）= 5

优点：提高代码重用性，增加可扩展性。 定义方法的时候使用父类类型作为方法的参数，实现方法时使用子类的类型来实现。

缺点：不能实现子类中独有的方法，只能实现父类和子类共有的方法。

理解：重写使一个方法可以重用，而多态使多个方法可以重用。如，猫和狗重写动物吃的方法，当动物类实例化后，猫和狗可以调用吃做不同的动作。
     但是如果猫和狗重写了吃喝拉撒很多方法，动物实例化后想要一个一个调用比较麻烦，可以将吃喝拉撒抽象出一个生活类，在里面定义生活方法包含吃喝拉撒。
     方法的参数类型用动物，以后不管是猫还是狗，或者new猪羊鱼鸡 都可以 动物 a = new 猪（）; 将a作为参数传递给生活类调用生活方法

解决多态不能实现子类独有方法的方法： 向下转换（强制类型转换） 如 Animal a = new Cat(); 此时a只可以访问Animal中的方法 Cat b = (Cat) a; 此时b可以访问Cat中的所有方法
                              其中(Cat) a 并不是将a转换为Cat类型 他只是将a所指向的 new Cat()的地址赋值给b 使b也指向该地址，本质上相当于新建了一个指向原来地址的Cat类型的b
                              建议类型转换之前 用instance of 来进行类型判断，以便将父类向下转换为正确的类型。如：上式的a为cat类型 则不能将其 Dog c = （Dog）a；

# 抽象类与接口

## 抽象类 

只有方法名没有方法体叫做抽象方法 有抽象方法的类一定为抽象类 抽象类不一定有抽象方法 抽象类可以有非抽象方法 使用abstract关键字修饰

    抽象类不可以直接实例化，需要有一个子类重写抽象类中所有的抽象方法，然后使用多态的形式来实例化。所以抽象类的子类必须重写抽象方法或者他也是一个抽象类，否则会报错
   		 抽象类可以有构造方法   父类抽象方法的作用就是限定子类中必须重写某个方法

## 枚举类

枚举类 修饰符 enum 枚举名称{
                    实例名称1，实例名称2...;
           }
           特点: 枚举类默认final，无法被继承
                 枚举类第一行是对象名称，默认是final
                 枚举类的构造器是私有的，但有多个对象，所以相当于多例设计模式

![image-20220503182951787](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220503182951787.png)

## 接口 interface

      类不能继承接口 只能实现 implements 接口 接口不能被实例化 只能通过多态的方式来实现实例化 接口的实现类要么重写接口中的所有方法 要么是一个抽象类

              接口中的属性默认被final修饰，无法改变 默认被static修饰，可以通过接口名调用  即 public static final int i = 0; 等价于 int i = 0;

              接口没有构造方法 接口不能有非抽象方法 所以接口的方法默认为抽象和公有权限 可以不加 public abstract

              接口可以多实现，还可以在继承一个类的同时，多实现多个接口。类只能单继承。接口和接口是继承的关系，并且接口和接口之间可以多继承。

类作为形参和返回值时 需要的是类的对象
抽象类作为形参和返回值时 需要的是抽象类的子类的对象
接口名作为形参和返回值时 需要的是接口的实现类的对象

![image-20220517215224859](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220517215224859.png)

![image-20220517215319399](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220517215319399.png)

## 内部类

在一个类内部定义一个类   public class 类名{
                                修饰符 public 类名{

                                           			 }
                                       												 }

内部类可以直接访问外部类的成员 包括私有 外部类访问内部类必须创建对象

内部类分为 成员内部类和局部内部类 成员内部类为类内部定义类 局部内部类为类的方法内部定义一个类

一般来说成员内部类的目的是不被外部所访问，所以成员内部类修饰符一般为 private 想要访问成员内部类需要在外部类定义一个方法实例化成员内部类然后在方法内部访问，这个方法可以被其他类所访问
当成员内部类的修饰符为 public 时，成员内部类可以被其他的类直接访问 访问的方法为
        外部类.内部类 实例化名称 = new 外部类对象.new 内部类对象 如 Outter.Inner a = new Outter().new Inner();

局部内部类不能被 public 和 private 等修饰 只能被 final 和 abstract 修饰

匿名内部类是局部内部类的一种，也是在方法内部定义的类 格式 new 类名或者接口名 {
                                            重写方法
                                             }.方法名;
匿名内部类本质是 new 了一个对象  注意有一个分号。

# 常用类





## Object  

是所有类的根类 public boolean Object.equals(Object o) 判断两个对象地址是否相等。但是我们更需要判断内容是否相等，所以一般需要重写该方法。
                    public String Object.ToString()    返回该对象的字符串地址，但是我们更需要返回该对象内容的字符串，所以也要重写。

## Objects 

是Object的子类  public static boolean equals(Object o1 , Object o2) 重写了Object的方法，同时还加入了空指针判定，更加安全。
                       当o1或者o2为null时，不会抛出异常。而且是静态方法，可以通过类名直接调用

## Date    

时间类       构造方法：public Date()
                            public Date(long time) 将无参构造得到的时间毫秒转换为当前日期对象
                    public long getTime();返回1970至今的毫秒数

## DateFormat

抽象类     子类为simpleDateFormat 可以把日期对象格式化成喜欢的形式
                      先用带参构造指定格式化形式 simpleDateFormat(yyyy MM dd HH:mm:ss EEEE a)
                      然后使用 public string format(Date date)来返回格式化后的时间的字符串形式
                              public Date parse(String date)将字符串形式的日期解析为Date类型

## Calendar    

抽象类     通常用 public static Calendar getInstance()来获取对象(类似于单例模式)
                            如: Calendar calendar = Calendar.getInstance();
                            sout(calendar)返回当前的日历信息
                            sout(calendar.get(Clendar.YEAR))返回当前的年信息
                            sout(calendar.set(Calendar.YEAR,2099))返回2099的年信息
                            public void add(int filed, int number)为某一个字段添加或者减少一个值，如加五年等

## Math        抽象类      全部都是静态方法不需要使用构造器

                        public static int abs (int a)取绝对值
                        public static double ceil (double a)向上取整
                        public static double floor(double a)向下取整
                        public static double round(double a)四舍五入取整
                        public static double pow(double a, double b)a的b次幂

## System                 

					 public static void exit(int status) 终止虚拟机 0代表正常终止 非0异常终止

                        public static long currentTimeMillis() 获取当前毫秒值

## BigDecimal  

大数据类      public static BigDecimal valueOf(double val) double转换为BigDecimal
                        public  double doubleValue() BigDecimal转化为double

## Arrays  

数组         Arrays.toString(array)  将数组转换为String
                    Arrays.sort(array)  数组排序 返回值是数组

## StringBuilder

  创建可修改的String          StringBuilder.append(.append.append)添加元素(可链式添加)
                                        StringBuilder.reverse 反转
                                        StringBuilder.toString 转换为St

正则表达式       用来校验

# 泛型

            <>           E,T,K,V在定义泛型的时候代表任何类型
                泛型类         class 类名称 <泛型标识：可以随便写任意标识号，标识指定的泛型的类型>
                泛型接口        Interface 接口名称 <泛型标识：可以随便写任意标识号，标识指定的泛型的类型>
                泛型方法        1）public 与 返回值中间<T>非常重要，可以理解为声明此方法为泛型方法。
                               2）只有声明了<T>的方法才是泛型方法，泛型类中的使用了泛型的成员方法并不是泛型方法。
                               3）<T>表明该方法将使用泛型类型T，此时才可以在方法中使用泛型类型T。
                               4）与泛型类的定义一样，此处T可以随便写为任意标识，常见的如T、E、K、V等形式的参数常用于表示泛型。
                               public <T> T genericMethod(Class<T> tClass)throws InstantiationException ,
                              ？ extends car在使用泛型的时候代表car和其子类
                              ？ super car  在使用泛型的时候代表car和其父类

# Collection

         集合    Collection.add
                             Collection.remove
                             Collection.isEmpty
                             Collection.clear
                             Collection.contains
                             Collection.size

## Iterator<E>  iterator            

迭代器     iterator.next()        使用iterator遍历时如果collection.add会抛出并						发修改异常 因		为实际修改次数不等于预期修改次数
                                           iterator.hasNext()

## List        

与set不同List有序允许重复
            是Collection的实现类
            ArrayList是他的实现类
            collection的方法都可以使用

##     ListIterator       

 List的接口         ListIterator遍历的时候不会抛出并发修改异常 它在底层修改了预期修改次数使其等于实际修改次数

##     ArrayList  

 List的数组实现 查询快 增删慢
    	ArrayList<String> 集合            ArrayList.size() 集合长度
                                                              ArrayList.get(index)
                                                              ArrayList.set(index,element)
                                                              ArrayList.remove(index or object)
                                                              ArrayList.add(E e)

##     LinkedList 

 List的链表实现 查询慢 增删快

## Set集合       

不包含重复元素 没有带索引的方法 所以不能用for循环遍历
              set认为地址不同就是不重复，如果要使set认为内容相同地址不同也是重复，需要重写hashcode和equals方法
              Object.hashCode可以返回对象的hash值

    HashSet集合   是Set的实现类 数据结构是哈希表 对迭代顺序不保证即不保证存储和取出的顺序一致 没有带索引的方法不能用for循环遍历 不包含重复元素

    LinkedHashSet集合     是set的实现类 linked保证顺序存储 hash保证不重复

    TreeSet集合       是set的实现类 元素可以按照自定义的规则进行排序 TreeSet() 无参构造方法使元素按照自然规则排序
                                                            TreeSet(Compare compare) 带参构造方法使元素按照指定方法排序
                                                        不能用for循环遍历
                                                        <>中的类需要实现TreeSet的接口 同时return 0 认为相等 不添加 1 升序存储 -1 降序存储
                                                        compare( o1 , 02 ) return o1 - o2 升序  return o2 - 01 降序存储
                                                        compareTo (object) return this.object - object 升序 object - this.object 降序

                                                        o1=5 o2=4 return 02-01<1 系统认为o1<o2 所以输出o1 o2 即5 4
                                                        o1=5 o2=4 return o1-o2>1 系统认为o1大 所以输出o2 o1 即 4 5
                            只需要记住1-2升序 2-1降序就行

## Map集合   

interface Map<k,v> 将键k映射到值v 不能包含重复的键 每个键可以映射多个值 创建对象使用多态的方式如：HashMap(无序),treemap(有序)
                                                            Map.put(k,v) 添加元素 键重复的时候就会覆盖原来的值
                                                            Map.remove(object key)返回值是value
                                                            Map.clear()
                                                            Map.containsKey(object key)判断是否存在该键
                                                            Map.containsValue(object value)判断是否存在该值
                                                            Map.isEmpty()
                                                            Map.size()
                                                            Map.get(key)返回value
                                                            Map.keySet()获取key
                                                            Map.values()获取值
                                    遍历map可以先获得键的集合 然后foreach键得到值
                                    也可以使用set<Map.Entry<String,Integer>> entrySets = maps.entrySet()将map集合转换为set集合
                                    然后foreach遍历set集合

Collections                         public static<T>  boolean addAll (Collection<? super T>c , T... elements)给集合批量添加元素
                                    public static void shuffle(List<?> list) 打乱集合中的顺序
                                    public static <T> void sort(List<T> list) 默认升序排列
                                    public static <T> void sort(List<T> list , Comparator<? super T> ) 自定义排序顺序

基本类型包装类 除了int 和 char 其余类型直接首字母大写 其中int为Integer char为Character

        将基本类型变为一个包装类目的是为了加入一些方法方便使用 也方便各种类型之间的相互转换

Integer     Integer.valueof(Element)； 将各种类型转换为Integer

        Integer.parseInt(String); 将String转换为int

        Integer 类中public int intValue(); 将Integer转换为int 注意不是static不能通过类名调用 必须实例化

        String.valueof(int); 将int转换为String

# 异常

异常处理：   try...catch...  

				 try{
                                可能出现异常的代码；
                                }catch(异常类名 变量名) {
                                            异常的处理代码；
                                }finally {
                                        finally的操作一定会被执行；
                                }


运行时异常   无需显示处理，在编译时才出现问题。如:数组索引越界异常 空指针异常 类型转换异常 数学操作异常：10/0

编译时异常   必须显示处理，否则程序发生错误，无法通过编译。

throws      throws 异常类名;  表示抛出异常由该方法调用者处理；表示执行某些语句可能会发生异常，但是不执行并不会发生异常。

throw       throw 异常对象名;  表示抛出异常由方法体内的语句处理；表示执行throw后面的语句一定会发生异常。   具体情况实例参照 package 异常.teacher

# 增强for      

 for(    元素数据类型 变量名： 数组或者集合 ){
                    该变量就是元素，可直接使用
                    }

# 泛型

          定义时采用泛型 使用时再传入具体的形参。 定义格式 <泛型> ：   这里的泛型可以看作形参 调用时在传入实参 并且传入的实参只能时引用数据类型
                                             泛型类定义： public class fun<T> { } T可以为任意标识符
                                             泛型方法定义： public <T> void show(T t){ }
                                             泛型接口：  public interface fun <T> { }


类型通配符<?>    List<?>表示元素类型未知的List 这种List仅仅表示各种泛型List的父类 不能将元素添加
                类型通配符上限<?extends类型> List<? extends number> 表示number类型及其子类
                类型通配符下限<?super类型> List<? super number> 表示number类型及其父类


可变参数        可变参数又叫参数个数可变 使方法的形参个数可变         定义： public void int fun(int...a) 其中a是一个数组 所以可以用加强for循环来遍历
                                                            但是有多个参数时需要将可变参数放在后面，如public void int fun(String b，  int...a)



Collections     他的方法都是抽象方法 可以通过类名调用             Collections.reverse反转
                                                           Collections.sort升序
                                                           Collections.shuffle 随机 斗地主洗牌可用

# File        

    是文件和目录名路径的抽象表示      构造方法File(String )                       File f1 = new File("E:\\download\\java.txt")
                                                   File(String parent,String child)     File f1 = new File("E:\\download","java.txt")
                                                   File(File parent,String child)        File f1 = new File("E:\\download")
                                                                                         File f2 = new File(f1,"java.txt")

                                         创建功能File.createNewFile() 创建一个新文件 文件不存在就创建并返回true 存在就返回false
                                                File.mkdir() 创建一个目录  不能创建多级目录
                                                File.mkdirs()创建该路径的目录包括任何必须但不存在的目录  可以创建多级目录
                                                File.delete()删除目录或者文件  目录中有文件不能直接删除 需要先删除文件
                                         判断和获取 public boolean isDirectory() 判断是否是一个目录
                                                  public boolean isFile() 判断是否是一个目文件
                                                  public boolean exists() 判断该目录下文件是否存在
                                                  public String getAbsolutePath() 判断是否是绝对路径
                                                  public String getPath()  将路径转换为字符串输出
                                                  public String getName()  返回用File封装的目录或者文件的名称
                                                  public File[] listFiles()  返回用File封装的目录或者文件的名称的File数组



俄罗斯套娃→→→递归              先规定出口再逐步递归



# 编码解码问题

   编码：将文字转换为一系列字节     byte[] getBytes() 使用平台默认的编码规则编码
                                       byte[] getBytes(String charsetName) 使用指定的编码规则编码 将该String数组编码为一系列字节，结果存放在新的字节数组中
             解码：将字节转换为文字         String(byte[] bytes)使用平台默认的编码规则解码
                                       String(byte[] bytes，String charsetName)使用指定的编码规则解码

# IO流分类

       输入流     字节输入流  能看懂的用字符流 看不懂的用字节流         																										FileInputStream(Stringname,boolean append)    																										获取文件的字节流
                        		字符输入流                                     FileReader(File file)
                                                                    					 方法 void read() 读入一个字节的数据 到达末尾返回-1
                                                                            int read(byte[] b) 读入该字节流的数据存入字节数组中 返回值为实																		际长度而不是数组的长度数组长度为5 但是只有四个字节返回值为4																		而不是5  到达末尾时返回-1
                                                                        读取时需要把字符数组转换为字符串 格式：new String(字符数																		组,offset,length)

      输出流     字节输出流                                     FileOutputStream(String name ,boolean append)   append																							为true 写入文档末尾
                    				字符输出流                                     FileWriter(File file)
                                                                 		方法  void write(int b) 指定字节写入文件输入流
                                                                      	Void write (byte[] b)
                                                                      	Void write (byte[] b, int off,int len)

        字符流读入  		 InputStreamReader(InputStream in,Stream charset) 创建一个指定编码的字符集
                 							   InputStreamReader()          创建默认编码的字符集                          简化为FileReader(String fileName)用于读取
        		字符流读出   OutputStreamWriter(OutputStream out,Stream charset)创建使用给定编码的writer
                   OutputStreamWriter()         创建默认编码的writer                         简化为FileWriter(String fileName)用于写入



缓冲流Buffered    字节缓冲流 BufferedOutputStream(OutputStream out)
                 字节缓冲流 BufferedInputStream(InputStream in)
                 字符缓冲流 BufferedWriter(Writer out)       void newLine() 写一行行分隔符，行分隔符字符串由系统定义
                 字符缓冲流 BufferedReader(Reader in)        public String readLine()读一行文字。包括行的内容的字符串，不包括行终止符，到达结尾则返回null。


总结： 字符流InputStreamReader 可简化为FileReader 字符缓冲流 BufferedReader(Reader in) 字符流写数据应该flush刷新 close会自动刷新
      字节流FileInputStream 字节流不能简化 方法都是read差不多 缓冲流直接前面加Buffered就可

标准输入流       public static final InputStream in          BufferedReader br =new BufferedReader(new InputStreamReader(System.in))
标准输出流       public static final PrintStream out         System.out

字节打印流       PrintStream(File or Stream).write(int)  会转码成相应的字符
                                          .print()      不会转码但不会换行
                                          .println()    不会转码同时换行

字符打印流       PrintWriter(String or Writer , boolean).write()     boolean为真时不需要手动刷新
                                                      .println()
                                                      .flush()

# 对象序列化

        ObjectOutputStream(OutputStream out).writeObject(Object obj)   将指定对象写入ObjectOutputStream
                对象想要序列化必须实现Serializable 该接口是一个标记接口不需要重写方法
                对象序列化过程中会自动计算一个SerialVersionUID,当类的属性做出改变就会重新计算，这时再读取以前写入的序列化文件就会造成两个UID不同会抛出异常
                所以在需要序列化的类中应该 private static final long SerialVersionUID = 任意值 这样改变属性的时候系统不会再计算新的UID值
                如果想要类的属性不被序列化 在属性面前加transient 如private transient int age

对象反序列化      ObjectInputStream(InputStream in).readObject(Object obj)

Properties extends Hashtable<Object , Object>   可以保存到流中或从流中加载 默认Object类型 不能指定类型
                                                方法：Object setProperties(String key,String value) 设置键和集
                                                     String getProperty(String key) 通过键获取值
                                                     Set<String> stringPropertyNames()返回一个不可修改的键集：通过集合获取键

Properties和IO流相结合                                 void load(Reader)
                                                    								 void store(Writer)

# 多线程

进程：正在运行的程序 是系统进行资源分配和调用的独立单位 每一个进程都有自己独立的内存空间和资源。

正在运行的程序 是系统进行资源分配和调用的独立单位 每一个进程都有自己独立的内存空间和资源。
线程：进程中的顺序控制流，是一条执行路径

多线程的实现方式 1、继承Thread类，并重写他的run方法，run方法中封装多线程实行的代码。实例化后启动对象。不能直接调用run来启动线程，而是应该用start()方法，
                 系统会自动调用run

            Thread得方法       String getName()
                              Void  setName()
                              Thread currentThread()
                              public final void setPriority(int)  设置优先级 1-10 默认是5 优先级高表示获取cpu概率高 但是不肯定
                              public final int getPriority()       获取优先级
                              static void sleep(long millis)        停留指定毫秒数
                              void join()                           等待该线程死亡
                              void setDaemon(boolean )              设定为守护线程，只有守护线程时java结束
                              static void run()
                              static void stop()

            Thread的构造方法     Thread()
                               Thread(Runnable)
                               Thread(Runnable , String name)
            优点：编码简单
                            缺点：线程类继承了thread类，无法继承其他类

         2、声明一个实现Runnable的类,该类实现了run方法，实例化该类。将实例化的对象作为参数传递给Thread，然后启动线程。
            相比于直接继承Thread类的方法，该方法避免了java只能单继承的局限性，可以在实现接口的同时，再继承一个父类
            适合多个相同程序的代码去处理同一个资源的情况，如：多终端秒杀某件商品 把线程和代码、数据有效分离


多线程程序得数据安全问题        条件：1、多线程
                                2、共享数据
                                3、多个操作语句操作恭共享数据
                           解决方法：使用同步代码块         synchronized(锁对象){
                                                            多条语句操作共享数据的代码
                                                        }
                                   同步方法             修饰符+synchronized+返回值+方法名(参数列表){

                                                    }
                                                    其中synchronized的对象为this
                               同步静态方法           修饰符+static+synchronized+返回值+方法名(参数列表){

                                                    }
                                                    其中synchronized的锁对象为类名.class

死锁的必要条件：    1、互斥使用：当资源被一个线程使用时，其他线程不能访问
                 2、不可抢占：资源请求者只能等待资源拥有者主动释放资源，而不能抢占
                 3、请求和保持：当资源请求者请求其他资源时，保持对原有资源的占有
                 4、循环等待：存在一个等待循环队列，即1请求2的资源，2同时请求1的资源
                 上述条件成立时就会形成死锁，同时，只要打破任意一个条件，就可以破坏死锁。
                 一般是锁的嵌套时会产生这种情况

变量不可见性：     并发编程时，一个线程修改变量的值，其他线程看不到最新值的情况
                 解决方法：加锁   清空工作内存 读取主内存中的最新值 所以可以看到最新值
                         对共享变量加volatile修饰  被volatile修饰的变量一旦被修改 主线程就同步到所有工作内存
                         但是volatile保证可见性，不能保证原子性 加锁可以保证原子性，但是性能比较差。为此java提供了一个原子类来保证原子性

# Lock接口



          方法void lock()               实例化对象ReentrantLock()
                     void unlock()

# Atomoic

  原子类         采用CAS(compare and swap)机制，每次写回主内存的时候比较主内存的值是否与自己刚才读取到的值相同，如果相同则直接将值写回主内存；如果不同，则重新取主内存的值重新运算，重复上面的逻辑。直到将值写回主内存。该锁被称为乐观锁，即认为大多数情况下不会发生原子性问题，当发生原子性问题时再进行相应的操作。
                              synchronize被称为悲观锁，假定每次都会发生问题，所以每次都将上锁，直到操作完毕将锁打开，其他线程才能访问，效率较低。

# ConcurrentHashMap()

并发包     HashMap线程不安全 HashTable效率较低 ConcurrentHashMap解决了以上的问题

# CountDownLatch()

 并发包     允许一个或多个线程等待其他线程结束再执行自己
                              public CountDownLatch(int count)
                              先用上面的构造器实例化一个CountDownLatch对象，然后在每个线程的有参构造器中加入监督器的形参
                              在线程的run方法中使用
                              public void await() throws InterruptedException 让当前线程等待，必须构造器中的计数为0才能重新启动
                              在需要操作的地方使用
                              public void countDown() 使计数器减1 当减为0时，await处等待的线程就会重新启动

# CyclicBarrier      

 并发包     某个线程任务必须等其他线程全部执行完毕才能触发自己执行

public final class StringBuffer     线程安全，可变的字符序列从JDK 5版本开始，这个类已经补充了一个设计用于单个线程的等效类StringBuilder
                                    通常应优先使用StringBuilder类，因为它支持所有相同的操作，但速度更快，因为它不执行同步。

public class Vector<E> implements List<E>   从Java 2平台v1.2开始，该类被改进以实现List接口，使其成为Java Collections Framework的成员。
                                            与新的集合实现不同， Vector是同步的。 如果不需要线程安全实现，建议使用ArrayList代替Vector

public class Hashtable<K,V> implements Map<K,V> 从Java 2平台v1.2开始，该类被改进以实现Map接口，使其成为Java Collections Framework的成员。
                                                与新的集合实现不同， Hashtable是同步的。 如果不需要线程安全实现，建议使用HashMap代替Hashtable
                                                如果需要线程安全的高度并发实现，则建议使用ConcurrentHashMap代替Hashtable 。



# 生产者消费者模式   

 		object.wait()
 	             object.notify()
 	             object.notifyAll()

# 线程池           

避免了频繁的创建和销毁线程，线程池里的线程可以复用，提高系统的效率，防止死机。
                  线程池的代表类：ExecutorService(接口)
                  该接口提供了一个静态方法来获取线程池对象：public static ExecutorService newFixedThreadPool(int nThreads)
                  public void submit(Runnable target / Callable target)加入一个线程进入线程池
                  public void shutdown()    等待任务结束 关闭线程池
                  public void shutdownNow() 不等待任务结束 关闭线程池

![image-20220504190753065](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220504190753065.png)

![image-20220504191159330](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220504191159330.png)

![image-20220504191255486](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220504191255486.png)

# lambda表达式       

（匿名内部类重写方法的形参列表）-> {
                        被重写的方法体;
                    }
                    lambda只能简化只有一个抽象方法的接口 如：runnable接口 Comparator接口

![image-20220503184756200](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220503184756200.png)

方法引用    ：：      简化lambda表达式     

静态方法引用：定义一个静态方法，把需要简化的代码放在静态方法中，被引用的方法的参数列表和接口中的抽象方法的参数列表必须相同，接口中的抽象方法有返回值，则静态方法也必须有。如果接口中的方法没有，静态方法中可有可无。

实例方法引用：对象：：实例方法

定义一个实例方法，把需要简化的代码放在实例方法中，被引用的方法的参数列表和接口中的抽象方法的参数列表必须相同。

 特定类型方法引用：特定类型 ：：方法

一般是String类型，第一个参数列表的形参的第一个参数作为后面方法的调用者，其余参数作为后面方法的形参，可以使用该方法。

构造器引用： 类名：：new

前后参数一致的情况下，有创建对象就可使用

如：s -> Student s  等价于 Student::new

![image-20220517215647723](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220517215647723.png)

![image-20220517215841289](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220517215841289.png)

![image-20220517220129857](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220517220129857.png)

![image-20220517220757100](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220517220757100.png)

 ![image-20220517221549214](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220517221549214.png)

# Stream流

取流：
                 Collection.stream()
                 Map.keySet().stream()
                 Map.values().stream()
                 Map.entrySet().stream()
                 Array.stream()
                 方法:
                 forEach() 逐一遍历
                 count()   统计个数
                 filter()  过滤元素
                 limit()   取前几个
                 skip()    跳过前几个
                 map()     将元素加工后放回
                 concat()  两个流合并 是静态方法，合并的两个流必须类型相同

             Stream.collection(Collections.toSet()) 将Stream转换为set同理可以转换为其他的集合
             Stream.toArray()转换为数组可以直接toArray

# 网络编程        

三要素：IP地址(标识设备)、端口(标识程序)、协议

IP地址分为两大类：IPV4和IPV6

            IPV4：给每个主机分配一个32位得地址，即四个字节，但是用二进制表示处理太费劲了，所以转换为十进制，即192.168.1.66，称为点分十进制表示法
            IPV6：IPV4不够用了，采用128位地址，即16个字节，采用十六进制表示，每个字节用一个16进制得数字表示

            常用命令： ipConfig：查看本机IP地址
                     ping Ip地址 ：查看该IP地址是否联通

            特殊IP地址：127.0.0.1 是回送地址，代表了本机地址，一般用来测试

InetAddress     IP地址类   static InetAddress getLocalHost() 获得本机IP地址对象
                          static InetAddress getByName(String Host) 根据IP地址字符串或者主机名字符串获得对应的IP地址对象
                          String getHostName()  获得主机名
                          String getHostAddress()  获得主机ip地址字符串

端口：应用程序得唯一标识    用两个字节表示，取值范围0~65535.其中0~1023是知名程序得端口号。

## 协议

连接和通讯规则         UDP(User Datagram Protocol)协议：无连接通信协议，即数据传输时发送端不会确认接收端是否存在，接收端也不会反馈接收信息消耗资源小，通信效率高
                         在音频、视频、普通数据时采用，偶尔丢失没关系。
                         TCP(Transmission Control Protocol)协议：面向连接得协议，传输可靠无差错，必须明确客户端和服务器端，由客户端向服务器端传输数据，每次传输
                         都需要经过“三次握手”。
                         三次握手： 第一次：客户端向服务器发出连接请求，等待服务器确认
                                  第二次：服务器向客户端回送一个响应，通知客户端收到连接请求
                                  第三次: 客户端再次向服务器发送确认信息，确认连接

                     上传下载文件采用TCP协议

### UDP协议

在通信两端建立一个socket对象，负责传输和接收信息。

    JAVA提供了DatagramSocket类实现socket   方法： void close() 关闭
                                              void send(DatagramPacket) 发送数据包
                                              void receive(DatagramPacket) 接收数据包
    DatagramPacket(byte[] buf, int length, InetAddress address, int port)构造一个数据包，
    将长度为length得数据发送到指定主机得端口号

### TCP协议

在通信两端建立一个socket对象，负责传输和接收信息。
        JAVA为客户端提供了socket类，为服务器端提供了ServerSocket类
        发送数据：创建客户端的socket对象     Socket(InetAddress address或者String address,int port )   Socket.getOutputStream()
                获取输出流，写数据          OutputStream s = Socket.getOutputStream()    s.write()
                释放资源        close

    接收数据：创建服务器得Socket对象     ServerSocket(int port)
            获取输入流，读数据，并显示在控制台   Socket accept() 获得Socket对象 通过Socket.getInputStream 获取输入流 然后read
            释放资源        close



# 单例模式

    每一个类只能有一个对象 防止占用过多系统资源 如：任务管理器

## 饿汉单例模式  

通过类获取对象的时候，对象已经做好了

            实现方法:定义一个单例类，将构造器隐藏起来。即将其设为private，并在该类中设置一个静态成员变量来存储被new的
                    一个对象，（因为是静态变量，所以只有一个）最后提供一个public方法将该对象提供给外部

## 懒汉单例模式  

通过类获取对象的时候，对象还没有做好

            实现方法:定义一个单例类，将构造器隐藏起来。即将其设为private，并在该类中设置一个静态成员变量来存储，但是此时不new
                    一个对象，提供一个public方法当第一次访问静态变量为空时new一个对象，然后返回该对象。

# 反射   

##    获得class对象

			   1、类名.class
                        2、类的对象.getClass()
                        3、Class.forName("类的全限名")
                                Public static Class<?> forName(String className)
                            String getSimpleName()  获取类名
                            String getName()        获取包名+类名

## 获得constructor对象

					  1、getConstructors() 获得public修饰的构造器
                               2、getDeclareConstructors()  获得所有声明的构造器
                               3、getConstructor(String.class,int.class) 根据参数列表寻找public修饰的构造器
                               4、getDeclareConstructor(String.class,int.class)  根据参数列表寻找声明的构造器
                                    void setAccessible(true)    改变访问权限
                                    T newInstance(参数列表) 创建对象

## 获取field成员变量

				      getDeclareFields()
                               getDeclareField()
                               void set(object obj, object value) 给目标对象（obj）的成员变量赋值
                               object get(object obj) 获得目标对象（obj）的成员变量的值

##  获取method方法

Method getDeclareMethod(String methodName,Class..args)  根据方法名和形参类型获得method方法
         Method[] getDeclareMethods()     获取所有方法
         object invoke(object obj,Class..args) 该obj对象的method方法触发执行

反射可以破坏类型的封装性和泛型的约束性。即改变访问权限和类型限制。

# 注解      

定义：@interface 注解名称 {                        使用：@注解名称
                    数据类型 数据名();
            }

![image-20220503190605988](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220503190605988.png)

## 元注解

![image-20220503190722029](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220503190722029.png)

![image-20220503190819696](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220503190819696.png)

# mysql

## 数据库操作 

 创建数据库     create database 数据库名称 character set 字符编码规则 collate 校对规则
          查看数据库     show databases      查看某个数据库字符集和校对规则 show create database 名称
          修改数据库     alter database  数据库名称 character set 字符编码规则 collate 校对规则
          删除数据库     drop database 数据库名称
          切换数据库     use 数据库名称
          查看当前数据库  select database()

## 表操作    

创建表        create table 表名称(字段名称 字段类型(长度) 约束，字段名称 字段类型(长度) 约束...)
      字段类型：
       java中的类型                                  mysql中的类型
                byte/short/int/long                          tinyint/smallint/int/bigint
                float                                        float
                double                                       double
                boolean                                      bit
                char/String                                  char(长度固定的字符或者字符串)和varchar(长度可变的字符或者字符串)
                Date                                         date/time/datetime(不初始化为null)/timestamp(不初始化为now)
                File                                         BLOB/TEXT
      约束：保证数据的完整性
            单表约束分类：
                主键约束：primary key 默认是唯一非空
                唯一约束：unique
                非空约束：not null
      查看表       show tables 查看该数据库下所有表
      查看表结构    desc 表名
      删除表       drop table 表名
      修改表

## 列操作

    添加列：alter table 表名 add 列名 类型（长度） 约束
            修改列类型，长度和约束：alter table 表名 modify 列名 类型（长度） 约束
            删除列：alter table 表名 drop 列名
            修改列名称：alter table 表名 change 旧列名 新列名 类型（长度） 约束
            修改表名： rename table 表名 to 新表名
            修改表的字符集： alter table 表名 character set 字符集

## 记录操作

添加表的记录    insert into 表名（列名1，列名2） values（值1，值2）
        修改表的记录    update 表名 set 列名=值，列名=值[where 条件]
        删除表的记录    delete from 表名 [where 条件] 没有条件时会删除表中所有记录可以用rollback恢复
                    				truncate table 表名 删除整个表并创建一个结构一样的 无法恢复
       查看表的记录    select[distinct去掉重复值] *|列名 from 表名 [where 条件]
                    where查询可用> < = and or not like(使用_和%作为占位符 其中_表示一位 %可以表示多位) in
                    排序查询 select * from 表名 order by 列名1 排序规则(默认升序asc,desc降序排序),列名2 排序规则 在列1相同时按照列2规则排序
                    聚合函数查询 select sum(列名，自动忽略null) from 表名；
                                      count
                                      min
                                      max
                                      avg
                    分组查询    select 列名1，列名2 from 表名 group by 列名 having  列名 筛选规则 order by 列名 排序规则

![image-20220122151123392](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220122151123392.png)

![image-20220122161050612](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220122161050612.png)

## 联表查询

![image-20220122163611670](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220122163611670.png)

select 列名1，列名2 

from 数据库1

操作（如inner join）数据库2

on 条件

![image-20220123120650801](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220123120650801.png)

分页

limit（起始位置，页面大小）

![image-20220123122609663](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220123122609663.png)

## 聚合函数

![image-20220123124424557](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220123124424557.png)

## 索引

![image-20220123144254901](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220123144254901.png)

## 用户管理

![image-20220123215338834](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220123215338834.png)

## 三大范式

![image-20220123221122995](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220123221122995.png)

# JDBC

![image-20211106181236355](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20211106181236355.png)

需要的依赖

java.sql

javax.sql

mysql-connector-java

# XML

![image-20220505155038708](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220505155038708.png)

![image-20220505155225011](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220505155225011.png)

![image-20220505155328818](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220505155328818.png)

## 解析

![image-20220505155637563](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220505155637563.png)

![image-20220505155900122](C:/Users/LN/AppData/Roaming/Typora/typora-user-images/image-20220505155900122.png)
