# RDBMS

![image-20221008133212527](D:\笔记\mysql.assets\image-20221008133212527.png)

![](D:\笔记\mysql.assets\image-20221008133533843.png)

# 非RDBMS

![image-20221008133652663](D:\笔记\mysql.assets\image-20221008133652663.png)

![image-20221008133838255](D:\笔记\mysql.assets\image-20221008133838255.png)

![image-20221008133937249](D:\笔记\mysql.assets\image-20221008133937249.png)

# 关系型数据库设计规则

![image-20221008134541272](D:\笔记\mysql.assets\image-20221008134541272.png)

# SQL语言

##  分类

![image-20221008145337372](D:\笔记\mysql.assets\image-20221008145337372.png)

## 规则

![image-20221008150618224](D:\笔记\mysql.assets\image-20221008150618224.png)

## 规范

![image-20221008150641065](D:\笔记\mysql.assets\image-20221008150641065-16652128016761.png)

## 注释

![image-20221008151109932](D:\笔记\mysql.assets\image-20221008151109932.png)

## 基本sql语句

### select

基本select 和别名不在此赘述

  ==distinct去重==

```sql
SELECT DISTINCT department_id FROM employees;
```

![image-20221009124741447](D:\笔记\mysql.assets\image-20221009124741447.png)

### 运算符

![image-20221009134310982](D:\笔记\mysql.assets\image-20221009134310982.png)

![image-20221009134433868](D:\笔记\mysql.assets\image-20221009134433868.png)

![image-20221009134515009](D:\笔记\mysql.assets\image-20221009134515009.png)

#### =

![image-20221009134802639](D:\笔记\mysql.assets\image-20221009134802639.png)

#### <=>

![image-20221009135351746](D:\笔记\mysql.assets\image-20221009135351746.png)

![image-20221009135528212](D:\笔记\mysql.assets\image-20221009135528212.png)

### order

![image-20221009143229474](D:\笔记\mysql.assets\image-20221009143229474.png)

![](D:\笔记\mysql.assets\image-20221009143751369.png)

### 分页

![image-20221009145048658](D:\笔记\mysql.assets\image-20221009145048658.png)

## 多表查询

### 自连接

![image-20221010125013152](D:\笔记\mysql.assets\image-20221010125013152.png)

### 内连接与外连接

![image-20221010125031461](D:\笔记\mysql.assets\image-20221010125031461.png)

### union

![image-20221010140202951](D:\笔记\mysql.assets\image-20221010140202951.png)

![image-20221010140252582](D:\笔记\mysql.assets\image-20221010140252582.png)

###   七种join

![image-20221010141117078](D:\笔记\mysql.assets\image-20221010141117078.png)

```mysql
# 中图：内连接
SELECT employee_id,department_name
FROM employees e JOIN departments d
ON e.`department_id` = d.`department_id`;

# 左上图：左外连接
SELECT employee_id,department_name
FROM employees e LEFT JOIN departments d
ON e.`department_id` = d.`department_id`;

# 右上图：右外连接
SELECT employee_id,department_name
FROM employees e RIGHT JOIN departments d
ON e.`department_id` = d.`department_id`;

# 左中图：
SELECT employee_id,department_name
FROM employees e LEFT JOIN departments d
ON e.`department_id` = d.`department_id`
WHERE d.`department_id` IS NULL;

# 右中图：
SELECT employee_id,department_name
FROM employees e RIGHT JOIN departments d
ON e.`department_id` = d.`department_id`
WHERE e.`department_id` IS NULL;


# 左下图：满外连接
# 方式1：左上图 UNION ALL 右中图
SELECT employee_id,department_name
FROM employees e LEFT JOIN departments d
ON e.`department_id` = d.`department_id`
UNION ALL
SELECT employee_id,department_name
FROM employees e RIGHT JOIN departments d
ON e.`department_id` = d.`department_id`
WHERE e.`department_id` IS NULL;


# 方式2：左中图 UNION ALL 右上图
SELECT employee_id,department_name
FROM employees e LEFT JOIN departments d
ON e.`department_id` = d.`department_id`
WHERE d.`department_id` IS NULL
UNION ALL
SELECT employee_id,department_name
FROM employees e RIGHT JOIN departments d
ON e.`department_id` = d.`department_id`;

# 右下图：左中图  UNION ALL 右中图
SELECT employee_id,department_name
FROM employees e LEFT JOIN departments d
ON e.`department_id` = d.`department_id`
WHERE d.`department_id` IS NULL
UNION ALL
SELECT employee_id,department_name
FROM employees e RIGHT JOIN departments d
ON e.`department_id` = d.`department_id`
WHERE e.`department_id` IS NULL;
```

### SQL99新特性

#### 自然连接

SQL99 在 SQL92 的基础上提供了一些特殊语法，比如 NATURAL JOIN 用来表示自然连接。我们可以把 自然连接理解为 SQL92 中的等值连接。它会帮你自动查询两张连接表中 所有相同的字段 ，然后进行 等值 连接 。

在SQL92标准中：

```mysql
SELECT employee_id,last_name,department_name
FROM employees e JOIN departments d
ON e.`department_id` = d.`department_id`
AND e.`manager_id` = d.`manager_id`;
```

在 SQL99 中你可以写成：

```mysql
SELECT employee_id,last_name,department_name
FROM employees e NATURAL JOIN departments d;
```

#### using

当我们进行连接的时候，SQL99还支持使用 USING 指定数据表里的 同名字段 进行等值连接。但是只能配 合JOIN一起使用。比如：

```mysql
SELECT employee_id,last_name,department_name
FROM employees e JOIN departments d
USING (department_id);
```

你能看出与自然连接 NATURAL JOIN 不同的是，USING 指定了具体的相同的字段名称，你需要在 USING 的括号 () 中填入要指定的同名字段。同时使用 JOIN...USING 可以简化 JOIN ON 的等值连接。它与下 面的 SQL 查询结果是相同的：

```mysql
SELECT employee_id,last_name,department_name
FROM employees e ,departments d
WHERE e.department_id = d.department_id;
```

## 小结

表连接的约束条件可以有三种方式：WHERE, ON, USING

- WHERE：适用于所有关联查询
- ON ：只能和JOIN一起使用，只能写关联条件。虽然关联条件可以并到WHERE中和其他条件一起 写，但分开写可读性更好。
- USING：只能和JOIN一起使用，而且要求两个关联字段在关联表中名称一致，而且只能表示关联字 段值相等

> 我们要控制连接表的数量 。
>
> 多表连接就相当于嵌套 for 循环一样，非常消耗资源，会让 SQL 查询性能下 降得很严重，因此不要连接不必要的表。
>
> 在许多 DBMS 中，也都会有最大连接表的限制。

```mysql
# 习题巩固
# 注意：当两个表外连接之后，组成主表和从表，主表的连接字段是不为空的，从表的连接字段可能为空，因此从表的关键字段用来判断是否为空。

# 1.查询哪些部门没有员工
# 方式一
SELECT d.department_id
FROM departments d LEFT JOIN employees e
ON d.`department_id` = e.`department_id`
WHERE e.`department_id` IS NULL;

# 方式二
SELECT department_id
FROM departments d
WHERE NOT EXISTS (
		SELECT *
    	FROM employees e
    	WHERE e.`department_id` = d.`department_id`
);

# 2.查询哪个城市没有部门
SELECT l.location_id, l.city
FROM locations l LEFT JOIN departments d
ON l.`location_id` = d.`location_id`
WHERE d.`location_id` IS NULL;

# 3.查询部门名为 Sales 或 IT 的员工信息
SELECT e.employee_id, e.last_name, e.department_id
FROM employees e JOIN department d
ON e.`department_id` = d.`department_id`
WHERE d.`department_name` IN ('Sales', 'IT');
```

# 单行函数:funeral_urn:



![image-20221011132732850](D:\笔记\mysql.assets\image-20221011132732850.png)

##  数值函数

###  基本函数

| 函数                | 用法                                                         |
| ------------------- | ------------------------------------------------------------ |
| ABS(x)              | 返回x的绝对值                                                |
| SIGN(X)             | 单元格                                                       |
| PI()                | 返回圆周率的值                                               |
| CEIL(x)，CEILING(x) | 返回大于或等于某个值的最小整数                               |
| FLOOR(x)            | 返回小于或等于某个值的最大整数                               |
| LEAST(e1,e2,e3…)    | 返回列表中的最小值                                           |
| GREATEST(e1,e2,e3…) | 返回列表中的最大值                                           |
| MOD(x,y)            | 返回X除以Y后的余数                                           |
| RAND()              | 返回0~1的随机值                                              |
| RAND(x)             | 返回0~1的随机值，其中x的值用作种子值，相同的X值会产生相同的随机 数 |
| ROUND(x)            | 返回一个对x的值进行四舍五入后，最接近于X的整数               |
| ROUND(x,y)          | 返回一个对x的值进行四舍五入后最接近X的值，并保留到小数点后面Y位 |
| TRUNCATE(x,y)       | 返回数字x截断为y位小数的结果                                 |
| SQRT(x)             | 返回x的平方根。当X的值为负数时，返回NULL                     |

###  角度与弧度互换函数

| 函数       | 用法                                  |
| ---------- | ------------------------------------- |
| RADIANS(x) | 将角度转化为弧度，其中，参数x为角度值 |
| DEGREES(x) | 将弧度转化为角度，其中，参数x为弧度值 |

###  三角函数

| 函数       | 用法                                                         |
| ---------- | ------------------------------------------------------------ |
| SIN(x)     | 返回x的正弦值，其中，参数x为弧度值                           |
| ASIN(x)    | 返回x的反正弦值，即获取余弦为x的值。如果x的值不在-1到1之间，则返回NULL |
| COS(x)     | 返回x的余弦值，其中，参数x为弧度值                           |
| ACOS(x)    | 返回x的反余弦值，即获取余弦为x的值。如果x的值不在-1到1之间，则返回NULL |
| TAN(x)     | 返回x的正切值，其中，参数x为弧度值                           |
| ATAN(x)    | 返回x的反正切值，即返回正切值为x的值                         |
| ATAN2(m,n) | 返回两个参数的反正切值                                       |
| COT(x)     | 返回x的余切值，其中，X为弧度值                               |

###  指数与对数函数

| 函数                 | 用法                                                 |
| -------------------- | ---------------------------------------------------- |
| POW(x,y)，POWER(X,Y) | 返回x的y次方                                         |
| EXP(X)               | 返回e的X次方，其中e是一个常数，2.718281828459045     |
| LN(X)，LOG(X)        | 返回以e为底的X的对数，当X <= 0 时，返回的结果为NULL  |
| LOG10(X)             | 返回以10为底的X的对数，当X <= 0 时，返回的结果为NULL |
| LOG2(X)              | 返回以2为底的X的对数，当X <= 0 时，返回NULL          |

### 进制间的转换

| 函数          | 用法                     |
| ------------- | ------------------------ |
| BIN(x)        | 返回x的二进制编码        |
| HEX(x)        | 返回x的十六进制编码      |
| OCT(x)        | 返回x的八进制编码        |
| CONV(x,f1,f2) | 返回f1进制数变成f2进制数 |

## 2. 字符串函数

| 函数                              | 用法                                                         |
| --------------------------------- | ------------------------------------------------------------ |
| ASCII(S)                          | 返回字符串S中的第一个字符的ASCII码值                         |
| CHAR_LENGTH(s)                    | 返回字符串s的字符数。作用与CHARACTER_LENGTH(s)相同           |
| LENGTH(s)                         | 返回字符串s的字节数，和字符集有关                            |
| CONCAT(s1,s2,......,sn)           | 连接s1,s2,......,sn为一个字符串                              |
| CONCAT_WS(x, s1,s2,......,sn)     | 同CONCAT(s1,s2,...)函数，但是每个字符串之间要加上x           |
| INSERT(str, idx, len, replacestr) | 将字符串str从第idx位置开始，len个字符长的子串替换为字符串replacestr |
| REPLACE(str, a, b)                | 用字符串b替换字符串str中所有出现的字符串a                    |
| UPPER(s) 或 UCASE(s)              | 将字符串s的所有字母转成大写字母                              |
| LOWER(s) 或LCASE(s)               | 将字符串s的所有字母转成小写字母                              |
| LEFT(str,n)                       | 返回字符串str最左边的n个字符                                 |
| RIGHT(str,n)                      | 返回字符串str最右边的n个字符                                 |
| LPAD(str, len, pad)               | 用字符串pad对str最左边进行填充，直到str的长度为len个字符     |
| RPAD(str ,len, pad)               | 用字符串pad对str最右边进行填充，直到str的长度为len个字符     |
| LTRIM(s)                          | 去掉字符串s左侧的空格                                        |
| RTRIM(s)                          | 去掉字符串s右侧的空格                                        |
| TRIM(s)                           | 去掉字符串s开始与结尾的空格                                  |
| TRIM(s1 FROM s)                   | 去掉字符串s开始与结尾的s1                                    |
| TRIM(LEADING s1 FROM s)           | 去掉字符串s开始处的s1                                        |
| TRIM(TRAILING s1 FROM s)          | 去掉字符串s结尾处的s1                                        |
| REPEAT(str, n)                    | 返回str重复n次的结果                                         |
| SPACE(n)                          | 返回n个空格                                                  |
| STRCMP(s1,s2)                     | 比较字符串s1,s2的ASCII码值的大小                             |
| SUBSTR(s,index,len)               | 返回从字符串s的index位置其len个字符，作用与SUBSTRING(s,n,len)、 MID(s,n,len)相同 |
| LOCATE(substr,str)                | 返回字符串substr在字符串str中首次出现的位置，作用于POSITION(substr IN str)、INSTR(str,substr)相同。未找到，返回0 |
| ELT(m,s1,s2,…,sn)                 | 返回指定位置的字符串，如果m=1，则返回s1，如果m=2，则返回s2，如果m=n，则返回sn |
| FIELD(s,s1,s2,…,sn)               | 返回字符串s在字符串列表中第一次出现的位置                    |
| FIND_IN_SET(s1,s2)                | 返回字符串s1在字符串s2中出现的位置。其中，字符串s2是一个以逗号分隔的字符串 |
| REVERSE(s)                        | 返回s反转后的字符串                                          |
| NULLIF(value1,value2)             | 比较两个字符串，如果value1与value2相等，则返回NULL，否则返回 value1 |

> 注意：MySQL中，字符串的位置是从1开始的。

## 3. 日期和时间函数

### 获取日期、时间

| 函数                                                         | 用法                            |
| ------------------------------------------------------------ | ------------------------------- |
| CURDATE() ，CURRENT_DATE()                                   | 返回当前日期，只包含年、 月、日 |
| CURTIME() ， CURRENT_TIME()                                  | 返回当前时间，只包含时、 分、秒 |
| NOW() / SYSDATE() / CURRENT_TIMESTAMP() / LOCALTIME() / LOCALTIMESTAMP() | 返回当前系统日期和时间          |
| UTC_DATE()                                                   | 返回UTC（世界标准时间） 日期    |
| UTC_TIME()                                                   | 返回UTC（世界标准时间） 时间    |

###  日期与时间戳的转换

| 函数                     | 用法                                                         |
| ------------------------ | ------------------------------------------------------------ |
| UNIX_TIMESTAMP()         | 以UNIX时间戳的形式返回当前时间。SELECT UNIX_TIMESTAMP() - >1634348884 |
| UNIX_TIMESTAMP(date)     | 将时间date以UNIX时间戳的形式返回。                           |
| FROM_UNIXTIME(timestamp) | 将UNIX时间戳的时间转换为普通格式的时间                       |

###  获取月份、星期、星期数、天数等函数

| 函数                                     | 用法                                             |
| ---------------------------------------- | ------------------------------------------------ |
| YEAR(date) / MONTH(date) / DAY(date)     | 返回具体的日期值                                 |
| HOUR(time) / MINUTE(time) / SECOND(time) | 返回具体的时间值                                 |
| FROM_UNIXTIME(timestamp)                 | 将UNIX时间戳的时间转换为普通格式的时间           |
| MONTHNAME(date)                          | 返回月份：January，...                           |
| DAYNAME(date)                            | 返回星期几：MONDAY，TUESDAY.....SUNDAY           |
| WEEKDAY(date)                            | 返回周几，注意，周1是0，周2是1，。。。周日是6    |
| QUARTER(date)                            | 返回日期对应的季度，范围为1～4                   |
| WEEK(date) ， WEEKOFYEAR(date)           | 返回一年中的第几周                               |
| DAYOFYEAR(date)                          | 返回日期是一年中的第几天                         |
| DAYOFMONTH(date)                         | 返回日期位于所在月份的第几天                     |
| DAYOFWEEK(date)                          | 返回周几，注意：周日是1，周一是2，。。。周六是 7 |

###  日期的操作函数

| 函数                    | 用法                                       |
| ----------------------- | ------------------------------------------ |
| EXTRACT(type FROM date) | 返回指定日期中特定的部分，type指定返回的值 |

EXTRACT(type FROM date)函数中type的取值与含义：

![image-20220601162705975](D:/笔记/jvm.assets/MySQL基础篇.assets/image-20220601162705975.png)

###  时间和秒钟转换的函数

| 函数                 | 用法                                                         |
| -------------------- | ------------------------------------------------------------ |
| TIME_TO_SEC(time)    | 将 time 转化为秒并返回结果值。转化的公式为： 小时*3600+分钟 *60+秒 |
| SEC_TO_TIME(seconds) | 将 seconds 描述转化为包含小时、分钟和秒的时间                |

### 计算日期和时间的函数

| 函数                                                         | 用法                                           |
| ------------------------------------------------------------ | ---------------------------------------------- |
| DATE_ADD(datetime, INTERVAL expr type)， ADDDATE(date,INTERVAL expr type) | 返回与给定日期时间相差INTERVAL时间段的日期时间 |
| DATE_SUB(date,INTERVAL expr type)， SUBDATE(date,INTERVAL expr type) | 返回与date相差INTERVAL时间间隔的日期           |

上述函数中type的取值：

![image-20220601165055639](D:/笔记/jvm.assets/MySQL基础篇.assets/image-20220601165055639.png)

| 函数                         | 用法                                                         |
| ---------------------------- | ------------------------------------------------------------ |
| ADDTIME(time1,time2)         | 返回time1加上time2的时间。当time2为一个数字时，代表的是 秒 ，可以为负数 |
| SUBTIME(time1,time2)         | 返回time1减去time2后的时间。当time2为一个数字时，代表的 是 秒 ，可以为负数 |
| DATEDIFF(date1,date2)        | 返回date1 - date2的日期间隔天数                              |
| TIMEDIFF(time1, time2)       | 返回time1 - time2的时间间隔                                  |
| FROM_DAYS(N)                 | 返回从0000年1月1日起，N天以后的日期                          |
| TO_DAYS(date)                | 返回日期date距离0000年1月1日的天数                           |
| LAST_DAY(date)               | 返回date所在月份的最后一天的日期                             |
| MAKEDATE(year,n)             | 针对给定年份与所在年份中的天数返回一个日期                   |
| MAKETIME(hour,minute,second) | 将给定的小时、分钟和秒组合成时间并返回                       |
| PERIOD_ADD(time,n)           | 返回time加上n后的时间                                        |

###   日期的格式化与解析

| 函数                              | 用法                                       |
| --------------------------------- | ------------------------------------------ |
| DATE_FORMAT(date,fmt)             | 按照字符串fmt格式化日期date值              |
| TIME_FORMAT(time,fmt)             | 按照字符串fmt格式化时间time值              |
| GET_FORMAT(date_type,format_type) | 返回日期字符串的显示格式                   |
| STR_TO_DATE(str, fmt)             | 按照字符串fmt对str进行解析，解析为一个日期 |

上述 非GET_FORMAT 函数中fmt参数常用的格式符：

| 格式符 | 说明                                                         | 格式符  | 说明                                                         |
| ------ | ------------------------------------------------------------ | ------- | ------------------------------------------------------------ |
| %Y     | 4位数字表示年份                                              | %y      | 表示两位数字表示年份                                         |
| %M     | 月名表示月份（January,....）                                 | %m      | 两位数字表示月份 （01,02,03。。。）                          |
| %b     | 缩写的月名（Jan.，Feb.，....）                               | %c      | 数字表示月份（1,2,3,...）                                    |
| %D     | 英文后缀表示月中的天数 （1st,2nd,3rd,...）                   | %d      | 两位数字表示月中的天数(01,02...)                             |
| %e     | 数字形式表示月中的天数 （1,2,3,4,5.....）                    |         |                                                              |
| %H     | 两位数字表示小数，24小时制 （01,02..）                       | %h 和%I | 两位数字表示小时，12小时制 （01,02..）                       |
| %k     | 数字形式的小时，24小时制(1,2,3)                              | %l      | 数字形式表示小时，12小时制 （1,2,3,4....）                   |
| %i     | 两位数字表示分钟（00,01,02）                                 | %S 和%s | 两位数字表示秒(00,01,02...)                                  |
| %W     | 一周中的星期名称（Sunday...）                                | %a      | 一周中的星期缩写（Sun.， Mon.,Tues.，..）                    |
| %w     | 以数字表示周中的天数 (0=Sunday,1=Monday....)                 |         |                                                              |
| %j     | 以3位数字表示年中的天数(001,002...)                          | %U      | 以数字表示年中的第几周， （1,2,3。。）其中Sunday为周中第一 天 |
| %u     | 以数字表示年中的第几周， （1,2,3。。）其中Monday为周中第一 天 |         |                                                              |
| %T     | 24小时制                                                     | %r      | 12小时制                                                     |
| %p     | AM或PM                                                       | %%      | 表示%                                                        |

## 4. 流程控制函数

流程处理函数可以根据不同的条件，执行不同的处理流程，可以在SQL语句中实现不同的条件选择。 MySQL中的流程处理函数主要包括IF()、IFNULL()和CASE()函数。

| 函数                                                         | 用法                                             |
| ------------------------------------------------------------ | ------------------------------------------------ |
| IF(value,value1,value2)                                      | 如果value的值为TRUE，返回value1， 否则返回value2 |
| IFNULL(value1, value2)                                       | 如果value1不为NULL，返回value1，否则返回value2   |
| CASE WHEN 条件1 THEN 结果1 WHEN 条件2 THEN 结果2 .... [ELSE resultn] END | 相当于Java的if...else if...else...               |
| CASE expr WHEN 常量值1 THEN 值1 WHEN 常量值1 THEN 值1 .... [ELSE 值n] END | 相当于Java的switch...case...                     |

## 5. 加密与解密函数

加密与解密函数主要用于对数据库中的数据进行加密和解密处理，以防止数据被他人窃取。这些函数在保证数据库安全时非常有用。

| 函数                        | 用法                                                         |
| --------------------------- | ------------------------------------------------------------ |
| PASSWORD(str)               | 返回字符串str的加密版本，41位长的字符串。加密结果不可逆 ，常用于用户的密码加密 |
| MD5(str)                    | 返回字符串str的md5加密后的值，也是一种加密方式。若参数为 NULL，则会返回NULL |
| SHA(str)                    | 从原明文密码str计算并返回加密后的密码字符串，当参数为 NULL时，返回NULL。 SHA加密算法比MD5更加安全 。 |
| ENCODE(value,password_seed) | 返回使用password_seed作为加密密码加密value                   |
| DECODE(value,password_seed) | 返回使用password_seed作为加密密码解密value                   |

## 6. MySQL信息函数

MySQL中内置了一些可以查询MySQL信息的函数，这些函数主要用于帮助数据库开发或运维人员更好地 对数据库进行维护工作。

| 函数                                                   | 用法                                                      |
| ------------------------------------------------------ | --------------------------------------------------------- |
| VERSION()                                              | 返回当前MySQL的版本号                                     |
| CONNECTION_ID()                                        | 返回当前MySQL服务器的连接数                               |
| DATABASE()，SCHEMA()                                   | 返回MySQL命令行当前所在的数据库                           |
| USER()，CURRENT_USER()、SYSTEM_USER()， SESSION_USER() | 返回当前连接MySQL的用户名，返回结果格式为 “主机名@用户名” |
| CHARSET(value)                                         | 返回字符串value自变量的字符集                             |
| COLLATION(value)                                       | 返回字符串value的比较规则                                 |

MySQL中有些函数无法对其进行具体的分类，但是这些函数在MySQL的开发和运维过程中也是不容忽视 的。

| 函数                           | 用法                                                         |
| ------------------------------ | ------------------------------------------------------------ |
| FORMAT(value,n)                | 返回对数字value进行格式化后的结果数据。n表示 四舍五入 后保留 到小数点后n位 |
| CONV(value,from,to)            | 将value的值进行不同进制之间的转换                            |
| INET_ATON(ipvalue)             | 将以点分隔的IP地址转化为一个数字                             |
| INET_NTOA(value)               | 将数字形式的IP地址转化为以点分隔的IP地址                     |
| BENCHMARK(n,expr)              | 将表达式expr重复执行n次。用于测试MySQL处理expr表达式所耗费 的时间 |
| CONVERT(value USING char_code) | 将value所使用的字符编码修改为char_code                       |

# 聚合函数

## 聚合函数介绍

* 什么是聚合函数

聚合函数作用于一组数据，并对一组数据返回一个值。

* 聚合函数类型
	* AVG()
	* SUM()
	* MAX()
	* MIN()
	* COUNT()

###  AVG和SUM函数

```mysql
SELECT AVG(salary), MAX(salary),MIN(salary), SUM(salary)
FROM employees
WHERE job_id LIKE '%REP%';
```

### MIN和MAX函数

可以对任意数据类型的数据使用 MIN 和 MAX 函数。

```mysql
SELECT MIN(hire_date), MAX(hire_date)
FROM employees;
```

###  COUNT函数

COUNT(*)返回表中记录总数，适用于任意数据类型。

```mysql
SELECT COUNT(*)
FROM employees
WHERE department_id = 50;
```

COUNT(expr) 返回expr不为空的记录总数。

```mysql
SELECT COUNT(commission_pct)
FROM employees
WHERE department_id = 50;
```

* 问题：用count(*)，count(1)，count(列名)谁好呢?

其实，对于MyISAM引擎的表是没有区别的。这种引擎内部有一计数器在维护着行数。 Innodb引擎的表用count(*),count(1)直接读行数，复杂度是O(n)，因为innodb真的要去数一遍。但好 于具体的count(列名)。

* 问题：能不能使用count(列名)替换count(*)?

不要使用 count(列名)来替代 count( * ) ， count( * ) 是 SQL92 定义的标准统计行数的语法，跟数 据库无关，跟 NULL 和非 NULL 无关。 说明：count(*)会统计值为 NULL 的行，而 count(列名)不会统计此列为 NULL 值的行。

## 2. GROUP BY

###  基本使用

可以使用GROUP BY子句将表中的数据分成若干组

```mysql
SELECT column, group_function(column)
FROM table
[WHERE condition]
[GROUP BY group_by_expression]
[ORDER BY column];
```

> 结论1：SELECT中出现的非组函数的字段必须声明在GROUP BY中。
>
> 			反之，GROUP BY中声明的字段可以不出现在SELECT中。
>
> 结论2：GROUP BY声明在FROM后面、WHERE后面、ORDER BY前面、LIMIT前面。
>
> ![image-20221012153254185](D:\笔记\mysql.assets\image-20221012153254185.png)

### 使用WITH ROLLUP

```mysql
SELECT department_id,AVG(salary)
FROM employees
WHERE department_id > 80
GROUP BY department_id WITH ROLLUP;
```

> 注意： 当使用ROLLUP时，不能同时使用ORDER BY子句进行结果排序，即ROLLUP和ORDER BY是互相排斥的。

## 3. HAVING

###  基本使用

过滤分组：HAVING子句 

1. 行已经被分组。 
2. 使用了聚合函数。 
3. 满足HAVING 子句中条件的分组将被显示。 
4. HAVING 不能单独使用，必须要跟 GROUP BY 一起使用。

```mysql
SELECT department_id, MAX(salary)
FROM employees
GROUP BY department_id
HAVING MAX(salary)>10000 ;
```

**要求**

+ 如果过滤条件中使用了聚合函数，则必须使用HAVING来替换WHERE。否则，报错。
+ 当过滤条件中没有聚合函数时，则次过滤条件声明在WHERE中或HAVING中都可以。但是，建议声明在WHERE中的执行效率高。
+ HAVING必须声明在GROUP BY 的后面
+ 开发中，我们使用HAVING的前提是SQL中使用了GROUP BY。

###  WHERE和HAVING的对比

**区别1：WHERE 可以直接使用表中的字段作为筛选条件，但不能使用分组中的计算函数作为筛选条件； HAVING 必须要与 GROUP BY 配合使用，可以把分组计算的函数和分组字段作为筛选条件。**

这决定了，在需要对数据进行分组统计的时候，HAVING 可以完成 WHERE 不能完成的任务。这是因为， 在查询语法结构中，WHERE 在 GROUP BY 之前，所以无法对分组结果进行筛选。HAVING 在 GROUP BY 之 后，可以使用分组字段和分组中的计算函数，对分组的结果集进行筛选，这个功能是 WHERE 无法完成 的。另外，WHERE排除的记录不再包括在分组中。

**区别2：如果需要通过连接从关联表中获取需要的数据，WHERE 是先筛选后连接，而 HAVING 是先连接 后筛选。**

这一点，就决定了在关联查询中，WHERE 比 HAVING 更高效。因为 WHERE 可以先筛选，用一 个筛选后的较小数据集和关联表进行连接，这样占用的资源比较少，执行效率也比较高。HAVING 则需要 先把结果集准备好，也就是用未被筛选的数据集进行关联，然后对这个大的数据集进行筛选，这样占用 的资源就比较多，执行效率也较低。

小结如下：

| 关键字 | 用法                         | 缺点                                   |
| ------ | ---------------------------- | -------------------------------------- |
| WHERE  | 先筛选数据再关联，执行效率高 | 不能使用分组中的计算函数进行筛选       |
| HAVING | 可以使用分组中的计算函数     | 在最后的结果集中进行筛选，执行效率较低 |

**开发中的选择：** 

WHERE 和 HAVING 也不是互相排斥的，我们可以在一个查询里面同时使用 WHERE 和 HAVING。包含分组 统计函数的条件用 HAVING，普通条件用 WHERE。这样，我们就既利用了 WHERE 条件的高效快速，又发 挥了 HAVING 可以使用包含分组统计函数的查询条件的优点。当数据量特别大的时候，运行效率会有很 大的差别。

## 4. SELECT的执行过程

### 查询的结构

```mysql
#方式1：
SELECT ...,....,...
FROM ...,...,....
WHERE 多表的连接条件
AND 不包含组函数的过滤条件
GROUP BY ...,...
HAVING 包含组函数的过滤条件
ORDER BY ... ASC/DESC
LIMIT ...,...
#方式2：
SELECT ...,....,...
FROM ... JOIN ...
ON 多表的连接条件
JOIN ...
ON ...
WHERE 不包含组函数的过滤条件
AND/OR 不包含组函数的过滤条件
GROUP BY ...,...
HAVING 包含组函数的过滤条件
ORDER BY ... ASC/DESC
LIMIT ...,...
#其中：
#（1）from：从哪些表中筛选
#（2）on：关联多表查询时，去除笛卡尔积
#（3）where：从表中筛选的条件
#（4）group by：分组依据
#（5）having：在统计结果中再次筛选
#（6）order by：排序
#（7）limit：分页
```

**需要记住 SELECT 查询时的两个顺序：**

<font color=red>1. 关键字的顺序是不能颠倒的：</font>

```mysql
SELECT ... FROM ... WHERE ... GROUP BY ... HAVING ... ORDER BY ... LIMIT...
```

<font color=red>1. SELECT 语句的执行顺序</font>（在 MySQL 和 Oracle 中，SELECT 执行顺序基本相同）：

```mysql
FROM -> WHERE -> GROUP BY -> HAVING -> SELECT 的字段 -> DISTINCT -> ORDER BY -> LIMIT
```

比如你写了一个 SQL 语句，那么它的关键字顺序和执行顺序是下面这样的：

```mysql
SELECT DISTINCT player_id, player_name, count(*) as num # 顺序 5
FROM player JOIN team ON player.team_id = team.team_id # 顺序 1
WHERE height > 1.80 # 顺序 2
GROUP BY player.team_id # 顺序 3
HAVING num > 2 # 顺序 4
ORDER BY num DESC # 顺序 6
LIMIT 2 # 顺序 7
```

在 SELECT 语句执行这些步骤的时候，每个步骤都会产生一个 虚拟表 ，然后将这个虚拟表传入下一个步 骤中作为输入。需要注意的是，这些步骤隐含在 SQL 的执行过程中，对于我们来说是不可见的。

###  SQL的执行原理

SELECT 是先执行 FROM 这一步的。在这个阶段，如果是多张表联查，还会经历下面的几个步骤：

1. 首先先通过 CROSS JOIN 求笛卡尔积，相当于得到虚拟表 vt（virtual table）1-1；
2. 通过 ON 进行筛选，在虚拟表 vt1-1 的基础上进行筛选，得到虚拟表 vt1-2；
3. 添加外部行。如果我们使用的是左连接、右链接或者全连接，就会涉及到外部行，也就是在虚拟 表 vt1-2 的基础上增加外部行，得到虚拟表 vt1-3。

* 当然如果我们操作的是两张以上的表，还会重复上面的步骤，直到所有表都被处理完为止。这个过程得 到是我们的原始数据。

* 然后进入第三步和第四步，也就是 GROUP 和 HAVING 阶段 。在这个阶段中，实际上是在虚拟表 vt2 的 基础上进行分组和分组过滤，得到中间的虚拟表 vt3 和 vt4 。
* 当我们完成了条件筛选部分之后，就可以筛选表中提取的字段，也就是进入到 SELECT 和 DISTINCT 阶段 。
* 首先在 SELECT 阶段会提取想要的字段，然后在 DISTINCT 阶段过滤掉重复的行，分别得到中间的虚拟表 vt5-1 和 vt5-2 。
* 当我们提取了想要的字段数据之后，就可以按照指定的字段进行排序，也就是 ORDER BY 阶段 ，得到 虚拟表 vt6 。
* 最后在 vt6 的基础上，取出指定行的记录，也就是 LIMIT 阶段 ，得到最终的结果，对应的是虚拟表 vt7 。
* 当然我们在写 SELECT 语句的时候，不一定存在所有的关键字，相应的阶段就会省略。

同时因为 SQL 是一门类似英语的结构化查询语言，所以我们在写 SELECT 语句的时候，还要注意相应的 关键字顺序，所谓底层运行的原理，就是我们刚才讲到的执行顺序。

# 子查询

## 单行子查询

###  单行比较操作符

| 操作符 | 含义                     |
| ------ | ------------------------ |
| =      | equal to                 |
| >      | greater than             |
| >=     | greater than or equal to |
| <      | less than                |
| <=     | less than or equal to    |
| <>     | not equal to             |

###  代码示例

* 题目：返回job_id与141号员工相同，salary比143号员工多的员工姓名，job_id和工资

```mysql
SELECT last_name, job_id, salary
FROM eployees
WHERE job_id = (
	SELECT job_id
	FROM eployees
    WHERE employee_id = 141
)
AND salary > (
	SELECT salary
	FROM eployees
    WHERE employee_id = 143
);
```

* 题目：查询与141号或174号员工的manager_id和department_id相同的其他员工的employee_id， manager_id，department_id

```mysql
# 实现方式一：不成对比较
SELECT employee_id, manager_id, department_id
FROM employees
WHERE manager_id IN
        (SELECT manager_id
        FROM employees
        WHERE employee_id IN (174,141))
AND department_id IN
        (SELECT department_id
        FROM employees
        WHERE employee_id IN (174,141))
AND employee_id NOT IN(174,141);

# 实现方式二：成对比较
SELECT employee_id, manager_id, department_id
FROM employees
WHERE (manager_id, department_id) IN
        (SELECT manager_id, department_id
        FROM employees
        WHERE employee_id IN (141,174))
AND employee_id NOT IN (141,174);
```

* 题目：查询最低工资大于50号部门最低工资的部门id和其最低工资

```mysql
SELECT department_id, MIN(salary)
FROM employees
GROUP BY department_id
HAVING MIN(salary) >
            (SELECT MIN(salary)
            FROM employees
            WHERE department_id = 50);
```

###  CASE中的子查询

题目：显式员工的employee_id,last_name和location。其中，若员工department_id与location_id为1800 的department_id相同，则location为’Canada’，其余则为’USA’。

```mysql
SELECT employee_id, last_name,
    (CASE department_id
    WHEN
        (SELECT department_id FROM departments
        WHERE location_id = 1800)
    THEN 'Canada' ELSE 'USA' END) location
FROM employees;
```

###  子查询中的空值问题

```mysql
SELECT last_name, job_id
FROM employees
WHERE job_id =
(SELECT job_id
FROM employees
WHERE last_name = 'Haas');
```

> 子查询不返回任何行

###  非法使用子查询

```mysql
SELECT employee_id, last_name
FROM employees
WHERE salary =
(SELECT MIN(salary)
FROM employees
GROUP BY department_id);
```

> 多行子查询使用单行比较符

##  多行子查询

* 也称为集合比较子查询
* 内查询返回多行
* 使用多行比较操作符

###  多行比较操作符

| 操作符 | 含义                                                     |
| ------ | -------------------------------------------------------- |
| IN     | 等于列表中的任意一个                                     |
| ANY    | 需要和单行比较操作符一起使用，和子查询返回的某一个值比较 |
| ALL    | 需要和单行比较操作符一起使用，和子查询返回的所有值比较   |
| SOME   | 实际上是ANY的别名，作用相同，一般常使用ANY               |

###  代码示例

* 题目：返回其它job_id中比job_id为‘IT_PROG’部门任一工资低的员工的员工号、姓名、job_id 以及salary

```mysql
SELECT employee_id, last_name, job_id, salary
FROM employees
WHERE job_id <> 'IT_PROG' 
AND salary < ANY(
	SELECT salary
    FROM emplyees
    WHERE job_id = 'IT_PROG'
);
```

* 题目：查询平均工资最低的部门id

```mysql
#方式1：
SELECT department_id
FROM employees
GROUP BY department_id
HAVING AVG(salary) = (
        SELECT MIN(avg_sal)
        FROM (
            SELECT AVG(salary) avg_sal
            FROM employees
            GROUP BY department_id
            ) dept_avg_sal
);
```

```mysql
#方式2：
SELECT department_id
FROM employees
GROUP BY department_id
HAVING AVG(salary) <= ALL (
        SELECT AVG(salary) avg_sal
        FROM employees
        GROUP BY department_id
);
```

###  空值问题

```mysql
SELECT last_name
FROM employees
WHERE employee_id NOT IN (
    SELECT manager_id
    FROM employees
    WHERE manager_id IS NOT NULL
);
```

## 相关子查询

如果子查询的执行依赖于外部查询，通常情况下都是因为子查询中的表用到了外部的表，并进行了条件 关联，因此每执行一次外部查询，子查询都要重新计算一次，这样的子查询就称之为 关联子查询 。 

相关子查询按照一行接一行的顺序执行，主查询的每一行都执行一次子查询。

![image-20221013182941366](D:\笔记\mysql.assets\image-20221013182941366.png)

> 说明：子查询中使用主查询中的列

### 代码示例

* 题目：查询员工中工资大于本部门平均工资的员工的last_name,salary和其department_id

```mysql
# 方式一：使用相关子查询
SELECT last_name, salary, department
FROM employees e1
WHERE salary > (
		SELECT AVG(salary)
    	FROM employees e2
    	WHERE department_id = e1.`department_id`
);
# 方式二：在FROM中声明子查询
SELECT e.last_name, e.salary, e.department_id
FROM employees e, (
    			SELECT department_id, AVG(salary) avg_sal
    			FROM employees
    			GROUP BY department_id) t_dept_avg_salary
WHERE e.department_id = t_dept_avg_salary.department_id
AND e.salary > t_dept_avg_salary.avg_sal;
```

在ORDER BY 中使用子查询：

* 查询员工的id,salary,按照department_name 排序

```mysql
SELECT employee_id, salary
FROM employees e
ORDER BY (
	SELECT department_name
    FROM departments d
    WHERE e.`department_id` = d.`department_id`
);
```

* 题目：若employees表中employee_id与job_history表中employee_id相同的数目不小于2，输出这些相同 id的员工的employee_id,last_name和其job_id

```mysql
SELECT e.employee_id, last_name,e.job_id
FROM employees e
WHERE 2 <= (SELECT COUNT(*)
        FROM job_history
        WHERE employee_id = e.employee_id
);
```

###  EXISTS 与 NOT EXISTS 关键字

* 关联子查询通常也会和 EXISTS操作符一起来使用，用来检查在子查询中是否存在满足条件的行。
* 如果在子查询中不存在满足条件的行：
	+ 条件返回 FALSE
	+ 继续在子查询中查找
* 如果在子查询中存在满足条件的行：
	+ 不在子查询中继续查找
	+ 条件返回 TRUE
* NOT EXISTS关键字表示如果不存在某种条件，则返回TRUE，否则返回FALSE。

题目：查询公司管理者的employee_id，last_name，job_id，department_id信息

```mysql
# 方式一：EXISTS
SELECT employee_id, last_name, job_id, department_id
FROM employees e1
WHERE EXISTS ( SELECT *
        FROM employees e2
        WHERE e2.manager_id =
        e1.employee_id
);

# 方式二：自连接
SELECT DISTINCT e1.employee_id, e1.last_name, e1.job_id, e1.department_id
FROM employees e1 JOIN employees e2
ON e1.employee_id = e2.manager_id;

# 方式三：IN
SELECT employee_id, last_name, job_id, department_id
WHERE employee_id IN (
        SELECT DISTINCT manager_id
        FROM employees
);
```

题目：查询departments表中，不存在于employees表中的部门的department_id和department_name

```mysql
# 方式一：
SELECT d.department_id, d.department_name
FROM departments e RIGHT JOIN departments d
ON e.`department_id` = d.`department_id`
WHERE e.`department_id` IS NULL;

# 方式二：
SELECT department_id, department_name
FROM departments d
WHERE NOT EXISTS (
	SELECT *
    FROM employees e
    WHERE d.`department_id` = e.`department_id`
);
```

###  相关更新

```mysql
UPDATE table1 alias1
SET column = (SELECT expression
FROM table2 alias2
WHERE alias1.column = alias2.column);
```

使用相关子查询依据一个表中的数据更新另一个表的数据。

题目：在employees中增加一个department_name字段，数据为员工对应的部门名称

```mysql
# 1）
ALTER TABLE employees
ADD(department_name VARCHAR2(14));

# 2）
UPDATE employees e
SET department_name = (SELECT department_name
FROM departments d
WHERE e.department_id = d.department_id);
```

###  相关删除

```mysql
DELETE FROM table1 alias1
WHERE column operator (SELECT expression
FROM table2 alias2
WHERE alias1.column = alias2.column);
```

使用相关子查询依据一个表中的数据删除另一个表的数据。

题目：删除表employees中，其与emp_history表皆有的数据

```mysql
DELETE FROM employees e
WHERE employee_id in(
    SELECT employee_id
    FROM emp_history
    WHERE employee_id = e.employee_id
);
```

##  思考题

问题：谁的工资比Abel的高？ 解答：

```mysql
#方式1：自连接
SELECT e2.last_name,e2.salary
FROM employees e1,employees e2
WHERE e1.last_name = 'Abel'
AND e1.`salary` < e2.`salary`;
```

```mysql
#方式2：子查询
SELECT last_name,salary
FROM employees
WHERE salary > (
    SELECT salary
    FROM employees
    WHERE last_name = 'Abel'
);
```

问题：以上两种方式有好坏之分吗？ 

解答：自连接方式好！ 

题目中可以使用子查询，也可以使用自连接。一般情况建议你使用自连接，因为在许多 DBMS 的处理过 程中，对于自连接的处理速度要比子查询快得多。 可以这样理解：子查询实际上是通过未知表进行查询后的条件判断，而自连接是通过已知的自身数据表 进行条件判断，因此在大部分 DBMS 中都对自连接处理进行了优化。

##  课后练习

1. 查询和Zlotkey相同部门的员工姓名和工资

```mysql
SELECT last_name, salary
FROM employees
WHERE department_id = (
	SELECT department_id
    FROM employees
    WHERE last_name = 'Zlotkey'
);
```

2. 查询工资比公司平均工资高的员工的员工号，姓名和工资。

```mysql
SELECT employee_id, last_name, salary
FROM employees
WHERE salary > (
	SELECT AVG(salary)
    FROM employee
);
```

3. 选择工资大于所有JOB_ID = 'SA_MAN' 的员工的工资的员工的last_name, job_id, salary

```mysql
SELECT last_name, job_id, salary
FROM employees
WHERE salary > ALL (
	SELECT salary
    FROM employees
    WHERE job_id = 'SA_MAN'
);
```

4. 查询和姓名中包含字母u的员工在相同部门的员工的员工号和姓名

```mysql
SELECT employee_id, last_name
FROM eployees
WHERE department_id IN (
    SELECT DISTINCT department_id
    FROM employees
    WHERE last_name LIKE '%u%'
);
```

5. 查询在部门的location_id为1700的部门工作的员工的员工号

```mysql
SELECT employee_id
FROM employees
WHERE department_id IN (
	SELECT department_id
    FROM departments
    WHERE location_id = 1700
);
```

6. 查询管理者是King的员工姓名和工资

```mysql
SELECT last_name, salary
FROM employees
WHERE manage_id IN (
	SELECT employee_id
    FROM employees
    WHERE last_name = 'King'
);
```

7. 查询工资最低的员工信息 (last_name, salary)

```mysql
SELECT last_name, salary
FROM employees
WHERE salary = (
	SELECT MIN(salary)
    FROM employees
);
```

8. 查询平均工资最低的部门信息

```mysql
# 方式一
SELECT *
FROM departments
WHERE department_id = (
	SELECT department_id
    FROM employees
    GROUP BY department_id
    HAVING AVG(salary) = (
    	SELECT MIN(avg_sal)
        FROM (
        	SELECT AVG(salary) avg_sal
            FROM employees
            GROUP BY department_id
        ) t_dept_avg_sal
    )
);

# 方式二
SELECT *
FROM departments
WHERE department_id = (
	SELECT department_id
    FROM employees
    GROUP BY department_id
    HAVING AVG(salary) <= ALL (
        SELECT AVG(salary) avg_sal
        FROM employees
        GROUP BY department_id
    )
);

# 方式三: LIMIT
SELECT *
FROM departments
WHERE department_id IN (
    SELECT department_id
    FROM employees
    GROUP BY department_id
    HAVING AVG(salary) = (
    	SELECT AVG(salary) avg_sal
        FROM employees
        GROUP BY department_id
        ORDER BY avg_sal ASC
        LIMIT 1
    )
);

# 方式四
SELECT d.*
FROM departments d, (
	SELECT department_id, AVG(salary) avg_sal
    FROM employees
    GROUP BY department_id
    ORDER BY avg_sal ASC
    LIMIT 0,1
) t_dept_avg_sal
WHERE d.`department_id` = t_dept_avg_sal.`department_id`;
```

9. 查询平均工资最低的部门信息和该部门的平均工资 (相关子查询)

```mysql
SELECT d.*, (SELECT AVG(salary) FROM employees WHERE department_id = d.`department_id`) avg_sal
FROM departments d, (
	SELECT department_id, AVG(salary) avg_sal
    FROM employees
    GROUP BY department_id
    ORDER BY avg_sal ASC
    LIMIT 0,1
) t_dept_avg_sal
WHERE d.`department_id` = t_dept_avg_sal.`department_id`;
```

10. 查询平均工资最高的job信息

```mysql
SELECT *
FROM jobs
WHERE job_id = (
	SELECT job_id
    FROM employees
    GROUP BY job_id
    HAVING AVG(salary) = (
    	SELECT MAX(avg_sal)
        FROM (
        	SELECT AVG(salary) avg_sal
            FROM employees
            GROUP BY job_id
        ) t_job_avg_sal
    )
);
```

11. 查询平均工资高于公司平均工资的部门有哪些？

```mysql
SELECT depatment_id
FROM employees
WHERE department_id IS NOT NULL
GROUP BY department_id
HAVING AVG(salary) > (
	SELECT AVG(salary)
    FROM eployees
);
```

12. 查询出公司中所有manager的详细信息

```mysql
# 方式1：自连接
SELECT DISTINCT *
FROM employees emp, employees manager
WHERE emp.`manager_id` = manager.`employee_id`;

SELECT DISTINCT *
FROM employees emp JOIN employees manager
ON emp.`manager_id` = manager.`employee_id`; 

# 方式2：子查询
SELECT *
FROM employees
WHERE employee_id IN (
	SELECT manager_id
    FROM employees
);

# 方式3：EXISTS
SELECT *
FROM employees manager
WHERE EXISTS (
	SELECT *
    FROM employees emp
    WHERE manager.`employee_id` = emp.`manager_id`
);
```

13. 各个部门中，最高工资中最低的那个部门的最低工资是多少？

```mysql
# 方式一：
SELECT MIN(salary)
FROM employees
WHERE department_id = (
    SELECT department_id
    FROM employees
	GROUP BY department_id
	HAVING MAX(salary) = (
    	SELECT MIN(max_sal)
        FROM (
        	SELECT MAX(salary) max_sal
            FROM employees
            GROUP BY department_id
        ) t_dept_max_sal
    ) 
);

# 方式二：
SELECT MIN(salary)
FROM employees
WHERE department_id = (
    SELECT department_id
    FROM employees
	GROUP BY department_id
	HAVING MAX(salary) <= ALL (
        SELECT MAX(salary)
        FROM employees
        GROUP BY department_id
    ) 
);

# 方式三：
SELECT MIN(salary)
FROM employees
WHERE department_id = (
    SELECT department_id
    FROM employees
	GROUP BY department_id
	HAVING MAX(salary) = (
        SELECT MAX(salary) max_sal
        FROM employees
        GROUP BY department_id
        ORDER BY max_sal ASC
        LIMIT 0,1
    ) 
);

# 方式四：
FROM employees e, (
	SELECT department_id, MAX(salary) max_sal
    FROM employees
    GROUP BY department_id
    ORDER BY max_sal ASC
    LIMIT 0,1
) t_dept_max_sal
WHERE e.`department_id` = t_dept_max_sal.`department_id`;
```

14. 查询平均工资最高的部门的manager的详细信息：last_name, department_id, email, salary

```mysql
SELECT last_name, department_id, email, salary
FROM employees
WHERE employee_id IN (
	SELECT DISTINCT manager_id
    FROM employees
    WHERE department_id = (
    	SELECT department_id
        FROM employees
        GROUP BY department_id
        HAVING AVG(salary) = (
        	SELECT MAX(avg_sal)
            FROM (
            	SELECT AVG(salary) avg_sal
                FROM employees
                GROUP BY department_id
            ) t_dept_avg_sal
        )
    )
);

SELECT last_name, department_id, email, salary
FROM employees
WHERE employee_id IN (
    SELECT DISTINCT manager_id
    FROM employees e, (
        SELECT department_id, AVG(salary) avg_sal
        FROM employees
        GROUP BY department_id
        ORDER BY avg_sal DESC
        LIMIT 0,1
    ) t_dept_avg_sal
    WHERE e.`department_id` = t_dept_avg_sal.`department_id`
);
```

15. 查询部门的部门号，其中不包括job_id是"ST_CLERK"的部门号

```mysql
SELECT department_id
FROM departments
WHERE department_id NOT IN (
	SELECT DISTINCT department_id
    FROM employees
    WHERE job_id = `ST_CLERK`
);

SELECT department_id
FROM department d
WHERE NOT EXISTS (
	SELECT *
    FROM employees e
    WHERE d.`department_id` = e.`department_id`
    AND e.`job_id` = 'ST_CLERK'
);
```

16. 选择所有没有管理者的员工的last_name

```mysql
SELECT last_name
FROM employees emp
WHERE NOT EXISTS (
	SELECT *
    FROM employees mgr
    WHERE emp.`manager_id` = mgr.`employee_id`
);
```

17. 查询员工号、姓名、雇用时间、工资，其中员工的管理者为 ‘De Haan'

```mysql
SELECT employee_id, last_name, hire_date, salary
FROM employee
WHERE manager_id IN (
	SELECT manager_id
    FROM employee
    WHERE last_name = 'De Haan'
);
```

18. 查询各部门中工资比本部门平均工资高的员工的员工号，姓名和工资（相关子查询）

```mysql
SELECT department_id, last_name, salary
FROM employees e1
WHERE salary > (
	SELECT AVG(salary)
    FROM employees e2
    WHERE e2.`department_id` = e1.`department_id`
);

SELECT e.last_name, e.salary, e.department_id
FROM employees e, (
	SELECT department_id, AVG(salary) avg_sal
    FROM employees
    GROUP BY department_id
) t_dept_avg_sal
WHERE e.`department_id` = t_dept_avg_sal.`department_id`
AND e.`salary` > t_dept_avg_sal.`avg_sal`;
```

19. 查询每个部门下的部门人数大于5的部门名称（相关子查询）

```mysql
SELECT department_name
FROM departments d
WHERE 5 < (
	SELECT COUNT(*)
    FROM employees e
    WHERE d.`department_id` = e.`department_id`
);
```

20. 查询每个国家下的部门个数大于2的国家编号（相关子查询）

```mysql
SELECT country_id
FROM locations l
WHERE 2 < (
	SELECT COUNT(*)
    FROM department d
    WHERE l.`location_id` = d.`location_id`
);
```

# 数据库操作

## 命名规则

![image-20221015201655869](D:\笔记\mysql.assets\image-20221015201655869.png)

## 创建数据库

![image-20221015202046085](D:\笔记\mysql.assets\image-20221015202046085.png)

## 管理数据库

![image-20221015202724399](D:\笔记\mysql.assets\image-20221015202724399.png)

![image-20221015202739212](D:\笔记\mysql.assets\image-20221015202739212.png)

![image-20221015202918911](D:\笔记\mysql.assets\image-20221015202918911.png)

## 数据类型概述

![image-20221015203153232](D:\笔记\mysql.assets\image-20221015203153232.png)

| 类型             | 数据变量                                                     |
| ---------------- | ------------------------------------------------------------ |
| 整数类型         | TINYINT、SMALLINT、MEDIUMINT、INT(或INTEGER)、BIGINT         |
| 浮点类型         | FLOAT、DOUBLE                                                |
| 定点数类型       | DECIMAL                                                      |
| 位类型           | BIT                                                          |
| 日期时间类型     | YEAR、TIME、DATE、DATETIME、TIMESTAMP                        |
| 文本字符串类型   | CHAR、VARCHAR、TINYTEXT、TEXT、MEDIUMTEXT、LONGTEXT          |
| 枚举类型         | ENUM                                                         |
| 集合类型         | SET                                                          |
| 二进制字符串类型 | BINARY、VARBINARY、TINYBLOB、BLOB、MEDIUMBLOB、LONGBLOB      |
| JSON类型         | JSON对象、JSON数组                                           |
| 空间数据类型     | 单值：GEOMETRY、POINT、LINESTRING、POLYGON； 集合：MULTIPOINT、MULTILINESTRING、MULTIPOLYGON、 GEOMETRYCOLLECTION |

其中，常用的几类类型介绍如下：

| 数据类型      | 描述                                                    |
| ------------- | ------------------------------------------------------- |
| INT           | 从-2^31到2^31-1的整型数据。存储大小为 4个字节           |
| CHAR(size)    | FLOAT、DOUBLE                                           |
| VARCHAR(size) | DECIMAL                                                 |
| FLOAT(M,D)    | BIT                                                     |
| DOUBLE(M,D)   | YEAR、TIME、DATE、DATETIME、TIMESTAMP                   |
| DECIMAL(M,D)  | CHAR、VARCHAR、TINYTEXT、TEXT、MEDIUMTEXT、LONGTEXT     |
| DATE          | ENUM                                                    |
| BLOB          | SET                                                     |
| TEXT          | BINARY、VARBINARY、TINYBLOB、BLOB、MEDIUMBLOB、LONGBLOB |

## 管理表

###  创建方式1

* 语法格式：

```mysql
CREATE TABLE [IF NOT EXISTS] 表名(
字段1, 数据类型 [约束条件] [默认值],
字段2, 数据类型 [约束条件] [默认值],
字段3, 数据类型 [约束条件] [默认值],
……
[表约束条件]
);
```

> 加上了IF NOT EXISTS关键字，则表示：如果当前数据库中不存在要创建的数据表，则创建数据表； 如果当前数据库中已经存在要创建的数据表，则忽略建表语句，不再创建数据表。

###  创建方式2

* 使用 AS subquery 选项，将创建表和插入数据结合起来

```mysql
CREATE TABLE 表名
	[(column, column, ...)]
AS subquery;
```

* 指定的列和子查询中的列要一一对应
* 通过列名和默认值定义列

```mysql
CREATE TABLE dept80
AS
SELECT employee_id, last_name, salary*12 ANNSAL, hire_date
FROM employees
WHERE department_id = 80;
```

###  查看数据表结构

在MySQL中创建好数据表之后，可以查看数据表的结构。MySQL支持使用 DESCRIBE/DESC 语句查看数据 表结构，也支持使用 SHOW CREATE TABLE 语句查看数据表结构。

语法格式如下：

```mysql
SHOW CREATE TABLE 表名\G
```

使用SHOW CREATE TABLE语句不仅可以查看表创建时的详细语句，还可以查看存储引擎和字符编码。

### 修改表

修改表指的是修改数据库中已经存在的数据表的结构。

使用 ALTER TABLE 语句可以实现：

+ 向已有的表中添加列
+ 修改现有表中的列
+ 删除现有表中的列
+ 重命名现有表中的列

#### 追加一个列

语法格式如下：

```mysql
ALTER TABLE 表名 ADD 【COLUMN】 字段名 字段类型 【FIRST|AFTER 字段名】;
```

举例：

```mysql
ALTER TABLE dept80
ADD job_id varchar(15);
```

####  修改一个列

* 可以修改列的数据类型，长度、默认值和位置 
* 修改字段数据类型、长度、默认值、位置的语法格式如下：

```mysql
ALTER TABLE 表名 MODIFY 【COLUMN】 字段名1 字段类型 【DEFAULT 默认值】【FIRST|AFTER 字段名2】;
```

* 举例：

```mysql
ALTER TABLE dept80
MODIFY salary double(9,2) default 1000;
```

* 对默认值的修改只影响今后对表的修改
* 此外，还可以通过此种方式修改列的约束。

####  重命名一个列

使用 CHANGE old_column new_column dataType子句重命名列。语法格式如下：

```mysql
ALTER TABLE 表名 CHANGE 【column】 列名 新列名 新数据类型;
```

举例：

```mysql
ALTER TABLE dept80
CHANGE department_name dept_name varchar(15);
```

####  删除一个列

删除表中某个字段的语法格式如下：

```mysql
ALTER TABLE 表名 DROP 【COLUMN】字段名
```

####  更改表名

* 方式一：使用RENAME

```mysql
RENAME TABLE emp
TO myemp;
```

* 方式二：

```mysql
ALTER table dept
RENAME [TO] detail_dept; -- [TO]可以省略
```

* 必须是对象的拥有者

### 删除表

* 在MySQL中，当一张数据表 没有与其他任何数据表形成关联关系 时，可以将当前数据表直接删除。 
* 数据和结构都被删除 
* 所有正在运行的相关事务被提交 
* 所有相关索引被删除 
* 语法格式：

```mysql
DROP TABLE [IF EXISTS] 数据表1 [, 数据表2, …, 数据表n];
```

IF EXISTS 的含义为：如果当前数据库中存在相应的数据表，则删除数据表；如果当前数据库中不存 在相应的数据表，则忽略删除语句，不再执行删除数据表的操作。

举例：

```mysql
DROP TABLE dept80;
```

* DROP TABLE 语句不能回滚

###  清空表

* TRUNCATE TABLE语句：
	* 删除表中所有的数据
	* 释放表的存储空间
* 举例：

```mysql
TRUNCATE TABLE detail_dept;
```

* TRUNCATE语句不能回滚，而使用 DELETE 语句删除数据，可以回滚

> 阿里开发规范： 【参考】TRUNCATE TABLE 比 DELETE 速度快，且使用的系统和事务日志资源少，但 TRUNCATE 无 事务且不触发 TRIGGER，有可能造成事故，故不建议在开发代码中使用此语句。 说明：TRUNCATE TABLE 在功能上与不带 WHERE 子句的 DELETE 语句相同。



# 数据操作

##  插入数据

### 方式1：VALUES的方式添加

使用这种语法一次只能向表中插入一条数据。

**情况1：为表的所有字段按默认顺序插入数据**

```mysql
INSERT INTO 表名
VALUES (value1,value2,....);
```

值列表中需要为表的每一个字段指定值，并且值的顺序必须和数据表中字段定义时的顺序相同。

举例：

```mysql
INSERT INTO departments
VALUES (70, 'Pub', 100, 1700);
```

**情况2: 指定字段名插入数据**

为表的指定字段插入数据，就是在INSERT语句中只向部分字段中插入值，而其他字段的值为表定义时的 默认值。 在 INSERT 子句中随意列出列名，但是一旦列出，VALUES中要插入的value1,....valuen需要与 column1,...columnn列一一对应。如果类型不同，将无法插入，并且MySQL会产生错误。 

举例：

```mysql
INSERT INTO departments(department_id, department_name)
VALUES (80, 'IT');
```

**情况3：同时插入多条记录**

INSERT语句可以同时向数据表中插入多条记录，插入时指定多个值列表，每个值列表之间用逗号分隔 开，基本语法格式如下：

```mysql
INSERT INTO table_name
VALUES
(value1 [,value2, …, valuen]),
(value1 [,value2, …, valuen]),
……
(value1 [,value2, …, valuen]);
```

或者

```mysql
INSERT INTO table_name(column1 [, column2, …, columnn])
VALUES
(value1 [,value2, …, valuen]),
(value1 [,value2, …, valuen]),
……
(value1 [,value2, …, valuen]);
```

使用INSERT同时插入多条记录时，MySQL会返回一些在执行单行插入时没有的额外信息，这些信息的含 义如下：

* Records：表明插入的记录条数。 
* Duplicates：表明插入时被忽略的记录，原因可能是这 些记录包含了重复的主键值。 
* Warnings：表明有问题的数据值，例如发生数据类型转换。

> 一个同时插入多行记录的INSERT语句等同于多个单行插入的INSERT语句，但是多行的INSERT语句 在处理过程中 效率更高 。因为MySQL执行单条INSERT语句插入多行数据比使用多条INSERT语句 快，所以在插入多条记录时最好选择使用单条INSERT语句的方式插入。

### 方式2：将查询结果插入到表中

INSERT还可以将SELECT语句查询的结果插入到表中，此时不需要把每一条记录的值一个一个输入，只需要使用一条INSERT语句和一条SELECT语句组成的组合语句即可快速地从一个或多个表中向一个表中插入多行

```mysql
INSET INTO 目标表名
(tar_column1 [, tar_column2, ..., tar_columnn])
SELECT
(src_column1 [, src_column2, …, src_columnn])
FROM 源表名
[WHERE condition]
```

* 在 INSERT 语句中加入子查询。 
* 不必书写 VALUES 子句。 
* 子查询中的值列表应与 INSERT 子句中的列名对应。

```mysql
INSERT INTO emp2
SELECT *
FROM employees
WHERE department_id = 90;
```

```mysql
INSERT INTO sales_reps(id, name, salary, commission_pct)
SELECT employee_id, last_name, salary, commission_pct
FROM employees
WHERE job_id LIKE '%REP%';
```

##  更新数据

* 使用 UPDATE 语句更新数据。语法如下：

```mysql
UPDATE table_name
SET column1=value1, column2=value2, ..., column=valuen
[WHERE condition]
```

* 可以一次更新多条数据。
* 如果需要回滚数据，需要保证在DML前，进行设置：SET AUTOCOMMIT = FALSE;

* 使用 WHERE 子句指定需要更新的数据。

```mysql
UPDATE employees
SET department_id = 70
WHERE employee_id = 113;
```

* 如果省略 WHERE 子句，则表中的所有数据都将被更新。

##  删除数据

```mysql
DELETE FROM table_name [WHERE <condition>];
```

table_name指定要执行删除操作的表；“[WHERE ]”为可选参数，指定删除条件，如果没有WHERE子句， DELETE语句将删除表中的所有记录。

##  MySQL8新特性：计算列

什么叫计算列呢？简单来说就是某一列的值是通过别的列计算得来的。例如，a列值为1、b列值为2，c列 不需要手动插入，定义a+b的结果为c的值，那么c就是计算列，是通过别的列计算得来的。

在MySQL 8.0中，CREATE TABLE 和 ALTER TABLE 中都支持增加计算列。下面以CREATE TABLE为例进行讲解。

举例：定义数据表tb1，然后定义字段id、字段a、字段b和字段c，其中字段c为计算列，用于计算a+b的 值。 首先创建测试表tb1，语句如下：

```mysql
CREATE TABLE tb1(
id INT,
a INT,
b INT,
c INT GENERATED ALWAYS AS (a + b) VIRTUAL
);
```

# 数据类型

##  MySQL中的数据类型

| 类型             | 举例                                                         |
| ---------------- | ------------------------------------------------------------ |
| 整数类型         | TINYINT、SMALLINT、MEDIUMINT、INT(或INTEGER)、BIGINT         |
| 浮点类型         | FLOAT、DOUBLE                                                |
| 定点数类型       | DECIMAL                                                      |
| 位类型           | BIT                                                          |
| 日期时间类型     | YEAR、TIME、DATE、DATETIME、TIMESTAMP                        |
| 文本字符串类型   | CHAR、VARCHAR、TINYTEXT、TEXT、MEDIUMTEXT、LONGTEXT          |
| 枚举类型         | ENUM                                                         |
| 集合类型         | SET                                                          |
| 二进制字符串类型 | BINARY、VARBINARY、TINYBLOB、BLOB、MEDIUMBLOB、LONGBLOB      |
| JSON类型         | JSON对象、JSON数组                                           |
| 空间数据类型     | 单值类型：GEOMETRY、POINT、LINESTRING、POLYGON； 集合类型：MULTIPOINT、MULTILINESTRING、MULTIPOLYGON、 GEOMETRYCOLLECTION |

常见数据类型的属性，如下：

| MySQL关键字        | 含义                                                 |
| ------------------ | ---------------------------------------------------- |
| NULL               | TINYINT、SMALLINT、MEDIUMINT、INT(或INTEGER)、BIGINT |
| NOT NULL           | FLOAT、DOUBLE                                        |
| DEFAULT            | DECIMAL                                              |
| PRIMARY KEY        | BIT                                                  |
| AUTO_INCREMENT     | YEAR、TIME、DATE、DATETIME、TIMESTAMP                |
| UNSIGNED           | CHAR、VARCHAR、TINYTEXT、TEXT、MEDIUMTEXT、LONGTEXT  |
| CHARACTER SET name | ENUM                                                 |

##  整数类型

### 类型介绍

整数类型一共有 5 种，包括 TINYINT、SMALLINT、MEDIUMINT、INT（INTEGER）和 BIGINT。 

它们的区别如下表所示：

| 整数类型     | 字节 | 有符号数取值范围                         | 无符号数取值范围       |
| ------------ | ---- | ---------------------------------------- | ---------------------- |
| TINYINT      | 1    | -128~127                                 | 0~255                  |
| SMALLINT     | 2    | -32768~32767                             | 0~65535                |
| MEDIUMINT    | 3    | -8388608~8388607                         | 0~16777215             |
| INT、INTEGER | 4    | -2147483648~2147483647                   | 0~4294967295           |
| BIGINT       | 8    | -9223372036854775808~9223372036854775807 | 0~18446744073709551615 |

### 可选属性

整数类型的可选属性有三个：

* M

M : 表示显示宽度，M的取值范围是(0, 255)。例如，int(5)：当数据宽度小于5位的时候在数字前面需要用 字符填满宽度。该项功能需要配合“ ==ZEROFILL== ”使用，表示用“0”填满宽度，否则指定显示宽度无效。 如果设置了显示宽度，那么插入的数据宽度超过显示宽度限制，会不会截断或插入失败？ 

答案：不会对插入的数据有任何影响，还是按照类型的实际宽度进行保存，即 显示宽度与类型可以存储的 值范围无关 。从MySQL 8.0.17开始，整数数据类型不推荐使用显示宽度属性。 整型数据类型可以在定义表结构时指定所需要的显示宽度，如果不指定，则系统为每一种类型指定默认 的宽度值。

举例：

```mysql
CREATE TABLE test_int1 ( x TINYINT, y SMALLINT, z MEDIUMINT, m INT, n BIGINT );
```

查看表结构 （MySQL5.7中显式如下，MySQL8中不再显式范围）

```mysql
mysql> desc test_int1;
+-------+--------------+------+-----+---------+-------+
| Field | Type | Null | Key | Default | Extra |
+-------+--------------+------+-----+---------+-------+
| x | tinyint(4) | YES | | NULL | |
| y | smallint(6) | YES | | NULL | |
| z | mediumint(9) | YES | | NULL | |
| m | int(11) | YES | | NULL | |
| n | bigint(20) | YES | | NULL | |
+-------+--------------+------+-----+---------+-------+
5 rows in set (0.00 sec)
```

TINYINT有符号数和无符号数的取值范围分别为-128\~127和0\~255，由于负号占了一个数字位，因此 TINYINT默认的显示宽度为4。同理，其他整数类型的默认显示宽度与其有符号数的最小值的宽度相同。

* UNSIGNED

UNSIGNED : 无符号类型（非负），所有的整数类型都有一个可选的属性UNSIGNED（无符号属性），无 符号整数类型的最小取值为0。所以，如果需要在MySQL数据库中保存非负整数值时，可以将整数类型设 置为无符号类型。 int类型默认显示宽度为int(11)，无符号int类型默认显示宽度为int(10)。

* ZEROFILL

ZEROFILL : 0填充,（如果某列是ZEROFILL，那么MySQL会自动为当前列添加UNSIGNED属性），如果指 定了ZEROFILL只是表示不够M位时，用0在左边填充，如果超过M位，只要不超过数据存储范围即可。 

原来，在 int(M) 中，M 的值跟 int(M) 所占多少存储空间并无任何关系。 int(3)、int(4)、int(8) 在磁盘上都 是占用 4 bytes 的存储空间。也就是说，int(M)，必须和UNSIGNED ZEROFILL一起使用才有意义。如果整 数值超过M位，就按照实际位数存储。只是无须再用字符 0 进行填充。

### 适用场景

TINYINT ：一般用于枚举数据，比如系统设定取值范围很小且固定的场景。 

SMALLINT ：可以用于较小范围的统计数据，比如统计工厂的固定资产库存数量等。 

MEDIUMINT ：用于较大整数的计算，比如车站每日的客流量等。 

INT、INTEGER ：取值范围足够大，一般情况下不用考虑超限问题，用得最多。比如商品编号。 

BIGINT ：只有当你处理特别巨大的整数时才会用到。比如双十一的交易量、大型门户网站点击量、证 券公司衍生产品持仓等。

### 如何选择？

在评估用哪种整数类型的时候，你需要考虑 存储空间 和 可靠性 的平衡问题：一方 面，用占用字节数少 的整数类型可以节省存储空间；另一方面，要是为了节省存储空间， 使用的整数类型取值范围太小，一 旦遇到超出取值范围的情况，就可能引起 系统错误 ，影响可靠性。 

举个例子，商品编号采用的数据类型是 INT。原因就在于，客户门店中流通的商品种类较多，而且，每 天都有旧商品下架，新商品上架，这样不断迭代，日积月累。 

如果使用 SMALLINT 类型，虽然占用字节数比 INT 类型的整数少，但是却不能保证数据不会超出范围 65535。相反，使用 INT，就能确保有足够大的取值范围，不用担心数据超出范围影响可靠性的问题。 

你要注意的是，在实际工作中，系统故障产生的成本远远超过增加几个字段存储空间所产生的成本。因 此，我建议你首先确保数据不会超过取值范围，在这个前提之下，再去考虑如何节省存储空间。

##  浮点类型

###  类型介绍

浮点数和定点数类型的特点是可以 处理小数 ，你可以把整数看成小数的一个特例。因此，浮点数和定点 数的使用场景，比整数大多了。 MySQL支持的浮点数类型，分别是 FLOAT、DOUBLE、REAL。

* FLOAT 表示单精度浮点数； 

* DOUBLE 表示双精度浮点数；

* REAL默认就是 DOUBLE。如果你把 SQL 模式设定为启用“ REAL_AS_FLOAT ”，那 么，MySQL 就认为 REAL 是 FLOAT。如果要启用“REAL_AS_FLOAT”，可以通过以下 SQL 语句实现：

	```mysql
	SET sql_mode = “REAL_AS_FLOAT”;
	```

**问题：为什么浮点数类型的无符号数取值范围，只相当于有符号数取值范围的一半，也就是只相当于 有符号数取值范围大于等于零的部分呢？**

MySQL 存储浮点数的格式为： 符号(S) 、 尾数(M) 和 阶码(E) 。因此，无论有没有符号，MySQL 的浮 点数都会存储表示符号的部分。因此， 所谓的无符号数取值范围，其实就是有符号数取值范围大于等于 零的部分。

###  数据精度说明

对于浮点类型，在MySQL中单精度值使用 4 个字节，双精度值使用 8 个字节。

* MySQL允许使用 非标准语法 （其他数据库未必支持，因此如果涉及到数据迁移，则最好不要这么 用）： FLOAT(M,D) 或 DOUBLE(M,D) 。这里，M称为 精度 ，D称为 标度 。(M,D)中 M=整数位+小数 位，D=小数位。 D<=M<=255，0<=D<=30。 

	例如，定义为FLOAT(5,2)的一个列可以显示为-999.99-999.99。如果超过这个范围会报错。

* FLOAT和DOUBLE类型在不指定(M,D)时，默认会按照实际的精度（由实际的硬件和操作系统决定） 来显示。

* 说明：浮点类型，也可以加 UNSIGNED ，但是不会改变数据范围，例如：FLOAT(3,2) UNSIGNED仍然 只能表示0-9.99的范围。

* 不管是否显式设置了精度(M,D)，这里MySQL的处理方案如下：

	* 如果存储时，整数部分超出了范围，MySQL就会报错，不允许存这样的值
	* 如果存储时，小数点部分若超出范围，就分以下情况：
		+ 若四舍五入后，整数部分没有超出范围，则只警告，但能成功操作并四舍五入删除多余 的小数位后保存。例如在FLOAT(5,2)列内插入999.009，近似结果是999.01。
		+ 若四舍五入后，整数部分超出范围，则MySQL报错，并拒绝处理。如FLOAT(5,2)列内插入 999.995和-999.995都会报错。

* 从MySQL 8.0.17开始，FLOAT(M,D) 和DOUBLE(M,D)用法在官方文档中已经明确不推荐使用，将来可 能被移除。另外，关于浮点型FLOAT和DOUBLE的UNSIGNED也不推荐使用了，将来也可能被移除。

###  精度误差说明

浮点数类型有个缺陷，就是不精准。下面我来重点解释一下为什么 MySQL 的浮点数不够精准。比如，我们设计一个表，有f1这个字段，插入值分别为0.47,0.44,0.19，我们期待的运行结果是：0.47 + 0.44 + 0.19 = 1.1。而使用sum之后查询：

```mysql
CREATE TABLE test_double2(
f1 DOUBLE
);
INSERT INTO test_double2
VALUES(0.47),(0.44),(0.19);
```

```mysql
mysql> SELECT SUM(f1)
-> FROM test_double2;
+--------------------+
| SUM(f1) |
+--------------------+
| 1.0999999999999999 |
+--------------------+
1 row in set (0.00 sec)
```

查询结果是 1.0999999999999999。看到了吗？虽然误差很小，但确实有误差。 你也可以尝试把数据类型 改成 FLOAT，然后运行求和查询，得到的是， 1.0999999940395355。显然，误差更大了。

那么，为什么会存在这样的误差呢？问题还是出在 MySQL 对浮点类型数据的存储方式上。

MySQL 用 4 个字节存储 FLOAT 类型数据，用 8 个字节来存储 DOUBLE 类型数据。无论哪个，都是采用二 进制的方式来进行存储的。比如 9.625，用二进制来表达，就是 1001.101，或者表达成 1.001101×2^3。如 果尾数不是 0 或 5（比如 9.624），你就无法用一个二进制数来精确表达。进而，就只好在取值允许的范 围内进行四舍五入。

在编程中，如果用到浮点数，要特别注意误差问题，因为浮点数是不准确的，所以我们要避免使用“=”来 判断两个数是否相等。同时，在一些对精确度要求较高的项目中，千万不要使用浮点数，不然会导致结 果错误，甚至是造成不可挽回的损失。那么，MySQL 有没有精准的数据类型呢？当然有，这就是定点数 类型： DECIMAL 。

##  定点数类型

###  类型介绍

* MySQL中的定点数类型只有 DECIMAL 一种类型。

| 类型                     | 字节    | 有符号数取值范围   |
| ------------------------ | ------- | ------------------ |
| DECIMAL(M,D),DEC,NUMERIC | M+2字节 | 有效范围由M和D决定 |

使用 DECIMAL(M,D) 的方式表示高精度小数。其中，M被称为精度，D被称为标度。0<=M<=65， 0<=D<=30，D

* DECIMAL(M,D)的最大取值范围与DOUBLE类型一样，但是有效的数据范围是由M和D决定的。 DECIMAL 的存储空间并不是固定的，由精度值M决定，总共占用的存储空间为M+2个字节。也就是 说，在一些对精度要求不高的场景下，比起占用同样字节长度的定点数，浮点数表达的数值范围可 以更大一些。
* 定点数在MySQL内部是以 字符串 的形式进行存储，这就决定了它一定是精准的。
* 当DECIMAL类型不指定精度和标度时，其默认为DECIMAL(10,0)。当数据的精度超出了定点数类型的 精度范围时，则MySQL同样会进行四舍五入处理。
* 浮点数 vs 定点数
	* 浮点数相对于定点数的优点是在长度一定的情况下，浮点类型取值范围大，但是不精准，适用 于需要取值范围大，又可以容忍微小误差的科学计算场景（比如计算化学、分子建模、流体动 力学等）
	* 定点数类型取值范围相对小，但是精准，没有误差，适合于对精度要求极高的场景 （比如涉 及金额计算的场景）

###  开发中的经验

“由于 DECIMAL 数据类型的精准性，在我们的项目中，除了极少数（比如商品编号）用到整数类型外，其他的数值都用的是 DECIMAL，原因就是这个项目所处的零售行业，要求精准，一分钱也不能差。 ” ——来自某项目经理

## 位类型：BIT

BIT类型中存储的是二进制值，类似010110。

| 二进制字符串类型 | 长度 | 长度范围     | 占用空间            |
| ---------------- | ---- | ------------ | ------------------- |
| BIT(M)           | M    | 1 <= M <= 64 | 约为(M + 7)/8个字节 |

BIT类型，如果没有指定(M)，默认是1位。这个1位，表示只能存1位的二进制值。这里(M)是表示二进制的 位数，位数最小值为1，最大值为64。

## 日期与时间类型

日期与时间是重要的信息，在我们的系统中，几乎所有的数据表都用得到。原因是客户需要知道数据的 时间标签，从而进行数据查询、统计和处理。

MySQL有多种表示日期和时间的数据类型，不同的版本可能有所差异，MySQL8.0版本支持的日期和时间 类型主要有：YEAR类型、TIME类型、DATE类型、DATETIME类型和TIMESTAMP类型。

* YEAR 类型通常用来表示年 
* DATE 类型通常用来表示年、月、日 
* TIME 类型通常用来表示时、分、秒 
* DATETIME 类型通常用来表示年、月、日、时、分、秒 
* TIMESTAMP 类型通常用来表示带时区的年、月、日、时、分、秒

| 类型      | 名称     | 字节 | 日期格式            | 最小值                  | 最大值                 |
| --------- | -------- | ---- | ------------------- | ----------------------- | ---------------------- |
| YEAR      | 年       | 1    | YYYY或YY            | 1901                    | 2155                   |
| TIME      | 时间     | 3    | HH:MM:SS            | -838:59:59              | 838:59:59              |
| DATE      | 日期     | 3    | YYYY-MM-DD          | 1000-01-01              | 9999-12-03             |
| DATETIME  | 日期时间 | 8    | YYYY-MM-DD HH:MM:SS | 1000-01-01 00:00:00     | 9999-12-31 23:59:59    |
| TIMESTAMP | 日期时间 | 4    | YYYY-MM-DD HH:MM:SS | 1970-01-01 00:00:00 UTC | 2038-01-19 03:14:07UTC |

可以看到，不同数据类型表示的时间内容不同、取值范围不同，而且占用的字节数也不一样，你要根据 实际需要灵活选取。

为什么时间类型 TIME 的取值范围不是 -23:59:59～23:59:59 呢？原因是 MySQL 设计的 TIME 类型，不光表 示一天之内的时间，而且可以用来表示一个时间间隔，这个时间间隔可以超过 24 小时。

##  文本字符串类型

MySQL中，文本字符串总体上分为 CHAR 、 VARCHAR 、 TINYTEXT 、 TEXT 、 MEDIUMTEXT 、 LONGTEXT 、 ENUM 、 SET 等类型。

##  ENUM类型

ENUM类型也叫作枚举类型，ENUM类型的取值范围需要在定义字段时进行指定。设置字段值时，ENUM 类型只允许从成员中选取单个值，不能一次选取多个值。 其所需要的存储空间由定义ENUM类型时指定的成员个数决定。

| 文本字符串类型 | 长度 | 长度范围        | 占用的存储空间 |
| -------------- | ---- | --------------- | -------------- |
| ENUM           | L    | 1 <= L <= 65535 | 1或2个字节     |

* 当ENUM类型包含1～255个成员时，需要1个字节的存储空间； 
* 当ENUM类型包含256～65535个成员时，需要2个字节的存储空间。 
* ENUM类型的成员个数的上限为65535个。

##  SET类型

当SET类型包含的成员个数不同时，其所占用的存储空间也是不同的，具体如下：

| 成员个数范围（L表示实际成员个数） | 占用的存储空间 |
| --------------------------------- | -------------- |
| 1 <= L <= 8                       | 1个字节        |
| 9 <= L <= 16                      | 2个字节        |
| 17 <= L <= 24                     | 3个字节        |
| 25 <= L <= 32                     | 4个字节        |
| 33 <= L <= 64                     | 8个字节        |

SET类型在存储数据时成员个数越多，其占用的存储空间越大。注意：SET类型在选取成员时，可以一次 选择多个成员，这一点与ENUM类型不同。

## 小结及选择建议

在定义数据类型时，如果确定是 整数 ，就用 INT ； 如果是 小数 ，一定用定点数类型 DECIMAL(M,D) ； 如果是日期与时间，就用 DATETIME 。 这样做的好处是，首先确保你的系统不会因为数据类型定义出错。不过，凡事都是有两面的，可靠性 好，并不意味着高效。比如，TEXT 虽然使用方便，但是效率不如 CHAR(M) 和 VARCHAR(M)。

**阿里巴巴《Java开发手册》之MySQL数据库：**

* 任何字段如果为非负数，必须是 UNSIGNED 

* 【 强制 】小数类型为 DECIMAL，禁止使用 FLOAT 和 DOUBLE。 

	说明：在存储的时候，FLOAT 和 DOUBLE 都存在精度损失的问题，很可能在比较值的时候，得到不正确的结果。如果存储的数据范围超过 DECIMAL 的范围，建议将数据拆成整数和小数并分开存储。 

* 【 强制 】如果存储的字符串长度几乎相等，使用 CHAR 定长字符串类型。

* 【 强制 】VARCHAR 是可变长字符串，不预先分配存储空间，长度不要超过 5000。如果存储长度大于此值，定义字段类型为 TEXT，独立出来一张表，用主键来对应，避免影响其它字段索引效率。

# 约束

##  约束的分类

* 根据约束数据列的限制，约束可分为：
	+ 单列约束：每个约束只约束一列
	+ 多列约束：每个约束可约束多列数据
* 根据约束的作用范围，约束可分为：
	+ 列级约束：只能作用在一个列上，跟在列的定义后面
	+ 表级约束：可以作用在多个列上，不与列一起，而是单独定义
* 根据约束起的作用，约束可分为：
	+ NOT NULL 非空约束，规定某个字段不能为空 
	+ UNIQUE 唯一约束，规定某个字段在整个表中是唯一的 
	+ PRIMARY KEY 主键(非空且唯一)约束 
	+ FOREIGN KEY 外键约束 
	+ CHECK 检查约束 
	+ DEFAULT 默认值约束

> 注意： MySQL不支持check约束，但可以使用check约束，而没有任何效果 

* 如何添加/ 删除约束？

CREATE TABLE时添加约束

ALTER TABLE时增加约束、删除约束

* 查看某个表已有的约束

```mysql
#information_schema数据库名（系统库）
#table_constraints表名称（专门存储各个表的约束）
SELECT * FROM information_schema.table_constraints
WHERE table_name = '表名称';
```

##  非空约束

###  作用

限定某个字段/ 某列的值不允许为空

###  关键字

NOT NULL

###  特点

* 默认，所有的类型的值都可以是NULL，包括INT、FLOAT等数据类型 
* 非空约束只能出现在表对象的列上，只能某个列单独限定非空，不能组合非空 
* 一个表可以有很多列都分别限定了非空 
* 空字符串''不等于NULL，0也不等于NULL

###  添加非空约束

**1. 建表时**

```mysql
CREATE TABLE 表名称(
字段名 数据类型,
字段名 数据类型 NOT NULL,
字段名 数据类型 NOT NULL
);
```

**2. 建表后**

```mysql
alter table 表名称 modify 字段名 数据类型 not null;
```

###  删除非空约束

```mysql
alter table 表名称 modify 字段名 数据类型 NULL;#去掉not null，相当于修改某个非注解字段，该字段允许为空
或
alter table 表名称 modify 字段名 数据类型;#去掉not null，相当于修改某个非注解字段，该字段允许为空
```

## 唯一性约束

###  作用

用来限制某个字段/某列的值不能重复。

###  关键字

UNIQUE

###  特点

* 同一个表可以有多个唯一约束。
* 唯一约束可以是某一个列的值唯一，也可以多个列组合的值唯一。 
* 唯一性约束允许列值为空。 
* 在创建唯一约束的时候，如果不给唯一约束命名，就默认和列名相同。 
* MySQL会给唯一约束的列上默认创建一个唯一索引。

###  添加唯一约束

**1. 建表时**

```mysql
create table 表名称(
字段名 数据类型,
字段名 数据类型 unique,
字段名 数据类型 unique key,
字段名 数据类型
);

create table 表名称(
字段名 数据类型,
字段名 数据类型,
字段名 数据类型,
[constraint 约束名] unique key(字段名)
);
```

举例：

```mysql
CREATE TABLE USER(
id INT NOT NULL,
NAME VARCHAR(25),
PASSWORD VARCHAR(16),
-- 使用表级约束语法
CONSTRAINT uk_name_pwd UNIQUE(NAME,PASSWORD)
);
```

> 表示用户名和密码组合不能重复

**2. 建表后指定唯一键约束**

```mysql
#字段列表中如果是一个字段，表示该列的值唯一。如果是两个或更多个字段，那么复合唯一，即多个字段的组合是唯
一的
#方式1：
alter table 表名称 add unique key(字段列表);
#方式2：
alter table 表名称 modify 字段名 字段类型 unique;
```

###  关于复合唯一约束

```mysql
create table 表名称(
字段名 数据类型,
字段名 数据类型,
字段名 数据类型,
unique key(字段列表) #字段列表中写的是多个字段名，多个字段名用逗号分隔，表示那么是复合唯一，即多个字段的组合是唯一的
);
```

###  删除唯一约束

* 添加唯一性约束的列上也会自动创建唯一索引。 
* 删除唯一约束只能通过删除唯一索引的方式删除。 
* 删除时需要指定唯一索引名，唯一索引名就和唯一约束名一样。 
* 如果创建唯一约束时未指定名称，如果是单列，就默认和列名相同；
* 如果是组合列，那么默认和() 中排在第一个的列名相同。也可以自定义唯一性约束名。

```mysql
SELECT * FROM information_schema.table_constraints WHERE table_name = '表名'; #查看都有哪些约束
```

```mysql
ALTER TABLE USER
DROP INDEX uk_name_pwd;
```

> 注意：可以通过 show index from 表名称;        #查看表的索引

##  PRIMARY KEY 约束

###  作用

用来唯一标识表中的一行记录。

###  关键字

primary key

###  特点

主键约束相当于唯一约束+非空约束的组合，主键约束列不允许重复，也不允许出现空值。

* 一个表最多只能有一个主键约束，建立主键约束可以在列级别创建，也可以在表级别上创建。 
* 主键约束对应着表中的一列或者多列（复合主键） 
* 如果是多列组合的复合主键约束，那么这些列都不允许为空值，并且组合的值不允许重复。 
* MySQL的主键名总是PRIMARY，就算自己命名了主键约束名也没用。 
* 当创建主键约束时，系统默认会在所在的列或列组合上建立对应的主键索引（能够根据主键查询的，就根据主键查询，效率更高。如果删除主键约束了，主键约束对应的索引就自动删除了。 
* 需要注意的一点是，不要修改主键字段的值。因为主键是数据记录的唯一标识，如果修改了主键的值，就有可能会破坏数据的完整性。

###  添加主键约束

**1. 建表时指定主键约束**

```mysql
create table 表名称(
字段名 数据类型 primary key, #列级模式
字段名 数据类型,
字段名 数据类型
);

create table 表名称(
字段名 数据类型,
字段名 数据类型,
字段名 数据类型,
[constraint 约束名] primary key(字段名) #表级模式
);
```

**2. 建表后增加主键约束**

```mysql
ALTER TABLE 表名称 ADD PRIMARY KEY(字段列表); #字段列表可以是一个字段，也可以是多个字段，如果是多个字段的话，是复合主键
```

###  关于复合主键

```mysql
create table 表名称(
字段名 数据类型,
字段名 数据类型,
字段名 数据类型,
primary key(字段名1,字段名2) #表示字段1和字段2的组合是唯一的，也可以有更多个字段
);
```

###  删除主键约束

```mysql
alter table 表名称 drop primary key
```

> 说明：删除主键约束，不需要指定主键名，因为一个表只有一个主键，删除主键约束后，非空还存在。

##  自增列：AUTO_INCREMENT

###  作用

某个字段的值自增

### 关键字

auto_increment

###  特点

（1）一个表最多只能有一个自增长列 

（2）当需要产生唯一标识符或顺序值时，可设置自增长 

（3）自增长列约束的列必须是键列（主键列，唯一键列） 

（4）自增约束的列的数据类型必须是整数类型 

（5）如果自增列指定了 0 和 null，会在当前最大值的基础上自增；如果自增列手动指定了具体值，直接赋值为具体值。

###  如何指定自增约束

**1. 建表时**

```mysql
create table 表名称(
字段名 数据类型 primary key auto_increment,
字段名 数据类型 unique key not null,
字段名 数据类型 unique key,
字段名 数据类型 not null default 默认值,
);
create table 表名称(
字段名 数据类型 default 默认值 ,
字段名 数据类型 unique key auto_increment,
字段名 数据类型 not null default 默认值,
primary key(字段名)
);
```

**2. 建表后**

```mysql
alter table 表名称 modify 字段名 数据类型 auto_increment;
```

###  删除自增约束

```mysql
#alter table 表名称 modify 字段名 数据类型 auto_increment;#给这个字段增加自增约束
alter table 表名称 modify 字段名 数据类型; #去掉auto_increment相当于删除
```

###  MySQL 8.0新特性—自增变量的持久化

在MySQL 8.0之前，自增主键AUTO_INCREMENT的值如果大于max(primary key)+1，在MySQL重启后，会重置AUTO_INCREMENT=max(primary key)+1，这种现象在某些情况下会导致业务主键冲突或者其他难以发现的问题。 下面通过案例来对比不同的版本中自增变量是否持久化。 在MySQL 5.7版本中，测试步骤如 下： 创建的数据表中包含自增主键的id字段，语句如下：

```mysql
CREATE TABLE test1(
id INT PRIMARY KEY AUTO_INCREMENT
);
```

在MySQL 5.7系统中，对于自增主键的分配规则，是由InnoDB数据字典 内部一个 计数器 来决定的，而该计数器只在 内存中维护 ，并不会持久化到磁盘中。当数据库重启时，该 计数器会被初始化。

在MySQL 8.0将自增主键的计数器持久化到 重做日志 中。每次计数器发生改变，都会将其写入重做日志 中。如果数据库重启，InnoDB会根据重做日志中的信息来初始化计数器的内存值。

##  FOREIGN KEY 约束

### 作用

限定某个表的某个字段的引用完整性。

###  关键字

FOREIGN KEY

###  主表和从表/父表和子表

主表（父表）：被引用的表，被参考的表 

从表（子表）：引用别人的表，参考别人的表

###  特点

（1）从表的外键列，必须引用/参考主表的主键或唯一约束的列为什么？因为被依赖/被参考的值必须是唯一的 

（2）在创建外键约束时，如果不给外键约束命名，默认名不是列名，而是自动产生一个外键名（例如 student_ibfk_1;），也可以指定外键约束名。 

（3）创建(CREATE)表时就指定外键约束的话，**先创建主表**，再创建从表 

（4）删表时，**先删从表**（或先删除外键约束），再删除主表 

（5）当主表的记录被从表参照时，主表的记录将不允许删除，如果要删除数据，需要先删除从表中依赖该记录的数据，然后才可以删除主表的数据 

（6）在“从表”中指定外键约束，并且一个表可以建立多个外键约束 

（7）从表的外键列与主表被参照的列名字可以不相同，但是数据类型必须一样，逻辑意义一致。如果类型不一样，创建子表时，就会出现错误“ERROR 1005 (HY000): Can't create table'database.tablename'(errno: 150)”。 例如：都是表示部门编号，都是int类型。

（8）当创建外键约束时，系统默认会在所在的列上建立对应的普通索引。但是索引名是外键的约束名。（根据外键查询效率很高） 

（9）删除外键约束后，必须手动删除对应的索引

###  添加外键约束

**1. 建表时**

```mysql
create table 主表名称(
字段1 数据类型 primary key,
字段2 数据类型
);

create table 从表名称(
字段1 数据类型 primary key,
字段2 数据类型,
[CONSTRAINT <外键约束名称>] FOREIGN KEY（从表的某个字段) references 主表名(被参考字段)
);
#(从表的某个字段)的数据类型必须与主表名(被参考字段)的数据类型一致，逻辑意义也一样
#(从表的某个字段)的字段名可以与主表名(被参考字段)的字段名一样，也可以不一样
-- FOREIGN KEY: 在表级指定子表中的列
-- REFERENCES: 标示在父表中的列
```

```mysql
create table dept( #主表
did int primary key, #部门编号
dname varchar(50) #部门名称
);
create table emp(#从表
eid int primary key, #员工编号
ename varchar(5), #员工姓名
deptid int, #员工所在的部门
foreign key (deptid) references dept(did) #在从表中指定外键约束
#emp表的deptid和和dept表的did的数据类型一致，意义都是表示部门的编号
);
说明：
（1）主表dept必须先创建成功，然后才能创建emp表，指定外键成功。
（2）删除表时，先删除从表emp，再删除主表dept
```

**2. 建表后**

一般情况下，表与表的关联都是提前设计好了的，因此，会在创建表的时候就把外键约束定义好。不 过，如果需要修改表的设计（比如添加新的字段，增加新的关联关系），但没有预先定义外键约束，那 么，就要用修改表的方式来补充定义。

格式：

```mysql
ALTER TABLE 从表名 ADD [CONSTRAINT 约束名] FOREIGN KEY (从表的字段) REFERENCES 主表名(被引用字段) [on update xx][on delete xx];
```

举例：

```mysql
ALTER TABLE emp1
ADD [CONSTRAINT emp_dept_id_fk] FOREIGN KEY(dept_id) REFERENCES dept(dept_id);
```

###  约束等级

* `Cascade方式 `：在父表上update/delete记录时，同步update/delete掉子表的匹配记录 
* `Set null方式` ：在父表上update/delete记录时，将子表上匹配记录的列设为null，但是要注意子 表的外键列不能为not null 
* `No action方式` ：如果子表中有匹配的记录，则不允许对父表对应候选键进行update/delete操作 
* `Restrict方式` ：同no action， 都是立即检查外键约束 
* `Set default方式` （在可视化工具SQLyog中可能显示空白）：父表有变更时，子表将外键列设置 成一个默认的值，但Innodb不能识别x

如果没有指定等级，就相当于Restrict方式。 对于外键约束，最好是采用: ON UPDATE CASCADE ON DELETE RESTRICT 的方式。

###  删除外键约束

流程如下：

```mysql
(1)第一步先查看约束名和删除外键约束
SELECT * FROM information_schema.table_constraints WHERE table_name = '表名称';  #查看某个表的约束名
ALTER TABLE 从表名 DROP FOREIGN KEY 外键约束名;

（2）第二步查看索引名和删除索引。（注意，只能手动删除）
SHOW INDEX FROM 表名称; #查看某个表的索引名
ALTER TABLE 从表名 DROP INDEX 索引名;
```

###  开发场景

**问题1：如果两个表之间有关系（一对一、一对多），比如：员工表和部门表（一对多），它们之间是否 一定要建外键约束？**

答：不是的

**问题2：建和不建外键约束有什么区别？**

答：建外键约束，你的操作（创建表、删除表、添加、修改、删除）会受到限制，从语法层面受到限 制。例如：在员工表中不可能添加一个员工信息，它的部门的值在部门表中找不到。 

不建外键约束，你的操作（创建表、删除表、添加、修改、删除）不受限制，要保证数据的 引用完整 性 ，只能依靠程序员的自觉 ，或者是 在Java程序中进行限定 。例如：在员工表中，可以添加一个员工的 信息，它的部门指定为一个完全不存在的部门。

**问题3：那么建和不建外键约束和查询有没有关系？**

答：没有

> 在 MySQL 里，外键约束是有成本的，需要消耗系统资源。对于大并发的 SQL 操作，有可能会不适合。比如大型网站的中央数据库，可能会因为外键约束的系统开销而变得非常慢 。所以， MySQL 允许你不使用系统自带的外键约束，在 应用层面 完成检查数据一致性的逻辑。也就是说，即使你不 用外键约束，也要想办法通过应用层面的附加逻辑，来实现外键约束的功能，确保数据的一致性。

###   阿里开发规范

【 强制 】不得使用外键与级联，一切外键概念必须在应用层解决。 

说明：（概念解释）学生表中的 student_id 是主键，那么成绩表中的 student_id 则为外键。如果更新学 生表中的 student_id，同时触发成绩表中的 student_id 更新，即为级联更新。外键与级联更新适用于 单 机低并发 ，不适合 分布式 、 高并发集群 ；级联更新是强阻塞，存在数据库 更新风暴 的风险；外键影响 数据库的 插入速度 。

##  CHECK 约束

###  作用

检查某个字段的值是否符号xx要求，一般指的是值的范围

###  关键字

CHECK

###  说明

MySQL5.7 可以使用check约束，但check约束对数据验证没有任何作用。添加数据时，没有任何错误或警告

但是**MySQL 8.0中可以使用check约束了**。

```mysql
create table employee(
eid int primary key,
ename varchar(5),
gender char check ('男' or '女')
);
```

## DEFAULT约束

###  作用

给某个字段/某列指定默认值，一旦设置默认值，在插入数据时，如果此字段没有显式赋值，则赋值为默认值。

###  关键字

DEFAULT

###  添加默认值

**1. 建表时**

```mysql
create table 表名称(
字段名 数据类型 primary key,
字段名 数据类型 unique key not null,
字段名 数据类型 unique key,
字段名 数据类型 not null default 默认值,
);
```

**2. 建表后**

```mysql
alter table 表名称 modify 字段名 数据类型 default 默认值;
#如果这个字段原来有非空约束，你还保留非空约束，那么在加默认值约束时，还得保留非空约束，否则非空约束就被删除了
#同理，在给某个字段加非空约束也一样，如果这个字段原来有默认值约束，你想保留，也要在modify语句中保留默认值约束，否则就删除了
alter table 表名称 modify 字段名 数据类型 default 默认值 not null;
```

**删除默认值**

```mysql
alter table 表名称 modify 字段名 数据类型; #删除默认值约束，也不保留非空约束
alter table 表名称 modify 字段名 数据类型 not null; #删除默认值约束，保留非空约束
```

##  面试

**面试1、为什么建表时，加 not null default '' 或 default 0**

答：不想让表中出现null值。

**面试2、为什么不想要 null 的值**

答:

（1）不好比较。null是一种特殊值，比较时只能用专门的is null 和 is not null来比较。碰到运算符，通 常返回null。 

（2）效率不高。影响提高索引效果。因此，我们往往在建表时 not null default '' 或 default 0

**面试3、带AUTO_INCREMENT约束的字段值是从1开始的吗？**

在MySQL中，默认AUTO_INCREMENT的初始 值是1，每新增一条记录，字段值自动加1。设置自增属性（AUTO_INCREMENT）的时候，还可以指定第 一条插入记录的自增字段的值，这样新插入的记录的自增字段值从初始值开始递增，如在表中插入第一 条记录，同时指定id值为5，则以后插入的记录的id值就会从6开始往上增加。添加主键约束时，往往需要 设置字段自动增加属性。

**面试4、并不是每个表都可以任意选择存储引擎？**

外键约束（FOREIGN KEY）不能跨引擎使用。

MySQL支持多种存储引擎，每一个表都可以指定一个不同的存储引擎，需要注意的是：外键约束是用来 保证数据的参照完整性的，如果表之间需要关联外键，却指定了不同的存储引擎，那么这些表之间是不 能创建外键约束的。所以说，存储引擎的选择也不完全是随意的。

# 视图

##  常见的数据库对象

| 对象                 | 描述                                                         |
| -------------------- | ------------------------------------------------------------ |
| 表(TABLE)            | 表是存储数据的逻辑单元，以行和列的形式存在，列就是字段，行就是记录 |
| 数据字典             | 就是系统表，存放数据库相关信息的表。系统表的数据通常由数据库系统维护， 程序员通常不应该修改，只可查看 |
| 约束 (CONSTRAINT)    | 执行数据校验的规则，用于保证数据完整性的规则                 |
| 视图(VIEW)           | 一个或者多个数据表里的数据的逻辑显示，视图并不存储数据       |
| 索引(INDEX)          | 用于提高查询性能，相当于书的目录                             |
| 存储过程 (PROCEDURE) | 用于完成一次完整的业务处理，没有返回值，但可通过传出参数将多个值传给调 用环境 |
| 存储函数 (FUNCTION)  | 用于完成一次特定的计算，具有一个返回值                       |
| 触发器 (TRIGGER)     | 相当于一个事件监听器，当数据库发生特定事件后，触发器被触发，完成相应的处理 |

##  视图概述



* 视图是一种 虚拟表 ，本身是 不具有数据 的，占用很少的内存空间，它是 SQL 中的一个重要概念。 
* 视图建立在已有表的基础上, 视图赖以建立的这些表称为基表。

* 视图的创建和删除只影响视图本身，不影响对应的基表。但是当对视图中的数据进行增加、删除和 修改操作时，数据表中的数据会相应地发生变化，反之亦然。
* 视图提供数据内容的语句为 SELECT 语句, 可以将视图理解为存储起来的 SELECT 语句 
* 在数据库中，视图不会保存数据，数据真正保存在数据表中。当对视图中的数据进行增加、删 除和修改操作时，数据表中的数据会相应地发生变化；反之亦然。
* 视图，是向用户提供基表数据的另一种表现形式。通常情况下，小型项目的数据库可以不使用视 图，但是在大型项目中，以及数据表比较复杂的情况下，视图的价值就凸显出来了，它可以帮助我 们把经常查询的结果集放到虚拟表中，提升使用效率。理解和使用起来都非常方便。

## 创建视图

* 在 CREATE VIEW 语句中嵌入子查询

```mysql
CREATE [OR REPLACE]
[ALGORITHM = {UNDEFINED | MERGE | TEMPTABLE}]
VIEW 视图名称 [(字段列表)]
AS 查询语句
[WITH [CASCADED|LOCAL] CHECK OPTION]
```

* 精简版

```mysql
CREATE VIEW 视图名称
AS 查询语句
```

### 创建单表视图

举例：

```mysql
# 方式一：
CREATE VIEW empvu80
AS
SELECT employee_id, last_name, salary
FROM employees
WHERE department_id = 80;

# 方式二：
CREATE VIEW empsalary8000(emp_id, NAME, monthly_sal) # 小括号内字段个数与SELECT中字段个数相同
AS
SELECT employee_id, last_name, salary
FROM employees
WHERE salary > 8000;
```

查询视图：

```mysql
SELECT *
FROM salvu80;
```

###  创建多表联合视图

举例：

```mysql
CREATE VIEW empview
AS
SELECT employee_id emp_id,last_name NAME,department_name
FROM employees e,departments d
WHERE e.department_id = d.department_id;
```

```mysql
CREATE VIEW dept_sum_vu
(name, minsal, maxsal, avgsal)
AS
SELECT d.department_name, MIN(e.salary), MAX(e.salary),AVG(e.salary)
FROM employees e, departments d
WHERE e.department_id = d.department_id
GROUP BY d.department_name;
```

* 利用视图对数据进行格式化

常需要输出某个格式的内容，比如我们想输出员工姓名和对应的部门名，对应格式为 emp_name(department_name)，就可以使用视图来完成数据格式化的操作：

```mysql
CREATE VIEW emp_depart
AS
SELECT CONCAT(last_name,'(',department_name,')') AS emp_dept
FROM employees e JOIN departments d
WHERE e.department_id = d.department_id;
```

### 基于视图创建视图

当我们创建好一张视图之后，还可以在它的基础上继续创建视图。

举例：联合“emp_dept”视图和“emp_year_salary”视图查询员工姓名、部门名称、年薪信息创建 “emp_dept_ysalary”视图。

```mysql
CREATE VIEW emp_dept_ysalary
AS
SELECT emp_dept.ename,dname,year_salary
FROM emp_dept INNER JOIN emp_year_salary
ON emp_dept.ename = emp_year_salary.ename;
```

## 查看视图

语法1：查看数据库的表对象、视图对象

```mysql
SHOW TABLES;
```

语法2：查看视图的结构

```mysql
DESC / DESCRIBE 视图名称;
```

语法3：查看视图的属性信息

```mysql
# 查看视图信息（显示数据表的存储引擎、版本、数据行数和数据大小等）
SHOW TABLE STATUS LIKE '视图名称'\G
```

执行结果显示，注释Comment为VIEW，说明该表为视图，其他的信息为NULL，说明这是一个虚表。 语法4：查看视图的详细定义信息

```mysql
SHOW CREATE VIEW 视图名称;
```

## 更新视图的数据

### 一般情况

MySQL支持使用INSERT、UPDATE和DELETE语句对视图中的数据进行插入、更新和删除操作。当视图中的 数据发生变化时，数据表中的数据也会发生变化，反之亦然。

举例：UPDATE操作

```mysql
UPDATE emp_tel SET tel = '13789091234' WHERE ename = '孙洪亮';
```

举例：DELETE操作

```mysql
 DELETE FROM emp_tel WHERE ename = '孙洪亮';
```

### 不可更新的视图

要使视图可更新，视图中的行和底层基本表中的行之间必须存在 一对一 的关系。另外当视图定义出现如下情况时，视图不支持更新操作：

* 在定义视图的时候指定了“ALGORITHM = TEMPTABLE”，视图将不支持INSERT和DELETE操作； 
* 视图中不包含基表中所有被定义为非空又未指定默认值的列，视图将不支持INSERT操作； 
* 在定义视图的SELECT语句中使用了 JOIN联合查询 ，视图将不支持INSERT和DELETE操作； 
* 在定义视图的SELECT语句后的字段列表中使用了 数学表达式 或 子查询 ，视图将不支持INSERT，也 不支持UPDATE使用了数学表达式、子查询的字段值； 
* 在定义视图的SELECT语句后的字段列表中使用 DISTINCT 、 聚合函数 、 GROUP BY 、 HAVING 、 UNION 等，视图将不支持INSERT、UPDATE、DELETE； 
* 在定义视图的SELECT语句中包含了子查询，而子查询中引用了FROM后面的表，视图将不支持 INSERT、UPDATE、DELETE； 
* 视图定义基于一个 不可更新视图 ； 常量视图。

> 虽然可以更新视图数据，但总的来说，视图作为虚拟表 ，主要用于方便查询 ，不建议更新视图的数据。对视图数据的更改，都是通过对实际数据表里数据的操作来完成的。

##  修改、删除视图

###  修改视图

方式1：使用CREATE OR REPLACE VIEW 子句修改视图

```mysql
CREATE OR REPLACE VIEW empvu80
(id_number, name, sal, department_id)
AS
SELECT employee_id, first_name || ' ' || last_name, salary, department_id
FROM employees
WHERE department_id = 80;
```

> 说明：CREATE VIEW 子句中各列的别名应和子查询中各列相对应。

方式2：ALTER VIEW

修改视图的语法是：

```mysql
ALTER VIEW 视图名称
AS
查询语句
```

### 删除视图

* 删除视图只是删除视图的定义，并不会删除基表的数据。 
* 删除视图的语法是：

```mysql
DROP VIEW IF EXISTS 视图名称;
```

+ 举例：

```mysql
DROP VIEW empvu80;
```

+ 说明：基于视图a、b创建了新的视图c，如果将视图a或者视图b删除，会导致视图c的查询失败。这 样的视图c需要手动删除或修改，否则影响使用。

## 总结

###  优点

**1. 操作简单**

将经常使用的查询操作定义为视图，可以使开发人员不需要关心视图对应的数据表的结构、表与表之间的关联关系，也不需要关心数据表之间的业务逻辑和查询条件，而只需要简单地操作视图即可，极大简化了开发人员对数据库的操作。

**2. 减少数据冗余**

视图跟实际数据表不一样，它存储的是查询语句。所以，在使用的时候，我们要通过定义视图的查询语 句来获取结果集。而视图本身不存储数据，不占用数据存储的资源，减少了数据冗余。

**3. 数据安全**

MySQL将用户对数据的 访问限制 在某些数据的结果集上，而这些数据的结果集可以使用视图来实现。用 户不必直接查询或操作数据表。这也可以理解为视图具有 隔离性 。视图相当于在用户和实际的数据表之间加了一层虚拟表。

同时，MySQL可以根据权限将用户对数据的访问限制在某些视图上，用户不需要查询数据表，可以直接通过视图获取数据表中的信息。这在一定程度上保障了数据表中数据的安全性。

**4. 适应灵活多变的需求**

当业务系统的需求发生变化后，如果需要改动数据表的结构，则工作量相对较 大，可以使用视图来减少改动的工作量。这种方式在实际工作中使用得比较多。

**5. 能够分解复杂的查询逻辑**

 数据库中如果存在复杂的查询逻辑，则可以将问题进行分解，创建多个视图 获取数据，再将创建的多个视图结合起来，完成复杂的查询逻辑。

###  不足

如果我们在实际数据表的基础上创建了视图，那么，如果实际数据表的结构变更了，我们就需要及时对相关的视图进行相应的维护。特别是嵌套的视图（就是在视图的基础上创建视图），维护会变得比较复杂， 可读性不好 ，容易变成系统的潜在隐患。因为创建视图的 SQL 查询可能会对字段重命名，也可能包含复杂的逻辑，这些都会增加维护的成本。 

实际项目中，如果视图过多，会导致数据库维护成本的问题。 

所以，在创建视图的时候，你要结合实际项目需求，综合考虑视图的优点和不足，这样才能正确使用视图，使系统整体达到最优。

# 存储过程与函数

MySQL从5.0版本开始支持存储过程和函数。存储过程和函数能够将复杂的SQL逻辑封装在一起，应用程 序无须关注存储过程和函数内部复杂的SQL逻辑，而只需要简单地调用存储过程和函数即可。

##  存储过程概述

### 理解

**含义：**存储过程的英文是 Stored Procedure 。它的思想很简单，就是一组经过 预先编译的 SQL 语句 的封装。

执行过程：存储过程预先存储在 MySQL 服务器上，需要执行的时候，客户端只需要向服务器端发出调用存储过程的命令，服务器端就可以把预先存储好的这一系列 SQL 语句全部执行。

**好处：**

* 1、简化操作，提高了sql语句的重用性，减少了开发程序员的压力。
* 2、减少操作过程中的失误，提高效率。
* 3、减少网络传输量（客户端不需要把所有的 SQL 语句通过网络发给服务器）。
* 4、减少了 SQL 语句暴露在 网上的风险，也提高了数据查询的安全性。

**和视图、函数的对比：**

它和视图有着同样的优点，清晰、安全，还可以减少网络传输量。不过它和视图不同，视图是虚拟表 ，通常不对底层数据表直接操作，而存储过程是程序化的 SQL，可以 直接操作底层数据表 ，相比于面向集合的操作方式，能够实现一些更复杂的数据处理。

一旦存储过程被创建出来，使用它就像使用函数一样简单，我们直接通过调用存储过程名即可。相较于函数，存储过程是 没有返回值 的。

###  分类

存储过程的参数类型可以是IN、OUT和INOUT。根据这点分类如下：

1、没有参数（无参数无返回） 

2、仅仅带 IN 类型（有参数无返回） 

3、仅仅带 OUT 类型（无参数有返回） 

4、既带 IN 又带 OUT（有参数有返回） 

5、带 INOUT（有参数有返回）

注意：IN、OUT、INOUT 都可以在一个存储过程中带多个。

##  创建存储过程

###  语法分析

**语法：**

```mysql
CREATE PROCEDURE 存储过程名(IN|OUT|INOUT 参数名 参数类型,...)
[characteristics ...]
BEGIN
存储过程体
END
```

**说明：**

1、参数前面的符号的意思

* IN ：当前参数为输入参数，也就是表示入参；

	存储过程只是读取这个参数的值。如果没有定义参数种类， 默认就是 IN ，表示输入参数。

* OUT ：当前参数为输出参数，也就是表示出参；

	执行完成之后，调用这个存储过程的客户端或者应用程序就可以读取这个参数返回的值了。

* INOUT ：当前参数既可以为输入参数，也可以为输出参数。

2、形参类型可以是 MySQL数据库中的任意类型。

3、characteristics 表示创建存储过程时指定的对存储过程的约束条件，其取值信息如下：

```mysql
LANGUAGE SQL
| [NOT] DETERMINISTIC
| { CONTAINS SQL | NO SQL | READS SQL DATA | MODIFIES SQL DATA }
| SQL SECURITY { DEFINER | INVOKER }
| COMMENT 'string'
```

* LANGUAGE SQL ：说明存储过程执行体是由SQL语句组成的，当前系统支持的语言为SQL。
* [NOT] DETERMINISTIC ：指明存储过程执行的结果是否确定。DETERMINISTIC表示结果是确定 的。每次执行存储过程时，相同的输入会得到相同的输出。NOT DETERMINISTIC表示结果是不确定 的，相同的输入可能得到不同的输出。如果没有指定任意一个值，默认为NOT DETERMINISTIC。
* { CONTAINS SQL | NO SQL | READS SQL DATA | MODIFIES SQL DATA } ：指明子程序使 用SQL语句的限制。
	+ CONTAINS SQL表示当前存储过程的子程序包含SQL语句，但是并不包含读写数据的SQL语句；
	+ NO SQL表示当前存储过程的子程序中不包含任何SQL语句； 
	+ READS SQL DATA表示当前存储过程的子程序中包含读数据的SQL语句； 
	+ MODIFIES SQL DATA表示当前存储过程的子程序中包含写数据的SQL语句。 
	+ 默认情况下，系统会指定为CONTAINS SQL。
* SQL SECURITY { DEFINER | INVOKER } ：执行当前存储过程的权限，即指明哪些用户能够执行当前存储过程。
	+ DEFINER 表示只有当前存储过程的创建者或者定义者才能执行当前存储过程；
	+ INVOKER 表示拥有当前存储过程的访问权限的用户能够执行当前存储过程。

* COMMENT 'string' ：注释信息，可以用来描述存储过程。

4、存储过程体中可以有多条 SQL 语句，如果仅仅一条SQL 语句，则可以省略 BEGIN 和 END

```mysql
1. BEGIN…END：BEGIN…END 中间包含了多个语句，每个语句都以（;）号为结束符。
2. DECLARE：DECLARE 用来声明变量，使用的位置在于 BEGIN…END 语句中间，而且需要在其他语句使用之前进
行变量的声明。
3. SET：赋值语句，用于对变量进行赋值。
4. SELECT… INTO：把从数据表中查询的结果存放到变量中，也就是为变量赋值。
```

5、需要设置新的结束标记

```mysql
DELIMITER 新的结束标记
```

因为MySQL默认的语句结束符号为分号‘;’。为了避免与存储过程中SQL语句结束符相冲突，需要使用 DELIMITER改变存储过程的结束符。

比如：“DELIMITER //”语句的作用是将MySQL的结束符设置为//，并以“END //”结束存储过程。存储过程定 义完毕之后再使用“DELIMITER ;”恢复默认结束符。DELIMITER也可以指定其他符号作为结束符。

当使用DELIMITER命令时，应该避免使用反斜杠（‘\’）字符，因为反斜线是MySQL的转义字符。 

示例：

```mysql
DELIMITER $
CREATE PROCEDURE 存储过程名(IN|OUT|INOUT 参数名 参数类型,...)
[characteristics ...]
BEGIN
sql语句1;
sql语句2;
END $
```

###   代码举例

举例1：创建存储过程select_all_data()，查看 emps 表的所有数据

```mysql
DELIMITER $
CREATE PROCEDURE select_all_data()
BEGIN
SELECT * FROM emps;
END $
DELIMITER ;
```

举例2：创建存储过程avg_employee_salary()，返回所有员工的平均工资

```mysql
DELIMITER //
CREATE PROCEDURE avg_employee_salary ()
BEGIN
SELECT AVG(salary) AS avg_salary FROM emps;
END //
DELIMITER ;
```

## 调用存储过程

###  调用格式

存储过程有多种调用方法。存储过程必须使用CALL语句调用，并且存储过程和数据库相关，如果要执行其他数据库中的存储过程，需要指定数据库名称，例如CALL dbname.procname。

```mysql
CALL 存储过程名(实参列表)
```

**格式：**

1、调用in模式的参数：

```mysql
CALL sp1('值');
```

2、调用out模式的参数：

```mysql
SET @name;
CALL sp1(@name);
SELECT @name;
```

3、调用inout模式的参数：

```mysql
SET @name=值;
CALL sp1(@name);
SELECT @name;
```

### 代码举例

**举例1：**

```mysql
DELIMITER //
CREATE PROCEDURE CountProc(IN sid INT,OUT num INT)
BEGIN
SELECT COUNT(*) INTO num FROM fruits
WHERE s_id = sid;
END //
DELIMITER ;
```

调用存储过程：

```mysql
CALL CountProc (101, @num);
```

查看返回结果：

```mysql
SELECT @num;
```

**举例2：**创建存储过程，实现累加运算，计算 1+2+…+n 等于多少。具体的代码如下：

```mysql
DELIMITER //
CREATE PROCEDURE `add_num`(IN n INT)
BEGIN
DECLARE i INT;
DECLARE sum INT;
SET i = 1;
SET sum = 0;
WHILE i <= n DO
SET sum = sum + i;
SET i = i +1;
END WHILE;
SELECT sum;
END //
DELIMITER ;
```

直接使用 CALL add_num(50); 即可。这里我传入的参数为 50，也就是统计 1+2+…+50 的积累之和。

###  如何调试

在 MySQL 中，存储过程不像普通的编程语言（比如 VC++、Java 等）那样有专门的集成开发环境。因 此，你可以通过 SELECT 语句，把程序执行的中间结果查询出来，来调试一个 SQL 语句的正确性。调试 成功之后，把 SELECT 语句后移到下一个 SQL 语句之后，再调试下一个 SQL 语句。这样 逐步推进 ，就可以完成对存储过程中所有操作的调试了。当然，你也可以把存储过程中的 SQL 语句复制出来，逐段单独 调试。

## 存储函数的使用

###  语法分析

学过的函数：LENGTH、SUBSTR、CONCAT等

语法格式：

```mysql
CREATE FUNCTION 函数名(参数名 参数类型,...)
RETURNS 返回值类型
[characteristics ...]
BEGIN
函数体 #函数体中肯定有 RETURN 语句
END
```

说明：

1、参数列表：指定参数为IN、OUT或INOUT只对PROCEDURE是合法的，FUNCTION中总是默认为IN参数。 

2、RETURNS type 语句表示函数返回数据的类型； RETURNS子句只能对FUNCTION做指定，对函数而言这是 强制 的。它用来指定函数的返回类型，而且函 数体必须包含一个 RETURN value 语句。 

3、characteristic 创建函数时指定的对函数的约束。取值与创建存储过程时相同，这里不再赘述。 

4、函数体也可以用BEGIN…END来表示SQL代码的开始和结束。如果函数体只有一条语句，也可以省略 BEGIN…END。

###  调用存储函数

在MySQL中，存储函数的使用方法与MySQL内部函数的使用方法是一样的。换言之，用户自己定义的存储函数与MySQL内部函数是一个性质的。区别在于，存储函数是 用户自己定义 的，而内部函数是MySQL 的 开发者定义 的。

```mysql
SELECT 函数名(实参列表)
```

###  代码举例

**举例1：**

创建存储函数，名称为email_by_name()，参数定义为空，该函数查询Abel的email，并返回，数据类型为字符串型。

```mysql
DELIMITER //
CREATE FUNCTION email_by_name()
RETURNS VARCHAR(25)
DETERMINISTIC
CONTAINS SQL
BEGIN
RETURN (SELECT email FROM employees WHERE last_name = 'Abel');
END //
DELIMITER ;
```

调用：

```mysql
SELECT email_by_name();
```

**举例2：**

创建存储函数，名称为email_by_id()，参数传入emp_id，该函数查询emp_id的email，并返回，数据类型 为字符串型。

```mysql
DELIMITER //
CREATE FUNCTION email_by_id(emp_id INT)
RETURNS VARCHAR(25)
DETERMINISTIC
CONTAINS SQL
BEGIN
RETURN (SELECT email FROM employees WHERE employee_id = emp_id);
END //
DELIMITER ;
```

调用：

```mysql
SET @emp_id = 102;
SELECT email_by_id(@emp_id);
```

**注意：**

若在创建存储函数中报错“ you might want to use the less safe log_bin_trust_function_creators variable ”，有两种处理方法：

* 方式1：

	加上必要的函数特性“[NOT] DETERMINISTIC”和“{CONTAINS SQL | NO SQL | READS SQL DATA | MODIFIES SQL DATA}”

* 方式2：

```mysql
SET GLOBAL log_bin_trust_function_creators = 1;
```

###  对比存储函数与存储过程

|          | 关键字    | 调用语法        | 返回值            | 应用场景                         |
| -------- | --------- | --------------- | ----------------- | -------------------------------- |
| 存储过程 | PROCEDURE | CALL 存储过程() | 理解为有0个或多个 | 一般用于更新                     |
| 存储函数 | FUNCTION  | SELECT 函数 ()  | 只能是一个        | 一般用于查询结果为一个值并返回时 |

此外，**存储函数可以放在查询语句中使用，存储过程不行**。反之，存储过程的功能更加强大，包括能够 执行对表的操作（比如创建表，删除表等）和事务操作，这些功能是存储函数不具备的。

## 存储过程和函数的查看、修改、删除

###  查看

 创建完之后，怎么知道我们创建的存储过程、存储函数是否成功了呢？

MySQL存储了存储过程和函数的状态信息，用户可以使用SHOW STATUS语句或SHOW CREATE语句来查 看，也可直接从系统的information_schema数据库中查询。这里介绍3种方法。

1. 使用SHOW CREATE语句查看存储过程和函数的创建信息

```mysql
SHOW CREATE {PROCEDURE | FUNCTION} 存储过程名或函数名
```

2. 使用SHOW STATUS语句查看存储过程和函数的状态信息

```mysql
SHOW {PROCEDURE | FUNCTION} STATUS [LIKE 'pattern']
```

3. 从information_schema.Routines表中查看存储过程和函数的信息

MySQL中存储过程和函数的信息存储在information_schema数据库下的Routines表中。可以通过查询该表的记录来查询存储过程和函数的信息。其基本语法形式如下：

```mysql
SELECT * FROM information_schema.Routines
WHERE ROUTINE_NAME='存储过程或函数的名' [AND ROUTINE_TYPE = {'PROCEDURE|FUNCTION'}];
```

说明：如果在MySQL数据库中存在存储过程和函数名称相同的情况，最好指定ROUTINE_TYPE查询条件来 指明查询的是存储过程还是函数。

###  修改

修改存储过程或函数，不影响存储过程或函数功能，只是修改相关特性。使用ALTER语句实现。

```mysql
ALTER {PROCEDURE | FUNCTION} 存储过程或函数的名 [characteristic ...]
```

其中，characteristic指定存储过程或函数的特性，其取值信息与创建存储过程、函数时的取值信息略有不同。

```mysql
{ CONTAINS SQL | NO SQL | READS SQL DATA | MODIFIES SQL DATA }
| SQL SECURITY { DEFINER | INVOKER }
| COMMENT 'string'
```

* CONTAINS SQL ，表示子程序包含SQL语句，但不包含读或写数据的语句。 
* NO SQL ，表示子程序中不包含SQL语句。 
* READS SQL DATA ，表示子程序中包含读数据的语句。 
* MODIFIES SQL DATA ，表示子程序中包含写数据的语句。 
* SQL SECURITY { DEFINER | INVOKER } ，指明谁有权限来执行。 
	* DEFINER ，表示只有定义者自己才能够执行。 
	* INVOKER ，表示调用者可以执行。 

* COMMENT 'string' ，表示注释信息。

> 修改存储过程使用ALTER PROCEDURE语句，修改存储函数使用ALTER FUNCTION语句。但是，这两 个语句的结构是一样的，语句中的所有参数也是一样的。

###  删除

删除存储过程和函数，可以使用DROP语句，其语法结构如下：

```mysql
DROP {PROCEDURE | FUNCTION} [IF EXISTS] 存储过程或函数的名
```

## 关于存储过程使用的争议

###  优点

1、存储过程可以一次编译多次使用。存储过程只在创建时进行编译，之后的使用都不需要重新编译， 这就提升了 SQL 的执行效率。

2、可以减少开发工作量。将代码 封装 成模块，实际上是编程的核心思想之一，这样可以把复杂的问题 拆解成不同的模块，然后模块之间可以 重复使用 ，在减少开发工作量的同时，还能保证代码的结构清 晰。 

3、存储过程的安全性强。我们在设定存储过程的时候可以 设置对用户的使用权限 ，这样就和视图一样具 有较强的安全性。 

4、可以减少网络传输量。因为代码封装到存储过程中，每次使用只需要调用存储过程即可，这样就减 少了网络传输量。 

5、良好的封装性。在进行相对复杂的数据库操作时，原本需要使用一条一条的 SQL 语句，可能要连接 多次数据库才能完成的操作，现在变成了一次存储过程，只需要 连接一次即可 。

###  缺点

> 阿里开发规范 【强制】禁止使用存储过程，存储过程难以调试和扩展，更没有移植性。

1、可移植性差。存储过程不能跨数据库移植，比如在 MySQL、Oracle 和 SQL Server 里编写的存储过 程，在换成其他数据库时都需要重新编写。 

2、调试困难。只有少数 DBMS 支持存储过程的调试。对于复杂的存储过程来说，开发和维护都不容 易。虽然也有一些第三方工具可以对存储过程进行调试，但要收费。 

3、存储过程的版本管理很困难。比如数据表索引发生变化了，可能会导致存储过程失效。我们在开发 软件的时候往往需要进行版本管理，但是存储过程本身没有版本控制，版本迭代更新的时候很麻烦。 

4、它不适合高并发的场景。高并发的场景需要减少数据库的压力，有时数据库会采用分库分表的方式，而且对可扩展性要求很高，在这种情况下，存储过程会变得难以维护， 增加数据库的压力 ，显然就不适用了。

### 小结

存储过程既方便，又有局限性。尽管不同的公司对存储过程的态度不一，但是对于我们开发人员来说， 不论怎样，掌握存储过程都是必备的技能之一。

# 变量、流程控制、游标

在MySQL数据库的存储过程和函数中，可以使用变量来存储查询或计算的中间结果数据，或者输出最终的结果数据。

## 变量

在MySQL数据库的存储过程和函数中，可以使用变量来存储查询或计算的中间结果数据，或者输出最终 的结果数据。 

在 MySQL 数据库中，变量分为 系统变量 以及 用户自定义变量 。

### 系统变量

**系统变量分类**

变量由系统定义，不是用户定义，属于 服务器 层面。启动MySQL服务，生成MySQL服务实例期间， MySQL将为MySQL服务器内存中的系统变量赋值，这些系统变量定义了当前MySQL服务实例的属性、特 征。这些系统变量的值要么是 编译MySQL时参数 的默认值，要么是 配置文件 （例如my.ini等）中的参数 值。大家可以通过网址 https://dev.mysql.com/doc/refman/8.0/en/server-systemvariables.html 查看MySQL文档的系统变量。

系统变量分为全局系统变量（需要添加 global 关键字）以及会话系统变量（需要添加 session 关键字），有时也把全局系统变量简称为全局变量，有时也把会话系统变量称为local变量。如果不写，默认会话级别。静态变量（在 MySQL 服务实例运行期间它们的值不能使用 set 动态修改）属于特殊的全局系统变量。

每一个MySQL客户机成功连接MySQL服务器后，都会产生与之对应的会话。会话期间，MySQL服务实例会在MySQL服务器内存中生成与该会话对应的会话系统变量，这些会话系统变量的初始值是全局系统变量值的复制。如下图：

* 全局系统变量针对于所有会话（连接）有效，但 不能跨重启
* 会话系统变量仅针对于当前会话（连接）有效。会话期间，当前会话对某个会话系统变量值的修改，不会影响其他会话同一个会话系统变量的值。 
* 会话1对某个全局系统变量值的修改会导致会话2中同一个全局系统变量值的修改。

在MySQL中有些系统变量只能是全局的，例如 max_connections 用于限制服务器的最大连接数；有些系 统变量作用域既可以是全局又可以是会话，例如 character_set_client 用于设置客户端的字符集；有些系 统变量的作用域只能是当前会话，例如 pseudo_thread_id 用于标记当前会话的 MySQL 连接 ID。

**查看系统变量**

* 查看所有或部分系统变量

```mysql
#查看所有全局变量
SHOW GLOBAL VARIABLES;
#查看所有会话变量
SHOW SESSION VARIABLES;
或
SHOW VARIABLES;
```

```mysql
#查看满足条件的部分系统变量。
SHOW GLOBAL VARIABLES LIKE '%标识符%';
#查看满足条件的部分会话变量
SHOW SESSION VARIABLES LIKE '%标识符%';
```

**查看指定系统变量**

作为 MySQL 编码规范，MySQL 中的系统变量以 两个“@” 开头，其中“@@global”仅用于标记全局系统变量，“@@session”仅用于标记会话系统变量。“@@”首先标记会话系统变量，如果会话系统变量不存在， 则标记全局系统变量。

```mysql
#查看指定的系统变量的值
SELECT @@global.变量名;
#查看指定的会话变量的值
SELECT @@session.变量名;
#或者
SELECT @@变量名;
```

**修改系统变量的值**

有些时候，数据库管理员需要修改系统变量的默认值，以便修改当前会话或者MySQL服务实例的属性、 特征。具体方法：

方式1：修改MySQL 配置文件 ，继而修改MySQL系统变量的值（该方法需要重启MySQL服务） 

方式2：在MySQL服务运行期间，使用“set”命令重新设置系统变量的值

```mysql
#为某个系统变量赋值
#方式1：
SET @@global.变量名=变量值;
#方式2：
SET GLOBAL 变量名=变量值;
#为某个会话变量赋值
#方式1：
SET @@session.变量名=变量值;
#方式2：
SET SESSION 变量名=变量值;
```

###  用户变量

**用户变量分类**

用户变量是用户自己定义的，作为 MySQL 编码规范，MySQL 中的用户变量以一个“@” 开头。根据作用范围不同，又分为 会话用户变量 和 局部变量 。 

* 会话用户变量：作用域和会话变量一样，只对 当前连接 会话有效。 
* 局部变量：只在 BEGIN 和 END 语句块中有效。局部变量只能在 存储过程和函数 中使用。

**会话用户变量**

* 变量的定义

```mysql
#方式1：“=”或“:=”
SET @用户变量 = 值;
SET @用户变量 := 值;
#方式2：“:=” 或 INTO关键字
SELECT @用户变量 := 表达式 [FROM 等子句];
SELECT 表达式 INTO @用户变量 [FROM 等子句];
```

* 查看用户变量的值 (查看、比较、运算等)

```mysql
SELECT @用户变量
```

**局部变量**

定义：可以使用 DECLARE 语句定义一个局部变量 

作用域：仅仅在定义它的 BEGIN ... END 中有效 

位置：只能放在 BEGIN ... END 中，而且只能放在第一句

```mysql
BEGIN
#声明局部变量
DECLARE 变量名1 变量数据类型 [DEFAULT 变量默认值];
DECLARE 变量名2,变量名3,... 变量数据类型 [DEFAULT 变量默认值];
#为局部变量赋值
SET 变量名1 = 值;
SELECT 值 INTO 变量名2 [FROM 子句];
#查看局部变量的值
SELECT 变量1,变量2,变量3;
END
```

1. 定义变量

```mysql
DECLARE 变量名 类型 [default 值]; # 如果没有DEFAULT子句，初始值为NULL
```

2. 变量赋值

方式1：一般用于赋简单的值

```mysql
SET 变量名=值;
SET 变量名:=值;
```

方式2：一般用于赋表中的字段值

```mysql
SELECT 字段名或表达式 INTO 变量名 FROM 表;
```

3. 使用变量 (查看、比较、运算等)

```mysql
SELECT 局部变量名;
```

举例1：声明局部变量，并分别赋值为employees表中employee_id为102的last_name和salary

```mysql
DELIMITER //
CREATE PROCEDURE set_value()
BEGIN
DECLARE emp_name VARCHAR(25);
DECLARE sal DOUBLE(10,2);
SELECT last_name, salary INTO emp_name,sal
FROM employees
WHERE employee_id = 102;
SELECT emp_name, sal;
END //
DELIMITER ;
```

举例2：声明两个变量，求和并打印 （分别使用会话用户变量、局部变量的方式实现）

```mysql
#方式1：使用用户变量
SET @m=1;
SET @n=1;
SET @sum=@m+@n;
SELECT @sum;
```

```mysql
#方式2：使用局部变量
DELIMITER //
CREATE PROCEDURE add_value()
BEGIN
#局部变量
DECLARE m INT DEFAULT 1;
DECLARE n INT DEFAULT 3;
DECLARE SUM INT;
SET SUM = m+n;
SELECT SUM;
END //
DELIMITER ;
```

**对比会话用户变量与局部变量**

|              | 作用域              | 定义位置            | 语法                     |
| ------------ | ------------------- | ------------------- | ------------------------ |
| 会话用户变量 | 当前会话            | 会话的任何地方      | 加@符号，不用指定类型    |
| 局部变量     | 定义它的BEGIN END中 | BEGIN END的第一句话 | 一般不用加@,需要指定类型 |

##  定义条件与处理程序

定义条件 是事先定义程序执行过程中可能遇到的问题， 处理程序 定义了在遇到问题时应当采取的处理方式，并且保证存储过程或函数在遇到警告或错误时能继续执行。这样可以增强存储程序处理问题的能力，避免程序异常停止运行。

说明：定义条件和处理程序在存储过程、存储函数中都是支持的。

###  案例分析

案例分析：创建一个名称为“UpdateDataNoCondition”的存储过程。代码如下：

```mysql
DELIMITER //
CREATE PROCEDURE UpdateDataNoCondition()
BEGIN
SET @x = 1;
UPDATE employees SET email = NULL WHERE last_name = 'Abel';
SET @x = 2;
UPDATE employees SET email = 'aabbel' WHERE last_name = 'Abel';
SET @x = 3;
END //
DELIMITER ;
```

调用存储过程：

```mysql
mysql> CALL UpdateDataNoCondition();
ERROR 1048 (23000): Column 'email' cannot be null
mysql> SELECT @x;
+------+
| @x |
+------+
| 1 |
+------+
1 row in set (0.00 sec)
```

可以看到，此时@x变量的值为1。结合创建存储过程的SQL语句代码可以得出：在存储过程中未定义条件 和处理程序，且当存储过程中执行的SQL语句报错时，MySQL数据库会抛出错误，并退出当前SQL逻辑， 不再向下继续执行。

### 定义条件

定义条件就是给MySQL中的错误码命名，这有助于存储的程序代码更清晰。它将一个 错误名字 和 指定的 错误条件 关联起来。这个名字可以随后被用在定义处理程序的 DECLARE HANDLER 语句中。

定义条件使用DECLARE语句，语法格式如下：

```mysql
DECLARE 错误名称 CONDITION FOR 错误码（或错误条件）
```

错误码的说明：

+ MySQL_error_code 和 sqlstate_value 都可以表示MySQL的错误。
	+ MySQL_error_code是数值类型错误代码。 
	+ sqlstate_value是长度为5的字符串类型错误代码。 

例如，在ERROR 1418 (HY000)中，1418是MySQL_error_code，'HY000'是sqlstate_value。 

例如，在ERROR 1142（42000）中，1142是MySQL_error_code，'42000'是sqlstate_value。

举例1：定义“Field_Not_Be_NULL”错误名与MySQL中违反非空约束的错误类型是“ERROR 1048 (23000)”对应。

```mysql
#使用MySQL_error_code
DECLARE Field_Not_Be_NULL CONDITION FOR 1048;
#使用sqlstate_value
DECLARE Field_Not_Be_NULL CONDITION FOR SQLSTATE '23000';
```

###  定义处理程序

可以为SQL执行过程中发生的某种类型的错误定义特殊的处理程序。定义处理程序时，使用DECLARE语句 的语法如下：

```mysql
DECLARE 处理方式 HANDLER FOR 错误类型 处理语句
```

* 处理方式：处理方式有3个取值：CONTINUE、EXIT、UNDO。
	* CONTINUE ：表示遇到错误不处理，继续执行。
	* EXIT ：表示遇到错误马上退出。
	* UNDO ：表示遇到错误后撤回之前的操作。MySQL中暂时不支持这样的操作。
* 错误类型（即条件）可以有如下取值：
	* SQLSTATE '字符串错误码' ：表示长度为5的sqlstate_value类型的错误代码； 
	* MySQL_error_code ：匹配数值类型错误代码； 
	* 错误名称 ：表示DECLARE ... CONDITION定义的错误条件名称。 
	* SQLWARNING ：匹配所有以01开头的SQLSTATE错误代码； 
	* NOT FOUND ：匹配所有以02开头的SQLSTATE错误代码； 
	* SQLEXCEPTION ：匹配所有没有被SQLWARNING或NOT FOUND捕获的SQLSTATE错误代码；

* 处理语句：如果出现上述条件之一，则采用对应的处理方式，并执行指定的处理语句。语句可以是 像“ SET 变量 = 值 ”这样的简单语句，也可以是使用 BEGIN ... END 编写的复合语句。

定义处理程序的几种方式，代码如下：

```mysql
#方法1：捕获sqlstate_value
DECLARE CONTINUE HANDLER FOR SQLSTATE '42S02' SET @info = 'NO_SUCH_TABLE';
#方法2：捕获mysql_error_value
DECLARE CONTINUE HANDLER FOR 1146 SET @info = 'NO_SUCH_TABLE';
#方法3：先定义条件，再调用
DECLARE no_such_table CONDITION FOR 1146;
DECLARE CONTINUE HANDLER FOR NO_SUCH_TABLE SET @info = 'NO_SUCH_TABLE';
#方法4：使用SQLWARNING
DECLARE EXIT HANDLER FOR SQLWARNING SET @info = 'ERROR';
#方法5：使用NOT FOUND
DECLARE EXIT HANDLER FOR NOT FOUND SET @info = 'NO_SUCH_TABLE';
#方法6：使用SQLEXCEPTION
DECLARE EXIT HANDLER FOR SQLEXCEPTION SET @info = 'ERROR';
```

###  案例解决

在存储过程中，定义处理程序，捕获sqlstate_value值，当遇到MySQL_error_code值为1048时，执行 CONTINUE操作，并且将@proc_value的值设置为-1。

```mysql
DELIMITER //
CREATE PROCEDURE UpdateDataNoCondition()
BEGIN
    #定义处理程序
    DECLARE CONTINUE HANDLER FOR 1048 SET @proc_value = -1;
    SET @x = 1;
    UPDATE employees SET email = NULL WHERE last_name = 'Abel';
    SET @x = 2;
    UPDATE employees SET email = 'aabbel' WHERE last_name = 'Abel';
    SET @x = 3;
END //
DELIMITER ;
```

##  流程控制

解决复杂问题不可能通过一个 SQL 语句完成，我们需要执行多个 SQL 操作。流程控制语句的作用就是控 制存储过程中 SQL 语句的执行顺序，是我们完成复杂操作必不可少的一部分。只要是执行的程序，流程就分为三大类：

* 顺序结构 ：程序从上往下依次执行 
* 分支结构 ：程序按条件进行选择执行，从两条或多条路径中选择一条执行 
* 循环结构 ：程序满足一定条件下，重复执行一组语句

针对于MySQL 的流程控制语句主要有 3 类。注意：只能用于存储程序。

* 条件判断语句 ：IF 语句和 CASE 语句 
* 循环语句 ：LOOP、WHILE 和 REPEAT 语句 
* 跳转语句 ：ITERATE 和 LEAVE 语句

### 分支结构之 IF

* IF 语句的语法结构是：

```mysql
IF 表达式1 THEN 操作1
[ELSEIF 表达式2 THEN 操作2]……
[ELSE 操作N]
END IF
```

根据表达式的结果为TRUE或FALSE执行相应的语句。这里“[]”中的内容是可选的。

* 特点：① 不同的表达式对应不同的操作 ② 使用在begin end中

* 举例1：

```mysql
IF val IS NULL
	THEN SELECT 'val is null';
ELSE SELECT 'val is not null';
END IF;
```

* 举例2：声明存储过程“update_salary_by_eid1”，定义IN参数emp_id，输入员工编号。判断该员工薪资如果低于8000元并且入职时间超过5年，就涨薪500元；否则就不变。

```mysql
DELIMITER //
CREATE PROCEDURE update_salary_by_eid1(IN emp_id INT)
BEGIN
    DECLARE emp_salary DOUBLE;
    DECLARE hire_year DOUBLE;
    SELECT salary INTO emp_salary FROM employees WHERE employee_id = emp_id;
    SELECT DATEDIFF(CURDATE(),hire_date)/365 INTO hire_year
    FROM employees WHERE employee_id = emp_id;
    IF emp_salary < 8000 AND hire_year > 5
    THEN UPDATE employees SET salary = salary + 500 WHERE employee_id = emp_id;
    END IF;
END //
DELIMITER ;
```

### 分支结构之 CASE

* CASE 语句的语法结构1：

```mysql
#情况一：类似于switch
CASE 表达式
WHEN 值1 THEN 结果1或语句1(如果是语句，需要加分号)
WHEN 值2 THEN 结果2或语句2(如果是语句，需要加分号)
...
ELSE 结果n或语句n(如果是语句，需要加分号)
END [case]（如果是放在begin end中需要加上case，如果放在select后面不需要）
```

* CASE 语句的语法结构2：

```mysql
#情况二：类似于多重if
CASE
WHEN 条件1 THEN 结果1或语句1(如果是语句，需要加分号)
WHEN 条件2 THEN 结果2或语句2(如果是语句，需要加分号)
...
ELSE 结果n或语句n(如果是语句，需要加分号)
END [case]（如果是放在begin end中需要加上case，如果放在select后面不需要）
```

* 举例1：使用CASE流程控制语句的第1种格式，判断val值等于1、等于2，或者两者都不等。

```mysql
CASE val
    WHEN 1 THEN SELECT 'val is 1';
    WHEN 2 THEN SELECT 'val is 2';
    ELSE SELECT 'val is not 1 or 2';
END CASE;
```

* 举例2：声明存储过程“update_salary_by_eid4”，定义IN参数emp_id，输入员工编号。判断该员工 薪资如果低于9000元，就更新薪资为9000元；薪资大于等于9000元且低于10000的，但是奖金比例 为NULL的，就更新奖金比例为0.01；其他的涨薪100元。

```mysql
DELIMITER //
CREATE PROCEDURE update_salary_by_eid4(IN emp_id INT)
BEGIN
    DECLARE emp_sal DOUBLE;
    DECLARE bonus DECIMAL(3,2);
    SELECT salary INTO emp_sal FROM employees WHERE employee_id = emp_id;
    SELECT commission_pct INTO bonus FROM employees WHERE employee_id = emp_id;
    CASE
    WHEN emp_sal<9000
    	THEN UPDATE employees SET salary=9000 WHERE employee_id = emp_id;
    WHEN emp_sal<10000 AND bonus IS NULL
    	THEN UPDATE employees SET commission_pct=0.01 WHERE employee_id = emp_id;
    ELSE
    	UPDATE employees SET salary=salary+100 WHERE employee_id = emp_id;
    END CASE;
END //
DELIMITER ;
```

* 举例3：声明存储过程update_salary_by_eid5，定义IN参数emp_id，输入员工编号。判断该员工的 入职年限，如果是0年，薪资涨50；如果是1年，薪资涨100；如果是2年，薪资涨200；如果是3年， 薪资涨300；如果是4年，薪资涨400；其他的涨薪500。

```mysql
DELIMITER //
CREATE PROCEDURE update_salary_by_eid5(IN emp_id INT)
BEGIN
    DECLARE emp_sal DOUBLE;
    DECLARE hire_year DOUBLE;
    SELECT salary INTO emp_sal FROM employees WHERE employee_id = emp_id;
    SELECT ROUND(DATEDIFF(CURDATE(),hire_date)/365) INTO hire_year FROM employees
    WHERE employee_id = emp_id;
    CASE hire_year
        WHEN 0 THEN UPDATE employees SET salary=salary+50 WHERE employee_id = emp_id;
        WHEN 1 THEN UPDATE employees SET salary=salary+100 WHERE employee_id = emp_id;
        WHEN 2 THEN UPDATE employees SET salary=salary+200 WHERE employee_id = emp_id;
        WHEN 3 THEN UPDATE employees SET salary=salary+300 WHERE employee_id = emp_id;
        WHEN 4 THEN UPDATE employees SET salary=salary+400 WHERE employee_id = emp_id;
        ELSE UPDATE employees SET salary=salary+500 WHERE employee_id = emp_id;
    END CASE;
END //
DELIMITER ;
```

###  循环结构之LOOP

LOOP循环语句用来重复执行某些语句。LOOP内的语句一直重复执行直到循环被退出（使用LEAVE子 句），跳出循环过程。

LOOP语句的基本格式如下：

```mysql
[loop_label:] LOOP
循环执行的语句
END LOOP [loop_label]
```

其中，loop_label表示LOOP语句的标注名称，该参数可以省略。

举例1：使用LOOP语句进行循环操作，id值小于10时将重复执行循环过程。

```mysql
DECLARE id INT DEFAULT 0;
add_loop:LOOP
    SET id = id +1;
    IF id >= 10 THEN LEAVE add_loop;
    END IF;
END LOOP add_loop;
```

举例2：当市场环境变好时，公司为了奖励大家，决定给大家涨工资。声明存储过程 “update_salary_loop()”，声明OUT参数num，输出循环次数。存储过程中实现循环给大家涨薪，薪资涨为 原来的1.1倍。直到全公司的平均薪资达到12000结束。并统计循环次数。

```mysql
DELIMITER //
CREATE PROCEDURE update_salary_loop(OUT num INT)
BEGIN
	DECLARE avg_salary DOUBLE;
	DECLARE loop_count INT DEFAULT 0;
	SELECT AVG(salary) INTO avg_salary FROM employees;
	label_loop:LOOP
        IF avg_salary >= 12000 THEN LEAVE label_loop;
        END IF;
        UPDATE employees SET salary = salary * 1.1;
        SET loop_count = loop_count + 1;
        SELECT AVG(salary) INTO avg_salary FROM employees;
    END LOOP label_loop;
    SET num = loop_count;
END //
DELIMITER ;
```

### 循环结构之WHILE

WHILE语句创建一个带条件判断的循环过程。WHILE在执行语句执行时，先对指定的表达式进行判断，如 果为真，就执行循环内的语句，否则退出循环。WHILE语句的基本格式如下：

```mysql
[while_label:] WHILE 循环条件 DO
循环体
END WHILE [while_label];
```

while_label为WHILE语句的标注名称；如果循环条件结果为真，WHILE语句内的语句或语句群被执行，直 至循环条件为假，退出循环。

* 举例1：WHILE语句示例，i值小于10时，将重复执行循环过程，代码如下：

```mysql
DELIMITER //
CREATE PROCEDURE test_while()
BEGIN
    DECLARE i INT DEFAULT 0;
    WHILE i < 10 DO
    	SET i = i + 1;
    END WHILE;
    SELECT i;
END //
DELIMITER ;
#调用
CALL test_while();
```

* 举例2：市场环境不好时，公司为了渡过难关，决定暂时降低大家的薪资。声明存储过程 “update_salary_while()”，声明OUT参数num，输出循环次数。存储过程中实现循环给大家降薪，薪资降 为原来的90%。直到全公司的平均薪资达到5000结束。并统计循环次数。

```mysql
DELIMITER //
CREATE PROCEDURE update_salary_while(OUT num INT)
BEGIN
    DECLARE avg_sal DOUBLE ;
    DECLARE while_count INT DEFAULT 0;
    SELECT AVG(salary) INTO avg_sal FROM employees;
    WHILE avg_sal > 5000 DO
        UPDATE employees SET salary = salary * 0.9;
        SET while_count = while_count + 1;
        SELECT AVG(salary) INTO avg_sal FROM employees;
    END WHILE;
    SET num = while_count;
END //
DELIMITER ;
```

### 循环结构之REPEAT

REPEAT语句创建一个带条件判断的循环过程。与WHILE循环不同的是，REPEAT 循环首先会执行一次循环，然后在 UNTIL 中进行表达式的判断，如果满足条件就退出，即 END REPEAT；如果条件不满足，则会就继续执行循环，直到满足退出条件为止。

REPEAT语句的基本格式如下：

```mysql
[repeat_label:] REPEAT
循环体的语句
UNTIL 结束循环的条件表达式
END REPEAT [repeat_label]
```

repeat_label为REPEAT语句的标注名称，该参数可以省略；REPEAT语句内的语句或语句群被重复，直至 expr_condition为真。

举例1：

```mysql
DELIMITER //
CREATE PROCEDURE test_repeat()
BEGIN
    DECLARE i INT DEFAULT 0;
    REPEAT
    	SET i = i + 1;
    UNTIL i >= 10
    END REPEAT;
    SELECT i;
END //
DELIMITER ;
```

举例2：当市场环境变好时，公司为了奖励大家，决定给大家涨工资。声明存储过程 “update_salary_repeat()”，声明OUT参数num，输出循环次数。存储过程中实现循环给大家涨薪，薪资涨 为原来的1.15倍。直到全公司的平均薪资达到13000结束。并统计循环次数。

```mysql
DELIMITER //
CREATE PROCEDURE update_salary_repeat(OUT num INT)
BEGIN
    DECLARE avg_sal DOUBLE ;
    DECLARE repeat_count INT DEFAULT 0;
    SELECT AVG(salary) INTO avg_sal FROM employees;
    REPEAT
    	UPDATE employees SET salary = salary * 1.15;
    	SET repeat_count = repeat_count + 1;
    	SELECT AVG(salary) INTO avg_sal FROM employees;
    UNTIL avg_sal >= 13000
    END REPEAT;
    SET num = repeat_count;
END //
DELIMITER ;
```

**对比三种循环结构：**

1. 这三种循环都可以省略名称，但如果循环中添加了循环控制语句（LEAVE或ITERATE）则必须添加名称。 

2. LOOP：一般用于实现简单的"死"循环 WHILE：先判断后执行 

3. REPEAT：先执行后判断，无条件至少执行一次

###  跳转语句之LEAVE语句

LEAVE语句：可以用在循环语句内，或者以 BEGIN 和 END 包裹起来的程序体内，表示跳出循环或者跳出 程序体的操作。如果你有面向过程的编程语言的使用经验，你可以把 LEAVE 理解为 break。

基本格式如下：

```mysql
LEAVE 标记名
```

其中，label参数表示循环的标志。LEAVE和BEGIN ... END或循环一起被使用。

举例1：创建存储过程 “leave_begin()”，声明INT类型的IN参数num。给BEGIN...END加标记名，并在 BEGIN...END中使用IF语句判断num参数的值。

如果num<=0，则使用LEAVE语句退出BEGIN...END； 如果num=1，则查询“employees”表的平均薪资； 如果num=2，则查询“employees”表的最低薪资； 如果num>2，则查询“employees”表的最高薪资。

IF语句结束后查询“employees”表的总人数。

```mysql
DELIMITER //
CREATE PROCEDURE leave_begin(IN num INT)
    begin_label: BEGIN
        IF num<=0
        	THEN LEAVE begin_label;
        ELSEIF num=1
        	THEN SELECT AVG(salary) FROM employees;
        ELSEIF num=2
        	THEN SELECT MIN(salary) FROM employees;
        ELSE
        	SELECT MAX(salary) FROM employees;
        END IF;
        SELECT COUNT(*) FROM employees;
    END //
DELIMITER ;
```

举例2： 当市场环境不好时，公司为了渡过难关，决定暂时降低大家的薪资。声明存储过程“leave_while()”，声明 OUT参数num，输出循环次数，存储过程中使用WHILE循环给大家降低薪资为原来薪资的90%，直到全公司的平均薪资小于等于10000，并统计循环次数。

```mysql
DELIMITER //
CREATE PROCEDURE leave_while(OUT num INT)
BEGIN
    DECLARE avg_sal DOUBLE;#记录平均工资
    DECLARE while_count INT DEFAULT 0; #记录循环次数
    SELECT AVG(salary) INTO avg_sal FROM employees; #① 初始化条件
    while_label:WHILE TRUE DO #② 循环条件
    #③ 循环体
    IF avg_sal <= 10000 THEN
    LEAVE while_label;
    END IF;
    UPDATE employees SET salary = salary * 0.9;
    SET while_count = while_count + 1;
    #④ 迭代条件
    SELECT AVG(salary) INTO avg_sal FROM employees;
    END WHILE;
    #赋值
    SET num = while_count;
END //
DELIMITER ;
```

### 跳转语句之ITERATE语句

ITERATE语句：只能用在循环语句（LOOP、REPEAT和WHILE语句）内，表示重新开始循环，将执行顺序转到语句段开头处。如果你有面向过程的编程语言的使用经验，你可以把 ITERATE 理解为 continue，意思为“再次循环”。

语句基本格式如下：

```mysql
ITERATE label
```

label参数表示循环的标志。ITERATE语句必须跟在循环标志前面。

举例： 定义局部变量num，初始值为0。循环结构中执行num + 1操作。

* 如果num < 10，则继续执行循环；
* 如果num > 15，则退出循环结构；

```mysql
DELIMITER //
CREATE PROCEDURE test_iterate()
BEGIN
    DECLARE num INT DEFAULT 0;
    my_loop:LOOP
    	SET num = num + 1;
        IF num < 10
        	THEN ITERATE my_loop;
        ELSEIF num > 15
        	THEN LEAVE my_loop;
        END IF;
        SELECT 'MySQL';
    END LOOP my_loop;
END //
DELIMITER ;
```

##  游标

###   什么是游标（或光标）

虽然我们也可以通过筛选条件 WHERE 和 HAVING，或者是限定返回记录的关键字 LIMIT 返回一条记录， 但是，却无法在结果集中像指针一样，向前定位一条记录、向后定位一条记录，或者是随意定位到某一 条记录 ，并对记录的数据进行处理。

这个时候，就可以用到游标。游标，提供了一种灵活的操作方式，让我们能够对结果集中的每一条记录进行定位，并对指向的记录中的数据进行操作的数据结构。游标让 SQL 这种面向集合的语言有了面向过程开发的能力。

在 SQL 中，游标是一种临时的数据库对象，可以指向存储在数据库表中的数据行指针。这里游标 充当了 指针的作用 ，我们可以通过操作游标来对数据行进行操作。

MySQL中游标可以在存储过程和函数中使用。 

###  使用游标步骤

游标必须在声明处理程序之前被声明，并且变量和条件还必须在声明游标或处理程序之前被声明。 

如果我们想要**使用游标，**一般需要经历四个步骤。不同的 DBMS 中，使用游标的语法可能略有不同。

**第一步，声明游标**

在MySQL中，使用DECLARE关键字来声明游标，其语法的基本形式如下：

```mysql
DECLARE cursor_name CURSOR FOR select_statement;
```

这个语法适用于 MySQL，SQL Server，DB2 和 MariaDB。如果是用 Oracle 或者 PostgreSQL，需要写成：

```mysql
DECLARE cursor_name CURSOR IS select_statement;
```

要使用 SELECT 语句来获取数据结果集，而此时还没有开始遍历数据，这里 select_statement 代表的是 SELECT 语句，返回一个用于创建游标的结果集。

比如：

```mysql
DECLARE cur_emp CURSOR FOR
SELECT employee_id,salary FROM employees;
```

**第二步，打开游标**

打开游标的语法如下：

```mysql
OPEN cursor_name
```

当我们定义好游标之后，如果想要使用游标，必须先打开游标。打开游标的时候 SELECT 语句的查询结果集就会送到游标工作区，为后面游标的 逐条读取 结果集中的记录做准备。

```mysql
OPEN cur_emp;
```

**第三步，使用游标（从游标中取得数据）**

语法如下：

```mysql
FETCH cursor_name INTO var_name [, var_name] ...
```

这句的作用是使用 cursor_name 这个游标来读取当前行，并且将数据保存到 var_name 这个变量中，游标指针指到下一行。如果游标读取的数据行有多个列名，则在 INTO 关键字后面赋值给多个变量名即可。

注意：var_name必须在声明游标之前就定义好。

```mysql
FETCH cur_emp INTO emp_id, emp_sal ;
```

注意：**游标的查询结果集中的字段数，必须跟 INTO 后面的变量数一致**，否则，在存储过程执行的时 候，MySQL 会提示错误。

**第四步，关闭游标**

```mysql
CLOSE cursor_name
```

有 OPEN 就会有 CLOSE，也就是打开和关闭游标。当我们使用完游标后需要关闭掉该游标。因为游标会 占用系统资源 ，如果不及时关闭，游标会一直保持到存储过程结束，影响系统运行的效率。而关闭游标 的操作，会释放游标占用的系统资源。

关闭游标之后，我们就不能再检索查询结果中的数据行，如果需要检索只能再次打开游标。

```mysql
CLOSE cur_emp;
```

###  举例

创建存储过程“get_count_by_limit_total_salary()”，声明IN参数 limit_total_salary，DOUBLE类型；声明 OUT参数total_count，INT类型。函数的功能可以实现累加薪资最高的几个员工的薪资值，直到薪资总和达到limit_total_salary参数的值，返回累加的人数给total_count。

```mysql
DELIMITER //
CREATE PROCEDURE get_count_by_limit_total_salary(IN limit_total_salary DOUBLE, OUT total_count INT)
BEGIN
	DECLARE sum_salary DOUBLE DEFAULT 0; # 记录累加的总工资
	DECLARE cursor_salary DOUBLE DEFAULT 0; # 记录某一个工资值
	DECLARE emp_count INT DEFAULT 0; # 记录循环个数
	# 定义游标
	DECLARE emp_cursor CURSOR FOR SELECT salary FROM employees ORDER BY salary DESC;
	# 打开游标
	OPEN emp_cursor;
	
	REPEAT
		# 使用游标(从游标中获取数据)
		FETCH emp_cursor INTO cursor_salary;
		SET sum_salary = sum_salary + cursor_salary;
		SET emp_count = emp_count + 1;
		UNTIL sum_salary >= limit_total_salary
	END REPEAT;
	set total_count = emp_count;
	# 关闭游标
	CLOSE emp_cursor;
END //
DELIMITER;
```

###  小结

游标是 MySQL 的一个重要的功能，为 逐条读取 结果集中的数据，提供了完美的解决方案。跟在应用层面实现相同的功能相比，游标可以在存储程序中使用，效率高，程序也更加简洁。 

但同时也会带来一些性能问题，比如在使用游标的过程中，会对数据行进行 加锁 ，这样在业务并发量大 的时候，不仅会影响业务之间的效率，还会 消耗系统资源 ，造成内存不足，这是因为游标是在内存中进行的处理。 

建议：养成用完之后就关闭的习惯，这样才能提高系统的整体效率。

## 补充：MySQL 8.0的新特性—全局变量的持久化

在MySQL数据库中，全局变量可以通过SET GLOBAL语句来设置。例如，设置服务器语句超时的限制，可 以通过设置系统变量max_execution_time来实现：

```mysql
SET GLOBAL MAX_EXECUTION_TIME=2000;
```

使用SET GLOBAL语句设置的变量值只会 临时生效 。 数据库重启 后，服务器又会从MySQL配置文件中读取 变量的默认值。 MySQL 8.0版本新增了 SET PERSIST 命令。例如，设置服务器的最大连接数为1000：

```mysql
SET PERSIST global max_connections = 1000;
```

MySQL会将该命令的配置保存到数据目录下的 mysqld-auto.cnf 文件中，下次启动时会读取该文件，用其中的配置来覆盖默认的配置文件。

# 触发器

在实际开发中，我们经常会遇到这样的情况：有 2 个或者多个相互关联的表，如 商品信息 和 库存信息 分 别存放在 2 个不同的数据表中，我们在添加一条新商品记录的时候，为了保证数据的完整性，必须同时 在库存表中添加一条库存记录。 

这样一来，我们就必须把这两个关联的操作步骤写到程序里面，而且要用 事务 包裹起来，确保这两个操 作成为一个 原子操作 ，要么全部执行，要么全部不执行。要是遇到特殊情况，可能还需要对数据进行手动维护，这样就很 容易忘记其中的一步 ，导致数据缺失。 

这个时候，咱们可以使用触发器。你可以创建一个触发器，让商品信息数据的插入操作自动触发库存数据的插入操作。这样一来，就不用担心因为忘记添加库存数据而导致的数据缺失了。

## 触发器概述

触发器是由 事件来触发 某个操作，这些事件包括 INSERT 、 UPDATE 、 DELETE 事件。所谓事件就是指用户的动作或者触发某项行为。如果定义了触发程序，当数据库执行这些语句时候，就相当于事件发生 了，就会 自动 激发触发器执行相应的操作。

当对数据表中的数据执行插入、更新和删除操作，需要自动执行一些数据库逻辑时，可以使用触发器来实现。

## 触发器的创建

###  语法

```mysql
CREATE TRIGGER 触发器名称
{BEFORE|AFTER} {INSERT|UPDATE|DELETE} ON 表名
FOR EACH ROW
触发器执行的语句块
```

说明：

* 表名 ：表示触发器监控的对象。 
* BEFORE|AFTER ：表示触发的时间。BEFORE 表示在事件之前触发；AFTER 表示在事件之后触发。 
* INSERT|UPDATE|DELETE ：表示触发的事件。
	* INSERT 表示插入记录时触发； 
	* UPDATE 表示更新记录时触发； 
	* DELETE 表示删除记录时触发。
* 触发器执行的语句块 ：可以是单条SQL语句，也可以是由BEGIN…END结构组成的复合语句块。

###  代码举例

**举例1：**

1. 创建数据表：

```mysql
CREATE TABLE test_trigger (
id INT PRIMARY KEY AUTO_INCREMENT,
t_note VARCHAR(30)
);

CREATE TABLE test_trigger_log (
id INT PRIMARY KEY AUTO_INCREMENT,
t_log VARCHAR(30)
);
```

2. 创建触发器：创建名称为before_insert的触发器，向test_trigger数据表插入数据之前，向 test_trigger_log数据表中插入before_insert的日志信息。

```mysql
DELIMITER //
CREATE TRIGGER before_insert
BEFORE INSERT ON test_trigger
FOR EACH ROW
BEGIN
    INSERT INTO test_trigger_log (t_log)
    VALUES('before_insert');
END //
DELIMITER ;
```

3. 向test_trigger数据表中插入数据

```mysql
INSERT INTO test_trigger (t_note) VALUES ('测试 BEFORE INSERT 触发器');
```

4. 查看test_trigger_log数据表中的数据

```mysql
mysql> SELECT * FROM test_trigger_log;
+----+---------------+
| id | t_log |
+----+---------------+
| 1 | before_insert |
+----+---------------+
1 row in set (0.00 sec)
```

**举例2：**

定义触发器“salary_check_trigger”，基于员工表“employees”的INSERT事件，在INSERT之前检查 将要添加的新员工薪资是否大于他领导的薪资，如果大于领导薪资，则报sqlstate_value为'HY000'的错 误，从而使得添加失败。

```mysql
DELIMITER //
CREATE TRIGGER salary_check_trigger
BEFORE INSERT ON employees FOR EACH ROW
BEGIN
    DECLARE mgrsalary DOUBLE;
    SELECT salary INTO mgrsalary FROM employees WHERE employee_id = NEW.manager_id;
    IF NEW.salary > mgrsalary THEN
    	SIGNAL SQLSTATE 'HY000' SET MESSAGE_TEXT = '薪资高于领导薪资错误';
    END IF;
END //
DELIMITER ;
```

上面触发器声明过程中的NEW关键字代表INSERT添加语句的新记录。

##  查看、删除触发器

###   查看触发器

查看触发器是查看数据库中已经存在的触发器的定义、状态和语法信息等。

方式1：查看当前数据库的所有触发器的定义

```mysql
SHOW TRIGGERS\G
```

方式2：查看当前数据库中某个触发器的定义

```mysql
SHOW CREATE TRIGGER 触发器名
```

方式3：从系统库information_schema的TRIGGERS表中查询“salary_check_trigger”触发器的信息。

```mysql
SELECT * FROM information_schema.TRIGGERS;
```

###  删除触发器

触发器也是数据库对象，删除触发器也用DROP语句，语法格式如下：

```mysql
DROP TRIGGER IF EXISTS 触发器名称;
```

##  触发器的优缺点

### 优点

**1、触发器可以确保数据的完整性。**

假设我们用 进货单头表 （demo.importhead）来保存进货单的总体信息，包括进货单编号、供货商编号、仓库编号、总计进货数量、总计进货金额和验收日期。

| listnumber                  (进货单编号) | supplierid                 (进货商编号) | stockid             (参库编号) | quantity            (总计数量) | importvalue           (总计金额) | confirmationdate        （验收日期) |
| ---------------------------------------- | --------------------------------------- | ------------------------------ | ------------------------------ | -------------------------------- | ----------------------------------- |
|                                          |                                         |                                |                                |                                  |                                     |

用进货单明细表 （demo.importdetails）来保存进货商品的明细，包括进货单编号、商品编号、进货数 量、进货价格和进货金额。

| listnumber                          (进货单编号) | itemnumber                      (商品编号) | quantity                     (进货数量) | importprice                     (进货价格) | importvalue                   （进货金额) |
| ------------------------------------------------ | ------------------------------------------ | --------------------------------------- | ------------------------------------------ | ----------------------------------------- |
|                                                  |                                            |                                         |                                            |                                           |

每当我们录入、删除和修改一条进货单明细数据的时候，进货单明细表里的数据就会发生变动。这个时候，在进货单头表中的总计数量和总计金额就必须重新计算，否则，进货单头表中的总计数量和总计金 额就不等于进货单明细表中数量合计和金额合计了，这就是数据不一致。

为了解决这个问题，我们就可以使用触发器，规定每当进货单明细表有数据插入、修改和删除的操作 时，自动触发 2 步操作：

1）重新计算进货单明细表中的数量合计和金额合计；

2）用第一步中计算出来的值更新进货单头表中的合计数量与合计金额。

这样一来，进货单头表中的合计数量与合计金额的值，就始终与进货单明细表中计算出来的合计数量与 合计金额的值相同，数据就是一致的，不会互相矛盾。

**2、触发器可以帮助我们记录操作日志。**

利用触发器，可以具体记录什么时间发生了什么。比如，记录修改会员储值金额的触发器，就是一个很好的例子。这对我们还原操作执行时的具体场景，更好地定位问题原因很有帮助。

**3、触发器还可以用在操作数据前，对数据进行合法性检查。**

比如，超市进货的时候，需要库管录入进货价格。但是，人为操作很容易犯错误，比如说在录入数量的时候，把条形码扫进去了；录入金额的时候，看串了行，录入的价格远超售价，导致账面上的巨亏…… 这些都可以通过触发器，在实际插入或者更新操作之前，对相应的数据进行检查，及时提示错误，防止错误数据进入系统。

### 缺点

**1、触发器最大的一个问题就是可读性差。**

因为触发器存储在数据库中，并且由事件驱动，这就意味着触发器有可能不受应用层的控制 。这对系统维护是非常有挑战的。

**2、相关数据的变更，可能会导致触发器出错。**

特别是数据表结构的变更，都可能会导致触发器出错，进而影响数据操作的正常运行。这些都会由于触发器本身的隐蔽性，影响到应用中错误原因排查的效率。

###  注意点

注意，如果在子表中定义了外键约束，并且外键指定了ON UPDATE/DELETE CASCADE/SET NULL子句，此时修改父表被引用的键值或删除父表被引用的记录行时，也会引起子表的修改和删除操作，此时基于子表的UPDATE和DELETE语句定义的触发器并不会被激活。

例如：基于子表员工表（t_employee）的DELETE语句定义了触发器t1，而子表的部门编号（did）字段定义了外键约束引用了父表部门表（t_department）的主键列部门编号（did），并且该外键加了“ON DELETE SET NULL”子句，那么如果此时删除父表部门表（t_department）在子表员工表（t_employee） 有匹配记录的部门记录时，会引起子表员工表（t_employee）匹配记录的部门编号（did）修改为NULL， mysql> update demo.membermaster set memberdeposit=20 where memberid = 2; ERROR 1054 (42S22): Unknown column 'aa' in 'field list' 但是此时不会激活触发器t1。只有直接对子表员工表（t_employee）执行DELETE语句时才会激活触发器 t1。

# MySQL8

## MySQL8新特性概述

MySQL从5.7版本直接跳跃发布了8.0版本 ，可见这是一个令人兴奋的里程碑版本。MySQL 8版本在功能上做了显著的改进与增强，开发者对MySQL的源代码进行了重构，最突出的一点是多MySQL Optimizer优化器进行了改进。不仅在速度上得到了改善，还为用户带来了更好的性能和更棒的体验。

###  MySQL8.0 新增特性

1. 更简便的NoSQL支持 NoSQL泛指非关系型数据库和数据存储。随着互联网平台的规模飞速发展，传统 的关系型数据库已经越来越不能满足需求。从5.6版本开始，MySQL就开始支持简单的NoSQL存储功能。 MySQL 8对这一功能做了优化，以更灵活的方式实现NoSQL功能，不再依赖模式（schema）。 

2. 更好的索引 在查询中，正确地使用索引可以提高查询的效率。MySQL 8中新增了 隐藏索引 和 降序索 引 。隐藏索引可以用来测试去掉索引对查询性能的影响。在查询中混合存在多列索引时，使用降序索引 可以提高查询的性能。 

3. 更完善的JSON支持 MySQL从5.7开始支持原生JSON数据的存储，MySQL 8对这一功能做了优化，增加 了聚合函数 JSON_ARRAYAGG() 和 JSON_OBJECTAGG() ，将参数聚合为JSON数组或对象，新增了行内 操作符 ->>，是列路径运算符 ->的增强，对JSON排序做了提升，并优化了JSON的更新操作。 

4. 安全和账户管理 MySQL 8中新增了 caching_sha2_password 授权插件、角色、密码历史记录和FIPS 模式支持，这些特性提高了数据库的安全性和性能，使数据库管理员能够更灵活地进行账户管理工作。 

5. InnoDB的变化 InnoDB是MySQL默认的存储引擎 ，是事务型数据库的首选引擎，支持事务安全表 （ACID），支持行锁定和外键。在MySQL 8 版本中，InnoDB在自增、索引、加密、死锁、共享锁等方面 做了大量的 改进和优化 ，并且支持原子数据定义语言（DDL），提高了数据安全性，对事务提供更好的 支持。

6. 数据字典 在之前的MySQL版本中，字典数据都存储在元数据文件和非事务表中。从MySQL 8开始新增 了事务数据字典，在这个字典里存储着数据库对象信息，这些数据字典存储在内部事务表中。 

7. 原子数据定义语句 MySQL 8开始支持原子数据定义语句（Automic DDL），即 原子DDL 。目前，只有 InnoDB存储引擎支持原子DDL。原子数据定义语句（DDL）将与DDL操作相关的数据字典更新、存储引擎 操作、二进制日志写入结合到一个单独的原子事务中，这使得即使服务器崩溃，事务也会提交或回滚。 使用支持原子操作的存储引擎所创建的表，在执行DROP TABLE、CREATE TABLE、ALTER TABLE、 RENAME TABLE、TRUNCATE TABLE、CREATE TABLESPACE、DROP TABLESPACE等操作时，都支持原子操 作，即事务要么完全操作成功，要么失败后回滚，不再进行部分提交。 对于从MySQL 5.7复制到MySQL 8 版本中的语句，可以添加 IF EXISTS 或 IF NOT EXISTS 语句来避免发生错误。 

8. 资源管理 MySQL 8开始支持创建和管理资源组，允许将服务器内运行的线程分配给特定的分组，以便 线程根据组内可用资源执行。组属性能够控制组内资源，启用或限制组内资源消耗。数据库管理员能够 根据不同的工作负载适当地更改这些属性。 目前，CPU时间是可控资源，由“虚拟CPU”这个概念来表 示，此术语包含CPU的核心数，超线程，硬件线程等等。服务器在启动时确定可用的虚拟CPU数量。拥有 对应权限的数据库管理员可以将这些CPU与资源组关联，并为资源组分配线程。 资源组组件为MySQL中的资源组管理提供了SQL接口。资源组的属性用于定义资源组。MySQL中存在两个默认组，系统组和用户 组，默认的组不能被删除，其属性也不能被更改。对于用户自定义的组，资源组创建时可初始化所有的 属性，除去名字和类型，其他属性都可在创建之后进行更改。 在一些平台下，或进行了某些MySQL的配 置时，资源管理的功能将受到限制，甚至不可用。例如，如果安装了线程池插件，或者使用的是macOS 系统，资源管理将处于不可用状态。在FreeBSD和Solaris系统中，资源线程优先级将失效。在Linux系统 中，只有配置了CAP_SYS_NICE属性，资源管理优先级才能发挥作用。

9. 字符集支持 MySQL 8中默认的字符集由 latin1 更改为 utf8mb4 ，并首次增加了日语所特定使用的集 合，utf8mb4_ja_0900_as_cs。 

10. 优化器增强 MySQL优化器开始支持隐藏索引和降序索引。隐藏索引不会被优化器使用，验证索引的必 要性时不需要删除索引，先将索引隐藏，如果优化器性能无影响就可以真正地删除索引。降序索引允许 优化器对多个列进行排序，并且允许排序顺序不一致。 

11. 公用表表达式 公用表表达式（Common Table Expressions）简称为CTE，MySQL现在支持递归和非递 归两种形式的CTE。CTE通过在SELECT语句或其他特定语句前 使用WITH语句对临时结果集 进行命名。

	基础语法如下：

	```mysql
	WITH cte_name (col_name1,col_name2 ...) AS (Subquery)
	SELECT * FROM cte_name;
	
		Subquery代表子查询，子查询前使用WITH语句将结果集命名为cte_name，在后续的查询中即可使用 cte_name进行查询。
	```

12. 窗口函数 MySQL 8开始支持窗口函数。在之前的版本中已存在的大部分 聚合函数 在MySQL 8中也可以 作为窗口函数来使用。

13. 正则表达式支持 MySQL在8.0.4以后的版本中采用支持Unicode的国际化组件库实现正则表达式操作， 这种方式不仅能提供完全的Unicode支持，而且是多字节安全编码。MySQL增加了REGEXP_LIKE()、 EGEXP_INSTR()、REGEXP_REPLACE()和 REGEXP_SUBSTR()等函数来提升性能。另外，regexp_stack_limit和 regexp_time_limit 系统变量能够通过匹配引擎来控制资源消耗。
14. 内部临时表 TempTable存储引擎取代MEMORY存储引擎成为内部临时表的默认存储引擎 。TempTable存储 引擎为VARCHAR和VARBINARY列提供高效存储。internal_tmp_mem_storage_engine会话变量定义了内部 临时表的存储引擎，可选的值有两个，TempTable和MEMORY，其中TempTable为默认的存储引擎。 temptable_max_ram系统配置项定义了TempTable存储引擎可使用的最大内存数量。
15. 日志记录 在MySQL 8中错误日志子系统由一系列MySQL组件构成。这些组件的构成由系统变量 log_error_services来配置，能够实现日志事件的过滤和写入。 WITH cte_name (col_name1,col_name2 ...) AS (Subquery) SELECT * FROM cte_name; 
16. 备份锁 新的备份锁允许在线备份期间执行数据操作语句，同时阻止可能造成快照不一致的操作。新 备份锁由 LOCK INSTANCE FOR BACKUP 和 UNLOCK INSTANCE 语法提供支持，执行这些操作需要备份管理 员特权。 
17. 增强的MySQL复制 MySQL 8复制支持对 JSON文档 进行部分更新的 二进制日志记录 ，该记录 使用紧凑 的二进制格式 ，从而节省记录完整JSON文档的空间。当使用基于语句的日志记录时，这种紧凑的日志记 录会自动完成，并且可以通过将新的binlog_row_value_options系统变量值设置为PARTIAL_JSON来启用。

###  MySQL8.0 移除的旧特性

在MySQL 5.7版本上开发的应用程序如果使用了MySQL8.0 移除的特性，语句可能会失败，或者产生不同 的执行结果。为了避免这些问题，对于使用了移除特性的应用，应当尽力修正避免使用这些特性，并尽 可能使用替代方法。

1. 查询缓存 查询缓存已被移除 ，删除的项有： （1）语句：FLUSH QUERY CACHE和RESET QUERY CACHE。 （2）系统变量：query_cache_limit、query_cache_min_res_unit、query_cache_size、 query_cache_type、query_cache_wlock_invalidate。 （3）状态变量：Qcache_free_blocks、 Qcache_free_memory、Qcache_hits、Qcache_inserts、Qcache_lowmem_prunes、Qcache_not_cached、 Qcache_queries_in_cache、Qcache_total_blocks。 （4）线程状态：checking privileges on cached query、checking query cache for query、invalidating query cache entries、sending cached result to client、storing result in query cache、waiting for query cache lock。
2. 加密相关 删除的加密相关的内容有：ENCODE()、DECODE()、ENCRYPT()、DES_ENCRYPT()和 DES_DECRYPT()函数，配置项des-key-file，系统变量have_crypt，FLUSH语句的DES_KEY_FILE选项， HAVE_CRYPT CMake选项。 对于移除的ENCRYPT()函数，考虑使用SHA2()替代，对于其他移除的函数，使 用AES_ENCRYPT()和AES_DECRYPT()替代。 
3. 空间函数相关 在MySQL 5.7版本中，多个空间函数已被标记为过时。这些过时函数在MySQL 8中都已被 移除，只保留了对应的ST_和MBR函数。 
4. \N和NULL 在SQL语句中，解析器不再将\N视为NULL，所以在SQL语句中应使用NULL代替\N。这项变化 不会影响使用LOAD DATA INFILE或者SELECT...INTO OUTFILE操作文件的导入和导出。在这类操作中，NULL 仍等同于\N。 
5. mysql_install_db 在MySQL分布中，已移除了mysql_install_db程序，数据字典初始化需要调用带着-- initialize或者--initialize-insecure选项的mysqld来代替实现。另外，--bootstrap和INSTALL_SCRIPTDIR CMake也已被删除。 
6. 通用分区处理程序 通用分区处理程序已从MySQL服务中被移除。为了实现给定表分区，表所使用的存 储引擎需要自有的分区处理程序。 提供本地分区支持的MySQL存储引擎有两个，即InnoDB和NDB，而在 MySQL 8中只支持InnoDB。 
7. 系统和状态变量信息 在INFORMATION_SCHEMA数据库中，对系统和状态变量信息不再进行维护。 GLOBAL_VARIABLES、SESSION_VARIABLES、GLOBAL_STATUS、SESSION_STATUS表都已被删除。另外，系 统变量show_compatibility_56也已被删除。被删除的状态变量有Slave_heartbeat_period、 Slave_last_heartbeat,Slave_received_heartbeats、Slave_retried_transactions、Slave_running。以上被删除 的内容都可使用性能模式中对应的内容进行替代。 
8. mysql_plugin工具 mysql_plugin工具用来配置MySQL服务器插件，现已被删除，可使用--plugin-load或- -plugin-load-add选项在服务器启动时加载插件或者在运行时使用INSTALL PLUGIN语句加载插件来替代该 工具。

## . 新特性1：窗口函数

###  使用窗口函数前后对比

假设我现在有这样一个数据表，它显示了某购物网站在每个城市每个区的销售额：

```mysql
CREATE TABLE sales(
id INT PRIMARY KEY AUTO_INCREMENT,
city VARCHAR(15),
county VARCHAR(15),
sales_value DECIMAL
);
INSERT INTO sales(city,county,sales_value)
VALUES
('北京','海淀',10.00),
('北京','朝阳',20.00),
('上海','黄埔',30.00),
('上海','长宁',10.00);
```

查询：

```mysql
mysql> SELECT * FROM sales;
+----+------+--------+-------------+
| id | city | county | sales_value |
+----+------+--------+-------------+
| 1  | 北京  |  海淀   |      10    |
| 2  | 北京  |  朝阳   |      20    |
| 3  | 上海  |  黄埔   |      30    |
| 4  | 上海  |  长宁   |      10    |
+----+------+--------+-------------+
4 rows in set (0.00 sec)
```

需求：现在计算这个网站在每个城市的销售总额、在全国的销售总额、每个区的销售额占所在城市销售额中的比率，以及占总销售额中的比率。

如果用分组和聚合函数，就需要分好几步来计算。

第一步，计算总销售金额，并存入临时表 a：

```mysql
CREATE TEMPORARY TABLE a -- 创建临时表
SELECT SUM(sales_value) AS sales_value -- 计算总计金额
FROM sales;
```

查看一下临时表 a ：

```mysql
mysql> SELECT * FROM a;
+-------------+
| sales_value |
+-------------+
| 70 |
+-------------+
1 row in set (0.00 sec)
```

第二步，计算每个城市的销售总额并存入临时表 b：

```mysql
CREATE TEMPORARY TABLE b -- 创建临时表
SELECT city, SUM(sales_value) AS sales_value -- 计算城市销售合计
FROM sales
GROUP BY city;
```

查看临时表 b ：

```mysql
mysql> SELECT * FROM b;
+------+-------------+
| city | sales_value |
+------+-------------+
| 北京  |     30      |
| 上海  |     40      |
+------+-------------+
2 rows in set (0.00 sec)
```

第三步，计算各区的销售占所在城市的总计金额的比例，和占全部销售总计金额的比例。我们可以通过下面的连接查询获得需要的结果：

```mysql
mysql> SELECT s.city AS 城市,s.county AS 区,s.sales_value AS 区销售额,
-> b.sales_value AS 市销售额,s.sales_value/b.sales_value AS 市比率,
-> a.sales_value AS 总销售额,s.sales_value/a.sales_value AS 总比率
-> FROM sales s
-> JOIN b ON (s.city=b.city) -- 连接市统计结果临时表
-> JOIN a -- 连接总计金额临时表
-> ORDER BY s.city,s.county;
+------+------+----------+----------+--------+----------+--------+
| 城市 | 区 | 区销售额 | 市销售额 | 市比率 | 总销售额 | 总比率 |
+------+------+----------+----------+--------+----------+--------+
| 上海 | 长宁 | 10 | 40 | 0.2500 | 70 | 0.1429 |
| 上海 | 黄埔 | 30 | 40 | 0.7500 | 70 | 0.4286 |
| 北京 | 朝阳 | 20 | 30 | 0.6667 | 70 | 0.2857 |
| 北京 | 海淀 | 10 | 30 | 0.3333 | 70 | 0.1429 |
+------+------+----------+----------+--------+----------+--------+
4 rows in set (0.00 sec)
```

结果显示：市销售金额、市销售占比、总销售金额、总销售占比都计算出来了。

同样的查询，如果用窗口函数，就简单多了。我们可以用下面的代码来实现：

```mysql
mysql> SELECT city AS 城市,county AS 区,sales_value AS 区销售额,
-> SUM(sales_value) OVER(PARTITION BY city) AS 市销售额, -- 计算市销售额
-> sales_value/SUM(sales_value) OVER(PARTITION BY city) AS 市比率,
-> SUM(sales_value) OVER() AS 总销售额, -- 计算总销售额
-> sales_value/SUM(sales_value) OVER() AS 总比率
-> FROM sales
-> ORDER BY city,county;
+------+------+----------+----------+--------+----------+--------+
| 城市 | 区 | 区销售额 | 市销售额 | 市比率 | 总销售额 | 总比率 |
+------+------+----------+----------+--------+----------+--------+
| 上海 | 长宁 | 10 | 40 | 0.2500 | 70 | 0.1429 |
| 上海 | 黄埔 | 30 | 40 | 0.7500 | 70 | 0.4286 |
| 北京 | 朝阳 | 20 | 30 | 0.6667 | 70 | 0.2857 |
| 北京 | 海淀 | 10 | 30 | 0.3333 | 70 | 0.1429 |
+------+------+----------+-----------+--------+----------+--------+
4 rows in set (0.00 sec)
```

结果显示，我们得到了与上面那种查询同样的结果。 

使用窗口函数，只用了一步就完成了查询。而且，由于没有用到临时表，执行的效率也更高了。很显 然，在这种需要用到分组统计的结果对每一条记录进行计算的场景下，使用窗口函数更好。

###  窗口函数分类

MySQL从8.0版本开始支持窗口函数。窗口函数的作用类似于在查询中对数据进行分组，不同的是，分组操作会把分组的结果聚合成一条记录，而窗口函数是将结果置于每一条数据记录中。

窗口函数可以分为 静态窗口函数 和 动态窗口函数 。

* 静态窗口函数的窗口大小是固定的，不会因为记录的不同而不同；
* 动态窗口函数的窗口大小会随着记录的不同而变化。

MySQL官方网站窗口函数的网址为https://dev.mysql.com/doc/refman/8.0/en/window-function-descriptio ns.html#function_row-number。 

窗口函数总体上可以分为序号函数、分布函数、前后函数、首尾函数和其他函数，如下表：

### 语法结构

窗口函数的语法结构是：

```mysql
函数 OVER（[PARTITION BY 字段名 ORDER BY 字段名 ASC|DESC]）
```

或者是：

```mysql
函数 OVER 窗口名 … WINDOW 窗口名 AS （[PARTITION BY 字段名 ORDER BY 字段名 ASC|DESC]）
```

* OVER 关键字指定函数窗口的范围。
	* 如果省略后面括号中的内容，则窗口会包含满足WHERE条件的所有记录，窗口函数会基于所有满足WHERE条件的记录进行计算。
	* 如果OVER关键字后面的括号不为空，则可以使用如下语法设置窗口。
* 窗口名：为窗口设置一个别名，用来标识窗口。
* PARTITION BY子句：指定窗口函数按照哪些字段进行分组。分组后，窗口函数可以在每个分组中分别执行。
* ORDER BY子句：指定窗口函数按照哪些字段进行排序。执行排序操作使窗口函数按照排序后的数据记录的顺序进行编号。
* FRAME子句：为分区中的某个子集定义规则，可以用来作为滑动窗口使用。

### 分类讲解

创建表：

```mysql
CREATE TABLE goods(
id INT PRIMARY KEY AUTO_INCREMENT,
category_id INT,
category VARCHAR(15),
NAME VARCHAR(30),
price DECIMAL(10,2),
stock INT,
upper_time DATETIME
);
```

添加数据：

```mysql
INSERT INTO goods(category_id,category,NAME,price,stock,upper_time)
VALUES
(1, '女装/女士精品', 'T恤', 39.90, 1000, '2020-11-10 00:00:00'),
(1, '女装/女士精品', '连衣裙', 79.90, 2500, '2020-11-10 00:00:00'),
(1, '女装/女士精品', '卫衣', 89.90, 1500, '2020-11-10 00:00:00'),
(1, '女装/女士精品', '牛仔裤', 89.90, 3500, '2020-11-10 00:00:00'),
(1, '女装/女士精品', '百褶裙', 29.90, 500, '2020-11-10 00:00:00'),
(1, '女装/女士精品', '呢绒外套', 399.90, 1200, '2020-11-10 00:00:00'),
(2, '户外运动', '自行车', 399.90, 1000, '2020-11-10 00:00:00'),
(2, '户外运动', '山地自行车', 1399.90, 2500, '2020-11-10 00:00:00'),
(2, '户外运动', '登山杖', 59.90, 1500, '2020-11-10 00:00:00'),
(2, '户外运动', '骑行装备', 399.90, 3500, '2020-11-10 00:00:00'),
(2, '户外运动', '运动外套', 799.90, 500, '2020-11-10 00:00:00'),
(2, '户外运动', '滑板', 499.90, 1200, '2020-11-10 00:00:00');
```

下面针对goods表中的数据来验证每个窗口函数的功能。

####  序号函数

**1. ROW_NUMBER()函数**

ROW_NUMBER()函数能够对数据中的序号进行顺序显示。

举例：查询 goods 数据表中每个商品分类下价格降序排列的各个商品信息。

```mysql
mysql> SELECT ROW_NUMBER() OVER(PARTITION BY category_id ORDER BY price DESC) AS
row_num, id, category_id, category, NAME, price, stock
FROM goods;
+---------+----+-------------+---------------+------------+---------+-------+
| row_num | id | category_id |    category   |     NAME   |  price  | stock |
+---------+----+-------------+---------------+------------+---------+-------+
|    1    |  6 |     1       |  女装/女士精品  | 呢绒外套     | 399.90  | 1200  |
|    2    |  3 |     1       |  女装/女士精品  | 卫衣        | 89.90   | 1500  |
|    3    |  4 |     1       |  女装/女士精品  | 牛仔裤       | 89.90   | 3500  |
|    4    |  2 |     1       |  女装/女士精品  | 连衣裙       | 79.90   | 2500  |
|    5    |  1 |     1       |  女装/女士精品  | T恤         | 39.90   | 1000  |
|    6    |  5 |     1       |  女装/女士精品  | 百褶裙       | 29.90   | 500   |
|    1    |  8 |     2       |     户外运动   | 山地自行车    | 1399.90 | 2500  |
|    2    | 11 |     2       |     户外运动   | 运动外套      | 799.90  | 500  |
|    3    | 12 |     2       |     户外运动   | 滑板         | 499.90  | 1200  |
|    4    |  7 |     2       |     户外运动   | 自行车       | 399.90  | 1000  |
|    5    | 10 |     2       |     户外运动   | 骑行装备     | 399.90  | 3500  |
|    6    |  9 |     2       |     户外运动   | 登山杖       | 59.90   | 1500  |
+---------+----+-------------+---------------+------------+---------+-------+
12 rows in set (0.00 sec)
```

举例：查询 goods 数据表中每个商品分类下价格最高的3种商品信息。

```mysql
mysql> SELECT *
-> FROM (
-> SELECT ROW_NUMBER() OVER(PARTITION BY category_id ORDER BY price DESC) AS
row_num,
-> id, category_id, category, NAME, price, stock
-> FROM goods) t
-> WHERE row_num <= 3;
+---------+----+-------------+---------------+------------+---------+-------+
| row_num | id | category_id |     category  |      NAME  |  price  | stock |
+---------+----+-------------+---------------+------------+---------+-------+
|     1   |  6 |      1      | 女装/女士精品   | 呢绒外套     | 399.90  | 1200  |
|     2   |  3 |      1      | 女装/女士精品   | 卫衣        | 89.90   | 1500  |
|     3   |  4 |      1      | 女装/女士精品   | 牛仔裤      | 89.90    | 3500 |
|     1   |  8 |      2      | 户外运动       | 山地自行车   | 1399.90  | 2500 |
|     2   | 11 |      2      | 户外运动       | 运动外套     | 799.90  | 500   |
|     3   | 12 |      2      | 户外运动       | 滑板        | 499.90   | 1200 |
+---------+----+-------------+---------------+------------+----------+-------+
6 rows in set (0.00 sec)
```

在名称为“女装/女士精品”的商品类别中，有两款商品的价格为89.90元，分别是卫衣和牛仔裤。两款商品 的序号都应该为2，而不是一个为2，另一个为3。此时，可以使用RANK()函数和DENSE_RANK()函数解 决。

**2．RANK()函数**

使用RANK()函数能够对序号进行并列排序，并且会跳过重复的序号，比如序号为1、1、3。 

举例：使用RANK()函数获取 goods 数据表中各类别的价格从高到低排序的各商品信息。

```mysql
mysql> SELECT RANK() OVER(PARTITION BY category_id ORDER BY price DESC) AS row_num,
-> id, category_id, category, NAME, price, stock
-> FROM goods;
+---------+----+-------------+---------------+------------+---------+-------+
| row_num | id | category_id | category      | NAME       | price   | stock |
+---------+----+-------------+---------------+------------+---------+-------+
|     1   | 6  |     1       | 女装/女士精品   | 呢绒外套     | 399.90  | 1200  |
|     2   | 3  |     1       | 女装/女士精品   | 卫衣        | 89.90   | 1500  |
|     2   | 4  |     1       | 女装/女士精品   | 牛仔裤      | 89.90   | 3500   |
|     4   | 2  |     1       | 女装/女士精品   | 连衣裙      | 79.90   | 2500   |
|     5   | 1  |     1       | 女装/女士精品   | T恤        | 39.90   | 1000   |
|     6   | 5  |     1       | 女装/女士精品   | 百褶裙      | 29.90   | 500    |
|     1   | 8  |     2       | 户外运动       | 山地自行车   | 1399.90 | 2500   |
|     2   | 11 |     2       | 户外运动       | 运动外套     | 799.90  | 500   |
|     3   | 12 |     2       | 户外运动       | 滑板        | 499.90  | 1200   |
|     4   | 7  |     2       | 户外运动       | 自行车      | 399.90   | 1000  |
|     4   | 10 |     2       | 户外运动       | 骑行装备    | 399.90   | 3500  |
|     6   | 9  |     2       | 户外运动       | 登山杖      | 59.90   | 1500   |
+---------+----+-------------+---------------+------------+---------+-------+
12 rows in set (0.00 sec)
```

**3．DENSE_RANK()函数**

DENSE_RANK()函数对序号进行并列排序，并且不会跳过重复的序号，比如序号为1、1、2。 举例：使用DENSE_RANK()函数获取 goods 数据表中各类别的价格从高到低排序的各商品信息。

```mysql
mysql> SELECT DENSE_RANK() OVER(PARTITION BY category_id ORDER BY price DESC) AS
row_num,
-> id, category_id, category, NAME, price, stock
-> FROM goods;
+---------+----+-------------+---------------+------------+---------+-------+
| row_num | id | category_id | category      | NAME       | price   | stock |
+---------+----+-------------+---------------+------------+---------+-------+
|    1    | 6  |      1      | 女装/女士精品   |     呢绒外套 | 399.90 | 1200   |
|    2    | 3  |      1      | 女装/女士精品   |     卫衣    | 89.90  | 1500   |
|    2    | 4  |      1      | 女装/女士精品   |     牛仔裤  | 89.90   | 3500  |
|    3    | 2  |      1      | 女装/女士精品   |     连衣裙  | 79.90   | 2500  |
|    4    | 1  |      1      | 女装/女士精品   |     T恤    | 39.90   | 1000  |
|    5    | 5  |      1      | 女装/女士精品   |     百褶裙  | 29.90   | 500   |
|    1    | 8  |      2      | 户外运动       |    山地自行车| 1399.90 | 2500 |
|    2    | 11 |      2      | 户外运动       |    运动外套  | 799.90 | 500    |
|    3    | 12 |      2      | 户外运动       |    滑板     | 499.90 | 1200   |
|    4    | 7  |      2      | 户外运动       |    自行车    | 399.90 | 1000   |
|    4    | 10 |      2      | 户外运动       |    骑行装备  | 399.90 | 3500   |
|    5    | 9  |      2      | 户外运动       |    登山杖    | 59.90 | 1500   |
+---------+----+-------------+---------------+------------+---------+-------+
12 rows in set (0.00 sec)
```

####  分布函数

**1．PERCENT_RANK()函数**

PERCENT_RANK()函数是等级值百分比函数。按照如下方式进行计算。

```mysql
(rank - 1) / (rows - 1)
```

其中，rank的值为使用RANK()函数产生的序号，rows的值为当前窗口的总记录数。

举例：计算 goods 数据表中名称为“女装/女士精品”的类别下的商品的PERCENT_RANK值。

```mysql
#写法一：
SELECT RANK() OVER (PARTITION BY category_id ORDER BY price DESC) AS r,
PERCENT_RANK() OVER (PARTITION BY category_id ORDER BY price DESC) AS pr,
id, category_id, category, NAME, price, stock
FROM goods
WHERE category_id = 1;
#写法二：
mysql> SELECT RANK() OVER w AS r,
-> PERCENT_RANK() OVER w AS pr,
-> id, category_id, category, NAME, price, stock
-> FROM goods
-> WHERE category_id = 1 WINDOW w AS (PARTITION BY category_id ORDER BY price
DESC);
+---+-----+----+-------------+---------------+----------+--------+-------+
| r | pr  | id | category_id | category      | NAME     | price  | stock |
+---+-----+----+-------------+---------------+----------+--------+-------+
| 1 | 0   | 6  |          1  | 女装/女士精品   |   呢绒外套 | 399.90 | 1200 |
| 2 | 0.2 | 3  |          1  | 女装/女士精品   |   卫衣    | 89.90 | 1500 |
| 2 | 0.2 | 4  |          1  | 女装/女士精品   |   牛仔裤  | 89.90 | 3500 |
| 4 | 0.6 | 2  |          1  | 女装/女士精品   |   连衣裙  | 79.90 | 2500 |
| 5 | 0.8 | 1  |          1  | 女装/女士精品   |   T恤    | 39.90 | 1000 |
| 6 | 1   | 5  |          1  | 女装/女士精品   |   百褶裙  | 29.90 | 500 |
+---+-----+----+-------------+---------------+----------+--------+-------+
6 rows in set (0.00 sec)
```

**2．CUME_DIST()函数**

CUME_DIST()函数主要用于查询小于或等于某个值的比例。 

举例：查询goods数据表中小于或等于当前价格的比例。

```mysql
mysql> SELECT CUME_DIST() OVER(PARTITION BY category_id ORDER BY price ASC) AS cd,
-> id, category, NAME, price
-> FROM goods;
+---------------------+----+---------------+------------+---------+
|                cd   | id | category      | NAME       | price   |
+---------------------+----+---------------+------------+---------+
| 0.16666666666666666 | 5  | 女装/女士精品   | 百褶裙      | 29.90 |
| 0.3333333333333333  | 1  | 女装/女士精品   | T恤        | 39.90 |
| 0.5                 | 2  | 女装/女士精品   | 连衣裙      | 79.90 |
| 0.8333333333333334  | 3  | 女装/女士精品   | 卫衣       | 89.90 |
| 0.8333333333333334  | 4  | 女装/女士精品   | 牛仔裤     | 89.90 |
| 1                   | 6  | 女装/女士精品   | 呢绒外套    | 399.90 |
| 0.16666666666666666 | 9  | 户外运动       | 登山杖      | 59.90 |
| 0.5                 | 7  | 户外运动       | 自行车      | 399.90 |
| 0.5                 | 10 | 户外运动       | 骑行装备     | 399.90 |
| 0.6666666666666666  | 12 | 户外运动       | 滑板        | 499.90 |
| 0.8333333333333334  | 11 | 户外运动       | 运动外套     | 799.90 |
| 1                   | 8  | 户外运动       | 山地自行车   | 1399.90 |
+---------------------+----+---------------+------------+---------+
12 rows in set (0.00 sec)
```

####  前后函数

**1．LAG(expr,n)函数**

LAG(expr,n)函数返回当前行的前n行的expr的值。 

举例：查询goods数据表中前一个商品价格与当前商品价格的差值。

```mysql
mysql> SELECT id, category, NAME, price, pre_price, price - pre_price AS diff_price
-> FROM (
-> SELECT id, category, NAME, price,LAG(price,1) OVER w AS pre_price
-> FROM goods
-> WINDOW w AS (PARTITION BY category_id ORDER BY price)) t;
+----+---------------+------------+---------+-----------+------------+
| id | category | NAME | price | pre_price | diff_price |
+----+---------------+------------+---------+-----------+------------+
| 5 | 女装/女士精品 | 百褶裙 | 29.90 | NULL | NULL |
| 1 | 女装/女士精品 | T恤 | 39.90 | 29.90 | 10.00 |
| 2 | 女装/女士精品 | 连衣裙 | 79.90 | 39.90 | 40.00 |
| 3 | 女装/女士精品 | 卫衣 | 89.90 | 79.90 | 10.00 |
| 4 | 女装/女士精品 | 牛仔裤 | 89.90 | 89.90 | 0.00 |
| 6 | 女装/女士精品 | 呢绒外套 | 399.90 | 89.90 | 310.00 |
| 9 | 户外运动 | 登山杖 | 59.90 | NULL | NULL |
| 7 | 户外运动 | 自行车 | 399.90 | 59.90 | 340.00 |
| 10 | 户外运动 | 骑行装备 | 399.90 | 399.90 | 0.00 |
| 12 | 户外运动 | 滑板 | 499.90 | 399.90 | 100.00 |
| 11 | 户外运动 | 运动外套 | 799.90 | 499.90 | 300.00 |
| 8 | 户外运动 | 山地自行车 | 1399.90 | 799.90 | 600.00 |
+----+---------------+------------+---------+-----------+------------+
12 rows in set (0.00 sec)
```

**2．LEAD(expr,n)函数**

LEAD(expr,n)函数返回当前行的后n行的expr的值。 

举例：查询goods数据表中后一个商品价格与当前商品价格的差值。

```mysql
mysql> SELECT id, category, NAME, behind_price, price,behind_price - price AS
diff_price
-> FROM(
-> SELECT id, category, NAME, price,LEAD(price, 1) OVER w AS behind_price
-> FROM goods WINDOW w AS (PARTITION BY category_id ORDER BY price)) t;
+----+---------------+------------+--------------+---------+------------+
| id | category      | NAME       | behind_price | price   | diff_price |
+----+---------------+------------+--------------+---------+------------+
| 5  | 女装/女士精品   | 百褶裙       | 39.90       | 29.90 | 10.00 |
| 1  | 女装/女士精品   | T恤         | 79.90       | 39.90 | 40.00 |
| 2  | 女装/女士精品   | 连衣裙      | 89.90        | 79.90 | 10.00 |
| 3  | 女装/女士精品   | 卫衣        | 89.90       | 89.90 | 0.00 |
| 4  | 女装/女士精品   | 牛仔裤       | 399.90     | 89.90 | 310.00 |
| 6  | 女装/女士精品   | 呢绒外套     | NULL       | 399.90 | NULL |
| 9  | 户外运动       | 登山杖       | 399.90    | 59.90 | 340.00 |
| 7  | 户外运动       | 自行车       | 399.90    | 399.90 | 0.00 |
| 10 | 户外运动       | 骑行装备     | 499.90     | 399.90 | 100.00 |
| 12 | 户外运动       | 滑板         | 799.90    | 499.90 | 300.00 |
| 11 | 户外运动       | 运动外套     | 1399.90    | 799.90 | 600.00 |
| 8  | 户外运动       | 山地自行车   | NULL       | 1399.90 | NULL |
+----+---------------+------------+--------------+---------+------------+
12 rows in set (0.00 sec)
```

####  首尾函数

**1．FIRST_VALUE(expr)函数**

FIRST_VALUE(expr)函数返回第一个expr的值。

举例：按照价格排序，查询第1个商品的价格信息。

```mysql
mysql> SELECT id, category, NAME, price, stock,FIRST_VALUE(price) OVER w AS
first_price
-> FROM goods WINDOW w AS (PARTITION BY category_id ORDER BY price);
+----+---------------+------------+---------+-------+-------------+
| id | category      | NAME | price | stock | first_price |
+----+---------------+------------+---------+-------+-------------+
| 5  | 女装/女士精品   | 百褶裙 | 29.90 | 500 | 29.90 |
| 1  | 女装/女士精品   | T恤 | 39.90 | 1000 | 29.90 |
| 2  | 女装/女士精品   | 连衣裙 | 79.90 | 2500 | 29.90 |
| 3  | 女装/女士精品   | 卫衣 | 89.90 | 1500 | 29.90 |
| 4  | 女装/女士精品   | 牛仔裤 | 89.90 | 3500 | 29.90 |
| 6  | 女装/女士精品   | 呢绒外套 | 399.90 | 1200 | 29.90 |
| 9  | 户外运动       | 登山杖 | 59.90 | 1500 | 59.90 |
| 7  | 户外运动       | 自行车 | 399.90 | 1000 | 59.90 |
| 10 | 户外运动       | 骑行装备 | 399.90 | 3500 | 59.90 |
| 12 | 户外运动       | 滑板 | 499.90 | 1200 | 59.90 |
| 11 | 户外运动       | 运动外套 | 799.90 | 500 | 59.90 |
| 8  | 户外运动       | 山地自行车 | 1399.90 | 2500 | 59.90 |
+----+---------------+------------+---------+-------+-------------+
12 rows in set (0.00 sec)
```

**LAST_VALUE(expr)函数**

LAST_VALUE(expr)函数返回最后一个expr的值。 

举例：按照价格排序，查询最后一个商品的价格信息。

```mysql
mysql> SELECT id, category, NAME, price, stock,LAST_VALUE(price) OVER w AS last_price
-> FROM goods WINDOW w AS (PARTITION BY category_id ORDER BY price);
+----+---------------+------------+---------+-------+------------+
| id | category      | NAME | price | stock | last_price |
+----+---------------+------------+---------+-------+------------+
| 5  | 女装/女士精品   | 百褶裙 | 29.90 | 500 | 29.90 |
| 1  | 女装/女士精品   | T恤 | 39.90 | 1000 | 39.90 |
| 2  | 女装/女士精品   | 连衣裙 | 79.90 | 2500 | 79.90 |
| 3  | 女装/女士精品   | 卫衣 | 89.90 | 1500 | 89.90 |
| 4  | 女装/女士精品   | 牛仔裤 | 89.90 | 3500 | 89.90 |
| 6  | 女装/女士精品   | 呢绒外套 | 399.90 | 1200 | 399.90 |
| 9  | 户外运动       | 登山杖 | 59.90 | 1500 | 59.90 |
| 7  | 户外运动       | 自行车 | 399.90 | 1000 | 399.90 |
| 10 | 户外运动       | 骑行装备 | 399.90 | 3500 | 399.90 |
| 12 | 户外运动       | 滑板 | 499.90 | 1200 | 499.90 |
| 11 | 户外运动       | 运动外套 | 799.90 | 500 | 799.90 |
| 8  | 户外运动       | 山地自行车 | 1399.90 | 2500 | 1399.90 |
+----+---------------+------------+---------+-------+------------+
12 rows in set (0.00 sec)
```

####  其他函数

**1．NTH_VALUE(expr,n)函数**

NTH_VALUE(expr,n)函数返回第n个expr的值。 举例：查询goods数据表中排名第2和第3的价格信息。

```mysql
mysql> SELECT id, category, NAME, price,NTH_VALUE(price,2) OVER w AS second_price,
-> NTH_VALUE(price,3) OVER w AS third_price
-> FROM goods WINDOW w AS (PARTITION BY category_id ORDER BY price);
+----+---------------+------------+---------+--------------+-------------+
| id | category      | NAME       | price   | second_price | third_price |
+----+---------------+------------+---------+--------------+-------------+
| 5  | 女装/女士精品   | 百褶裙 | 29.90 | NULL | NULL |
| 1  | 女装/女士精品   | T恤 | 39.90 | 39.90 | NULL |
| 2  | 女装/女士精品   | 连衣裙 | 79.90 | 39.90 | 79.90 |
| 3  | 女装/女士精品   | 卫衣 | 89.90 | 39.90 | 79.90 |
| 4  | 女装/女士精品   | 牛仔裤 | 89.90 | 39.90 | 79.90 |
| 6  | 女装/女士精品   | 呢绒外套 | 399.90 | 39.90 | 79.90 |
| 9  | 户外运动       | 登山杖 | 59.90 | NULL | NULL |
| 7  | 户外运动       | 自行车 | 399.90 | 399.90 | 399.90 |
| 10 | 户外运动       | 骑行装备 | 399.90 | 399.90 | 399.90 |
| 12 | 户外运动       | 滑板 | 499.90 | 399.90 | 399.90 |
| 11 | 户外运动       | 运动外套 | 799.90 | 399.90 | 399.90 |
| 8  | 户外运动       | 山地自行车 | 1399.90 | 399.90 | 399.90 |
+----+---------------+------------+---------+--------------+-------------+
12 rows in set (0.00 sec)
```

**2．NTILE(n)函数**

NTILE(n)函数将分区中的有序数据分为n个桶，记录桶编号。 

举例：将goods表中的商品按照价格分为3组。

```mysql
mysql> SELECT NTILE(3) OVER w AS nt,id, category, NAME, price
-> FROM goods WINDOW w AS (PARTITION BY category_id ORDER BY price);
+----+----+---------------+------------+---------+
| nt | id | category      | NAME       | price |
+----+----+---------------+------------+---------+
| 1  | 5  | 女装/女士精品   | 百褶裙 | 29.90 |
| 1  | 1  | 女装/女士精品   | T恤 | 39.90 |
| 2  | 2  | 女装/女士精品   | 连衣裙 | 79.90 |
| 2  | 3  | 女装/女士精品   | 卫衣 | 89.90 |
| 3  | 4  | 女装/女士精品   | 牛仔裤 | 89.90 |
| 3  | 6  | 女装/女士精品   | 呢绒外套 | 399.90 |
| 1  | 9  | 户外运动       | 登山杖 | 59.90 |
| 1  | 7  | 户外运动       | 自行车 | 399.90 |
| 2  | 10 | 户外运动       | 骑行装备 | 399.90 |
| 2  | 12 | 户外运动       | 滑板 | 499.90 |
| 3  | 11 | 户外运动       | 运动外套 | 799.90 |
| 3  | 8  | 户外运动       | 山地自行车 | 1399.90 |
+----+----+---------------+------------+---------+
12 rows in set (0.00 sec)
```

###  小结

窗口函数的特点是可以分组，而且可以在分组内排序。另外，窗口函数不会因为分组而减少原表中的行 数，这对我们在原表数据的基础上进行统计和排序非常有用。

## 新特性2：公用表表达式

公用表表达式（或通用表表达式）简称为CTE（Common Table Expressions）。CTE是一个命名的临时结 果集，作用范围是当前语句。CTE可以理解成一个可以复用的子查询，当然跟子查询还是有点区别的， CTE可以引用其他CTE，但子查询不能引用其他子查询。所以，可以考虑代替子查询。

依据语法结构和执行方式的不同，公用表表达式分为 普通公用表表达式 和 递归公用表表达式 2 种。

###  普通公用表表达式

普通公用表表达式的语法结构是：

```mysql
WITH CTE名称
AS （子查询）
SELECT|DELETE|UPDATE 语句;
```

普通公用表表达式类似于子查询，不过，跟子查询不同的是，它可以被多次引用，而且可以被其他的普 通公用表表达式所引用。

举例：查询员工所在的部门的详细信息。

```mysql
mysql> SELECT * FROM departments
-> WHERE department_id IN (
-> SELECT DISTINCT department_id
-> FROM employees
-> );
+---------------+------------------+------------+-------------+
| department_id | department_name  | manager_id | location_id |
+---------------+------------------+------------+-------------+
|     10        | Administration   | 200        | 1700        |
|     20        | Marketing        | 201        | 1800        |
|     30        | Purchasing       | 114        | 1700        |
|     40        | Human Resources  | 203        | 2400        |
|     50        | Shipping         | 121        | 1500        |
|     60        | IT               | 103        | 1400        |
|     70        | Public Relations | 204        | 2700        |
|     80        | Sales            | 145        | 2500        |
|     90        | Executive        | 100        | 1700        |
|     100       | Finance          | 108        | 1700        |
|     110       | Accounting       | 205        | 1700        |
+---------------+------------------+------------+-------------+
11 rows in set (0.00 sec)
```

这个查询也可以用普通公用表表达式的方式完成：

```mysql
mysql> WITH emp_dept_id
-> AS (SELECT DISTINCT department_id FROM employees)
-> SELECT *
-> FROM departments d JOIN emp_dept_id e
-> ON d.department_id = e.department_id;
+---------------+------------------+------------+-------------+---------------+
| department_id | department_name  | manager_id | location_id | department_id |
+---------------+------------------+------------+-------------+---------------+
|      90       | Executive        | 100        | 1700        | 90            |
|      60       | IT               | 103        | 1400        | 60            |
|      100      | Finance          | 108        | 1700        | 100           |
|      30       | Purchasing       | 114        | 1700        | 30            |
|      50       | Shipping         | 121        | 1500        | 50            |
|      80       | Sales            | 145        | 2500        | 80            |
|      10       | Administration   | 200        | 1700        | 10            |
|      20       | Marketing        | 201        | 1800        | 20            |
|      40       | Human Resources  | 203        | 2400        | 40            |
|      70       | Public Relations | 204        | 2700        | 70            |
|      110      | Accounting       | 205        | 1700        | 110           |
+---------------+------------------+------------+-------------+---------------+
11 rows in set (0.00 sec)
```

例子说明，公用表表达式可以起到子查询的作用。以后如果遇到需要使用子查询的场景，你可以在查询 之前，先定义公用表表达式，然后在查询中用它来代替子查询。而且，跟子查询相比，公用表表达式有 一个优点，就是定义过公用表表达式之后的查询，可以像一个表一样多次引用公用表表达式，而子查询 则不能。

###   递归公用表表达式

递归公用表表达式也是一种公用表表达式，只不过，除了普通公用表表达式的特点以外，它还有自己的特点，就是可以调用自己。它的语法结构是：

```mysql
WITH RECURSIVE
CTE名称 AS （子查询）
SELECT|DELETE|UPDATE 语句;
```

递归公用表表达式由 2 部分组成，分别是种子查询和递归查询，中间通过关键字 UNION [ALL]进行连接。 这里的种子查询，意思就是获得递归的初始值。这个查询只会运行一次，以创建初始数据集，之后递归 查询会一直执行，直到没有任何新的查询数据产生，递归返回。

案例：针对于我们常用的employees表，包含employee_id，last_name和manager_id三个字段。如果a是b 的管理者，那么，我们可以把b叫做a的下属，如果同时b又是c的管理者，那么c就是b的下属，是a的下下 属。

下面我们尝试用查询语句列出所有具有下下属身份的人员信息。

如果用我们之前学过的知识来解决，会比较复杂，至少要进行 4 次查询才能搞定：

* 第一步，先找出初代管理者，就是不以任何别人为管理者的人，把结果存入临时表； 
* 第二步，找出所有以初代管理者为管理者的人，得到一个下属集，把结果存入临时表； 
* 第三步，找出所有以下属为管理者的人，得到一个下下属集，把结果存入临时表。 
* 第四步，找出所有以下下属为管理者的人，得到一个结果集。

如果第四步的结果集为空，则计算结束，第三步的结果集就是我们需要的下下属集了，否则就必须继续 进行第四步，一直到结果集为空为止。比如上面的这个数据表，就需要到第五步，才能得到空结果集。 而且，最后还要进行第六步：把第三步和第四步的结果集合并，这样才能最终获得我们需要的结果集。

如果用递归公用表表达式，就非常简单了。我介绍下具体的思路。

* 用递归公用表表达式中的种子查询，找出初代管理者。字段 n 表示代次，初始值为 1，表示是第一 代管理者。
* 用递归公用表表达式中的递归查询，查出以这个递归公用表表达式中的人为管理者的人，并且代次 的值加 1。直到没有人以这个递归公用表表达式中的人为管理者了，递归返回。
* 在最后的查询中，选出所有代次大于等于 3 的人，他们肯定是第三代及以上代次的下属了，也就是 下下属了。这样就得到了我们需要的结果集。

这里看似也是 3 步，实际上是一个查询的 3 个部分，只需要执行一次就可以了。而且也不需要用临时表 保存中间结果，比刚刚的方法简单多了。

代码实现：

```mysql
WITH RECURSIVE cte
AS
(
SELECT employee_id,last_name,manager_id,1 AS n FROM employees WHERE employee_id = 100
-- 种子查询，找到第一代领导
UNION ALL
SELECT a.employee_id,a.last_name,a.manager_id,n+1 FROM employees AS a JOIN cte
ON (a.manager_id = cte.employee_id) -- 递归查询，找出以递归公用表表达式的人为领导的人
)
SELECT employee_id,last_name FROM cte WHERE n >= 3;
```

总之，递归公用表表达式对于查询一个有共同的根节点的树形结构数据，非常有用。它可以不受层级的 限制，轻松查出所有节点的数据。如果用其他的查询方式，就比较复杂了。

###  小结

公用表表达式的作用是可以替代子查询，而且可以被多次引用。递归公用表表达式对查询有一个共同根 节点的树形结构数据非常高效，可以轻松搞定其他查询方式难以处理的查询。

# Linux下MySQL安装使用

## 安装前说明

### Linux系统准备

![image-20221023164705149](D:\笔记\mysql.assets\image-20221023164705149.png)

### 查看是否安装过MySQL

![image-20221023164753595](D:\笔记\mysql.assets\image-20221023164753595.png)

```cmd
rpm -qa | grep -i mysql # -i 忽略大小写
systemctl status mysqld.service
```

![image-20221023164904834](D:\笔记\mysql.assets\image-20221023164904834.png)

### mysql卸载

![image-20221023164942124](D:\笔记\mysql.assets\image-20221023164942124.png)

```shell
systemctl stop mysqld.service
rpm -qa | grep -i mysql
# 或
yum list installed | grep mysql
yum remove mysql-xxx mysql-xxx mysql-xxx mysqk-xxxx
rpm -qa | grep -i mysql 
find / -name mysql
rm -rf xxx
rm -rf /etc/my.cnf
```

## 安装

### MySQL的4大版本  

![image-20221023165253246](D:\笔记\mysql.assets\image-20221023165253246.png)

### 下载MySQL指定版本  

![image-20221023165328610](D:\笔记\mysql.assets\image-20221023165328610.png)

![image-20221023165348561](D:\笔记\mysql.assets\image-20221023165348561.png)

![image-20221023165411098](D:\笔记\mysql.assets\image-20221023165411098.png)

![image-20221023165436508](D:\笔记\mysql.assets\image-20221023165436508.png)

![image-20221023165457101](D:\笔记\mysql.assets\image-20221023165457101.png)

### CentOS7下检查MySQL依赖  

![image-20221023165525939](D:\笔记\mysql.assets\image-20221023165525939.png)

```shell
chmod -R 777 /tmp
rpm -qa|grep libaio
rpm -qa|grep net-tools
rpm -qa|grep net-tools
```

### CentOS7下MySQL安装过程  

![image-20221023165622266](D:\笔记\mysql.assets\image-20221023165622266.png)

```shell
rpm -ivh mysql-community-common-8.0.25-1.el7.x86_64.rpm
rpm -ivh mysql-community-client-plugins-8.0.25-1.el7.x86_64.rpm
rpm -ivh mysql-community-libs-8.0.25-1.el7.x86_64.rpm
rpm -ivh mysql-community-client-8.0.25-1.el7.x86_64.rpm
rpm -ivh mysql-community-server-8.0.25-1.el7.x86_64.rpm
```

![image-20221023165648990](D:\笔记\mysql.assets\image-20221023165648990.png)

```shell
yum remove mysql-libs
```

![image-20221023165708376](D:\笔记\mysql.assets\image-20221023165708376.png)

```shell
mysql --version
#或
mysqladmin --version
rpm -qa|grep -i mysql
mysqld --initialize --user=mysql
cat /var/log/mysqld.log
```

![image-20221023165751209](D:\笔记\mysql.assets\image-20221023165751209.png)

```shell
#加不加.service后缀都可以
启动：systemctl start mysqld.service
关闭：systemctl stop mysqld.service
重启：systemctl restart mysqld.service
查看状态：systemctl status mysqld.service
```

![image-20221023165814783](D:\笔记\mysql.assets\image-20221023165814783.png)

```shell
ps -ef | grep -i mysql
systemctl list-unit-files|grep mysqld.service
systemctl enable mysqld.service
systemctl disable mysqld.service
```

## MySQL登录  

### 首次登录

通过  <font color = "orange">`mysql -hlocalhost -P3306 -uroot -p`</font>进行登录，在Enter password：录入初始化密码  

![image-20221023171114988](D:\笔记\mysql.assets\image-20221023171114988.png)

### 修改密码  

![image-20221023171129539](D:\笔记\mysql.assets\image-20221023171129539.png)

```shell
ALTER USER 'root'@'localhost' IDENTIFIED BY 'new_password';
```

### 设置远程登录  



![image-20221023171258373](D:\笔记\mysql.assets\image-20221023171258373.png)

![image-20221023171307112](D:\笔记\mysql.assets\image-20221023171307112.png)

![image-20221023171316330](D:\笔记\mysql.assets\image-20221023171316330.png)

```shell
telnet ip地址 端口号
```

![image-20221023171343352](D:\笔记\mysql.assets\image-20221023171343352.png)

![image-20221023171351600](D:\笔记\mysql.assets\image-20221023171351600.png)

```shell
systemctl start firewalld.service
systemctl status firewalld.service
systemctl stop firewalld.service
#设置开机启用防火墙
systemctl enable firewalld.service
#设置开机禁用防火墙
systemctl disable firewalld.service

firewall-cmd --list-all

firewall-cmd --add-service=http --permanent
firewall-cmd --add-port=3306/tcp --permanent

firewall-cmd --reload
```



![image-20221023171503353](D:\笔记\mysql.assets\image-20221023171503353.png)

```mysql
use mysql;
select Host,User from user;

update user set host = '%' where user ='root';

flush privileges;
```

![image-20221023171550289](D:\笔记\mysql.assets\image-20221023171550289.png)

```mysql
ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY 'abc123';
```

## MySQL8的密码强度评估（了解）  

### MySQL不同版本设置密码(可能出现)  

![image-20221023171624887](D:\笔记\mysql.assets\image-20221023171624887.png)

### MySQL8之前的安全策略  

![image-20221023171639211](D:\笔记\mysql.assets\image-20221023171639211.png)

```mysql
[mysqld]
plugin-load-add=validate_password.so
\#ON/OFF/FORCE/FORCE_PLUS_PERMANENT: 是否使用该插件(及强制/永久强制使用)
validate-password=FORCE_PLUS_PERMANENT
```

![image-20221023171709679](D:\笔记\mysql.assets\image-20221023171709679.png)

```mysql
mysql> INSTALL PLUGIN validate_password SONAME 'validate_password.so';
Query OK, 0 rows affected, 1 warning (0.11 sec)
```

### MySQL8的安全策略  

![image-20221023171755427](D:\笔记\mysql.assets\image-20221023171755427.png)

![image-20221023171803977](D:\笔记\mysql.assets\image-20221023171803977.png)

![image-20221023171823839](D:\笔记\mysql.assets\image-20221023171823839.png)

![image-20221023171832879](D:\笔记\mysql.assets\image-20221023171832879.png)

```mysql
SET GLOBAL validate_password_policy=LOW;
SET GLOBAL validate_password_policy=MEDIUM;
SET GLOBAL validate_password_policy=STRONG;
SET GLOBAL validate_password_policy=0; # For LOW
SET GLOBAL validate_password_policy=1; # For MEDIUM
SET GLOBAL validate_password_policy=2; # For HIGH
#注意，如果是插件的话,SQL为set global validate_password_policy=LOW

set global validate_password_length=1;
```

![image-20221023171902630](D:\笔记\mysql.assets\image-20221023171902630.png)

### 卸载插件、组件(了解)  

![image-20221023171920108](D:\笔记\mysql.assets\image-20221023171920108.png)

```mysql
UNINSTALL PLUGIN validate_password;

UNINSTALL COMPONENT 'file://component_validate_password';
```

## 字符集的相关操作  

### 修改MySQL5.7字符集  

![image-20221023172015583](D:\笔记\mysql.assets\image-20221023172015583.png)

```mysql
show variables like 'character%';
# 或者
show variables like '%char%';
```

![image-20221023172044047](D:\笔记\mysql.assets\image-20221023172044047.png)

![image-20221023172055595](D:\笔记\mysql.assets\image-20221023172055595.png)

```shell
vim /etc/my.cnf

character_set_server=utf8

systemctl restart mysqld
```

![image-20221023172126984](D:\笔记\mysql.assets\image-20221023172126984.png)

### 已有库&表字符集的变更  

![image-20221023172202196](D:\笔记\mysql.assets\image-20221023172202196.png)

```mysql
alter database dbtest1 character set 'utf8';

alter table t_emp convert to character set 'utf8';
```

>注意：但是原有的数据如果是用非'utf8'编码的话，数据本身编码不会发生改变。已有数据需要导
>出或删除，然后重新插入  

### 各级别的字符集  

![image-20221023174441091](D:\笔记\mysql.assets\image-20221023174441091.png)

```mysql
show variables like 'character%';
```

![image-20221023174501704](D:\笔记\mysql.assets\image-20221023174501704.png)

![image-20221023174541000](D:\笔记\mysql.assets\image-20221023174541000.png)



![image-20221023174628303](D:\笔记\mysql.assets\image-20221023174628303.png)

首先列 `col` 使用的字符集是 `gbk` ，一个字符` '我' `在 gbk 中的编码为 `0xCED2 `，占用两个字节，两个字符的实际数据就占用4个字节。如果把该列的字符集修改为` utf8 `的话，这两个字符就实际占用6个字节  

### 字符集与比较规则(了解)  

![image-20221023174735092](D:\笔记\mysql.assets\image-20221023174735092.png)

```mysql
#查看GBK字符集的比较规则
SHOW COLLATION LIKE 'gbk%';
#查看UTF-8字符集的比较规则
SHOW COLLATION LIKE 'utf8%';
#查看服务器的字符集和比较规则
SHOW VARIABLES LIKE '%_server';
#查看数据库的字符集和比较规则
SHOW VARIABLES LIKE '%_database';
#查看具体数据库的字符集
SHOW CREATE DATABASE dbtest1;
#修改具体数据库的字符集
ALTER DATABASE dbtest1 DEFAULT CHARACTER SET 'utf8' COLLATE 'utf8_general_ci';
#查看表的字符集
show create table employees;
#查看表的比较规则
show table status from atguigudb like 'employees';
#修改表的字符集和比较规则
ALTER TABLE emp1 DEFAULT CHARACTER SET 'utf8' COLLATE 'utf8_general_ci';
```

### 请求到响应过程中字符集的变化  

![image-20221023174834762](D:\笔记\mysql.assets\image-20221023174834762.png)

![image-20221023174845039](D:\笔记\mysql.assets\image-20221023174845039.png)

![image-20221023174852058](D:\笔记\mysql.assets\image-20221023174852058.png)

## SQL大小写规范  

### Windows和Linux平台区别  

![image-20221023174914360](D:\笔记\mysql.assets\image-20221023174914360.png)

### Linux下大小写规则设置  

![image-20221023174940330](D:\笔记\mysql.assets\image-20221023174940330.png)

### SQL编写建议  

![image-20221023174951712](D:\笔记\mysql.assets\image-20221023174951712.png)

## sql_mode的合理设置  

### 宽松模式 vs 严格模式  

![image-20221023175021286](D:\笔记\mysql.assets\image-20221023175021286.png)

![image-20221023175042601](D:\笔记\mysql.assets\image-20221023175042601.png)

### 宽松模式再举例  

![image-20221023175100934](D:\笔记\mysql.assets\image-20221023175100934.png)

### 模式查看和设置  

![image-20221023175123485](D:\笔记\mysql.assets\image-20221023175123485.png)

```mysql
select @@session.sql_mode
select @@global.sql_mode
#或者
show variables like 'sql_mode

SET GLOBAL sql_mode = 'modes...'; #全局
SET SESSION sql_mode = 'modes...'; #当前会话';

```

![image-20221023175158859](D:\笔记\mysql.assets\image-20221023175158859.png)

# MySQL的数据目录

## MySQL8的主要目录结构  

```shell
[root@atguigu01 ~]# find / -name mysql
```

### 数据库文件的存放路径  

<font color = brick red>MySQL数据库文件的存放路径：/var/lib/mysql/  </font>

```mysql
mysql> show variables like 'datadir';
+---------------+-----------------+
| Variable_name | Value |
+---------------+-----------------+
| datadir | /var/lib/mysql/ |
+---------------+-----------------+
1 row in set (0.04 sec)
```

从结果中可以看出，在我的计算机上MySQL的数据目录就是 `/var/lib/mysql/ `  

### 相关命令目录

<font color = brick red>相关命令目录：/usr/bin（mysqladmin、mysqlbinlog、mysqldump等命令）和/usr/sbin。    </font>

![image-20221024144545213](D:\笔记\mysql.assets\image-20221024144545213.png)

### 配置文件目录  

<font color = brick red>配置文件目录：/usr/share/mysql-8.0（命令及配置文件），/etc/mysql（如my.cnf）      </font>

![image-20221024144620112](D:\笔记\mysql.assets\image-20221024144620112.png)

## 数据库和文件系统的关系  

### 查看默认数据库  

查看一下在我的计算机上当前有哪些数据库：  

```mysql
mysql> SHOW DATABASES;
```

可以看到有4个数据库是属于MySQL自带的系统数据库。  

- `mysql  `

MySQL 系统自带的核心数据库，它存储了MySQL的用户账户和权限信息，一些存储过程、事件的定义信息，一些运行过程中产生的日志信息，一些帮助信息以及时区信息等。  

- `information_schema  `

MySQL 系统自带的数据库，这个数据库保存着MySQL服务器` 维护的所有其他数据库的信息` ，比如有哪些表、哪些视图、哪些触发器、哪些列、哪些索引。这些信息并不是真实的用户数据，而是一些描述性信息，有时候也称之为 `元数据` 。在系统数据库` information_schema `中提供了一些以`innodb_sys `开头的表，用于表示内部系统表。  

- `performance_schema `

MySQL 系统自带的数据库，这个数据库里主要保存MySQL服务器运行过程中的一些状态信息，可以用来` 监控 MySQL 服务的各类性能指标` 。包括统计最近执行了哪些语句，在执行过程的每个阶段都花费了多长时间，内存的使用情况等信息。  

- `sys  `

MySQL 系统自带的数据库，这个数据库主要是通过 `视图 `的形式把 `information_schema `和`performance_schema` 结合起来，帮助系统管理员和开发人员监控 MySQL 的技术性能。  

### 数据库在文件系统中的表示

看一下我的计算机上的数据目录下的内容：  

```shell
[root@atguigu01 mysql]# cd /var/lib/mysql
[root@atguigu01 mysql]# ll
总用量 189980
-rw-r-----. 1 mysql mysql 56 7月 28 00:27 auto.cnf
-rw-r-----. 1 mysql mysql 179 7月 28 00:27 binlog.000001
-rw-r-----. 1 mysql mysql 820 7月 28 01:00 binlog.000002
-rw-r-----. 1 mysql mysql 179 7月 29 14:08 binlog.000003
-rw-r-----. 1 mysql mysql 582 7月 29 16:47 binlog.000004
-rw-r-----. 1 mysql mysql 179 7月 29 16:51 binlog.000005
-rw-r-----. 1 mysql mysql 179 7月 29 16:56 binlog.000006
-rw-r-----. 1 mysql mysql 179 7月 29 17:37 binlog.000007
-rw-r-----. 1 mysql mysql 24555 7月 30 00:28 binlog.000008
-rw-r-----. 1 mysql mysql 179 8月 1 11:57 binlog.000009
-rw-r-----. 1 mysql mysql 156 8月 1 23:21 binlog.000010
-rw-r-----. 1 mysql mysql 156 8月 2 09:25 binlog.000011
-rw-r-----. 1 mysql mysql 1469 8月 4 01:40 binlog.000012
-rw-r-----. 1 mysql mysql 156 8月 6 00:24 binlog.000013
-rw-r-----. 1 mysql mysql 179 8月 6 08:43 binlog.000014
-rw-r-----. 1 mysql mysql 156 8月 6 10:56 binlog.000015
-rw-r-----. 1 mysql mysql 240 8月 6 10:56 binlog.index
-rw-------. 1 mysql mysql 1676 7月 28 00:27 ca-key.pem
-rw-r--r--. 1 mysql mysql 1112 7月 28 00:27 ca.pem
-rw-r--r--. 1 mysql mysql 1112 7月 28 00:27 client-cert.pem
-rw-------. 1 mysql mysql 1676 7月 28 00:27 client-key.pem
drwxr-x---. 2 mysql mysql 4096 7月 29 16:34 dbtest
-rw-r-----. 1 mysql mysql 196608 8月 6 10:58 #ib_16384_0.dblwr
-rw-r-----. 1 mysql mysql 8585216 7月 28 00:27 #ib_16384_1.dblwr
-rw-r-----. 1 mysql mysql 3486 8月 6 08:43 ib_buffer_pool
-rw-r-----. 1 mysql mysql 12582912 8月 6 10:56 ibdata1
-rw-r-----. 1 mysql mysql 50331648 8月 6 10:58 ib_logfile0
-rw-r-----. 1 mysql mysql 50331648 7月 28 00:27 ib_logfile1
-rw-r-----. 1 mysql mysql 12582912 8月 6 10:56 ibtmp1
drwxr-x---. 2 mysql mysql 4096 8月 6 10:56 #innodb_temp
drwxr-x---. 2 mysql mysql 4096 7月 28 00:27 mysql
-rw-r-----. 1 mysql mysql 26214400 8月 6 10:56 mysql.ibd
srwxrwxrwx. 1 mysql mysql 0 8月 6 10:56 mysql.sock
-rw-------. 1 mysql mysql 5 8月 6 10:56 mysql.sock.lock
drwxr-x---. 2 mysql mysql 4096 7月 28 00:27 performance_schema
-rw-------. 1 mysql mysql 1680 7月 28 00:27 private_key.pem
-rw-r--r--. 1 mysql mysql 452 7月 28 00:27 public_key.pem
-rw-r--r--. 1 mysql mysql 1112 7月 28 00:27 server-cert.pem
-rw-------. 1 mysql mysql 1680 7月 28 00:27 server-key.pem
drwxr-x---. 2 mysql mysql 4096 7月 28 00:27 sys
drwxr-x---. 2 mysql mysql 4096 7月 29 23:10 temp
-rw-r-----. 1 mysql mysql 16777216 8月 6 10:58 undo_001
-rw-r-----. 1 mysql mysql 16777216 8月 6 10:58 undo_002
```

这个数据目录下的文件和子目录比较多，除了` information_schema` 这个系统数据库外，其他的数据库在` 数据目录 `下都有对应的子目录。  

以我的 `temp` 数据库为例，在MySQL5.7 中打开：  

```shell
[root@atguigu02 mysql]# cd ./temp
[root@atguigu02 temp]# ll
总用量 1144
-rw-r-----. 1 mysql mysql 8658 8月 18 11:32 countries.frm
-rw-r-----. 1 mysql mysql 114688 8月 18 11:32 countries.ibd
-rw-r-----. 1 mysql mysql 61 8月 18 11:32 db.opt
-rw-r-----. 1 mysql mysql 8716 8月 18 11:32 departments.frm
-rw-r-----. 1 mysql mysql 147456 8月 18 11:32 departments.ibd
-rw-r-----. 1 mysql mysql 3017 8月 18 11:32 emp_details_view.frm
-rw-r-----. 1 mysql mysql 8982 8月 18 11:32 employees.frm
-rw-r-----. 1 mysql mysql 180224 8月 18 11:32 employees.ibd
-rw-r-----. 1 mysql mysql 8660 8月 18 11:32 job_grades.frm
-rw-r-----. 1 mysql mysql 98304 8月 18 11:32 job_grades.ibd
-rw-r-----. 1 mysql mysql 8736 8月 18 11:32 job_history.frm
-rw-r-----. 1 mysql mysql 147456 8月 18 11:32 job_history.ibd
-rw-r-----. 1 mysql mysql 8688 8月 18 11:32 jobs.frm
-rw-r-----. 1 mysql mysql 114688 8月 18 11:32 jobs.ibd
-rw-r-----. 1 mysql mysql 8790 8月 18 11:32 locations.frm
-rw-r-----. 1 mysql mysql 131072 8月 18 11:32 locations.ibd
-rw-r-----. 1 mysql mysql 8614 8月 18 11:32 regions.frm
-rw-r-----. 1 mysql mysql 114688 8月 18 11:32 regions.ibd
```

在MySQL8.0中打开：  

```shell
[root@atguigu01 mysql]# cd ./temp
[root@atguigu01 temp]# ll
总用量 1080
-rw-r-----. 1 mysql mysql 131072 7月 29 23:10 countries.ibd
-rw-r-----. 1 mysql mysql 163840 7月 29 23:10 departments.ibd
-rw-r-----. 1 mysql mysql 196608 7月 29 23:10 employees.ibd
-rw-r-----. 1 mysql mysql 114688 7月 29 23:10 job_grades.ibd
-rw-r-----. 1 mysql mysql 163840 7月 29 23:10 job_history.ibd
-rw-r-----. 1 mysql mysql 131072 7月 29 23:10 jobs.ibd
-rw-r-----. 1 mysql mysql 147456 7月 29 23:10 locations.ibd
-rw-r-----. 1 mysql mysql 131072 7月 29 23:10 regions.ibd

```

### 表在文件系统中的表示  

#### InnoDB存储引擎模式  

<font size = 6 color = #870000>1.表结构  </font>

为了保存表结构， `InnoDB `在 `数据目录 `下对应的数据库子目录下创建了一个专门用于` 描述表结构的文件` ，文件名是这样：  

```
表名.frm
```

比方说我们在 `atguigu `数据库下创建一个名为 `test `的表  

那在数据库 `atguigu `对应的子目录下就会创建一个名为 `test.frm` 的用于描述表结构的文件。.frm文件的格式在不同的平台上都是相同的。这个后缀名为.frm是以 `二进制格式` 存储的，我们直接打开是乱码的。  

<font size = 6 color = #870000>2.表中数据和索引    </font>

<font size = 5 color = #870000>① 系统表空间（system tablespace）      </font>

默认情况下，InnoDB会在数据目录下创建一个名为 `ibdata1 `、大小为 `12M `的文件，这个文件就是对应的 `系统表空间 `在文件系统上的表示。怎么才12M？注意这个文件是 自扩展文件 ，当不够用的时候它会自己增加文件大小 。

当然，如果你想让系统表空间对应文件系统上多个实际文件，或者仅仅觉得原来的 ibdata1 这个文件名难听，那可以在MySQL启动时配置对应的文件路径以及它们的大小，比如我们这样修改一下my.cnf 配置文件：  

```shell
[server]
innodb_data_file_path=data1:512M;data2:512M:autoextend
```

<font size = 5 color = #870000>② 独立表空间(file-per-table tablespace)        </font>

在MySQL5.6.6以及之后的版本中，InnoDB并不会默认的把各个表的数据存储到系统表空间中，而是为` 每一个表建立一个独立表空间` ，也就是说我们创建了多少个表，就有多少个独立表空间。使用 `独立表空间 `来存储表数据的话，会在该表所属数据库对应的子目录下创建一个表示该独立表空间的文件，文件名和表名相同，只不过添加了一个` .ibd `的扩展名而已，所以完整的文件名称长这样：  

```
表名.ibd
```

比如：我们使用了` 独立表空间` 去存储` atguigu `数据库下的` test `表的话，那么在该表所在数据库对应的 `atguigu` 目录下会为 `test `表创建这两个文件  :

```
test.frm
test.ibd
```

其中 `test.ibd `文件就用来存储 `test `表中的数据和索引.  

<font size = 5 color = #870000>③ 系统表空间与独立表空间的设置         </font>

我们可以自己指定使用 `系统表空间 `还是` 独立表空间 `来存储数据，这个功能由启动参数`innodb_file_per_table `控制，比如说我们想刻意将表数据都存储到` 系统表空间` 时，可以在启动MySQL服务器的时候这样配置：  

```
[server]
innodb_file_per_table=0 # 0：代表使用系统表空间； 1：代表使用独立表空间
```

<font size = 5 color = #870000>④ 其他类型的表空间          </font>

随着MySQL的发展，除了上述两种老牌表空间之外，现在还新提出了一些不同类型的表空间，比如通用表空间（general tablespace）、临时表空间（temporary tablespace）等。  

#### MyISAM存储引擎模式  

<font size = 6 color = #870000>1.表结构  </font>

在存储表结构方面， `MyISAM `和 `InnoDB `一样，也是在 `数据目录` 下对应的数据库子目录下创建了一个专门用于描述表结构的文件：  

```
表名.frm
```

<font size = 6 color = #870000>2. 表中数据和索引    </font>

在MyISAM中的索引全部都是 `二级索引 `，该存储引擎的 `数据和索引是分开存放 `的。所以在文件系统中也是使用不同的文件来存储数据文件和索引文件，同时表数据都存放在对应的数据库子目录下。假如 `test`表使用MyISAM存储引擎的话，那么在它所在数据库对应的 `atguigu `目录下会为 `test `表创建这三个文件：  

```
test.frm 存储表结构
test.MYD 存储数据 (MYData)
test.MYI 存储索引 (MYIndex)
```

### 小结  

举例：` 数据库a` ，`表b` 。 

1、如果表b采用 `InnoDB `，data\a中会产生1个或者2个文件： 

-  `b.frm `  ：描述表结构文件，字段长度等 
- 如果采用 `系统表空间`模式的，数据信息和索引信息都存储在 `ibdata1 `中 
- 如果采用` 独立表空间`存储模式，data\a中还会产生` b.ibd` 文件（存储数据信息和索引信息） 

此外： 

①  MySQL5.7  中会在data/a的目录下生成 `db.opt `文件用于保存数据库的相关配置。比如：字符集、比较规则。而MySQL8.0不再提供db.opt文件。 

②  MySQL8.0中不再单独提供b.frm ，而是合并在b.ibd文件中。 

2、如果表b采用 `MyISAM `，data\a中会产生3个文件： 

- MySQL5.7  中：` b.frm` ：描述表结构文件，字段长度等。 

	MySQL8.0  中 ` b.xxx.sdi` ：描述表结构文件，字段长度等 

- ` b.MYD` (MY Data) ：数据信息文件，存储数据信息(如果采用独立表存储模式) 

- ` b.MYI `(MY Index) ：存放索引信息文件 

# 用户与权限管理  

## 用户管理

### 登录MySQL服务器  

启动MySQL服务后，可以通过mysql命令来登录MySQL服务器，命令如下：  

```mysql
mysql –h hostname|hostIP –P port –u username –p DatabaseName –e "SQL语句"
```

下面详细介绍命令中的参数：  

- `-h参数  `后面接主机名或者主机IP，hostname为主机，hostIP为主机IP。

- `-P参数   `后面接MySQL服务的端口，通过该参数连接到指定的端口。MySQL服务的默认端口是3306，不使用该参数时自动连接到3306端口，port为连接的端口号。    

- `-u参数   `后面接用户名，username为用户名。  

- `-p参数  `会提示输入密码。  

- `DatabaseName参数   `指明登录到哪一个数据库中。如果没有该参数，就会直接登录到MySQL数据库中，然后可以使用USE命令来选择数据库。  

- `-e参数  `后面可以直接加SQL语句。登录MySQL服务器以后即可执行这个SQL语句，然后退出MySQL服务器。  

	举例：  

	```mysql
	mysql -uroot -p -hlocalhost -P3306 mysql -e "select host,user from user"
	```

### 创建用户

```mysql
CREATE USER语句的基本语法形式如下：  

CREATE USER 用户名 [IDENTIFIED BY '密码'][,用户名 [IDENTIFIED BY '密码']];

```

- 用户名参数表示新建用户的账户，由 `用户（User） `和 `主机名（Host）` 构成；  

- “[ ]”表示可选，也就是说，可以指定用户登录时需要密码验证，也可以不指定密码验证，这样用户可以直接登录。不过，不指定密码的方式不安全，不推荐使用。如果指定密码值，这里需要使用IDENTIFIED BY指定明文密码值。  

- CREATE USER语句可以同时创建多个用户。  

举例：  

```mysql
CREATE USER zhang3 IDENTIFIED BY '123123'; # 默认host是 %
```

```mysql
CREATE USER 'kangshifu'@'localhost' IDENTIFIED BY '123456';
```



### 修改用户

修改用户名：  

```mysql
UPDATE mysql.user SET USER='li4' WHERE USER='wang5';
FLUSH PRIVILEGES;
```

![image-20221024192515127](D:\笔记\mysql.assets\image-20221024192515127.png)

### 删除用户  

方式1：使用DROP方式删除（推荐）  

使用DROP USER语句来删除用户时，必须用于DROP USER权限。DROP USER语句的基本语法形式如下：  

```mysql
DROP USER user[,user]…;
```

举例：  

```mysql
DROP USER li4 ; # 默认删除host为%的用户
```

```mysql
DROP USER 'kangshifu'@'localhost';
```



![image-20221024192809093](D:\笔记\mysql.assets\image-20221024192809093.png)

方式2：使用DELETE方式删除  

```mysql
DELETE FROM mysql.user WHERE Host=’hostname’ AND User=’username’;
```

执行完DELETE命令后要使用FLUSH命令来使用户生效，命令如下：  

```mysql
FLUSH PRIVILEGES;
```

>注意：不推荐通过 DELETE FROM USER u WHERE USER='li4' 进行删除，系统会有残留信息保留。而drop user命令会删除用户以及对应的权限，执行命令后你会发现mysql.user表和mysql.db表的相应记录都消失了。  

### 设置当前用户密码  

`旧的写法如下   `:

```mysql
# 修改当前用户的密码：（MySQL5.7测试有效）
SET PASSWORD = PASSWORD('123456');
```

这里介绍 `推荐的写法` ：  

1. 使用ALTER USER命令来修改当前用户密码 用户可以使用ALTER命令来修改自身密码，如下语句代表修改当前登录用户的密码。基本语法如下：

	```mysql
	ALTER USER USER() IDENTIFIED BY 'new_password';
	```

2. 使用SET语句来修改当前用户密码 使用root用户登录MySQL后，可以使用SET语句来修改密码，具体SQL语句如下：  

	```mysql
	SET PASSWORD='new_password';
	```

	该语句会自动将密码加密后再赋给当前用户  。

### 修改其它用户密码

```mysql
1.使用ALTER语句来修改普通用户的密码 可以使用ALTER USER语句来修改普通用户的密码。基本语法形式如下：

ALTER USER user [IDENTIFIED BY '新密码']
[,user[IDENTIFIED BY '新密码']]…;

2.使用SET命令来修改普通用户的密码 使用root用户登录到MySQL服务器后，可以使用SET语句来修改普通用户的密码。SET语句的代码如下：  

 SET PASSWORD FOR 'username'@'hostname'='new_password';

 3.使用UPDATE语句修改普通用户的密码（不推荐）  

UPDATE MySQL.user SET authentication_string=PASSWORD("123456")
WHERE User = "username" AND Host = "hostname";

```


​	

### MySQL8密码管理(了解)  

1. 密码过期策略  

- 在MySQL中，数据库管理员可以` 手动设置` 账号密码过期，也可以建立一个 `自动 `密码过期策略。 

- 过期策略可以是 `全局的` ，也可以为` 每个账号 `设置单独的过期策略。

	```mysql
	ALTER USER user PASSWORD EXPIRE;
	```

	   练习：  

	```mysql
	ALTER USER 'kangshifu'@'localhost' PASSWORD EXPIRE;
	```

- 方式①：使用SQL语句更改该变量的值并持久化  

	```mysql
	SET PERSIST default_password_lifetime = 180; # 建立全局策略，设置密码每隔180天过期
	```

- 方式②：配置文件my.cnf中进行维护  

	```mysql
	[mysqld]
	default_password_lifetime=180 #建立全局策略，设置密码每隔180天过期
	```

	手动设置指定时间过期方式2：单独设置  

	每个账号既可延用全局密码过期策略，也可单独设置策略。在` CREATE USER `和` ALTER USER` 语句上加入 `PASSWORD EXPIRE `选项可实现单独设置策略。下面是一些语句示例。  

	```mysql
	#设置kangshifu账号密码每90天过期：
	CREATE USER 'kangshifu'@'localhost' PASSWORD EXPIRE INTERVAL 90 DAY;
	ALTER USER 'kangshifu'@'localhost' PASSWORD EXPIRE INTERVAL 90 DAY;
	#设置密码永不过期：
	CREATE USER 'kangshifu'@'localhost' PASSWORD EXPIRE NEVER;
	ALTER USER 'kangshifu'@'localhost' PASSWORD EXPIRE NEVER;
	#延用全局密码过期策略：
	CREATE USER 'kangshifu'@'localhost' PASSWORD EXPIRE DEFAULT;
	ALTER USER 'kangshifu'@'localhost' PASSWORD EXPIRE DEFAULT;
	```

	2. 密码重用策略  

- 手动设置密码重用方式1：全局  

方式①：使用SQL  

```mysql
SET PERSIST password_history = 6; #设置不能选择最近使用过的6个密码
SET PERSIST password_reuse_interval = 365; #设置不能选择最近一年内的密码
```

![image-20221024195442603](D:\笔记\mysql.assets\image-20221024195442603.png)

方式②：my.cnf配置文件  

```mysql
[mysqld]
password_history=6
password_reuse_interval=365
```

- 手动设置密码重用方式2：单独设置  

```mysql
#不能使用最近5个密码：
CREATE USER 'kangshifu'@'localhost' PASSWORD HISTORY 5;
ALTER USER 'kangshifu'@'localhost' PASSWORD HISTORY 5;
#不能使用最近365天内的密码：
CREATE USER 'kangshifu'@'localhost' PASSWORD REUSE INTERVAL 365 DAY;
ALTER USER 'kangshifu'@'localhost' PASSWORD REUSE INTERVAL 365 DAY;
#既不能使用最近5个密码，也不能使用365天内的密码
CREATE USER 'kangshifu'@'localhost'
PASSWORD HISTORY 5
PASSWORD REUSE INTERVAL 365 DAY;
ALTER USER 'kangshifu'@'localhost'
PASSWORD HISTORY 5
PASSWORD REUSE INTERVAL 365 DAY;
```

## 权限管理  

### 权限列表  

MySQL到底都有哪些权限呢？  

```mysql
mysql> show privileges;
```

（1）` CREATE和DROP权限 `，可以创建新的数据库和表，或删除（移掉）已有的数据库和表。如果将MySQL数据库中的DROP权限授予某用户，用户就可以删MySQL访问权限保存的数据库。   

（2）`SELECT、INSERT、UPDATE和DELETE权限 `允许在一个数据库现有的表上实施操作。  

（3）` SELECT权限`只有在它们真正从一个表中检索行时才被用到。   

（4）` INDEX权限` 允许创建或删除索引，INDEX适用于已有的表。如果具有某个表的CREATE权限，就可以在CREATE TABLE语句中包括索引定义。   

（5）` ALTER权限` 可以使用ALTER TABLE来更改表的结构和重新命名表。   

（6） `CREATE ROUTINE权限 `用来创建保存的程序（函数和程序），ALTER ROUTINE权限用来更改和删除保存的程序，` EXECUTE权限 `用来执行保存的程序。   

（7）` GRANT权限 `允许授权给其他用户，可用于数据库、表和保存的程序。

（8）` FILE权限` 使用户可以使用LOAD DATA INFILE和SELECT ... INTO OUTFILE语句读或写服务器上的文件，任何被授予FILE权限的用户都能读或写MySQL服务器上的任何文件（说明用户可以读任何数据库目录下的文件，因为服务器可以访问这些文件）。  

###  授予权限的原则  

权限控制主要是出于安全因素，因此需要遵循以下几个 `经验原则 `：  

1、只授予能 `满足需要的最小权限 `，防止用户干坏事。比如用户只是需要查询，那就只给select权限就可以了，不要给用户赋予update、insert或者delete权限。 

 2、创建用户的时候` 限制用户的登录主机 `，一般是限制成指定IP或者内网IP段。 

 3、为每个用户` 设置满足密码复杂度的密码` 。  

4、` 定期清理不需要的用户 `，回收权限或者删除用户。  

### 授予权限  

给用户授权的方式有 2 种，分别是通过把 `角色赋予用户给用户授权 `和` 直接给用户授权` 。用户是数据库的使用者，我们可以通过给用户授予访问数据库中资源的权限，来控制使用者对数据库的访问，消除安全隐患。  

授权命令：  

```mysql
GRANT 权限1,权限2,…权限n ON 数据库名称.表名称 TO 用户名@用户地址 [IDENTIFIED BY ‘密码口令’];
```

- 该权限如果发现没有该用户，则会直接新建一个用户。  

	比如：  

- 给li4用户用本地命令行方式，授予atguigudb这个库下的所有表的插删改查的权限。  

	```mysql
	GRANT SELECT,INSERT,DELETE,UPDATE ON atguigudb.* TO li4@localhost ;
	```

- 授予通过网络方式登录的joe用户 ，对所有库所有表的全部权限，密码设为123。注意这里唯独不包括grant的权限  

```mysql
GRANT ALL PRIVILEGES ON *.* TO joe@'%' IDENTIFIED BY '123';
```

> 我们在开发应用的时候，经常会遇到一种需求，就是要根据用户的不同，对数据进行横向和纵向的分组。  
>
> - 所谓横向的分组，就是指用户可以接触到的数据的范围，比如可以看到哪些表的数据；  
> - 所谓纵向的分组，就是指用户对接触到的数据能访问到什么程度，比如能看、能改，甚至是删除。  

### 查看权限  

- 查看当前用户权限  

```mysql
SHOW GRANTS;
# 或
SHOW GRANTS FOR CURRENT_USER;
# 或
SHOW GRANTS FOR CURRENT_USER();
```

- 查看某用户的全局权限  

```mysql
SHOW GRANTS FOR 'user'@'主机地址' ;
```

### 收回权限  

收回权限就是取消已经赋予用户的某些权限。收回用户不必要的权限可以在一定程度上保证系统的安全性。MySQL中使用 `REVOKE语句 `取消用户的某些权限。使用REVOKE收回权限之后，用户账户的记录将从db、host、tables_priv和columns_priv表中删除，但是用户账户记录仍然在user表中保存（删除user表中的账户记录使用DROP USER语句）。  

> 注意：在将用户账户从user表删除之前，应该收回相应用户的所有权限。  

- 收回权限命令  

```mysql
REVOKE 权限1,权限2,…权限n ON 数据库名称.表名称 FROM 用户名@用户地址;
```

- 举例  

```mysql
#收回全库全表的所有权限
REVOKE ALL PRIVILEGES ON *.* FROM joe@'%';
#收回mysql库下的所有表的插删改查权限
REVOKE SELECT,INSERT,UPDATE,DELETE ON mysql.* FROM joe@localhost;
```

- 注意：` 须用户重新登录后才能生效  `

## 权限表  

### user表

user表是MySQL中最重要的一个权限表， `记录用户账号和权限信息 `，有49个字段。如下图：  

![image-20221024202338925](D:\笔记\mysql.assets\image-20221024202338925.png)

这些字段可以分成4类，分别是范围列（或用户列）、权限列、安全列和资源控制列。 

1.范围列（或用户列）  

-  host ： 表示连接类型  
	- `%`表示所有远程通过 TCP方式的连接  
	- `IP 地址  `如 (192.168.1.2、127.0.0.1) 通过制定ip地址进行的TCP方式的连接  
	- `机器名  `通过制定网络中的机器名进行的TCP方式的连接  
	- `::1  `IPv6的本地ip地址，等同于IPv4的 127.0.0.1  
	- `localhost   `本地方式通过命令行方式的连接 ，比如mysql -u xxx -p xxx 方式的连接。  

- user ： 表示用户名，同一用户通过不同方式链接的权限是不一样的。  
- password ： 密码  
	- 所有密码串通过 password(明文字符串) 生成的密文字符串。MySQL 8.0 在用户管理方面增加了角色管理，默认的密码加密方式也做了调整，由之前的` SHA1 `改为了` SHA2 `，不可逆 。同时加上 MySQL 5.7 的禁用用户和用户过期的功能，MySQL 在用户管理方面的功能和安全性都较之前版本大大的增强了。  
	- mysql 5.7 及之后版本的密码保存到` authentication_string `字段中不再使用password 字段。  

2. 权限列  

- Grant_priv字段  
	- 表示是否拥有GRANT权限  

- Shutdown_priv字段  
	- 表示是否拥有停止MySQL服务的权限  

- Super_priv字段  
	- 表示是否拥有超级权限  

- Execute_priv字段  
	- 表示是否拥有EXECUTE权限。拥有EXECUTE权限，可以执行存储过程和函数。  

- Select_priv , Insert_priv等  
	- 为该用户所拥有的权限  。

3. 安全列  

安全列只有6个字段，其中两个是ssl相关的（ssl_type、ssl_cipher），用于 `加密 `；两个是x509相关的（x509_issuer、x509_subject），用于 `标识用户 `；另外两个Plugin字段用于` 验证用户身份` 的插件，该字段不能为空。如果该字段为空，服务器就使用内建授权验证机制验证用户身份。  

4. 资源控制列  

资源控制列的字段用来 `限制用户使用的资源 `，包含4个字段，分别为：  

①max_questions，用户每小时允许执行的查询操作次数；  

②max_updates，用户每小时允许执行的更新操作次数；   

③max_connections，用户每小时允许执行的连接操作次数；   

④max_user_connections，用户允许同时建立的连接次数。  

查看字段：  

```mysql
DESC mysql.user;
```

查看用户, 以列的方式显示数据：  

```mysql
SELECT * FROM mysql.user \G;
```

查询特定字段：  

```mysql
SELECT host,user,authentication_string,select_priv,insert_priv,drop_priv
FROM mysql.user;
```

![image-20221024203547737](D:\笔记\mysql.assets\image-20221024203547737.png)

### db表  

使用DESCRIBE查看db表的基本结构：  

```mysql
DESCRIBE mysql.db;
```

1. 用户列  

db表用户列有3个字段，分别是Host、User、Db。这3个字段分别表示主机名、用户名和数据库名。表示从某个主机连接某个用户对某个数据库的操作权限，这3个字段的组合构成了db表的主键。  

2. 权限列  

Create_routine_priv和Alter_routine_priv这两个字段决定用户是否具有创建和修改存储过程的权限  。

### tables_priv表和columns_priv表  

tables_priv表用来` 对表设置操作权限` ，columns_priv表用来对表的` 某一列设置权限` 。tables_priv表和columns_priv表的结构分别如图：  

```mysql
desc mysql.tables_priv;
```

tables_priv表有8个字段，分别是Host、Db、User、Table_name、Grantor、Timestamp、Table_priv和Column_priv，各个字段说明如下： 

- `Host  `、`Db   `、`User  `和`Table_name   `四个字段分别表示主机名、数据库名、用户名和表名。  
- Grantor表示修改该记录的用户。  
- Timestamp表示修改该记录的时间。  
- `Table_priv   `表示对象的操作权限。包括Select、Insert、Update、Delete、Create、Drop、Grant、References、Index和Alter。  
- Column_priv字段表示对表中的列的操作权限，包括Select、Insert、Update和References。  

```mysql
desc mysql.columns_priv;
```

### procs_priv表  

procs_priv表可以对 `存储过程和存储函数设置操作权限 `，表结构如图：  

```mysql
desc mysql.procs_priv;
```

![image-20221024204408639](D:\笔记\mysql.assets\image-20221024204408639.png)

## 访问控制(了解)  

### 连接核实阶段  

当用户试图连接MySQL服务器时，服务器基于用户的身份以及用户是否能提供正确的密码验证身份来确定接受或者拒绝连接。即客户端用户会在连接请求中提供用户名、主机地址、用户密码，MySQL服务器接收到用户请求后，会使用user表中的host、user和authentication_string这3个字段匹配客户端提供信息。  

服务器只有在user表记录的Host和User字段匹配客户端主机名和用户名，并且提供正确的密码时才接受连接。如果连接核实没有通过，服务器就完全拒绝访问；否则，服务器接受连接，然后进入阶段2等待用户请求。

###   请求核实阶段  

一旦建立了连接，服务器就进入了访问控制的阶段2，也就是请求核实阶段。对此连接上进来的每个请求，服务器检查该请求要执行什么操作、是否有足够的权限来执行它，这正是需要授权表中的权限列发挥作用的地方。这些权限可以来自user、db、table_priv和column_priv表。  

确认权限时，MySQL首先 `检查user表` ，如果指定的权限没有在user表中被授予，那么MySQL就会继续 `检查db表 `，db表是下一安全层级，其中的权限限定于数据库层级，在该层级的SELECT权限允许用户查看指定数据库的所有表中的数据；如果在该层级没有找到限定的权限，则MySQL继续` 检查tables_priv表` 以及 `columns_priv表` ，如果所有权限表都检查完毕，但还是没有找到允许的权限操作，MySQL将 `返回错误信息` ，用户请求的操作不能执行，操作失败。  

> 提示： MySQL通过向下层级的顺序（从user表到columns_priv表）检查权限表，但并不是所有的权限都要执行该过程。例如，一个用户登录到MySQL服务器之后只执行对MySQL的管理操作，此时只涉及管理权限，因此MySQL只检查user表。另外，如果请求的权限操作不被允许，MySQL也不会继续检查下一层级的表。  

## 角色管理  

### 角色的理解

引入角色的目的是` 方便管理拥有相同权限的用户 `。恰当的权限设定，可以确保数据的安全性，这是至关重要的。

![image-20221024204902931](D:\笔记\mysql.assets\image-20221024204902931.png)

### 创建角色  

创建角色使用 `CREATE ROLE `语句，语法如下：  

```mysql
CREATE ROLE 'role_name'[@'host_name'] [,'role_name'[@'host_name']]...
```

角色名称的命名规则和用户名类似。如果 `host_name省略，默认为% ， role_name不可省略` ，不可为空。  

练习：我们现在需要创建一个经理的角色，就可以用下面的代码：  

```mysql
CREATE ROLE 'manager'@'localhost';
```

### 给角色赋予权限  

创建角色之后，默认这个角色是没有任何权限的，我们需要给角色授权。给角色授权的语法结构是：  

```mysql
GRANT privileges ON table_name TO 'role_name'[@'host_name'];
```

上述语句中privileges代表权限的名称，多个权限以逗号隔开。可使用SHOW语句查询权限名称，图11-43列出了部分权限列表。 

 ```mysql
 SHOW PRIVILEGES\G;
 ```

![image-20221024205233184](D:\笔记\mysql.assets\image-20221024205233184.png)

![image-20221024205250974](D:\笔记\mysql.assets\image-20221024205250974.png)

练习1：我们现在想给经理角色授予商品信息表、盘点表和应付账款表的只读权限，就可以用下面的代码来实现：  

```mysql
GRANT SELECT ON demo.settlement TO 'manager';
GRANT SELECT ON demo.goodsmaster TO 'manager';
GRANT SELECT ON demo.invcount TO 'manager';
```

### 查看角色的权限  

赋予角色权限之后，我们可以通过 SHOW GRANTS 语句，来查看权限是否创建成功了：  

```mysql
mysql> SHOW GRANTS FOR 'manager';
+-------------------------------------------------------+
| Grants for manager@% |
+-------------------------------------------------------+
| GRANT USAGE ON *.* TO `manager`@`%` |
| GRANT SELECT ON `demo`.`goodsmaster` TO `manager`@`%` |
| GRANT SELECT ON `demo`.`invcount` TO `manager`@`%` |
| GRANT SELECT ON `demo`.`settlement` TO `manager`@`%` |
+-------------------------------------------------------+
```

只要你创建了一个角色，系统就会自动给你一个“` USAGE `”权限，意思是` 连接登录数据库的权限 `。代码的最后三行代表了我们给角色“manager”赋予的权限，也就是对商品信息表、盘点表和应付账款表的只读权限。  

结果显示，库管角色拥有商品信息表的只读权限和盘点表的增删改查权限。  

### 回收角色的权限  

角色授权后，可以对角色的权限进行维护，对权限进行添加或撤销。添加权限使用GRANT语句，与角色授权相同。撤销角色或角色权限使用REVOKE语句。  

修改了角色的权限，会影响拥有该角色的账户的权限。  

撤销角色权限的SQL语法如下：  

```mysql
REVOKE privileges ON tablename FROM 'rolename';
```

练习1：撤销school_write角色的权限。  

（1）使用如下语句撤销school_write角色的权限。  

```mysql
REVOKE INSERT, UPDATE, DELETE ON school.* FROM 'school_write';
```

（2）撤销后使用SHOW语句查看school_write对应的权限，语句如下。  

```mysql
SHOW GRANTS FOR 'school_write';
```

### 删除角色  

当我们需要对业务重新整合的时候，可能就需要对之前创建的角色进行清理，删除一些不会再使用的角色。删除角色的操作很简单，你只要掌握语法结构就行了。  

```mysql
DROP ROLE role [,role2]...
```

注意，  `如果你删除了角色，那么用户也就失去了通过这个角色所获得的所有权限  `。

练习：执行如下SQL删除角色school_read。  

```mysql
DROP ROLE 'school_read';
```

### 给用户赋予角色  

角色创建并授权后，要赋给用户并处于 `激活状态` 才能发挥作用。给用户添加角色可使用GRANT语句，语法形式如下：  

```mysql
GRANT role [,role2,...] TO user [,user2,...];
```

在上述语句中，role代表角色，user代表用户。可将多个角色同时赋予多个用户，用逗号隔开即可。

练习：给kangshifu用户添加角色school_read权限。  

（1）使用GRANT语句给kangshifu添加school_read权限，SQL语句如下。  

```mysql
GRANT 'school_read' TO 'kangshifu'@'localhost';
```

（2）添加完成后使用SHOW语句查看是否添加成功，SQL语句如下。  

```mysql
SHOW GRANTS FOR 'kangshifu'@'localhost';
```

（3）使用kangshifu用户登录，然后查询当前角色，如果角色未激活，结果将显示NONE。SQL语句如下。 

 ```mysql
 SELECT CURRENT_ROLE();
 ```

![image-20221024210240505](D:\笔记\mysql.assets\image-20221024210240505.png)

### 激活角色  

方式1：使用set default role 命令激活角色  

举例：  

```mysql
SET DEFAULT ROLE ALL TO 'kangshifu'@'localhost';
```

举例：使用` SET DEFAULT ROLE` 为下面4个用户默认激活所有已拥有的角色如下：  

```mysql
SET DEFAULT ROLE ALL TO
'dev1'@'localhost',
'read_user1'@'localhost',
'read_user2'@'localhost',
'rw_user1'@'localhost';
```

方式2：将activate_all_roles_on_login设置为ON  

- 默认情况：  

```mysql
mysql> show variables like 'activate_all_roles_on_login';
+-----------------------------+-------+
| Variable_name | Value |
+-----------------------------+-------+
| activate_all_roles_on_login | OFF |
+-----------------------------+-------+
1 row in set (0.00 sec)
```

- 设置：  

```mysql
SET GLOBAL activate_all_roles_on_login=ON;
```

这条 SQL 语句的意思是，对 `所有角色永久激活` 。运行这条语句之后，用户才真正拥有了赋予角色的所有权限。  

### 撤销用户的角色  

撤销用户角色的SQL语法如下：  

```mysql
REVOKE role FROM user;
```

练习：撤销kangshifu用户的school_read角色。  

（1）撤销的SQL语句如下  

```mysql
REVOKE 'school_read' FROM 'kangshifu'@'localhost';
```

（2）撤销后，执行如下查询语句，查看kangshifu用户的角色信息  

```mysql
SHOW GRANTS FOR 'kangshifu'@'localhost';
```

执行发现，用户kangshifu之前的school_read角色已被撤销。  

### 设置强制角色(mandatory role)

方式1：服务启动前设置  

```mysql
[mysqld]
mandatory_roles='role1,role2@localhost,r3@%.atguigu.com'
```

方式2：运行时设置  

```mysql
SET PERSIST mandatory_roles = 'role1,role2@localhost,r3@%.example.com'; #系统重启后仍然有效
SET GLOBAL mandatory_roles = 'role1,role2@localhost,r3@%.example.com'; #系统重启后失效
```

#    逻辑架构

##  逻辑架构剖析

###  服务器处理客户端请求

首先MySQL是典型的C/S架构，即`Clinet/Server 架构`，服务端程序使用的mysqld。

不论客户端进程和服务器进程是采用哪种方式进行通信，最后实现的效果是：**客户端进程向服务器进程发送一段文本（SQL语句），服务器进程处理后再向客户端进程发送一段文本（处理结果）**。

那服务器进程对客户端进程发送的请求做了什么处理，才能产生最后的处理结果呢？这里以查询请求为 例展示：

![image-20220615133227202](D:\笔记\mysql.assets\image-20220615133227202.png)

下面具体展开如下：

![image-20220615133420251](D:\笔记\mysql.assets\image-20220615133420251.png)

###  Connectors

Connectors, 指的是不同语言中与SQL的交互。MySQL首先是一个网络程序，在TCP之上定义了自己的应用层协议。所以要使用MySQL，我们可以编写代码，跟MySQL Server `建立TCP连接`，之后按照其定义好的协议进行交互。或者比较方便的方法是调用SDK，比如Native C API、JDBC、PHP等各语言MySQL Connecotr,或者通过ODBC。但**通过SDK来访问MySQL，本质上还是在TCP连接上通过MySQL协议跟MySQL进行交互**

**接下来的MySQL Server结构可以分为如下三层：**

### 第一层：连接层

系统（客户端）访问 MySQL 服务器前，做的第一件事就是建立 TCP 连接。 经过三次握手建立连接成功后， MySQL 服务器对 TCP 传输过来的账号密码做身份认证、权限获取。

* 用户名或密码不对，会收到一个Access denied for user错误，客户端程序结束执行 
* 用户名密码认证通过，会从权限表查出账号拥有的权限与连接关联，之后的权限判断逻辑，都将依赖于此时读到的权限

TCP 连接收到请求后，必须要分配给一个线程专门与这个客户端的交互。所以还会有个线程池，去走后面的流程。每一个连接从线程池中获取线程，省去了创建和销毁线程的开销。

所以**连接管理**的职责是负责认证、管理连接、获取权限信息。

###  第二层：服务层

第二层架构主要完成大多数的核心服务功能，如SQL接口，并完成`缓存的查询`，SQL的分析和优化及部分内置函数的执行。所有跨存储引擎的功能也在这一层实现，如过程、函数等。

在该层，服务器会`解析查询`并创建相应的内部`解析树`，并对其完成相应的`优化`：如确定查询表的顺序，是否利用索引等，最后生成相应的执行操作。

如果是SELECT语句，服务器还会`查询内部的缓存`。如果缓存空间足够大，这样在解决大量读操作的环境中能够很好的提升系统的性能。

* SQL Interface: SQL接口 

	* 接收用户的SQL命令，并且返回用户需要查询的结果。比如SELECT ... FROM就是调用SQL Interface 
	* MySQL支持DML（数据操作语言）、DDL（数据定义语言）、存储过程、视图、触发器、自定 义函数等多种SQL语言接口

* Parser: 解析器

	* 在解析器中对 SQL 语句进行语法分析、语义分析。将SQL语句分解成数据结构，并将这个结构 传递到后续步骤，以后SQL语句的传递和处理就是基于这个结构的。如果在分解构成中遇到错 误，那么就说明这个SQL语句是不合理的。 
	* 在SQL命令传递到解析器的时候会被解析器验证和解析，并为其创建 语法树 ，并根据数据字 典丰富查询语法树，会 验证该客户端是否具有执行该查询的权限 。创建好语法树后，MySQL还 会对SQl查询进行语法上的优化，进行查询重写。

* Optimizer: 查询优化器

	* SQL语句在语法解析之后、查询之前会使用查询优化器确定 SQL 语句的执行路径，生成一个 执行计划 。 
	* 这个执行计划表明应该 使用哪些索引 进行查询（全表检索还是使用索引检索），表之间的连 接顺序如何，最后会按照执行计划中的步骤调用存储引擎提供的方法来真正的执行查询，并将 查询结果返回给用户。
	* 它使用“ 选取-投影-连接 ”策略进行查询。例如：

	```mysql
	SELECT id,name FROM student WHERE gender = '女';
	```

	这个SELECT查询先根据WHERE语句进行 选取 ，而不是将表全部查询出来以后再进行gender过 滤。 这个SELECT查询先根据id和name进行属性 投影 ，而不是将属性全部取出以后再进行过 滤，将这两个查询条件 连接 起来生成最终查询结果。

* Caches & Buffers： 查询缓存组件

	* MySQL内部维持着一些Cache和Buffer，比如Query Cache用来缓存一条SELECT语句的执行结 果，如果能够在其中找到对应的查询结果，那么就不必再进行查询解析、优化和执行的整个过 程了，直接将结果反馈给客户端。 
	* 这个缓存机制是由一系列小缓存组成的。比如表缓存，记录缓存，key缓存，权限缓存等 。 这个查询缓存可以在 不同客户端之间共享 。 
	* 从MySQL 5.7.20开始，不推荐使用查询缓存，并在 MySQL 8.0中删除 。

### 第三层：引擎层

插件式存储引擎层（ Storage Engines），**真正的负责了MySQL中数据的存储和提取，对物理服务器级别维护的底层数据执行操作**，服务器通过API与存储引擎进行通信。不同的存储引擎具有的功能不同，这样 我们可以根据自己的实际需要进行选取。

MySQL 8.0.25默认支持的存储引擎如下：

![image-20220615140556893](D:\笔记\mysql.assets\image-20220615140556893.png)

###  存储层

所有的数据，数据库、表的定义，表的每一行的内容，索引，都是存在文件系统 上，以`文件`的方式存在的，并完成与存储引擎的交互。当然有些存储引擎比如InnoDB，也支持不使用文件系统直接管理裸设备，但现代文件系统的实现使得这样做没有必要了。在文件系统之下，可以使用本地磁盘，可以使用 DAS、NAS、SAN等各种存储系统。

###  小结

MySQL架构图本节开篇所示。下面为了熟悉SQL执行流程方便，我们可以简化如下：

![image-20220615140710351](D:\笔记\mysql.assets\image-20220615140710351.png)

简化为三层结构： 

1. 连接层：客户端和服务器端建立连接，客户端发送 SQL 至服务器端； 
2. SQL 层（服务层）：对 SQL 语句进行查询处理；与数据库文件的存储方式无关；
3. 存储引擎层：与数据库文件打交道，负责数据的存储和读取。

## SQL执行流程

### MySQL中的SQL执行流程

![image-20220615141934531](D:\笔记\mysql.assets\image-20220615141934531.png)

MySQL的查询流程：

**查询缓存**：Server 如果在查询缓存中发现了这条 SQL 语句，就会直接将结果返回给客户端；如果没 有，就进入到解析器阶段。需要说明的是，因为查询缓存往往效率不高，所以在 MySQL8.0 之后就抛弃了这个功能。

**总之，因为查询缓存往往弊大于利，查询缓存的失效非常频繁。**

一般建议大家在静态表里使用查询缓存，什么叫`静态表`呢？就是一般我们极少更新的表。比如，一个系统配置表、字典表，这张表上的查询才适合使用查询缓存。好在MySQL也提供了这种“`按需使用`”的方式。你可以将 my.cnf 参数 query_cache_type 设置成 DEMAND，代表当 sql 语句中有 SQL_CACHE关键字时才缓存。比如：

```mysql
# query_cache_type 有3个值。 0代表关闭查询缓存OFF，1代表开启ON，2代表(DEMAND)
query_cache_type=2
```

这样对于默认的SQL语句都不使用查询缓存。而对于你确定要使用查询缓存的语句，可以供SQL_CACHE显示指定，像下面这个语句一样：

```mysql
SELECT SQl_CACHE * FROM test WHERE ID=5;
```

查看当前 mysql 实例是否开启缓存机制

```mysql
# MySQL5.7中：
show global variables like "%query_cache_type%";
```

监控查询缓存的命中率：

```mysql
show status like '%Qcache%';
```

<img src="D:\笔记\mysql.assets\image-20220615144537260.png" alt="image-20220615144537260" style="float:left;" />

运行结果解析：

`Qcache_free_blocks`: 表示查询缓存中海油多少剩余的blocks，如果该值显示较大，则说明查询缓存中的`内部碎片`过多了，可能在一定的时间进行整理。

`Qcache_free_memory`: 查询缓存的内存大小，通过这个参数可以很清晰的知道当前系统的查询内存是否够用，DBA可以根据实际情况做出调整。

`Qcache_hits`: 表示有 `多少次命中缓存`。我们主要可以通过该值来验证我们的查询缓存的效果。数字越大，缓存效果越理想。

`Qcache_inserts`: 表示`多少次未命中然后插入`，意思是新来的SQL请求在缓存中未找到，不得不执行查询处理，执行查询处理后把结果insert到查询缓存中。这样的情况的次数越多，表示查询缓存应用到的比较少，效果也就不理想。当然系统刚启动后，查询缓存是空的，这也正常。

`Qcache_lowmem_prunes`: 该参数记录有`多少条查询因为内存不足而被移除`出查询缓存。通过这个值，用户可以适当的调整缓存大小。

`Qcache_not_cached`: 表示因为query_cache_type的设置而没有被缓存的查询数量。

`Qcache_queries_in_cache`: 当前缓存中`缓存的查询数量`。

`Qcache_total_blocks`: 当前缓存的block数量。

2. **解析器**：在解析器中对 SQL 语句进行语法分析、语义分析。

![image-20220615142301226](D:\笔记\mysql.assets\image-20220615142301226.png)

如果没有命中查询缓存，就要开始真正执行语句了。首先，MySQL需要知道你要做什么，因此需要对SQL语句做解析。SQL语句的分析分为词法分析与语法分析。

分析器先做“ `词法分析` ”。你输入的是由多个字符串和空格组成的一条 SQL 语句，MySQL 需要识别出里面 的字符串分别是什么，代表什么。 

MySQL 从你输入的"select"这个关键字识别出来，这是一个查询语 句。它也要把字符串“T”识别成“表名 T”，把字符串“ID”识别成“列 ID”。

接着，要做“ `语法分析` ”。根据词法分析的结果，语法分析器（比如：Bison）会根据语法规则，判断你输 入的这个 SQL 语句是否 `满足 MySQL 语法` 。

select department_id,job_id, avg(salary) from employees group by department_id; 

如果SQL语句正确，则会生成一个这样的语法树：

![image-20220615162031427](D:\笔记\mysql.assets\image-20220615162031427.png)

下图是SQL分词分析的过程步骤:

![image-20220615163338495](D:\笔记\mysql.assets\image-20220615163338495.png)

至此解析器的工作任务也基本圆满了。

3. **优化器**：在优化器中会确定 SQL 语句的执行路径，比如是根据 `全表检索` ，还是根据 `索引检索` 等。 

经过解释器，MySQL就知道你要做什么了。在开始执行之前，还要先经过优化器的处理。**一条查询可以有很多种执行方式，最后都返回相同的结果。优化器的作用就是找到这其中最好的执行计划**。

比如：优化器是在表里面有多个索引的时候，决定使用哪个索引；或者在一个语句有多表关联 (join) 的时候，决定各个表的连接顺序，还有表达式简化、子查询转为连接、外连接转为内连接等。

举例：如下语句是执行两个表的 join：

```mysql
select * from test1 join test2 using(ID)
where test1.name='zhangwei' and test2.name='mysql高级课程';
```

```mysql
方案1：可以先从表 test1 里面取出 name='zhangwei'的记录的 ID 值，再根据 ID 值关联到表 test2，再判
断 test2 里面 name的值是否等于 'mysql高级课程'。

方案2：可以先从表 test2 里面取出 name='mysql高级课程' 的记录的 ID 值，再根据 ID 值关联到 test1，
再判断 test1 里面 name的值是否等于 zhangwei。

这两种执行方法的逻辑结果是一样的，但是执行的效率会有不同，而优化器的作用就是决定选择使用哪一个方案。优化
器阶段完成后，这个语句的执行方案就确定下来了，然后进入执行器阶段。
如果你还有一些疑问，比如优化器是怎么选择索引的，有没有可能选择错等。后面讲到索引我们再谈。
```

在查询优化器中，可以分为 `逻辑查询` 优化阶段和 `物理查询` 优化阶段。

逻辑查询优化就是通过改变SQL语句的内容来使得SQL查询更高效，同时为物理查询优化提供更多的候选执行计划。通常采用的方式是对SQL语句进行`等价变换`，对查询进行`重写`，而查询重写的数学基础就是关系代数。对条件表达式进行等价谓词重写、条件简化，对视图进行重写，对子查询进行优化，对连接语义进行了外连接消除、嵌套连接消除等。

物理查询优化是基于关系代数进行的查询重写，而关系代数的每一步都对应着物理计算，这些物理计算往往存在多种算法，因此需要计算各种物理路径的代价，从中选择代价最小的作为执行计划。在这个阶段里，对于单表和多表连接的操作，需要高效地`使用索引`，提升查询效率。

4. **执行器**：

截止到现在，还没有真正去读写真实的表，仅仅只是产出了一个执行计划。于是就进入了执行器阶段 。

![image-20220615162613806](D:\笔记\mysql.assets\image-20220615162613806.png)

在执行之前需要判断该用户是否 `具备权限` 。如果没有，就会返回权限错误。如果具备权限，就执行 SQL 查询并返回结果。在 MySQL8.0 以下的版本，如果设置了查询缓存，这时会将查询结果进行缓存。

```mysql
select * from test where id=1;
```

比如：表 test 中，ID 字段没有索引，那么执行器的执行流程是这样的：

```mysql
调用 InnoDB 引擎接口取这个表的第一行，判断 ID 值是不是1，如果不是则跳过，如果是则将这行存在结果集中；
调用引擎接口取“下一行”，重复相同的判断逻辑，直到取到这个表的最后一行。
执行器将上述遍历过程中所有满足条件的行组成的记录集作为结果集返回给客户端。
```

至此，这个语句就执行完成了。对于有索引的表，执行的逻辑也差不多。

SQL 语句在 MySQL 中的流程是： `SQL语句`→`查询缓存`→`解析器`→`优化器`→`执行器` 。

![image-20220615164722975](D:\笔记\mysql.assets\image-20220615164722975.png)

### MySQL8中SQL执行原理

####  确认profiling是否开启

了解查询语句底层执行的过程：`select @profiling` 或者 `show variables like '%profiling'` 查看是否开启计划。开启它可以让MySQL收集在SQL执行时所使用的资源情况，命令如下：

```mysql
mysql> select @@profiling;
mysql> show variables like 'profiling';
```

profiling=0 代表关闭，我们需要把 profiling 打开，即设置为 1：

```mysql
mysql> set @@profiling=1;
```

#### 多次执行相同SQL查询

然后我们执行一个 SQL 查询（你可以执行任何一个 SQL 查询）：

```mysql
mysql> select * from employees;
```

#### 查看profiles

查看当前会话所产生的所有 profiles：

```mysql
mysql> show profiles; # 显示最近的几次查询
```

####  查看profile

显示执行计划，查看程序的执行步骤：

```mysql
mysql> show profile;
```

![image-20220615172149919](D:\笔记\mysql.assets\image-20220615172149919.png)

当然你也可以查询指定的 Query ID，比如：

```mysql
mysql> show profile for query 7;
```

查询 SQL 的执行时间结果和上面是一样的。

此外，还可以查询更丰富的内容：

```mysql
mysql> show profile cpu,block io for query 6;
```

![image-20220615172409967](D:\笔记\mysql.assets\image-20220615172409967.png)

继续：

```mysql
mysql> show profile cpu,block io for query 7;
```

![image-20220615172438338](D:\笔记\mysql.assets\image-20220615172438338.png)

1、除了查看cpu、io阻塞等参数情况，还可以查询下列参数的利用情况。

```mysql
Syntax:
SHOW PROFILE [type [, type] ... ]
	[FOR QUERY n]
	[LIMIT row_count [OFFSET offset]]

type: {
	| ALL -- 显示所有参数的开销信息
	| BLOCK IO -- 显示IO的相关开销
	| CONTEXT SWITCHES -- 上下文切换相关开销
	| CPU -- 显示CPU相关开销信息
	| IPC -- 显示发送和接收相关开销信息
	| MEMORY -- 显示内存相关开销信息
	| PAGE FAULTS -- 显示页面错误相关开销信息
	| SOURCE -- 显示和Source_function,Source_file,Source_line 相关的开销信息
	| SWAPS -- 显示交换次数相关的开销信息
}
```

2、发现两次查询当前情况都一致，说明没有缓存。

`在 8.0 版本之后，MySQL 不再支持缓存的查询`。一旦数据表有更新，缓存都将清空，因此只有数据表是静态的时候，或者数据表很少发生变化时，使用缓存查询才有价值，否则如果数据表经常更新，反而增加了 SQL 的查询时间。

### MySQL5.7中SQL执行原理

上述操作在MySQL5.7中测试，发现前后两次相同的sql语句，执行的查询过程仍然是相同的。不是会使用 缓存吗？这里我们需要 显式开启查询缓存模式 。在MySQL5.7中如下设置：

#### 配置文件中开启查询缓存

在 /etc/my.cnf 中新增一行：

```mysql
query_cache_type=1
```

####  重启mysql服务

```mysql
systemctl restart mysqld
```

####  开启查询执行计划

由于重启过服务，需要重新执行如下指令，开启profiling。

```mysql
mysql> set profiling=1;
```

####  执行语句两次：

```mysql
mysql> select * from locations;
```

####  查看profiles

![image-20220615173727345](D:\笔记\mysql.assets\image-20220615173727345.png)

####  查看profile

显示执行计划，查看程序的执行步骤：

```mysql
mysql> show profile for query 1;
```

![image-20220615173803835](D:\笔记\mysql.assets\image-20220615173803835.png)

```mysql
mysql> show profile for query 2;
```

![image-20220615173822079](D:\笔记\mysql.assets\image-20220615173822079.png)

结论不言而喻。执行编号2时，比执行编号1时少了很多信息，从截图中可以看出查询语句直接从缓存中 获取数据。

###  SQL语法顺序

随着Mysql版本的更新换代，其优化器也在不断的升级，优化器会分析不同执行顺序产生的性能消耗不同 而动态调整执行顺序。

##  数据库缓冲池（buffer pool）

`InnoDB` 存储引擎是以页为单位来管理存储空间的，我们进行的增删改查操作其实本质上都是在访问页面（包括读页面、写页面、创建新页面等操作）。而磁盘 I/O 需要消耗的时间很多，而在内存中进行操作，效率则会高很多，为了能让数据表或者索引中的数据随时被我们所用，DBMS 会申请`占用内存来作为数据缓冲池` ，在真正访问页面之前，需要把在磁盘上的页缓存到内存中的 Buffer Pool 之后才可以访问。

这样做的好处是可以让磁盘活动最小化，从而 `减少与磁盘直接进行 I/O 的时间 `。要知道，这种策略对提升 SQL 语句的查询性能来说至关重要。如果索引的数据在缓冲池里，那么访问的成本就会降低很多。

###  缓冲池 vs 查询缓存

缓冲池和查询缓存是一个东西吗？不是。

####  缓冲池（Buffer Pool）

首先我们需要了解在 InnoDB 存储引擎中，缓冲池都包括了哪些。

在 InnoDB 存储引擎中有一部分数据会放到内存中，缓冲池则占了这部分内存的大部分，它用来存储各种数据的缓存，如下图所示：

![image-20220615175309751](D:\笔记\mysql.assets\image-20220615175309751.png)

从图中，你能看到 InnoDB 缓冲池包括了数据页、索引页、插入缓冲、锁信息、自适应 Hash 和数据字典信息等。

**缓存池的重要性：**

**缓存原则：**

“ `位置 * 频次` ”这个原则，可以帮我们对 I/O 访问效率进行优化。

首先，位置决定效率，提供缓冲池就是为了在内存中可以直接访问数据。

其次，频次决定优先级顺序。因为缓冲池的大小是有限的，比如磁盘有 200G，但是内存只有 16G，缓冲池大小只有 1G，就无法将所有数据都加载到缓冲池里，这时就涉及到优先级顺序，会`优先对使用频次高的热数据进行加载 `。

**缓冲池的预读特性:**

缓冲池的作用就是提升 I/O 效率，而我们进行读取数据的时候存在一个“局部性原理”，也就是说我们使用了一些数据，**大概率还会使用它周围的一些数据**，因此采用“预读”的机制提前加载，可以减少未来可能的磁盘 I/O 操作。

####  查询缓存

那么什么是查询缓存呢？ 

查询缓存是提前把 查询结果缓存起来，这样下次不需要执行就可以直接拿到结果。需要说明的是，在 MySQL 中的查询缓存，不是缓存查询计划，而是查询对应的结果。因为命中条件苛刻，而且只要数据表 发生变化，查询缓存就会失效，因此命中率低。

### 缓冲池如何读取数据

缓冲池管理器会尽量将经常使用的数据保存起来，在数据库进行页面读操作的时候，首先会判断该页面 是否在缓冲池中，如果存在就直接读取，如果不存在，就会通过内存或磁盘将页面存放到缓冲池中再进行读取。

缓存在数据库中的结构和作用如下图所示：

![image-20220615193131719](D:\笔记\mysql.assets\image-20220615193131719.png)

**如果我们执行 SQL 语句的时候更新了缓存池中的数据，那么这些数据会马上同步到磁盘上吗？**

实际上，当我们对数据库中的记录进行修改的时候，首先会修改缓冲池中页里面的记录信息，然后数据库会`以一定的频率刷新`到磁盘中。注意并不是每次发生更新操作，都会立即进行磁盘回写。缓冲池会采用一种叫做 `checkpoint 的机制` 将数据回写到磁盘上，这样做的好处就是提升了数据库的整体性能。

比如，当`缓冲池不够用`时，需要释放掉一些不常用的页，此时就可以强行采用checkpoint的方式，将不常用的脏页回写到磁盘上，然后再从缓存池中将这些页释放掉。这里的脏页 (dirty page) 指的是缓冲池中被修改过的页，与磁盘上的数据页不一致。

###  查看/设置缓冲池的大小

如果你使用的是 MySQL MyISAM 存储引擎，它只缓存索引，不缓存数据，对应的键缓存参数为`key_buffer_size`，你可以用它进行查看。

如果你使用的是 InnoDB 存储引擎，可以通过查看 innodb_buffer_pool_size 变量来查看缓冲池的大小。命令如下：

```mysql
show variables like 'innodb_buffer_pool_size';
```

<img src="D:\笔记\mysql.assets\image-20220615214847480.png" alt="image-20220615214847480" style="zoom:80%;" />

你能看到此时 InnoDB 的缓冲池大小只有 134217728/1024/1024=128MB。我们可以修改缓冲池大小，比如改为256MB，方法如下：

```mysql
set global innodb_buffer_pool_size = 268435456;
```

或者：

```mysql
[server]
innodb_buffer_pool_size = 268435456
```

###  多个Buffer Pool实例

```mysql
[server]
innodb_buffer_pool_instances = 2
```

这样就表明我们要创建2个 `Buffer Pool` 实例。

我们看下如何查看缓冲池的个数，使用命令：

```mysql
show variables like 'innodb_buffer_pool_instances';
```

那每个 Buffer Pool 实例实际占多少内存空间呢？其实使用这个公式算出来的：

```mysql
innodb_buffer_pool_size/innodb_buffer_pool_instances
```

也就是总共的大小除以实例的个数，结果就是每个 Buffer Pool 实例占用的大小。

不过也不是说 Buffer Pool 实例创建的越多越好，分别管理各个 Buffer Pool 也是需要性能开销的，InnDB规定：当innodb_buffer_pool_size的值小于1G的时候设置多个实例是无效的，InnoDB会默认把innodb_buffer_pool_instances的值修改为1。而我们鼓励在 Buffer Pool 大于等于 1G 的时候设置多个 Buffer Pool 实例。

###  引申问题

Buffer Pool是MySQL内存结构中十分核心的一个组成，你可以先把它想象成一个黑盒子。

黑盒下的更新数据流程

当我们查询数据的时候，会先去 Buffer Pool 中查询。如果 Buffer Pool 中不存在，存储引擎会先将数据从磁盘加载到 

# 存储引擎

##  查看存储引擎

* 查看mysql提供什么存储引擎

```mysql
show engines;
```

![image-20220615223831995](D:\笔记\mysql.assets\image-20220615223831995.png)

##  设置系统默认的存储引擎

* 查看默认的存储引擎

```mysql
show variables like '%storage_engine%';
#或
SELECT @@default_storage_engine;
```

<img src="D:\笔记\mysql.assets\image-20220615224249491.png" alt="image-20220615224249491" style="zoom:60%;" />

* 修改默认的存储引擎

如果在创建表的语句中没有显式指定表的存储引擎的话，那就会默认使用 InnoDB 作为表的存储引擎。 如果我们想改变表的默认存储引擎的话，可以这样写启动服务器的命令行：

```mysql
SET DEFAULT_STORAGE_ENGINE=MyISAM;
```

或者修改 my.cnf 文件：

```mysql
default-storage-engine=MyISAM
# 重启服务
systemctl restart mysqld.service
```

## 设置表的存储引擎

存储引擎是负责对表中的数据进行提取和写入工作的，我们可以为 不同的表设置不同的存储引擎 ，也就是 说不同的表可以有不同的物理存储结构，不同的提取和写入方式。

###  创建表时指定存储引擎

我们之前创建表的语句都没有指定表的存储引擎，那就会使用默认的存储引擎 InnoDB 。如果我们想显 式的指定一下表的存储引擎，那可以这么写：

```mysql
CREATE TABLE 表名(
建表语句;
) ENGINE = 存储引擎名称;
```

###  修改表的存储引擎

如果表已经建好了，我们也可以使用下边这个语句来修改表的存储引擎：

```mysql
ALTER TABLE 表名 ENGINE = 存储引擎名称;
```

比如我们修改一下 engine_demo_table 表的存储引擎：

```mysql
mysql> ALTER TABLE engine_demo_table ENGINE = InnoDB;
```

这时我们再查看一下 engine_demo_table 的表结构：

```mysql
mysql> SHOW CREATE TABLE engine_demo_table\G
*************************** 1. row ***************************
Table: engine_demo_table
Create Table: CREATE TABLE `engine_demo_table` (
`i` int(11) DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8
1 row in set (0.01 sec)
```

## 引擎介绍

### InnoDB 引擎：具备外键支持功能的事务存储引擎

* MySQL从3.23.34a开始就包含InnoDB存储引擎。 `大于等于5.5之后，默认采用InnoDB引擎` 。
* InnoDB是MySQL的 默认事务型引擎 ，它被设计用来处理大量的短期(short-lived)事务。可以确保事务的完整提交(Commit)和回滚(Rollback)。 
* 除了增加和查询外，还需要更新、删除操作，那么，应优先选择InnoDB存储引擎。 除非有非常特别的原因需要使用其他的存储引擎，否则应该优先考虑InnoDB引擎。 
* 数据文件结构：（在《第02章_MySQL数据目录》章节已讲） 
	* 表名.frm 存储表结构（MySQL8.0时，合并在表名.ibd中） 
	* 表名.ibd 存储数据和索引 
* InnoDB是 为处理巨大数据量的最大性能设计 。 
	* 在以前的版本中，字典数据以元数据文件、非事务表等来存储。现在这些元数据文件被删除 了。比如： .frm ， .par ， .trn ， .isl ， .db.opt 等都在MySQL8.0中不存在了。 
* 对比MyISAM的存储引擎， InnoDB写的处理效率差一些 ，并且会占用更多的磁盘空间以保存数据和索引。 
* MyISAM只缓存索引，不缓存真实数据；InnoDB不仅缓存索引还要缓存真实数据， 对内存要求较 高 ，而且内存大小对性能有决定性的影响。

### MyISAM 引擎：主要的非事务处理存储引擎

* MyISAM提供了大量的特性，包括全文索引、压缩、空间函数(GIS)等，但MyISAM`不支持事务、行级锁、外键` ，有一个毫无疑问的缺陷就是`崩溃后无法安全恢复 。`
* 5.5之前默认的存储引擎 
* 优势是访问的`速度快` ，对事务完整性没有要求或者以SELECT、INSERT为主的应用 
* 针对数据统计有额外的常数存储。故而 count(*) 的查询效率很高 数据文件结构：（在《第02章_MySQL数据目录》章节已讲） 
	* 表名.frm 存储表结构 
	* 表名.MYD 存储数据 (MYData) 
	* 表名.MYI 存储索引 (MYIndex) 
* 应用场景：只读应用或者以读为主的业务

###  Archive 引擎：用于数据存档

![image-20221028144543384](D:\笔记\mysql.assets\image-20221028144543384.png)

* **下表展示了ARCHIVE 存储引擎功能**

<img src="D:\笔记\mysql.assets\image-20220616124743732.png" alt="image-20220616124743732" style="float:left;" />

###  Blackhole 引擎：丢弃写操作，读操作会返回空内容

![image-20221028144930861](D:\笔记\mysql.assets\image-20221028144930861.png)

###  CSV 引擎：存储数据时，以逗号分隔各个数据项

![image-20221028145014379](D:\笔记\mysql.assets\image-20221028145014379.png)

使用案例如下

```mysql
mysql> CREATE TABLE test (i INT NOT NULL, c CHAR(10) NOT NULL) ENGINE = CSV;
Query OK, 0 rows affected (0.06 sec)
mysql> INSERT INTO test VALUES(1,'record one'),(2,'record two');
Query OK, 2 rows affected (0.05 sec)
Records: 2 Duplicates: 0 Warnings: 0
mysql> SELECT * FROM test;
+---+------------+
| i |      c     |
+---+------------+
| 1 | record one |
| 2 | record two |
+---+------------+
2 rows in set (0.00 sec)
```

创建CSV表还会创建相应的元文件 ，用于 存储表的状态 和 表中存在的行数 。此文件的名称与表的名称相 同，后缀为 CSM 。如图所示

![image-20220616125342599](D:\笔记\mysql.assets\image-20220616125342599.png)

如果检查 test.CSV 通过执行上述语句创建的数据库目录中的文件，其内容使用Notepad++打开如下：

```mysql
"1","record one"
"2","record two"
```

这种格式可以被 Microsoft Excel 等电子表格应用程序读取，甚至写入。使用Microsoft Excel打开如图所示

![image-20220616125448555](D:\笔记\mysql.assets\image-20220616125448555.png)

### Memory 引擎：置于内存的表

**概述：**

Memory采用的逻辑介质是`内存` ，`响应速度很快 `，但是当mysqld守护进程崩溃的时候`数据会丢失` 。另外，要求存储的数据是数据长度不变的格式，比如，Blob和Text类型的数据不可用(长度不固定的)。

**主要特征：**

* `Memory同时 支持哈希（HASH）索引 `和` B+树索引 `。 
* Memory表至少比MyISAM表要`快一个数量级` 。 
* MEMORY `表的大小是受到限制` 的。表的大小主要取决于两个参数，分别是 `max_rows` 和` max_heap_table_size` 。其中，max_rows可以在创建表时指定；max_heap_table_size的大小默 认为16MB，可以按需要进行扩大。 
* 数据文件与索引文件分开存储。

	- 每个基于MEMORY存储引擎的表实际对应-个磁盘文件，该文件的文件名与表名相同，类型为frm类型，该文件中只存储表的结构，而其`数据文件都是存储在内存中的`。

	- 这样有利于数据的快速处理，提供整个表的处理效率。

* 缺点：其数据易丢失，生命周期短。基于这个缺陷，选择MEMORY存储引擎时需要特别小心。

**使用Memory存储引擎的场景：**

1. `目标数据比较小` ，而且非常`频繁的进行访问` ，在内存中存放数据，如果太大的数据会造成`内存溢出 `。可以通过参数 `max_heap_table_size` 控制Memory表的大小，限制Memory表的最大的大小。 
2. 如果`数据是临时`的 ，而且`必须立即可用`得到，那么就可以放在内存中。
3. 存储在Memory表中的数据如果突然间`丢失的话也没有太大的关系` 。

###  Federated 引擎：访问远程表

- Federated引擎是访问其他MySQL服务器的一个` 代理` ，尽管该引擎看起来提供了一种很好的 `跨服务 器的灵活性` ，但也经常带来问题，因此 `默认是禁用的 `。

###  Merge引擎：管理多个MyISAM表构成的表集合

###  NDB引擎：MySQL集群专用存储引擎

也叫做 NDB Cluster 存储引擎，主要用于` MySQL Cluster 分布式集群 `环境，类似于 Oracle 的 RAC 集 群。

###  引擎对比

MySQL中同一个数据库，不同的表可以选择不同的存储引擎。如下表对常用存储引擎做出了对比。

![image-20220616125928861](D:\笔记\mysql.assets\image-20220616125928861.png)

![image-20220616125945304](D:\笔记\mysql.assets\image-20220616125945304.png)

其实这些东西大家没必要立即就给记住，列出来的目的就是想让大家明白不同的存储引擎支持不同的功能。

其实我们最常用的就是 InnoDB 和 MyISAM ，有时会提一下 Memory 。其中 InnoDB 是 MySQL 默认的存储引擎。

## MyISAM和InnoDB

很多人对 InnoDB 和 MyISAM 的取舍存在疑问，到底选择哪个比较好呢？ 

MySQL5.5之前的默认存储引擎是MyISAM，5.5之后改为了InnoDB。

# 索引

## 为什么使用索引

索引是存储引擎用于快速找到数据记录的一种数据结构，就好比一本教科书的目录部分，通过目录中找到对应文章的页码，便可快速定位到需要的文章。MySQL中也是一样的道理，进行数据查找时，首先查看查询条件是否命中某条索引，符合则`通过索引查找`相关数据，如果不符合则需要`全表扫描`，即需要一条一条地查找记录，直到找到与条件符合的记录。

![image-20220616141351236](D:\笔记\mysql.assets\image-20220616141351236.png)

如上图所示，数据库没有索引的情况下，数据`分布在硬盘不同的位置上面`，读取数据时，摆臂需要前后摆动查询数据，这样操作非常消耗时间。如果`数据顺序摆放`，那么也需要从1到6行按顺序读取，这样就相当于进行了6次IO操作，`依旧非常耗时`。如果我们不借助任何索引结构帮助我们快速定位数据的话，我们查找 Col 2 = 89 这条记录，就要逐行去查找、去比较。从Col 2 = 34 开始，进行比较，发现不是，继续下一行。我们当前的表只有不到10行数据，但如果表很大的话，有`上千万条数据`，就意味着要做`很多很多次硬盘I/0`才能找到。现在要查找 Col 2 = 89 这条记录。CPU必须先去磁盘查找这条记录，找到之后加载到内存，再对数据进行处理。这个过程最耗时间就是磁盘I/O（涉及到磁盘的旋转时间（速度较快），磁头的寻道时间(速度慢、费时)）

假如给数据使用 `二叉树` 这样的数据结构进行存储，如下图所示

![image-20220616142723266](D:\笔记\mysql.assets\image-20220616142723266.png)

对字段 Col 2 添加了索引，就相当于在硬盘上为 Col 2 维护了一个索引的数据结构，即这个 `二叉搜索树`。二叉搜索树的每个结点存储的是 `(K, V) 结构`，key 是 Col 2，value 是该 key 所在行的文件指针（地址）。比如：该二叉搜索树的根节点就是：`(34, 0x07)`。现在对 Col 2 添加了索引，这时再去查找 Col 2 = 89 这条记录的时候会先去查找该二叉搜索树（二叉树的遍历查找）。读 34 到内存，89 > 34; 继续右侧数据，读 89 到内存，89==89；找到数据返回。找到之后就根据当前结点的 value 快速定位到要查找的记录对应的地址。我们可以发现，只需要 `查找两次` 就可以定位到记录的地址，查询速度就提高了。

这就是我们为什么要建索引，目的就是为了 `减少磁盘I/O的次数`，加快查询速率。

## 索引及其优缺点

###  索引概述

MySQL官方对索引的定义为：**索引（Index）是帮助MySQL高效获取数据的数据结构。**

**索引的本质**：索引是数据结构。你可以简单理解为“排好序的快速查找数据结构”，满足特定查找算法。 这些数据结构以某种方式指向数据， 这样就可以在这些数据结构的基础上实现 `高级查找算法` 。

`索引是在存储引擎中实现的`，因此每种存储引擎的索引不一定完全相同，并且每种存储引擎不一定支持所有索引类型。同时，存储引擎可以定义每个表的 `最大索引数`和 `最大索引长度`。所有存储引擎支持每个表至少16个索引，总索引长度至少为256字节。有些存储引擎支持更多的索引数和更大的索引长度。

###  优点

（1）类似大学图书馆建书目索引，提高数据检索的效率，降低 **数据库的IO成本** ，这也是创建索引最主 要的原因。 

（2）通过创建唯一索引，可以保证数据库表中每一行 **数据的唯一性** 。 

（3）在实现数据的 参考完整性方面，可以 **加速表和表之间的连接** 。换句话说，对于有依赖关系的子表和父表联合查询时， 可以提高查询速度。 

（4）在使用分组和排序子句进行数据查询时，可以显著 **减少查询中分组和排序的时间** ，降低了CPU的消耗。

###  缺点

增加索引也有许多不利的方面，主要表现在如下几个方面： 

（1）创建索引和维护索引要 **耗费时间** ，并 且随着数据量的增加，所耗费的时间也会增加。 

（2）索引需要占 **磁盘空间** ，除了数据表占数据空间之 外，每一个索引还要占一定的物理空间， 存储在磁盘上 ，如果有大量的索引，索引文件就可能比数据文 件更快达到最大文件尺寸。 

（3）虽然索引大大提高了查询速度，同时却会 **降低更新表的速度** 。当对表 中的数据进行增加、删除和修改的时候，索引也要动态地维护，这样就降低了数据的维护速度。 因此，选择使用索引时，需要综合考虑索引的优点和缺点。

因此，选择使用索引时，需要综合考虑索引的优点和缺点。

> 提示：
>
> 索引可以提高查询的速度，但是会影响插入记录的速度。这种情况下，最好的办法是先删除表中的索引，然后插入数据，插入完成后再创建索引。

## InnoDB中索引的推演

###  索引之前的查找

先来看一个精确匹配的例子：

```mysql
SELECT [列名列表] FROM 表名 WHERE 列名 = xxx;
```

#### . 在一个页中的查找

假设目前表中的记录比较少，所有的记录都可以被存放到一个页中，在查找记录的时候可以根据搜索条件的不同分为两种情况：

* 以主键为搜索条件

	可以在页目录中使用 `二分法` 快速定位到对应的槽，然后再遍历该槽对用分组中的记录即可快速找到指定记录。

* 以其他列作为搜索条件

	因为在数据页中并没有对非主键列简历所谓的页目录，所以我们无法通过二分法快速定位相应的槽。这种情况下只能从 `最小记录` 开始 `依次遍历单链表中的每条记录`， 然后对比每条记录是不是符合搜索条件。很显然，这种查找的效率是非常低的。

####  在很多页中查找

在很多页中查找记录的活动可以分为两个步骤：

1. 定位到记录所在的页。
2. 从所在的页内中查找相应的记录。

在没有索引的情况下，不论是根据主键列或者其他列的值进行查找，由于我们并不能快速的定位到记录所在的页，所以只能 从第一个页沿着双向链表 一直往下找，在每一个页中根据我们上面的查找方式去查 找指定的记录。因为要遍历所有的数据页，所以这种方式显然是 超级耗时 的。如果一个表有一亿条记录呢？此时 索引 应运而生。

###  设计索引

建一个表：

```mysql
mysql> CREATE TABLE index_demo(
-> c1 INT,
-> c2 INT,
-> c3 CHAR(1),
-> PRIMARY KEY(c1)
-> ) ROW_FORMAT = Compact;
```

这个新建的 **index_demo** 表中有2个INT类型的列，1个CHAR(1)类型的列，而且我们规定了c1列为主键， 这个表使用 **Compact** 行格式来实际存储记录的。这里我们简化了index_demo表的行格式示意图：

![image-20220616152453203](D:\笔记\mysql.assets\image-20220616152453203.png)

我们只在示意图里展示记录的这几个部分：

* `record_type` ：记录头信息的一项属性，表示记录的类型， 0 表示普通记录、 2 表示最小记 录、 3 表示最大记录、 1 暂时还没用过，下面讲。 
* `next_record` ：记录头信息的一项属性，表示下一条地址相对于本条记录的地址偏移量，我们用 箭头来表明下一条记录是谁。 
* `各个列的值` ：这里只记录在 index_demo 表中的三个列，分别是 c1 、 c2 和 c3 。 
* `其他信息` ：除了上述3种信息以外的所有信息，包括其他隐藏列的值以及记录的额外信息。

将记录格式示意图的其他信息项暂时去掉并把它竖起来的效果就是这样：

<img src="D:\笔记\mysql.assets\image-20220616152727234.png" alt="image-20220616152727234" style="zoom:80%;" />

把一些记录放到页里的示意图就是：

![image-20220616152651878](D:\笔记\mysql.assets\image-20220616152651878.png)

#### 一个简单的索引设计方案

我们在根据某个搜索条件查找一些记录时为什么要遍历所有的数据页呢？因为各个页中的记录并没有规律，我们并不知道我们的搜索条件匹配哪些页中的记录，所以不得不依次遍历所有的数据页。所以如果我们 **想快速的定位到需要查找的记录在哪些数据页** 中该咋办？我们可以为快速定位记录所在的数据页而建立一个目录 ，建这个目录必须完成下边这些事：

* **下一个数据页中用户记录的主键值必须大于上一个页中用户记录的主键值。**

	假设：每个数据结构最多能存放3条记录（实际上一个数据页非常大，可以存放下好多记录）。

	```mysql
	INSERT INTO index_demo VALUES(1, 4, 'u'), (3, 9, 'd'), (5, 3, 'y');
	```

​       那么这些记录以及按照主键值的大小串联成一个单向链表了，如图所示：

![image-20220616153518456](D:\笔记\mysql.assets\image-20220616153518456.png)

从图中可以看出来， index_demo 表中的3条记录都被插入到了编号为10的数据页中了。此时我们再来插入一条记录

```mysql
INSERT INTO index_demo VALUES(4, 4, 'a');
```

因为 **页10** 最多只能放3条记录，所以我们不得不再分配一个新页：

![image-20220616155306705](D:\笔记\mysql.assets\image-20220616155306705.png)

注意：新分配的 **数据页编号可能并不是连续的**。它们只是通过维护者上一个页和下一个页的编号而建立了 **链表** 关系。另外，**页10**中用户记录最大的主键值是5，而**页28**中有一条记录的主键值是4，因为5>4，所以这就不符合下一个数据页中用户记录的主键值必须大于上一个页中用户记录的主键值的要求，所以在插入主键值为4的记录的时候需要伴随着一次 **记录移动**，也就是把主键值为5的记录移动到页28中，然后再把主键值为4的记录插入到页10中，这个过程的示意图如下：

![image-20220616160216525](D:\笔记\mysql.assets\image-20220616160216525.png)

这个过程表明了在对页中的记录进行增删改查操作的过程中，我们必须通过一些诸如 **记录移动** 的操作来始终保证这个状态一直成立：下一个数据页中用户记录的主键值必须大于上一个页中用户记录的主键值。这个过程称为 **页分裂**。

* **给所有的页建立一个目录项。**

由于数据页的 **编号可能是不连续** 的，所以在向 index_demo 表中插入许多条记录后，可能是这样的效果：

![image-20220616160619525](D:\笔记\mysql.assets\image-20220616160619525.png)

我们需要给它们做个 **目录**，每个页对应一个目录项，每个目录项包括下边两个部分：

1）页的用户记录中最小的主键值，我们用 **key** 来表示。

2）页号，我们用 **page_on** 表示。

![image-20220616160857381](D:\笔记\mysql.assets\image-20220616160857381.png)

以 页28 为例，它对应 目录项2 ，这个目录项中包含着该页的页号 28 以及该页中用户记录的最小主 键值 5 。我们只需要把几个目录项在物理存储器上连续存储（比如：数组），就可以实现根据主键 值快速查找某条记录的功能了。比如：查找主键值为 20 的记录，具体查找过程分两步：

1. 先从目录项中根据 二分法 快速确定出主键值为 20 的记录在 目录项3 中（因为 12 < 20 < 209 ），它对应的页是 页9 。 
2. 再根据前边说的在页中查找记录的方式去 页9 中定位具体的记录。

至此，针对数据页做的简易目录就搞定了。这个目录有一个别名，称为 **索引** 。

####  InnoDB中的索引方案

##### 迭代1次：目录项纪录的页

InnoDB怎么区分一条记录是普通的 **用户记录** 还是 **目录项记录** 呢？使用记录头信息里的 **record_type** 属性，它的各自取值代表的意思如下：

* 0：普通的用户记录
* 1：目录项记录
* 2：最小记录
* 3：最大记录

我们把前边使用到的目录项放到数据页中的样子就是这样：

![image-20220616162944404](D:\笔记\mysql.assets\image-20220616162944404.png)

从图中可以看出来，我们新分配了一个编号为30的页来专门存储目录项记录。这里再次强调 **目录项记录** 和普通的 **用户记录** 的不同点：

* **目录项记录** 的 record_type 值是1，而 **普通用户记录** 的 record_type 值是0。 
* 目录项记录只有 **主键值和页的编号** 两个列，而普通的用户记录的列是用户自己定义的，可能包含 **很多列** ，另外还有InnoDB自己添加的隐藏列。 
* 了解：记录头信息里还有一个叫 **min_rec_mask** 的属性，只有在存储 **目录项记录** 的页中的主键值最小的 **目录项记录** 的 **min_rec_mask** 值为 **1** ，其他别的记录的 **min_rec_mask** 值都是 **0** 。

**相同点**：两者用的是一样的数据页，都会为主键值生成 **Page Directory （页目录）**，从而在按照主键值进行查找时可以使用 **二分法** 来加快查询速度。

现在以查找主键为 20 的记录为例，根据某个主键值去查找记录的步骤就可以大致拆分成下边两步：

1. 先到存储 目录项记录 的页，也就是页30中通过 二分法 快速定位到对应目录项，因为 12 < 20 < 209 ，所以定位到对应的记录所在的页就是页9。 
2. 再到存储用户记录的页9中根据 二分法 快速定位到主键值为 20 的用户记录。

##### 迭代2次：多个目录项纪录的页

![image-20220616171135082](D:\笔记\mysql.assets\image-20220616171135082.png)

从图中可以看出，我们插入了一条主键值为320的用户记录之后需要两个新的数据页：

* 为存储该用户记录而新生成了 页31 。 
* 因为原先存储目录项记录的 页30的容量已满 （我们前边假设只能存储4条目录项记录），所以不得 不需要一个新的 页32 来存放 页31 对应的目录项。

现在因为存储目录项记录的页不止一个，所以如果我们想根据主键值查找一条用户记录大致需要3个步骤，以查找主键值为 20 的记录为例：

1. 确定 目录项记录页 我们现在的存储目录项记录的页有两个，即 页30 和 页32 ，又因为页30表示的目录项的主键值的 范围是 [1, 320) ，页32表示的目录项的主键值不小于 320 ，所以主键值为 20 的记录对应的目 录项记录在 页30 中。 
2. 通过目录项记录页 确定用户记录真实所在的页 。 在一个存储 目录项记录 的页中通过主键值定位一条目录项记录的方式说过了。 
3. 在真实存储用户记录的页中定位到具体的记录。

#####  迭代3次：目录项记录页的目录页

如果我们表中的数据非常多则会`产生很多存储目录项记录的页`，那我们怎么根据主键值快速定位一个存储目录项记录的页呢？那就为这些存储目录项记录的页再生成一个`更高级的目录`，就像是一个多级目录一样，`大目录里嵌套小目录`，小目录里才是实际的数据，所以现在各个页的示意图就是这样子：

![image-20220616173512780](D:\笔记\mysql.assets\image-20220616173512780.png)

如图，我们生成了一个存储更高级目录项的 页33 ，这个页中的两条记录分别代表页30和页32，如果用 户记录的主键值在 [1, 320) 之间，则到页30中查找更详细的目录项记录，如果主键值 不小于320 的 话，就到页32中查找更详细的目录项记录。

我们可以用下边这个图来描述它：

![image-20220616173717538](D:\笔记\mysql.assets\image-20220616173717538.png)

这个数据结构，它的名称是 B+树 。

##### B+Tree

一个B+树的节点其实可以分成好多层，规定最下边的那层，也就是存放我们用户记录的那层为第 0 层， 之后依次往上加。之前我们做了一个非常极端的假设：存放用户记录的页 最多存放3条记录 ，存放目录项 记录的页 最多存放4条记录 。其实真实环境中一个页存放的记录数量是非常大的，假设所有存放用户记录 的叶子节点代表的数据页可以存放 100条用户记录 ，所有存放目录项记录的内节点代表的数据页可以存 放 1000条目录项记录 ，那么：

* 如果B+树只有1层，也就是只有1个用于存放用户记录的节点，最多能存放 100 条记录。
* 如果B+树有2层，最多能存放 1000×100=10,0000 条记录。 
* 如果B+树有3层，最多能存放 1000×1000×100=1,0000,0000 条记录。 
* 如果B+树有4层，最多能存放 1000×1000×1000×100=1000,0000,0000 条记录。相当多的记录！

你的表里能存放 **100000000000** 条记录吗？所以一般情况下，我们用到的 **B+树都不会超过4层** ，那我们通过主键值去查找某条记录最多只需要做4个页面内的查找（查找3个目录项页和一个用户记录页），又因为在每个页面内有所谓的 **Page Directory** （页目录），所以在页面内也可以通过 **二分法** 实现快速 定位记录。

###  常见索引概念

索引按照物理实现方式，索引可以分为 2 种：聚簇（聚集）和非聚簇（非聚集）索引。我们也把非聚集 索引称为二级索引或者辅助索引。

#### 聚簇索引

聚簇索引并不是一种单独的索引类型，而是**一种数据存储方式**（所有的用户记录都存储在了叶子结点），也就是所谓的 `索引即数据，数据即索引`。

> 术语"聚簇"表示当前数据行和相邻的键值聚簇的存储在一起

**特点：**

* 使用记录主键值的大小进行记录和页的排序，这包括三个方面的含义： 

	* `页内` 的记录是按照主键的大小顺序排成一个 `单向链表` 。 
	* 各个存放 `用户记录的页` 也是根据页中用户记录的主键大小顺序排成一个 `双向链表` 。 
	* 存放 `目录项记录的页` 分为不同的层次，在同一层次中的页也是根据页中目录项记录的主键大小顺序排成一个 `双向链表` 。 

* B+树的 叶子节点 存储的是完整的用户记录。 

	所谓完整的用户记录，就是指这个记录中存储了所有列的值（包括隐藏列）。

我们把具有这两种特性的B+树称为聚簇索引，所有完整的用户记录都存放在这个`聚簇索引`的叶子节点处。这种聚簇索引并不需要我们在MySQL语句中显式的使用INDEX 语句去创建， `InnDB` 存储引擎会 `自动` 的为我们创建聚簇索引。

**优点：**

* `数据访问更快` ，因为聚簇索引将索引和数据保存在同一个B+树中，因此从聚簇索引中获取数据比非聚簇索引更快 
* 聚簇索引对于主键的 `排序查找` 和 `范围查找` 速度非常快 
* 按照聚簇索引排列顺序，查询显示一定范围数据的时候，由于数据都是紧密相连，数据库不用从多 个数据块中提取数据，所以 `节省了大量的io操作` 。

**缺点：**

* `插入速度严重依赖于插入顺序` ，按照主键的顺序插入是最快的方式，否则将会出现页分裂，严重影响性能。因此，对于InnoDB表，我们一般都会定义一个`自增的ID列为主键`
* `更新主键的代价很高` ，因为将会导致被更新的行移动。因此，对于InnoDB表，我们一般定义**主键为不可更新**
* `二级索引访问需要两次索引查找` ，第一次找到主键值，第二次根据主键值找到行数据

####  二级索引（辅助索引、非聚簇索引）

如果我们想以别的列作为搜索条件该怎么办？肯定不能是从头到尾沿着链表依次遍历记录一遍。

答案：我们可以`多建几颗B+树`，不同的B+树中的数据采用不同的排列规则。比方说我们用`c2`列的大小作为数据页、页中记录的排序规则，再建一课B+树，效果如下图所示：

![image-20220616203852043](D:\笔记\mysql.assets\image-20220616203852043.png)

这个B+树与上边介绍的聚簇索引有几处不同：

![image-20220616210404733](D:\笔记\mysql.assets\image-20220616210404733.png)

**概念：回表 **

我们根据这个以c2列大小排序的B+树只能确定我们要查找记录的主键值，所以如果我们想根 据c2列的值查找到完整的用户记录的话 ，仍然需要到 聚簇索引 中再查一遍，这个过程称为 回表 。也就 是根据c2列的值查询一条完整的用户记录需要使用到 2 棵B+树！

**问题**：为什么我们还需要一次 回表 操作呢？直接把完整的用户记录放到叶子节点不OK吗？

**回答**：

如果把完整的用户记录放到叶子结点是可以不用回表。但是`太占地方`了，相当于每建立一课B+树都需要把所有的用户记录再都拷贝一遍，这就有点太浪费存储空间了。

因为这种按照`非主键列`建立的B+树需要一次回表操作才可以定位到完整的用户记录，所以这种B+树也被称为`二级索引`，或者辅助索引。由于使用的是c2列的大小作为B+树的排序规则，所以我们也称这个B+树为c2列简历的索引。

非聚簇索引的存在不影响数据在聚簇索引中的组织，所以一张表可以有多个非聚簇索引。

![image-20220616213109383](D:\笔记\mysql.assets\image-20220616213109383.png)

小结：聚簇索引与非聚簇索引的原理不同，在使用上也有一些区别：

1. 聚簇索引的`叶子节点`存储的就是我们的`数据记录`, 非聚簇索引的叶子节点存储的是`数据位置`。非聚簇索引不会影响数据表的物理存储顺序。
2. 一个表`只能有一个聚簇索引`，因为只能有一种排序存储的方式，但可以有`多个非聚簇索引`，也就是多个索引目录提供数据检索。
3. 使用聚簇索引的时候，数据的`查询效率高`，但如果对数据进行插入，删除，更新等操作，效率会比非聚簇索引低。

#### 联合索引

我们也可以同时以多个列的大小作为排序规则，也就是同时为多个列建立索引，比方说我们想让B+树按 照 c2和c3列 的大小进行排序，这个包含两层含义： 

* 先把各个记录和页按照c2列进行排序。 
* 在记录的c2列相同的情况下，采用c3列进行排序 

为c2和c3建立的索引的示意图如下：

![image-20220616215251172](D:\笔记\mysql.assets\image-20220616215251172.png)

如图所示，我们需要注意以下几点：

* 每条目录项都有c2、c3、页号这三个部分组成，各条记录先按照c2列的值进行排序，如果记录的c2列相同，则按照c3列的值进行排序
* B+树叶子节点处的用户记录由c2、c3和主键c1列组成

注意一点，以c2和c3列的大小为排序规则建立的B+树称为 联合索引 ，本质上也是一个二级索引。它的意 思与分别为c2和c3列分别建立索引的表述是不同的，不同点如下： 

* 建立 `联合索引` 只会建立如上图一样的1棵B+树。 
* 为c2和c3列分别建立索引会分别以c2和c3列的大小为排序规则建立2棵B+树。

###  InnoDB的B+树索引的注意事项

####  根页面位置万年不动

实际上B+树的形成过程是这样的：

* 每当为某个表创建一个B+树索引（聚簇索引不是人为创建的，默认就有）的时候，都会为这个索引创建一个 `根结点` 页面。最开始表中没有数据的时候，每个B+树索引对应的 `根结点` 中即没有用户记录，也没有目录项记录。
* 随后向表中插入用户记录时，先把用户记录存储到这个`根节点` 中。
* 当根节点中的可用 `空间用完时` 继续插入记录，此时会将根节点中的所有记录复制到一个新分配的页，比如 `页a` 中，然后对这个新页进行 `页分裂` 的操作，得到另一个新页，比如`页b` 。这时新插入的记录根据键值（也就是聚簇索引中的主键值，二级索引中对应的索引列的值）的大小就会被分配到 `页a` 或者 `页b` 中，而 `根节点` 便升级为存储目录项记录的页。

这个过程特别注意的是：一个B+树索引的根节点自诞生之日起，便不会再移动。这样只要我们对某个表建议一个索引，那么它的根节点的页号便会被记录到某个地方。然后凡是 `InnoDB` 存储引擎需要用到这个索引的时候，都会从哪个固定的地方取出根节点的页号，从而来访问这个索引。

#### 内节点中目录项记录的唯一性

我们知道B+树索引的内节点中目录项记录的内容是 `索引列 + 页号` 的搭配，但是这个搭配对于二级索引来说有点不严谨。还拿 index_demo 表为例，假设这个表中的数据是这样的：

![image-20220617151918786](D:\笔记\mysql.assets\image-20220617151918786.png)

如果二级索引中目录项记录的内容只是 `索引列 + 页号` 的搭配的话，那么为 `c2` 列简历索引后的B+树应该长这样：

![image-20220617152906690](D:\笔记\mysql.assets\image-20220617152906690.png)

如果我们想新插入一行记录，其中 `c1` 、`c2` 、`c3` 的值分别是: `9`、`1`、`c`, 那么在修改这个为 c2 列建立的二级索引对应的 B+ 树时便碰到了个大问题：由于 `页3` 中存储的目录项记录是由 `c2列 + 页号` 的值构成的，`页3` 中的两条目录项记录对应的 c2 列的值都是1，而我们 `新插入的这条记录` 的 c2 列的值也是 `1`，那我们这条新插入的记录到底应该放在 `页4` 中，还是应该放在 `页5` 中？答案：对不起，懵了

为了让新插入记录找到自己在那个页面，我们需要**保证在B+树的同一层页节点的目录项记录除页号这个字段以外是唯一的**。所以对于二级索引的内节点的目录项记录的内容实际上是由三个部分构成的：

* 索引列的值
* 主键值
* 页号

也就是我们把`主键值`也添加到二级索引内节点中的目录项记录，这样就能保住 B+ 树每一层节点中各条目录项记录除页号这个字段外是唯一的，所以我们为c2建立二级索引后的示意图实际上应该是这样子的：

![image-20220617154135258](D:\笔记\mysql.assets\image-20220617154135258.png)

这样我们再插入记录`(9, 1, 'c')` 时，由于 `页3` 中存储的目录项记录是由 `c2列 + 主键 + 页号` 的值构成的，可以先把新纪录的 `c2` 列的值和 `页3` 中各目录项记录的 `c2` 列的值作比较，如果 `c2` 列的值相同的话，可以接着比较主键值，因为B+树同一层中不同目录项记录的 `c2列 + 主键`的值肯定是不一样的，所以最后肯定能定位唯一的一条目录项记录，在本例中最后确定新纪录应该被插入到 `页5` 中。

####  一个页面最少存储 2 条记录

一个B+树只需要很少的层级就可以轻松存储数亿条记录，查询速度相当不错！这是因为B+树本质上就是一个大的多层级目录，每经过一个目录时都会过滤掉许多无效的子目录，直到最后访问到存储真实数据的目录。那如果一个大的目录中只存放一个子目录是个啥效果呢？那就是目录层级非常非常多，而且最后的那个存放真实数据的目录中只存放一条数据。所以 **InnoDB 的一个数据页至少可以存放两条记录**。

##  MyISAM中的索引方案

B树索引使用存储引擎如表所示：

| 索引 / 存储引擎 | MyISAM | InnoDB | Memory |
| --------------- | ------ | ------ | ------ |
| B-Tree索引      | 支持   | 支持   | 支持   |

即使多个存储引擎支持同一种类型的索引，但是他们的实现原理也是不同的。Innodb和MyISAM默认的索 引是Btree索引；而Memory默认的索引是Hash索引。

MyISAM引擎使用` B+Tree `作为索引结构，叶子节点的data域存放的是 `数据记录的地址` 。

### MyISAM索引的原理

<img src="D:\笔记\mysql.assets\image-20220617160325201.png" alt="image-20220617160325201" style="float: left; zoom: 150%;" />

![image-20220617160413479](D:\笔记\mysql.assets\image-20220617160413479.png)

<img src="D:\笔记\mysql.assets\image-20220617160533122.png" alt="image-20220617160533122" style="float: left; zoom: 150%;" />

![image-20220617160625006](D:\笔记\mysql.assets\image-20220617160625006.png)

<img src="D:\笔记\mysql.assets\image-20220617160813548.png" alt="image-20220617160813548" style="float: left; zoom: 150%;" />

### 4.2 MyISAM 与 InnoDB对比

**MyISAM的索引方式都是“非聚簇”的，与InnoDB包含1个聚簇索引是不同的。小结两种引擎中索引的区别：**

① 在InnoDB存储引擎中，我们只需要根据主键值对 聚簇索引 进行一次查找就能找到对应的记录，而在 MyISAM 中却需要进行一次 回表 操作，意味着MyISAM中建立的索引相当于全部都是 二级索引 。

 ② InnoDB的数据文件本身就是索引文件，而MyISAM索引文件和数据文件是 分离的 ，索引文件仅保存数 据记录的地址。

 ③ InnoDB的非聚簇索引data域存储相应记录 主键的值 ，而MyISAM索引记录的是 地址 。换句话说， InnoDB的所有非聚簇索引都引用主键作为data域。

 ④ MyISAM的回表操作是十分 快速 的，因为是拿着地址偏移量直接到文件中取数据的，反观InnoDB是通 过获取主键之后再去聚簇索引里找记录，虽然说也不慢，但还是比不上直接用地址去访问。 

⑤ InnoDB要求表 必须有主键 （ MyISAM可以没有 ）。如果没有显式指定，则MySQL系统会自动选择一个 可以非空且唯一标识数据记录的列作为主键。如果不存在这种列，则MySQL自动为InnoDB表生成一个隐 含字段作为主键，这个字段长度为6个字节，类型为长整型。

**小结：**

<img src="D:\笔记\mysql.assets\image-20220617161126022.png" alt="image-20220617161126022" style="float: left; zoom: 150%;" />

![image-20220617161151125](D:\笔记\mysql.assets\image-20220617161151125.png)

## 索引的代价

索引是个好东西，可不能乱建，它在空间和时间上都会有消耗：

* 空间上的代价

	每建立一个索引都要为它建立一棵B+树，每一棵B+树的每一个节点都是一个数据页，一个页默认会 占用 16KB 的存储空间，一棵很大的B+树由许多数据页组成，那就是很大的一片存储空间。

* 时间上的代价

	每次对表中的数据进行 增、删、改 操作时，都需要去修改各个B+树索引。而且我们讲过，B+树每 层节点都是按照索引列的值 从小到大的顺序排序 而组成了 双向链表 。不论是叶子节点中的记录，还 是内节点中的记录（也就是不论是用户记录还是目录项记录）都是按照索引列的值从小到大的顺序 而形成了一个单向链表。而增、删、改操作可能会对节点和记录的排序造成破坏，所以存储引擎需 要额外的时间进行一些 记录移位 ， 页面分裂 、 页面回收 等操作来维护好节点和记录的排序。如果 我们建了许多索引，每个索引对应的B+树都要进行相关的维护操作，会给性能拖后腿。

> 一个表上索引建的越多，就会占用越多的存储空间，在增删改记录的时候性能就越差。为了能建立又好又少的索引，我们得学学这些索引在哪些条件下起作用的。

## MySQL数据结构选择的合理性

<img src="D:\笔记\mysql.assets\image-20220617161635521.png" alt="image-20220617161635521" style="float: left; zoom: 150%;" />

###  全表查询

这里都懒得说了。

###  Hash查询

<img src="D:\笔记\mysql.assets\image-20220617161946230.png" alt="image-20220617161946230" style="float: left; zoom: 150%;" />

**加快查找速度的数据结构，常见的有两类：**

(1) 树，例如平衡二叉搜索树，查询/插入/修改/删除的平均时间复杂度都是 `O(log2N)`;

(2)哈希，例如HashMap，查询/插入/修改/删除的平均时间复杂度都是 `O(1)`; (key, value)

![image-20220617162153587](D:\笔记\mysql.assets\image-20220617162153587.png)

<img src="D:\笔记\mysql.assets\image-20220617162548697.png" alt="image-20220617162548697" style="float: left; zoom: 150%;" />

![image-20220617162604272](D:\笔记\mysql.assets\image-20220617162604272.png)

上图中哈希函数h有可能将两个不同的关键字映射到相同的位置，这叫做 碰撞 ，在数据库中一般采用 链 接法 来解决。在链接法中，将散列到同一槽位的元素放在一个链表中，如下图所示：

![image-20220617162703006](D:\笔记\mysql.assets\image-20220617162703006.png)

实验：体会数组和hash表的查找方面的效率区别

```mysql
// 算法复杂度为 O(n)
@Test
public void test1(){
    int[] arr = new int[100000];
    for(int i = 0;i < arr.length;i++){
        arr[i] = i + 1;
    }
    long start = System.currentTimeMillis();
    for(int j = 1; j<=100000;j++){
        int temp = j;
        for(int i = 0;i < arr.length;i++){
            if(temp == arr[i]){
                break;
            }
        }
    }
    long end = System.currentTimeMillis();
    System.out.println("time： " + (end - start)); //time： 823
}
```

```mysql
// 算法复杂度为 O(1)
@Test
public void test2(){
    HashSet<Integer> set = new HashSet<>(100000);
    for(int i = 0;i < 100000;i++){
    	set.add(i + 1);
    }
    long start = System.currentTimeMillis();
    for(int j = 1; j<=100000;j++) {
        int temp = j;
        boolean contains = set.contains(temp);
    }
    long end = System.currentTimeMillis();
    System.out.println("time： " + (end - start)); //time： 5
}
```

**Hash结构效率高，那为什么索引结构要设计成树型呢？**

<img src="D:\笔记\mysql.assets\image-20220617163202156.png" alt="image-20220617163202156" style="float: left; zoom: 150%;" />

**Hash索引适用存储引擎如表所示：**

| 索引 / 存储引擎 | MyISAM | InnoDB | Memory |
| --------------- | ------ | ------ | ------ |
| HASH索引        | 不支持 | 不支持 | 支持   |

**Hash索引的适用性：**

<img src="D:\笔记\mysql.assets\image-20220617163619721.png" alt="image-20220617163619721" style="float: left; zoom: 150%;" />

![image-20220617163657697](D:\笔记\mysql.assets\image-20220617163657697.png)

采用自适应 Hash 索引目的是方便根据 SQL 的查询条件加速定位到叶子节点，特别是当 B+ 树比较深的时 候，通过自适应 Hash 索引可以明显提高数据的检索效率。

我们可以通过 innodb_adaptive_hash_index 变量来查看是否开启了自适应 Hash，比如：

```mysql
mysql> show variables like '%adaptive_hash_index';
```

###  二叉搜索树

如果我们利用二叉树作为索引结构，那么磁盘的IO次数和索引树的高度是相关的。

**1. 二叉搜索树的特点**

* 一个节点只能有两个子节点，也就是一个节点度不能超过2
* 左子节点 < 本节点; 右子节点 >= 本节点，比我大的向右，比我小的向左

**2. 查找规则**

<img src="D:\笔记\mysql.assets\image-20220617163952166.png" alt="image-20220617163952166" style="float: left; zoom: 150%;" />

![image-20220617164022728](D:\笔记\mysql.assets\image-20220617164022728.png)

但是特殊情况，就是有时候二叉树的深度非常大，比如：

![image-20220617164053134](D:\笔记\mysql.assets\image-20220617164053134.png)

为了提高查询效率，就需要 减少磁盘IO数 。为了减少磁盘IO的次数，就需要尽量 降低树的高度 ，需要把 原来“瘦高”的树结构变的“矮胖”，树的每层的分叉越多越好。

###  AVL树

<img src="D:\笔记\mysql.assets\image-20220617165045803.png" alt="image-20220617165045803" style="float: left; zoom: 150%;" />

![image-20220617165105005](D:\笔记\mysql.assets\image-20220617165105005.png)

`每访问一次节点就需要进行一次磁盘 I/O 操作，对于上面的树来说，我们需要进行 5次 I/O 操作。虽然平衡二叉树的效率高，但是树的深度也同样高，这就意味着磁盘 I/O 操作次数多，会影响整体数据查询的效率。

针对同样的数据，如果我们把二叉树改成 M 叉树 （M>2）呢？当 M=3 时，同样的 31 个节点可以由下面 的三叉树来进行存储：

![image-20220617165124685](D:\笔记\mysql.assets\image-20220617165124685.png)

你能看到此时树的高度降低了，当数据量 N 大的时候，以及树的分叉树 M 大的时候，M叉树的高度会远小于二叉树的高度 (M > 2)。所以，我们需要把 `树从“瘦高” 变 “矮胖”。

### B-Tree

B 树的英文是 Balance Tree，也就是 `多路平衡查找树`。简写为 B-Tree。它的高度远小于平衡二叉树的高度。

B 树的结构如下图所示：

![](D:\笔记\mysql.assets\image-20221028184256413.png)

![image-20221028184311185](D:\笔记\mysql.assets\image-20221028184311185.png)

一个 M 阶的 B 树（M>2）有以下的特性：

1. 根节点的儿子数的范围是 [2,M]。 
2. 每个中间节点包含 k-1 个关键字和 k 个孩子，孩子的数量 = 关键字的数量 +1，k 的取值范围为 [ceil(M/2), M]。 
3. 叶子节点包括 k-1 个关键字（叶子节点没有孩子），k 的取值范围为 [ceil(M/2), M]。 
4. 假设中间节点节点的关键字为：Key[1], Key[2], …, Key[k-1]，且关键字按照升序排序，即 Key[i]<Key[i+1]。此时 k-1 个关键字相当于划分了 k 个范围，也就是对应着 k 个指针，即为：P[1], P[2], …, P[k]，其中 P[1] 指向关键字小于 Key[1] 的子树，P[i] 指向关键字属于 (Key[i-1], Key[i]) 的子树，P[k] 指向关键字大于 Key[k-1] 的子树。
5. 所有叶子节点位于同一层。

上面那张图所表示的 B 树就是一棵 3 阶的 B 树。我们可以看下磁盘块 2，里面的关键字为（8，12），它 有 3 个孩子 (3，5)，(9，10) 和 (13，15)，你能看到 (3，5) 小于 8，(9，10) 在 8 和 12 之间，而 (13，15) 大于 12，刚好符合刚才我们给出的特征。

然后我们来看下如何用 B 树进行查找。假设我们想要 查找的关键字是 9 ，那么步骤可以分为以下几步：

1. 我们与根节点的关键字 (17，35）进行比较，9 小于 17 那么得到指针 P1； 
2. 按照指针 P1 找到磁盘块 2，关键字为（8，12），因为 9 在 8 和 12 之间，所以我们得到指针 P2； 
3. 按照指针 P2 找到磁盘块 6，关键字为（9，10），然后我们找到了关键字 9。

你能看出来在 B 树的搜索过程中，我们比较的次数并不少，但如果把数据读取出来然后在内存中进行比 较，这个时间就是可以忽略不计的。而读取磁盘块本身需要进行 I/O 操作，消耗的时间比在内存中进行 比较所需要的时间要多，是数据查找用时的重要因素。 B 树相比于平衡二叉树来说磁盘 I/O 操作要少 ， 在数据查询中比平衡二叉树效率要高。所以 只要树的高度足够低，IO次数足够少，就可以提高查询性能 。

![image-20221028184505248](D:\笔记\mysql.assets\image-20221028184505248.png)

**再举例1：**

![image-20220617170526488](D:\笔记\mysql.assets\image-20220617170526488.png)

### B+Tree

B+树也是一种多路搜索树，`基于B树做出了改进`，主流的DBMS都支持B+树的索引|方式,比如MySQL。相比
于B-Tree，`B+Tree适合文件索引系统`。

* MySQL官网说明：

![image-20220617170710329](D:\笔记\mysql.assets\image-20220617170710329.png)

**B+ 树和 B 树的差异在于以下几点：**

1. 有 k 个孩子的节点就有 k 个关键字。也就是孩子数量 = 关键字数，而 B 树中，孩子数量 = 关键字数 +1。
2. 非叶子节点的关键字也会同时存在在子节点中，并且是在子节点中所有关键字的最大（或最 小）。 
3. 非叶子节点仅用于索引，不保存数据记录，跟记录有关的信息都放在叶子节点中。而 B 树中， 非 叶子节点既保存索引，也保存数据记录 。 
4. 所有关键字都在叶子节点出现，叶子节点构成一个有序链表，而且叶子节点本身按照关键字的大 小从小到大顺序链接。

下图就是一棵B+树,阶数为3,根节点中的关键字1、 18、 35 分别是子节点(1, 8, 14)，(18, 24, 31)和
(35，41, 53) 中的最小值。每一层父节点的关键字都会出现在下一层的子节点的关键字中，因此在叶子节点中
包括了所有的关键字信息,并且每一个叶子节点都有一个指向下一个节点的指针，这样就形成了一一个链表。

![image-20220617171106671](D:\笔记\mysql.assets\image-20220617171106671.png)

![image-20220617171131747](D:\笔记\mysql.assets\image-20220617171131747.png)

<img src="D:\笔记\mysql.assets\image-20220617171331282.png" alt="image-20220617171331282" style="float: left; zoom: 150%;" />

<img src="D:\笔记\mysql.assets\image-20220617171434206.png" alt="image-20220617171434206" style="float: left; zoom: 150%;" />

> ​		B 树和 B+ 树都可以作为索引的数据结构，在 MySQL 中采用的是 B+ 树。 但B树和B+树各有自己的应用场景，不能说B+树完全比B树好，反之亦然。

**思考题：为了减少IO，索引树会一次性加载吗？**

>​		数据库索引是存储在磁盘上的，如果数据量很大，必然导致索引的大小也会很大，超过几个G。当我们利用索引查询时候，是不可能将全部几个G的索引|都加载进内存的，我们能做的只能是:逐一加载每一个磁盘页，因为磁盘页对应着索引树的节点。

**思考题：B+树的存储能力如何？为何说一般查找行记录，最多只需1~3次磁盘IO**

>​		InnoDB存储引擎中页的大小为16KB，一般表的主键类型为INT (占用4个字节)或BIGINT (占用8个字节)，指针类型也一般为4或8个字节，也就是说一个页(B+Tree 中的一个节点)中大概存储16KB/(8B+8B)=1K个键值。因为是估值，为方便计算，这里的K取值为10^3。 也就是说一个深度为3的B+Tree索引可以维护10^3 * 10^3 * 10^3= 10亿条记录。(这里假定一个数据页也存储10^3条行记录数据了)。
>
>​		实际情况中每个节点可能不能填充满，因此在数据库中，`B+Tree 的高度一般都在 2~4层`。MySQL 的InnoDB存储引擎在设计时是将根节点常驻内存的，也就是说查找某一键值的行记录时最多 只需要1~3次磁盘I/O操作。

**思考题：为什么说B+树比B-树更适合实际应用中操作系统的文件索引和数据库索引？**

>1、B+树的磁盘读写代价更低
>		B+树的内部结点并没有指向关键字具体信息的指针。因此其内部结点相对B树更小。如果把所有同一内部
>点的关键字存放在同一盘块中，那么盘块所能容纳的关键字数量也越多。一次性读入内存中的需要查找的
>键字也就越多。相对来说I0读写次数也就降低了。
>
>2、B+树的查询效率更加稳定
>		由于非终结点并不是最终指向文件内容的结点，而只是叶子结点中关键字的索引。所以任何关键字的查找
>须走一条从根结点到叶子结点的路。所有关键字查询的路径长度相同，导致每一个数据的查询效率相当。

**思考题：Hash 索引与 B+ 树索引的区别**

>1、Hash 索引`不能进行范围查询`，而B+树可以。这是因为Hash索引指向的数据是无序的，而B+树的叶子节
>点是个有序的链表。
>
>2、Hash 索引`不支持联合索引的最左侧原则`(即联合索引的部分索引无法使用)，而B+树可以。对于联合索
>引来说，Hash 索引在计算Hash值的时候是将索引键合并后再一起计算 Hash值，所以不会针对每个索引单独
>计算Hash值。因此如果用到联合索引的一个或者几个索引时，联合索引无法被利用。
>
>3、Hash索引`不支持ORDER BY排序` ，因为Hash索引指向的数据是无序的，因此无法起到排序优化的作
>用，而B+树索引数据是有序的，可以起到对该字段ORDER BY排序优化的作用。同理，我们也无法用Hash
>索引进行`模糊查询`，而B+树使用LIKE进行模糊查询的时候，LIKE 后面后模糊查询(比如%结尾)的话就可
>以起到优化作用。
>
>4、`InnoDB不支持哈希索引`

**思考题：Hash 索引与 B+ 树索引是在建索引的时候手动指定的吗？**

>如果使用的是MySQL的话，我们需要了解MySQL的存储引擎都支持哪些索引结构，如下图所示([参考来源](h
>tps://dev.mysql.com/doc/refman/8.0/en/create-index.html)。如果是其他的DBMS，可以参考相关的DBMS文档。
><img src="D:\笔记\mysql.assets\image-20221028191743114.png" alt="image-20221028191743114" style=" float: center;" />				
>
>你能看到，针对InnoDB和MyISAM存储引擎，都会默认采用B+树索引，无法使用Hash索引。InnoDB 提供
>的自适应Hash是不需要手动指定的。如果是Memory/Heap和NDB存储引擎，是可以进行选择Hash索引

###  R树

​		R-Tree在MySQL很少使用，仅支持 `geometry数据类型 `，支持该类型的存储引擎只有myisam、bdb、 innodb、ndb、archive几种。举个R树在现实领域中能够解决的例子：查找20英里以内所有的餐厅。如果 没有R树你会怎么解决？一般情况下我们会把餐厅的坐标(x,y)分为两个字段存放在数据库中，一个字段记 录经度，另一个字段记录纬度。这样的话我们就需要遍历所有的餐厅获取其位置信息，然后计算是否满 足要求。如果一个地区有100家餐厅的话，我们就要进行100次位置计算操作了，如果应用到谷歌、百度 地图这种超大数据库中，这种方法便必定不可行了。R树就很好的 `解决了这种高维空间搜索问题` 。它把B 树的思想很好的扩展到了多维空间，采用了B树分割空间的思想，并在添加、删除操作时采用合并、分解 结点的方法，保证树的平衡性。因此，R树就是一棵用来 `存储高维数据的平衡树` 。相对于B-Tree，R-Tree 的优势在于范围查找。

| 索引 / 存储引擎 | MyISAM | InnoDB | Memory   |
| --------------- | ------ | ------ | -------- |
| R-Tree索引      | 支持   | 支持   | `不支持` |

### 小结

使用索引可以帮助我们从海量的数据中快速定位想要查找的数据,不过索引也存在一些不足, 比如占用存储空间、降低数据库写操作的性能等，如果有多个索引还会增加索引选择的时间。当我们使用索引时，需要平衡索引的利(提升查询效率)和弊(维护索引所需的代价)。

在实际工作中，我们还需要基于需求和数据本身的分布情况来确定是否使用索引，尽管`索引不是万能的`，但`数据
量大的时候不使用索引是不可想象的`，毕竟索引的本质，是帮助我们提升数据检索的效率。

### 附录：算法的时间复杂度

同一问题可用不同算法解决，而一个算法的质量优劣将影响到算法乃至程序的效率。算法分析的目的在 于选择合适算法和改进算法。

![image-20220617175516191](D:\笔记\mysql.assets\image-20220617175516191.png)

# InnoDB数据存储结构

## 数据库的存储结构：页

<img src="D:\笔记\mysql.assets\image-20220617175755324.png" alt="image-20220617175755324" style="float:left;" />

###  磁盘与内存交互基本单位：页

<img src="D:\笔记\mysql.assets\image-20220617193033971.png" alt="image-20220617193033971" style="float:left;" />

![image-20220617193939742](D:\笔记\mysql.assets\image-20220617193939742.png)

### 页结构概述

<img src="D:\笔记\mysql.assets\image-20220617193218557.png" alt="image-20220617193218557" style="float:left;" />

### 页的大小

不同的数据库管理系统（简称DBMS）的页大小不同。比如在 MySQL 的 InnoDB 存储引擎中，默认页的大小是 `16KB`，我们可以通过下面的命令来进行查看：

```mysql
show variables like '%innodb_page_size%'
```

SQL Server 中页的大小为 `8KB`，而在 Oracle 中我们用术语 "`块`" （Block）来表示 "页"，Oracle 支持的快大小为2KB, 4KB, 8KB, 16KB, 32KB 和 64KB。

###  页的上层结构

另外在数据库中，还存在着区（Extent）、段（Segment）和表空间（Tablespace）的概念。行、页、区、段、表空间的关系如下图所示：

![image-20220617194256988](D:\笔记\mysql.assets\image-20220617194256988.png)

<img src="D:\笔记\mysql.assets\image-20220617194529699.png" alt="image-20220617194529699" style="float:left;" />

##  页的内部结构

页如果按类型划分的话，常见的有 `数据页（保存B+树节点）、系统表、Undo 页 和 事物数据页` 等。数据页是我们最常使用的页。

数据页的 `16KB` 大小的存储空间被划分为七个部分，分别是文件头（File Header）、页头（Page Header）、最大最小记录（Infimum + supremum）、用户记录（User Records）、空闲空间（Free Space）、页目录（Page Directory）和文件尾（File Tailer）。

页结构的示意图如下所示：

![image-20220617195012446](D:\笔记\mysql.assets\image-20220617195012446.png)

如下表所示：

![image-20220617195148164](D:\笔记\mysql.assets\image-20220617195148164.png)

我们可以把这7个结构分为3个部分 。

### 第一部分：File Header (文件头部) 和 File Trailer (文件尾部)

见文件InnoDB数据库存储结构.mmap

### 第二部分：User Records (用户记录)、最大最小记录、Free Space (空闲空间)

见文件InnoDB数据库存储结构.mmap

### 第三部分：Page Directory (页目录) 和 Page Header (页面头部)

见文件InnoDB数据库存储结构.mmap

###  从数据库页的角度看B+树如何查询

一颗B+树按照字节类型可以分为两部分：

1. 叶子节点，B+ 树最底层的节点，节点的高度为0，存储行记录。
2. 非叶子节点，节点的高度大于0，存储索引键和页面指针，并不存储行记录本身。

![image-20220620221112635](D:\笔记\mysql.assets\image-20220620221112635.png)

当我们从页结构来理解 B+ 树的结构的时候，可以帮我们理解一些通过索引进行检索的原理：

<img src="D:\笔记\mysql.assets\image-20220620221242561.png" alt="image-20220620221242561" style="float:left;" />

<img src="D:\笔记\mysql.assets\image-20220620221442954.png" alt="image-20220620221442954" style="float:left;" />

##  InnoDB行格式 (或记录格式)

见文件InnoDB数据库存储结构.mmap

##  区、段与碎片区

###  为什么要有区？

<img src="D:\笔记\mysql.assets\image-20220621134226624.png" alt="image-20220621134226624" style="float:left;" />

### 为什么要有段？

<img src="D:\笔记\mysql.assets\image-20220621140802887.png" alt="image-20220621140802887" style="float:left;" />

###  为什么要有碎片区？

<img src="D:\笔记\mysql.assets\image-20220621141225223.png" alt="image-20220621141225223" style="float:left;" />

### 区的分类

区大体上可以分为4种类型：

* 空闲的区 (FREE) : 现在还没有用到这个区中的任何页面。
* 有剩余空间的碎片区 (FREE_FRAG)：表示碎片区中还有可用的页面。
* 没有剩余空间的碎片区 (FULL_FRAG)：表示碎片区中的所有页面都被使用，没有空闲页面。
* 附属于某个段的区 (FSEG)：每一个索引都可以分为叶子节点段和非叶子节点段。

处于FREE、FREE_FRAG 以及 FULL_FRAG 这三种状态的区都是独立的，直属于表空间。而处于 FSEG 状态的区是附属于某个段的。

> 如果把表空间比作是一个集团军，段就相当于师，区就相当于团。一般的团都是隶属于某个师的，就像是处于 FSEG 的区全部隶属于某个段，而处于 FREE、FREE_FRAG 以及 FULL_FRAG 这三种状态的区却直接隶属于表空间，就像独立团直接听命于军部一样。

##  表空间

<img src="D:\笔记\mysql.assets\image-20220621142910222.png" alt="image-20220621142910222" style="float:left;" />

###  独立表空间

独立表空间，即每张表有一个独立的表空间，也就是数据和索引信息都会保存在自己的表空间中。独立的表空间 (即：单表) 可以在不同的数据库之间进行 `迁移`。

空间可以回收 (DROP TABLE 操作可自动回收表空间；其他情况，表空间不能自己回收) 。如果对于统计分析或是日志表，删除大量数据后可以通过：alter table TableName engine=innodb; 回收不用的空间。对于使用独立表空间的表，不管怎么删除，表空间的碎片不会太严重的影响性能，而且还有机会处理。

**独立表空间结构**

独立表空间由段、区、页组成。

**真实表空间对应的文件大小**

我们到数据目录里看，会发现一个新建的表对应的 .ibd 文件只占用了 96K，才6个页面大小 (MySQL5.7中)，这是因为一开始表空间占用的空间很小，因为表里边都没有数据。不过别忘了这些 .ibd 文件是自扩展的，随着表中数据的增多，表空间对应的文件也逐渐增大。

**查看 InnoDB 的表空间类型：**

```mysql
show variables like 'innodb_file_per_table'
```

你能看到 innodb_file_per_table=ON, 这就意味着每张表都会单词保存一个 .ibd 文件。

### 系统表空间

系统表空间的结构和独立表空间基本类似，只不过由于整个MySQL进程只有一个系统表空间，在系统表空间中会额外记录一些有关整个系统信息的页面，这部分是独立表空间中没有的。

**InnoDB数据字典**

<img src="D:\笔记\mysql.assets\image-20220621150648770.png" alt="image-20220621150648770" style="float:left;" />

删除这些数据并不是我们使用 INSERT 语句插入的用户数据，实际上是为了更好的管理我们这些用户数据而不得以引入的一些额外数据，这些数据页称为 元数据。InnoDB 存储引擎特意定义了一些列的 内部系统表 (internal system table) 来记录这些元数据：

<img src="D:\笔记\mysql.assets\image-20220621150924922.png" alt="image-20220621150924922" style="float:left;" />

这些系统表也称为 `数据字典`，它们都是以 B+ 树的形式保存在系统表空间的某个页面中。其中 `SYS_TABLES、SYS_COLUMNS、SYS_INDEXES、SYS_FIELDS` 这四个表尤其重要，称之为基本系统表 (basic system tables) ，我们先看看这4个表的结构：

<img src="D:\笔记\mysql.assets\image-20220621151139759.png" alt="image-20220621151139759" style="float:left;" />

<img src="D:\笔记\mysql.assets\image-20220621151158361.png" alt="image-20220621151158361" style="float:left;" />

<img src="D:\笔记\mysql.assets\image-20220621151215274.png" alt="image-20220621151215274" style="float:left;" />

<img src="D:\笔记\mysql.assets\image-20220621151238157.png" alt="image-20220621151238157" style="float:left;" />

注意：用户不能直接访问 InnoDB 的这些内部系统表，除非你直接去解析系统表空间对应文件系统上的文件。不过考虑到查看这些表的内容可能有助于大家分析问题，所以在系统数据库 `information_schema` 中提供了一些以 `innodb_sys` 开头的表:

```mysql
USE information_schema;
```

```mysql
SHOW TABLES LIKE 'innodb_sys%';
```

在 `information_scheme` 数据库中的这些以 `INNODB_SYS` 开头的表并不是真正的内部系统表 (内部系统表就是我们上边以 `SYS` 开头的那些表)，而是在存储引擎启动时读取这些以 `SYS` 开头的系统表，然后填充到这些以 `INNODB_SYS` 开头的表中。以 `INNODB_SYS` 开头的表和以 `SYS` 开头的表中的字段并不完全一样，但仅供大家参考已经足矣。

## 附录：数据页加载的三种方式

InnoDB从磁盘中读取数据 `最小单位` 是数据页。而你想得到的 id = xxx 的数据，就是这个数据页众多行中的一行。

对于MySQL存放的数据，逻辑概念上我们称之为表，在磁盘等物理层面而言是按 `数据页` 形式进行存放的，当其加载到 MySQL 中我们称之为 `缓存页`。

如果缓冲池没有该页数据，那么缓冲池有以下三种读取数据的方式，每种方式的读取速率是不同的：

**1. 内存读取**

如果该数据存在于内存中，基本上执行时间在 1ms 左右，效率还是很高的。

![image-20220621135638283](D:\笔记\mysql.assets\image-20220621135638283.png)

**2. 随机读取**

<img src="D:\笔记\mysql.assets\image-20220621135719847.png" alt="image-20220621135719847" style="float:left;" />

![image-20220621135737422](D:\笔记\mysql.assets\image-20220621135737422.png)

**3. 顺序读取**

<img src="MySQL索引及调优篇.assets/image-20220621135909197.png" alt="image-20220621135909197" style="float:left;"/>

# 索引

## 索引的声明与使用

### 索引的分类

MySQL的索引包括普通索引、唯一性索引、全文索引、单列索引、多列索引和空间索引等。

从 功能逻辑 上说，索引主要有 4 种，分别是普通索引、唯一索引、主键索引、全文索引。 

按照 物理实现方式 ，索引可以分为 2 种：聚簇索引和非聚簇索引。 

按照 作用字段个数 进行划分，分成单列索引和联合索引。

**1. 普通索引**

<img src="D:\笔记\mysql.assets\image-20220621202759576.png" alt="image-20220621202759576" style="float:left;" />

**2. 唯一性索引**

<img src="D:\笔记\mysql.assets\image-20220621202850551.png" alt="image-20220621202850551" style="float:left;" />

**3. 主键索引**

<img src="D:\笔记\mysql.assets\image-20220621203302303.png" alt="image-20220621203302303" style="float:left;" />

**4. 单列索引**

<img src="D:\笔记\mysql.assets\image-20220621203333925.png" alt="image-20220621203333925" style="float:left;" />

**5. 多列 (组合、联合) 索引**

<img src="D:\笔记\mysql.assets\image-20220621203454424.png" alt="image-20220621203454424" style="float:left;" />

**6. 全文检索**

<img src="D:\笔记\mysql.assets\image-20220621203645789.png" alt="image-20220621203645789" style="float:left;" />

**7. 补充：空间索引**

<img src="D:\笔记\mysql.assets\image-20220621203736098.png" alt="image-20220621203736098" style="float:left;" />

**小结：不同的存储引擎支持的索引类型也不一样 **

InnoDB ：支持 B-tree、Full-text 等索引，不支持 Hash 索引； 

MyISAM ： 支持 B-tree、Full-text 等索引，不支持 Hash 索引； 

Memory ：支持 B-tree、Hash 等 索引，不支持 Full-text 索引；

NDB ：支持 Hash 索引，不支持 B-tree、Full-text 等索引； 

Archive ：不支 持 B-tree、Hash、Full-text 等索引；

### 创建索引

MySQL支持多种方法在单个或多个列上创建索引：在创建表的定义语句 CREATE TABLE 中指定索引列，使用 ALTER TABLE 语句在存在的表上创建索引，或者使用 CREATE INDEX 语句在已存在的表上添加索引。

####  创建表的时候创建索引

使用CREATE TABLE创建表时，除了可以定义列的数据类型外，还可以定义主键约束、外键约束或者唯一性约束，而不论创建哪种约束，在定义约束的同时相当于在指定列上创建了一个索引。

举例：

```mysql
CREATE TABLE dept(
dept_id INT PRIMARY KEY AUTO_INCREMENT,
dept_name VARCHAR(20)
);

CREATE TABLE emp(
emp_id INT PRIMARY KEY AUTO_INCREMENT,
emp_name VARCHAR(20) UNIQUE,
dept_id INT,
CONSTRAINT emp_dept_id_fk FOREIGN KEY(dept_id) REFERENCES dept(dept_id)
)
```

但是，如果显式创建表时创建索引的话，基本语法格式如下：

```mysql
CREATE TABLE table_name [col_name data_type]
[UNIQUE | FULLTEXT | SPATIAL] [INDEX | KEY] [index_name] (col_name [length]) [ASC |
DESC]
```

* `UNIQUE `、 `FULLTEXT` 和 `SPATIAL` 为可选参数，分别表示唯一索引、全文索引和空间索引； 
* `INDEX `与 `KEY `为同义词，两者的作用相同，用来指定创建索引； 
* `index_name `指定索引的名称，为可选参数，如果不指定，那么MySQL默认col_name为索引名； 
* `col_name `为需要创建索引的字段列，该列必须从数据表中定义的多个列中选择； 
* `length `为可选参数，表示索引的长度，只有字符串类型的字段才能指定索引长度； 
* `ASC`或 `DESC `指定升序或者降序的索引值存储。

**1. 创建普通索引**

在book表中的year_publication字段上建立普通索引，SQL语句如下：

```mysql
CREATE TABLE book(
book_id INT ,
book_name VARCHAR(100),
authors VARCHAR(100),
info VARCHAR(100) ,
comment VARCHAR(100),
year_publication YEAR,
INDEX(year_publication)
);
```

**2. 创建唯一索引**

```mysql
CREATE TABLE test1(
id INT NOT NULL,
name varchar(30) NOT NULL,
UNIQUE INDEX uk_idx_id(id)
);
```

该语句执行完毕之后，使用SHOW CREATE TABLE查看表结构：

```mysql
SHOW INDEX FROM test1 \G
```

**3. 主键索引**

设定为主键后数据库会自动建立索引，innodb为聚簇索引，语法：

* 随表一起建索引：

```mysql
CREATE TABLE student (
id INT(10) UNSIGNED AUTO_INCREMENT ,
student_no VARCHAR(200),
student_name VARCHAR(200),
PRIMARY KEY(id)
);
```

* 删除主键索引：

```mysql
ALTER TABLE student
drop PRIMARY KEY;
```

* 修改主键索引：必须先删除掉(drop)原索引，再新建(add)索引

**4. 创建单列索引**

引举:

```mysql
CREATE TABLE test2(
id INT NOT NULL,
name CHAR(50) NULL,
INDEX single_idx_name(name(20))
);
```

该语句执行完毕之后，使用SHOW CREATE TABLE查看表结构：

```mysql
SHOW INDEX FROM test2 \G
```

**5. 创建组合索引**

举例：创建表test3，在表中的id、name和age字段上建立组合索引，SQL语句如下：

```mysql
CREATE TABLE test3(
id INT(11) NOT NULL,
name CHAR(30) NOT NULL,
age INT(11) NOT NULL,
info VARCHAR(255),
INDEX multi_idx(id,name,age)
);
```

该语句执行完毕之后，使用SHOW INDEX 查看：

```mysql
SHOW INDEX FROM test3 \G
```

在test3表中，查询id和name字段，使用EXPLAIN语句查看索引的使用情况：

```mysql
EXPLAIN SELECT * FROM test3 WHERE id=1 AND name='songhongkang' \G
```

可以看到，查询id和name字段时，使用了名称为MultiIdx的索引，如果查询 (name, age) 组合或者单独查询name和age字段，会发现结果中possible_keys和key值为NULL, 并没有使用在t3表中创建的索引进行查询。

**6. 创建全文索引**

FULLTEXT全文索引可以用于全文检索，并且只为 `CHAR` 、`VARCHAR` 和 `TEXT` 列创建索引。索引总是对整个列进行，不支持局部 (前缀) 索引。

举例1：创建表test4，在表中的info字段上建立全文索引，SQL语句如下：

```mysql
CREATE TABLE test4(
id INT NOT NULL,
name CHAR(30) NOT NULL,
age INT NOT NULL,
info VARCHAR(255),
FULLTEXT INDEX futxt_idx_info(info)
) ENGINE=MyISAM;
```

> 在MySQL5.7及之后版本中可以不指定最后的ENGINE了，因为在此版本中InnoDB支持全文索引。

语句执行完毕之后，使用SHOW CREATE TABLE查看表结构：

```mysql
SHOW INDEX FROM test4 \G
```

由结果可以看到，info字段上已经成功建立了一个名为futxt_idx_info的FULLTEXT索引。

举例2：

```mysql
CREATE TABLE articles (
id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
title VARCHAR (200),
body TEXT,
FULLTEXT index (title, body)
) ENGINE = INNODB;
```

创建了一个给title和body字段添加全文索引的表。

举例3：

```mysql
CREATE TABLE `papers` (
`id` int(10) unsigned NOT NULL AUTO_INCREMENT,
`title` varchar(200) DEFAULT NULL,
`content` text,
PRIMARY KEY (`id`),
FULLTEXT KEY `title` (`title`,`content`)
) ENGINE=MyISAM DEFAULT CHARSET=utf8;
```

不同于like方式的的查询：

```mysql
SELECT * FROM papers WHERE content LIKE ‘%查询字符串%’;
```

全文索引用match+against方式查询：

```mysql
SELECT * FROM papers WHERE MATCH(title,content) AGAINST (‘查询字符串’);
```

明显的提高查询效率。

> 注意点 
>
> 1. 使用全文索引前，搞清楚版本支持情况； 
> 2. 全文索引比 like + % 快 N 倍，但是可能存在精度问题；
> 3. 如果需要全文索引的是大量数据，建议先添加数据，再创建索引。

**7. 创建空间索引**

空间索引创建中，要求空间类型的字段必须为 非空 。

举例：创建表test5，在空间类型为GEOMETRY的字段上创建空间索引，SQL语句如下：

```mysql
CREATE TABLE test5(
geo GEOMETRY NOT NULL,
SPATIAL INDEX spa_idx_geo(geo)
) ENGINE=MyISAM;
```

该语句执行完毕之后，使用SHOW CREATE TABLE查看表结构：

```mysql
SHOW INDEX FROM test5 \G
```

可以看到，test5表的geo字段上创建了名称为spa_idx_geo的空间索引。注意创建时指定空间类型字段值的非空约束，并且表的存储引擎为MyISAM。

####  在已经存在的表上创建索引

在已经存在的表中创建索引可以使用ALTER TABLE语句或者CREATE INDEX语句。

**1. 使用ALTER TABLE语句创建索引** ALTER TABLE语句创建索引的基本语法如下：

```mysql
ALTER TABLE table_name ADD [UNIQUE | FULLTEXT | SPATIAL] [INDEX | KEY]
[index_name] (col_name[length],...) [ASC | DESC]
```

**2. 使用CREATE INDEX创建索引** CREATE INDEX语句可以在已经存在的表上添加索引，在MySQL中， CREATE INDEX被映射到一个ALTER TABLE语句上，基本语法结构为：

```mysql
CREATE [UNIQUE | FULLTEXT | SPATIAL] INDEX index_name
ON table_name (col_name[length],...) [ASC | DESC]
```

###  删除索引

**1. 使用ALTER TABLE删除索引**  ALTER TABLE删除索引的基本语法格式如下：

```mysql
ALTER TABLE table_name DROP INDEX index_name;
```

**2. 使用DROP INDEX语句删除索引** DROP INDEX删除索引的基本语法格式如下：

```mysql
DROP INDEX index_name ON table_name;
```

> 提示: 删除表中的列时，如果要删除的列为索引的组成部分，则该列也会从索引中删除。如果组成索引的所有列都被删除，则整个索引将被删除。
>
> 自增字段的唯一索引不能删除

##  MySQL8.0索引新特性

### 支持降序索引

降序索引以降序存储键值。虽然在语法上，从MySQL 4版本开始就已经支持降序索引的语法了，但实际上DESC定义是被忽略的，直到MySQL 8.x版本才开始真正支持降序索引 (仅限于InnoDBc存储引擎)。

MySQL在8.0版本之前创建的仍然是升序索引，使用时进行反向扫描，这大大降低了数据库的效率。在某些场景下，降序索引意义重大。例如，如果一个查询，需要对多个列进行排序，且顺序要求不一致，那么使用降序索引将会避免数据库使用额外的文件排序操作，从而提高性能。

举例：分别在MySQL 5.7版本和MySQL 8.0版本中创建数据表ts1，结果如下：

```mysql
CREATE TABLE ts1(a int,b int,index idx_a_b(a,b desc));
```

在MySQL 5.7版本中查看数据表ts1的结构，结果如下:

![image-20220622224124267](D:\笔记\mysql.assets\image-20220622224124267.png)

从结果可以看出，索引仍然是默认的升序

在MySQL 8.0版本中查看数据表ts1的结构，结果如下：

![image-20220622224205048](D:\笔记\mysql.assets\image-20220622224205048.png)

从结果可以看出，索引已经是降序了。下面继续测试降序索引在执行计划中的表现。

分别在MySQL 5.7版本和MySQL 8.0版本的数据表ts1中插入800条随机数据，执行语句如下：

```mysql
DELIMITER //
CREATE PROCEDURE ts_insert()
BEGIN
	DECLARE i INT DEFAULT 1;
	WHILE i < 800
	DO
		insert into ts1 select rand()*80000, rand()*80000;
		SET i = i+1;
	END WHILE;
	commit;
END //
DELIMITER;

# 调用
CALL ts_insert();
```

在MySQL 5.7版本中查看数据表ts1的执行计划，结果如下:

```mysql
EXPLAIN SELECT * FROM ts1 ORDER BY a, b DESC LIMIT 5;
```

在MySQL 8.0版本中查看数据表 ts1 的执行计划。

从结果可以看出，修改后MySQL 5.7 的执行计划要明显好于MySQL 8.0。

### 隐藏索引

在MySQL 5.7版本及之前，只能通过显式的方式删除索引。此时，如果发展删除索引后出现错误，又只能通过显式创建索引的方式将删除的索引创建回来。如果数据表中的数据量非常大，或者数据表本身比较 大，这种操作就会消耗系统过多的资源，操作成本非常高。

从MySQL 8.x开始支持 隐藏索引（invisible indexes） ，只需要将待删除的索引设置为隐藏索引，使 查询优化器不再使用这个索引（即使使用force index（强制使用索引），优化器也不会使用该索引）， 确认将索引设置为隐藏索引后系统不受任何响应，就可以彻底删除索引。 这种通过先将索引设置为隐藏索 引，再删除索引的方式就是软删除。

同时，如果你想验证某个索引删除之后的 `查询性能影响`，就可以暂时先隐藏该索引。

> 注意：
>
> 主键不能被设置为隐藏索引。当表中没有显式主键时，表中第一个唯一非空索引会成为隐式主键，也不能设置为隐藏索引。

索引默认是可见的，在使用CREATE TABLE, CREATE INDEX 或者 ALTER TABLE 等语句时可以通过 `VISIBLE` 或者 `INVISIBLE` 关键词设置索引的可见性。

**1. 创建表时直接创建**

在MySQL中创建隐藏索引通过SQL语句INVISIBLE来实现，其语法形式如下：

```mysql
CREATE TABLE tablename(
propname1 type1[CONSTRAINT1],
propname2 type2[CONSTRAINT2],
……
propnamen typen,
INDEX [indexname](propname1 [(length)]) INVISIBLE
);
```

上述语句比普通索引多了一个关键字INVISIBLE，用来标记索引为不可见索引。

**2. 在已经存在的表上创建**

可以为已经存在的表设置隐藏索引，其语法形式如下：

```mysql
CREATE INDEX indexname
ON tablename(propname[(length)]) INVISIBLE;
```

**3. 通过ALTER TABLE语句创建**

语法形式如下：

```mysql
ALTER TABLE tablename
ADD INDEX indexname (propname [(length)]) INVISIBLE;
```

**4. 切换索引可见状态**

已存在的索引可通过如下语句切换可见状态：

```mysql
ALTER TABLE tablename ALTER INDEX index_name INVISIBLE; #切换成隐藏索引
ALTER TABLE tablename ALTER INDEX index_name VISIBLE; #切换成非隐藏索引
```

如果将index_cname索引切换成可见状态，通过explain查看执行计划，发现优化器选择了index_cname索引。

> 注意 当索引被隐藏时，它的内容仍然是和正常索引一样实时更新的。如果一个索引需要长期被隐藏，那么可以将其删除，因为索引的存在会影响插入、更新和删除的性能。

通过设置隐藏索引的可见性可以查看索引对调优的帮助。

**5. 使隐藏索引对查询优化器可见**

在MySQL 8.x版本中，为索引提供了一种新的测试方式，可以通过查询优化器的一个开关 (use_invisible_indexes) 来打开某个设置，使隐藏索引对查询优化器可见。如果use_invisible_indexes 设置为off (默认)，优化器会忽略隐藏索引。如果设置为on，即使隐藏索引不可见，优化器在生成执行计 划时仍会考虑使用隐藏索引。

（1）在MySQL命令行执行如下命令查看查询优化器的开关设置。

```mysql
mysql> select @@optimizer_switch \G
```

在输出的结果信息中找到如下属性配置。

```mysql
use_invisible_indexes=off
```

此属性配置值为off，说明隐藏索引默认对查询优化器不可见。

（2）使隐藏索引对查询优化器可见，需要在MySQL命令行执行如下命令：

```mysql
mysql> set session optimizer_switch="use_invisible_indexes=on";
Query OK, 0 rows affected (0.00 sec)
```

SQL语句执行成功，再次查看查询优化器的开关设置。

```mysql
mysql> select @@optimizer_switch \G
*************************** 1. row ***************************
@@optimizer_switch:
index_merge=on,index_merge_union=on,index_merge_sort_union=on,index_merge_
intersection=on,engine_condition_pushdown=on,index_condition_pushdown=on,mrr=on,mrr_co
st_based=on,block_nested_loop=on,batched_key_access=off,materialization=on,semijoin=on
,loosescan=on,firstmatch=on,duplicateweedout=on,subquery_materialization_cost_based=on
,use_index_extensions=on,condition_fanout_filter=on,derived_merge=on,use_invisible_ind
exes=on,skip_scan=on,hash_join=on
1 row in set (0.00 sec)
```

此时，在输出结果中可以看到如下属性配置。

```mysql
use_invisible_indexes=on
```

use_invisible_indexes属性的值为on，说明此时隐藏索引对查询优化器可见。

（3）使用EXPLAIN查看以字段invisible_column作为查询条件时的索引使用情况。

```mysql
explain select * from classes where cname = '高一2班';
```

查询优化器会使用隐藏索引来查询数据。

（4）如果需要使隐藏索引对查询优化器不可见，则只需要执行如下命令即可。

```mysql
mysql> set session optimizer_switch="use_invisible_indexes=off";
Query OK, 0 rows affected (0.00 sec)
```

再次查看查询优化器的开关设置。

```mysql
mysql> select @@optimizer_switch \G
```

此时，use_invisible_indexes属性的值已经被设置为“off”。

## 索引的设计原则

为了使索引的使用效率更高，在创建索引时，必须考虑在哪些字段上创建索引和创建什么类型的索引。**索引设计不合理或者缺少索引都会对数据库和应用程序的性能造成障碍。**高效的索引对于获得良好的性能非常重要。设计索引时，应该考虑相应准则。

###  数据准备

**第1步：创建数据库、创建表**

```mysql
CREATE DATABASE atguigudb1;
USE atguigudb1;
#1.创建学生表和课程表
CREATE TABLE `student_info` (
`id` INT(11) NOT NULL AUTO_INCREMENT,
`student_id` INT NOT NULL ,
`name` VARCHAR(20) DEFAULT NULL,
`course_id` INT NOT NULL ,
`class_id` INT(11) DEFAULT NULL,
`create_time` DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
PRIMARY KEY (`id`)
) ENGINE=INNODB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;

CREATE TABLE `course` (
`id` INT(11) NOT NULL AUTO_INCREMENT,
`course_id` INT NOT NULL ,
`course_name` VARCHAR(40) DEFAULT NULL,
PRIMARY KEY (`id`)
) ENGINE=INNODB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;
```

**第2步：创建模拟数据必需的存储函数**

```mysql
#函数1：创建随机产生字符串函数
DELIMITER //
CREATE FUNCTION rand_string(n INT)
	RETURNS VARCHAR(255) #该函数会返回一个字符串
BEGIN
	DECLARE chars_str VARCHAR(100) DEFAULT
'abcdefghijklmnopqrstuvwxyzABCDEFJHIJKLMNOPQRSTUVWXYZ';
	DECLARE return_str VARCHAR(255) DEFAULT '';
    DECLARE i INT DEFAULT 0;
    WHILE i < n DO
    	SET return_str =CONCAT(return_str,SUBSTRING(chars_str,FLOOR(1+RAND()*52),1));
    	SET i = i + 1;
    END WHILE;
    RETURN return_str;
END //
DELIMITER ;
```

```mysql
#函数2：创建随机数函数
DELIMITER //
CREATE FUNCTION rand_num (from_num INT ,to_num INT) RETURNS INT(11)
BEGIN
DECLARE i INT DEFAULT 0;
SET i = FLOOR(from_num +RAND()*(to_num - from_num+1)) ;
RETURN i;
END //
DELIMITER ;
```

创建函数，假如报错：

```mysql
This function has none of DETERMINISTIC......
```

由于开启过慢查询日志bin-log, 我们就必须为我们的function指定一个参数。

主从复制，主机会将写操作记录在bin-log日志中。从机读取bin-log日志，执行语句来同步数据。如果使 用函数来操作数据，会导致从机和主键操作时间不一致。所以，默认情况下，mysql不开启创建函数设置。

* 查看mysql是否允许创建函数：

```mysql
show variables like 'log_bin_trust_function_creators';
```

* 命令开启：允许创建函数设置：

```mysql
set global log_bin_trust_function_creators=1; # 不加global只是当前窗口有效。
```

* mysqld重启，上述参数又会消失。永久方法：

	* windows下：my.ini[mysqld]加上：

		```mysql
		log_bin_trust_function_creators=1
		```

	* linux下：/etc/my.cnf下my.cnf[mysqld]加上：

		```mysql
		log_bin_trust_function_creators=1
		```

**第3步：创建插入模拟数据的存储过程**

```mysql
# 存储过程1：创建插入课程表存储过程
DELIMITER //
CREATE PROCEDURE insert_course( max_num INT )
BEGIN
DECLARE i INT DEFAULT 0;
SET autocommit = 0; #设置手动提交事务
REPEAT #循环
SET i = i + 1; #赋值
INSERT INTO course (course_id, course_name ) VALUES
(rand_num(10000,10100),rand_string(6));
UNTIL i = max_num
END REPEAT;
COMMIT; #提交事务
END //
DELIMITER ;
```

```mysql
# 存储过程2：创建插入学生信息表存储过程
DELIMITER //
CREATE PROCEDURE insert_stu( max_num INT )
BEGIN
DECLARE i INT DEFAULT 0;
SET autocommit = 0; #设置手动提交事务
REPEAT #循环
SET i = i + 1; #赋值
INSERT INTO student_info (course_id, class_id ,student_id ,NAME ) VALUES
(rand_num(10000,10100),rand_num(10000,10200),rand_num(1,200000),rand_string(6));
UNTIL i = max_num
END REPEAT;
COMMIT; #提交事务
END //
DELIMITER ;
```

**第4步：调用存储过程**

```mysql
CALL insert_course(100);
```

```mysql
CALL insert_stu(1000000);
```

###  哪些情况适合创建索引

#### 字段的数值有唯一性的限制

<img src="D:\笔记\mysql.assets\image-20220623154615702.png" alt="image-20220623154615702" style="float:left;" />

> 业务上具有唯一特性的字段，即使是组合字段，也必须建成唯一索引。（来源：Alibaba） 说明：不要以为唯一索引影响了 insert 速度，这个速度损耗可以忽略，但提高查找速度是明显的。

####  频繁作为 WHERE 查询条件的字段

某个字段在SELECT语句的 WHERE 条件中经常被使用到，那么就需要给这个字段创建索引了。尤其是在 数据量大的情况下，创建普通索引就可以大幅提升数据查询的效率。 

比如student_info数据表（含100万条数据），假设我们想要查询 student_id=123110 的用户信息。

####  经常 GROUP BY 和 ORDER BY 的列

索引就是让数据按照某种顺序进行存储或检索，因此当我们使用 GROUP BY 对数据进行分组查询，或者使用 ORDER BY 对数据进行排序的时候，就需要对分组或者排序的字段进行索引 。如果待排序的列有多个，那么可以在这些列上建立组合索引 。

#### UPDATE、DELETE 的 WHERE 条件列

对数据按照某个条件进行查询后再进行 UPDATE 或 DELETE 的操作，如果对 WHERE 字段创建了索引，就能大幅提升效率。原理是因为我们需要先根据 WHERE 条件列检索出来这条记录，然后再对它进行更新或删除。**如果进行更新的时候，更新的字段是非索引字段，提升的效率会更明显，这是因为非索引字段更新不需要对索引进行维护。**

#### DISTINCT 字段需要创建索引

有时候我们需要对某个字段进行去重，使用 DISTINCT，那么对这个字段创建索引，也会提升查询效率。 

比如，我们想要查询课程表中不同的 student_id 都有哪些，如果我们没有对 student_id 创建索引，执行 SQL 语句：

```mysql
SELECT DISTINCT(student_id) FROM `student_info`;
```

运行结果（600637 条记录，运行时间 0.683s ）

如果我们对 student_id 创建索引，再执行 SQL 语句：

```mysql
SELECT DISTINCT(student_id) FROM `student_info`;
```

运行结果（600637 条记录，运行时间 0.010s ）

你能看到 SQL 查询效率有了提升，同时显示出来的 student_id 还是按照递增的顺序 进行展示的。这是因为索引会对数据按照某种顺序进行排序，所以在去重的时候也会快很多。

####  多表 JOIN 连接操作时，创建索引注意事项

首先， `连接表的数量尽量不要超过 3 张` ，因为每增加一张表就相当于增加了一次嵌套的循环，数量级增 长会非常快，严重影响查询的效率。 

其次， `对 WHERE 条件创建索引` ，因为 WHERE 才是对数据条件的过滤。如果在数据量非常大的情况下， 没有 WHERE 条件过滤是非常可怕的。 

最后， `对用于连接的字段创建索引` ，并且该字段在多张表中的 类型必须一致 。比如 course_id 在 student_info 表和 course 表中都为 int(11) 类型，而不能一个为 int 另一个为 varchar 类型。

举个例子，如果我们只对 student_id 创建索引，执行 SQL 语句：

```mysql
SELECT s.course_id, name, s.student_id, c.course_name
FROM student_info s JOIN course c
ON s.course_id = c.course_id
WHERE name = '462eed7ac6e791292a79';
```

运行结果（1 条数据，运行时间 0.189s ）

这里我们对 name 创建索引，再执行上面的 SQL 语句，运行时间为 0.002s 。

####  使用列的类型小的创建索引

<img src="D:\笔记\mysql.assets\image-20220623175306282.png" alt="image-20220623175306282" style="float:left;" />

####  使用字符串前缀创建索引

<img src="D:\笔记\mysql.assets\image-20220623175513439.png" alt="image-20220623175513439" style="float:left;" />

创建一张商户表，因为地址字段比较长，在地址字段上建立前缀索引

```mysql
create table shop(address varchar(120) not null);
alter table shop add index(address(12));
```

问题是，截取多少呢？截取得多了，达不到节省索引存储空间的目的；截取得少了，重复内容太多，字 段的散列度(选择性)会降低。怎么计算不同的长度的选择性呢？

先看一下字段在全部数据中的选择度：

```mysql
select count(distinct address) / count(*) from shop
```

通过不同长度去计算，与全表的选择性对比：

公式：

```mysql
count(distinct left(列名, 索引长度))/count(*)
```

例如：

```mysql
select count(distinct left(address,10)) / count(*) as sub10, -- 截取前10个字符的选择度
count(distinct left(address,15)) / count(*) as sub11, -- 截取前15个字符的选择度
count(distinct left(address,20)) / count(*) as sub12, -- 截取前20个字符的选择度
count(distinct left(address,25)) / count(*) as sub13 -- 截取前25个字符的选择度
from shop;
```

> 越接近于1越好，说明越有区分度

**引申另一个问题：索引列前缀对排序的影响**

如果使用了索引列前缀，比方说前边只把address列的 `前12个字符` 放到了二级索引中，下边这个查询可能就有点尴尬了：

```mysql
SELECT * FROM shop
ORDER BY address
LIMIT 12;
```

因为二级索引中不包含完整的address列信息，所以无法对前12个字符相同，后边的字符不同的记录进行排序，也就是使用索引列前缀的方式 `无法支持使用索引排序` ，只能使用文件排序。

**拓展：Alibaba《Java开发手册》**

【 强制 】在 varchar 字段上建立索引时，必须指定索引长度，没必要对全字段建立索引，根据实际文本 区分度决定索引长度。 

说明：索引的长度与区分度是一对矛盾体，一般对字符串类型数据，长度为 20 的索引，区分度会高达 90% 以上 ，可以使用 count(distinct left(列名, 索引长度))/count(*)的区分度来确定。

####  区分度高(散列性高)的列适合作为索引

`列的基数` 指的是某一列中不重复数据的个数，比方说某个列包含值 `2, 5, 8, 2, 5, 8, 2, 5, 8`，虽然有`9`条记录，但该列的基数却是3。也就是说**在记录行数一定的情况下，列的基数越大，该列中的值越分散；列的基数越小，该列中的值越集中。**这个列的基数指标非常重要，直接影响我们是否能有效的利用索引。最好为列的基数大的列简历索引，为基数太小的列的简历索引效果可能不好。

可以使用公式`select count(distinct a) / count(*) from t1` 计算区分度，越接近1越好，一般超过33%就算比较高效的索引了。

扩展：联合索引把区分度搞(散列性高)的列放在前面。

#### 使用最频繁的列放到联合索引的左侧

这样也可以较少的建立一些索引。同时，由于"最左前缀原则"，可以增加联合索引的使用率。

####  在多个字段都要创建索引的情况下，联合索引优于单值索引

### 限制索引的数目

<img src="D:\笔记\mysql.assets\image-20220627151947786.png" alt="image-20220627151947786" style="float:left;" />

### 哪些情况不适合创建索引

####  在where中使用不到的字段，不要设置索引

WHERE条件 (包括 GROUP BY、ORDER BY) 里用不到的字段不需要创建索引，索引的价值是快速定位，如果起不到定位的字段通常是不需要创建索引的。举个例子：

```mysql
SELECT course_id, student_id, create_time
FROM student_info
WHERE student_id = 41251;
```

因为我们是按照 student_id 来进行检索的，所以不需要对其他字段创建索引，即使这些字段出现在SELECT字段中。

#### 数据量小的表最好不要使用索引

如果表记录太少，比如少于1000个，那么是不需要创建索引的。表记录太少，是否创建索引 `对查询效率的影响并不大`。甚至说，查询花费的时间可能比遍历索引的时间还要短，索引可能不会产生优化效果。

举例：创建表1：

```mysql
CREATE TABLE t_without_index(
a INT PRIMARY KEY AUTO_INCREMENT,
b INT
);
```

提供存储过程1：

```mysql
#创建存储过程
DELIMITER //
CREATE PROCEDURE t_wout_insert()
BEGIN
    DECLARE i INT DEFAULT 1;
    WHILE i <= 900
    DO
        INSERT INTO t_without_index(b) SELECT RAND()*10000;
        SET i = i + 1;
    END WHILE;
    COMMIT;
END //
DELIMITER ;

#调用
CALL t_wout_insert()
```

创建表2：

```mysql
CREATE TABLE t_with_index(
a INT PRIMARY KEY AUTO_INCREMENT,
b INT,
INDEX idx_b(b)
);
```

创建存储过程2：

```mysql
#创建存储过程
DELIMITER //
CREATE PROCEDURE t_with_insert()
BEGIN
    DECLARE i INT DEFAULT 1;
    WHILE i <= 900
    DO
        INSERT INTO t_with_index(b) SELECT RAND()*10000;
        SET i = i + 1;
    END WHILE;
    COMMIT;
END //
DELIMITER ;

#调用
CALL t_with_insert();
```

查询对比：

```mysql
mysql> select * from t_without_index where b = 9879;
+------+------+
| a | b |
+------+------+
| 1242 | 9879 |
+------+------+
1 row in set (0.00 sec)

mysql> select * from t_with_index where b = 9879;
+-----+------+
| a | b |
+-----+------+
| 112 | 9879 |
+-----+------+
1 row in set (0.00 sec)
```

你能看到运行结果相同，但是在数据量不大的情况下，索引就发挥不出作用了。

> 结论：在数据表中的数据行数比较少的情况下，比如不到 1000 行，是不需要创建索引的。

####  有大量重复数据的列上不要建立索引

在条件表达式中经常用到的不同值较多的列上建立索引，但字段中如果有大量重复数据，也不用创建索引。比如在学生表的"性别"字段上只有“男”与“女”两个不同值，因此无须建立索引。如果建立索引，不但不会提高查询效率，反而会`严重降低数据更新速度`。

举例1：要在 100 万行数据中查找其中的 50 万行（比如性别为男的数据），一旦创建了索引，你需要先 访问 50 万次索引，然后再访问 50 万次数据表，这样加起来的开销比不使用索引可能还要大。

举例2：假设有一个学生表，学生总数为 100 万人，男性只有 10 个人，也就是占总人口的 10 万分之 1。

学生表 student_gender 结构如下。其中数据表中的 student_gender 字段取值为 0 或 1，0 代表女性，1 代表男性。

```mysql
CREATE TABLE student_gender(
    student_id INT(11) NOT NULL,
    student_name VARCHAR(50) NOT NULL,
    student_gender TINYINT(1) NOT NULL,
    PRIMARY KEY(student_id)
)ENGINE = INNODB;
```

如果我们要筛选出这个学生表中的男性，可以使用：

```mysql
SELECT * FROM student_gender WHERE student_gender = 1;
```

> 结论：当数据重复度大，比如 高于 10% 的时候，也不需要对这个字段使用索引。

#### 避免对经常更新的表创建过多的索引

第一层含义：频繁更新的字段不一定要创建索引。因为更新数据的时候，也需要更新索引，如果索引太多，在更新索引的时候也会造成负担，从而影响效率。

第二层含义：避免对经常更新的表创建过多的索引，并且索引中的列尽可能少。此时，虽然提高了查询速度，同时却降低更新表的速度。

#### 不建议用无序的值作为索引

例如身份证、UUID(在索引比较时需要转为ASCII，并且插入时可能造成页分裂)、MD5、HASH、无序长字 符串等。

#### 删除不再使用或者很少使用的索引

表中的数据被大量更新，或者数据的使用方式被改变后，原有的一些索引可能不再需要。数据库管理员应当定期找出这些索引，将它们删除，从而减少索引对更新操作的影响。

####  不要定义夯余或重复的索引

① 冗余索引 

举例：建表语句如下

```mysql
CREATE TABLE person_info(
    id INT UNSIGNED NOT NULL AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    birthday DATE NOT NULL,
    phone_number CHAR(11) NOT NULL,
    country varchar(100) NOT NULL,
    PRIMARY KEY (id),
    KEY idx_name_birthday_phone_number (name(10), birthday, phone_number),
    KEY idx_name (name(10))
);
```

我们知道，通过 idx_name_birthday_phone_number 索引就可以对 name 列进行快速搜索，再创建一 个专门针对 name 列的索引就算是一个 冗余索引 ，维护这个索引只会增加维护的成本，并不会对搜索有 什么好处。

② 重复索引 

另一种情况，我们可能会对某个列 重复建立索引 ，比方说这样：

```mysql
CREATE TABLE repeat_index_demo (
col1 INT PRIMARY KEY,
col2 INT,
UNIQUE uk_idx_c1 (col1),
INDEX idx_c1 (col1)
);
```

我们看到，col1 既是主键、又给它定义为一个唯一索引，还给它定义了一个普通索引，可是主键本身就 会生成聚簇索引，所以定义的唯一索引和普通索引是重复的，这种情况要避免。

# 性能分析工具

在数据库调优中，我们的目标是 `响应时间更快, 吞吐量更大` 。利用宏观的监控工具和微观的日志分析可以帮我们快速找到调优的思路和方式。

##  数据库服务器的优化步骤

当我们遇到数据库调优问题的时候，该如何思考呢？这里把思考的流程整理成下面这张图。

整个流程划分成了 `观察（Show status）` 和 `行动（Action）` 两个部分。字母 S 的部分代表观察（会使 用相应的分析工具），字母 A 代表的部分是行动（对应分析可以采取的行动）。

![image-20220627162248635](D:\笔记\mysql.assets\image-20220627162248635.png)

![image-20220627162345815](D:\笔记\mysql.assets\image-20220627162345815.png)

我们可以通过观察了解数据库整体的运行状态，通过性能分析工具可以让我们了解执行慢的SQL都有哪些，查看具体的SQL执行计划，甚至是SQL执行中的每一步的成本代价，这样才能定位问题所在，找到了问题，再采取相应的行动。

**详细解释一下这张图：**

<img src="D:\笔记\mysql.assets\image-20220627164046438.png" alt="image-20220627164046438" style="float:left;" />

<img src="D:\笔记\mysql.assets\image-20220627164114562.png" alt="image-20220627164114562" style="float:left;" />

##  查看系统性能参数

在MySQL中，可以使用 `SHOW STATUS` 语句查询一些MySQL数据库服务器的`性能参数、执行频率`。

SHOW STATUS语句语法如下：

```mysql
SHOW [GLOBAL|SESSION] STATUS LIKE '参数';
```

一些常用的性能参数如下：

* `Connections`：连接MySQL服务器的次数。 
* `Uptime`：MySQL服务器的上线时间。 
* `Slow_queries`：慢查询的次数。 
* `Innodb_rows_read`：Select查询返回的行数 
* `Innodb_rows_inserted`：执行INSERT操作插入的行数 
* `Innodb_rows_updated`：执行UPDATE操作更新的 行数 
* `Innodb_rows_deleted`：执行DELETE操作删除的行数 
* `Com_select`：查询操作的次数。 
* `Com_insert`：插入操作的次数。对于批量插入的 INSERT 操作，只累加一次。 
* `Com_update`：更新操作 的次数。 
* `Com_delete`：删除操作的次数。

若查询MySQL服务器的连接次数，则可以执行如下语句:

```mysql
SHOW STATUS LIKE 'Connections';
```

若查询服务器工作时间，则可以执行如下语句:

```mysql
SHOW STATUS LIKE 'Uptime';
```

若查询MySQL服务器的慢查询次数，则可以执行如下语句:

```mysql
SHOW STATUS LIKE 'Slow_queries';
```

慢查询次数参数可以结合慢查询日志找出慢查询语句，然后针对慢查询语句进行`表结构优化`或者`查询语句优化`。

再比如，如下的指令可以查看相关的指令情况：

```mysql
SHOW STATUS LIKE 'Innodb_rows_%';
```

##  统计SQL的查询成本: last_query_cost

一条SQL查询语句在执行前需要查询执行计划，如果存在多种执行计划的话，MySQL会计算每个执行计划所需要的成本，从中选择`成本最小`的一个作为最终执行的执行计划。

如果我们想要查看某条SQL语句的查询成本，可以在执行完这条SQL语句之后，通过查看当前会话中的`last_query_cost`变量值来得到当前查询的成本。它通常也是我们`评价一个查询的执行效率`的一个常用指标。这个查询成本对应的是`SQL 语句所需要读取的读页的数量`。

我们依然使用第8章的 student_info 表为例：

```mysql
CREATE TABLE `student_info` (
    `id` INT(11) NOT NULL AUTO_INCREMENT,
    `student_id` INT NOT NULL ,
    `name` VARCHAR(20) DEFAULT NULL,
    `course_id` INT NOT NULL ,
    `class_id` INT(11) DEFAULT NULL,
    `create_time` DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    PRIMARY KEY (`id`)
) ENGINE=INNODB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;
```

如果我们想要查询 id=900001 的记录，然后看下查询成本，我们可以直接在聚簇索引上进行查找：

```mysql
SELECT student_id, class_id, NAME, create_time FROM student_info WHERE id = 900001;
```

运行结果（1 条记录，运行时间为 0.042s ）

然后再看下查询优化器的成本，实际上我们只需要检索一个页即可：

```mysql
mysql> SHOW STATUS LIKE 'last_query_cost';
+-----------------+----------+
| Variable_name   |   Value  |
+-----------------+----------+
| Last_query_cost | 1.000000 |
+-----------------+----------+
```

如果我们想要查询 id 在 900001 到 9000100 之间的学生记录呢？

```mysql
SELECT student_id, class_id, NAME, create_time FROM student_info WHERE id BETWEEN 900001 AND 900100;
```

运行结果（100 条记录，运行时间为 0.046s ）： 

然后再看下查询优化器的成本，这时我们大概需要进行 20 个页的查询。

```mysql
mysql> SHOW STATUS LIKE 'last_query_cost';
+-----------------+-----------+
| Variable_name   |   Value   |
+-----------------+-----------+
| Last_query_cost | 21.134453 |
+-----------------+-----------+
```

你能看到页的数量是刚才的 20 倍，但是查询的效率并没有明显的变化，实际上这两个 SQL 查询的时间 基本上一样，就是因为采用了顺序读取的方式将页面一次性加载到缓冲池中，然后再进行查找。虽然 页 数量（last_query_cost）增加了不少 ，但是通过缓冲池的机制，并 没有增加多少查询时间 。 

**使用场景：**它对于比较开销是非常有用的，特别是我们有好几种查询方式可选的时候。

> SQL查询时一个动态的过程，从页加载的角度来看，我们可以得到以下两点结论：
>
> 1. `位置决定效率`。如果页就在数据库 `缓冲池` 中，那么效率是最高的，否则还需要从 `内存` 或者 `磁盘` 中进行读取，当然针对单个页的读取来说，如果页存在于内存中，会比在磁盘中读取效率高很多。
> 2. `批量决定效率`。如果我们从磁盘中对单一页进行随机读，那么效率是很低的(差不多10ms)，而采用顺序读取的方式，批量对页进行读取，平均一页的读取效率就会提升很多，甚至要快于单个页面在内存中的随机读取。
>
> 所以说，遇到I/O并不用担心，方法找对了，效率还是很高的。我们首先要考虑数据存放的位置，如果是进程使用的数据就要尽量放到`缓冲池`中，其次我们可以充分利用磁盘的吞吐能力，一次性批量读取数据，这样单个页的读取效率也就得到了提升。

## 定位执行慢的 SQL：慢查询日志

<img src="D:\笔记\mysql.assets\image-20220628173022699.png" alt="image-20220628173022699" style="float:left;" />

###  开启慢查询日志参数

**1. 开启 slow_query_log**

在使用前，我们需要先查下慢查询是否已经开启，使用下面这条命令即可：

```mysql
mysql > show variables like '%slow_query_log';
```

<img src="D:\笔记\mysql.assets\image-20220628173525966.png" alt="image-20220628173525966" style="float:left;" />

我们可以看到 `slow_query_log=OFF`，我们可以把慢查询日志打开，注意设置变量值的时候需要使用 global，否则会报错：

```mysql
mysql > set global slow_query_log='ON';
```

然后我们再来查看下慢查询日志是否开启，以及慢查询日志文件的位置：

<img src="D:\笔记\mysql.assets\image-20220628175226812.png" alt="image-20220628175226812" style="float:left;" />

你能看到这时慢查询分析已经开启，同时文件保存在 `/var/lib/mysql/atguigu02-slow.log` 文件 中。

**2. 修改 long_query_time 阈值**

接下来我们来看下慢查询的时间阈值设置，使用如下命令：

```mysql
mysql > show variables like '%long_query_time%';
```

<img src="D:\笔记\mysql.assets\image-20220628175353233.png" alt="image-20220628175353233" style="float:left;" />

这里如果我们想把时间缩短，比如设置为 1 秒，可以这样设置：

```mysql
#测试发现：设置global的方式对当前session的long_query_time失效。对新连接的客户端有效。所以可以一并
执行下述语句
mysql > set global long_query_time = 1;
mysql> show global variables like '%long_query_time%';

mysql> set long_query_time=1;
mysql> show variables like '%long_query_time%';
```

<img src="D:\笔记\mysql.assets\image-20220628175425922.png" alt="image-20220628175425922" style="zoom:80%; float:left;" />

**补充：配置文件中一并设置参数**

如下的方式相较于前面的命令行方式，可以看做是永久设置的方式。

修改 `my.cnf` 文件，[mysqld] 下增加或修改参数 `long_query_time、slow_query_log` 和 `slow_query_log_file` 后，然后重启 MySQL 服务器。

```properties
[mysqld]
slow_query_log=ON  # 开启慢查询日志开关
slow_query_log_file=/var/lib/mysql/atguigu-low.log  # 慢查询日志的目录和文件名信息
long_query_time=3  # 设置慢查询的阈值为3秒，超出此设定值的SQL即被记录到慢查询日志
log_output=FILE
```

如果不指定存储路径，慢查询日志默认存储到MySQL数据库的数据文件夹下。如果不指定文件名，默认文件名为hostname_slow.log。

### 查看慢查询数目

查询当前系统中有多少条慢查询记录

```mysql
SHOW GLOBAL STATUS LIKE '%Slow_queries%';
```

###  案例演示

**步骤1. 建表**

```mysql
CREATE TABLE `student` (
    `id` INT(11) NOT NULL AUTO_INCREMENT,
    `stuno` INT NOT NULL ,
    `name` VARCHAR(20) DEFAULT NULL,
    `age` INT(3) DEFAULT NULL,
    `classId` INT(11) DEFAULT NULL,
    PRIMARY KEY (`id`)
) ENGINE=INNODB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;
```

**步骤2：设置参数 log_bin_trust_function_creators**

创建函数，假如报错：

```mysql
This function has none of DETERMINISTIC......
```

* 命令开启：允许创建函数设置：

```mysql
set global log_bin_trust_function_creators=1; # 不加global只是当前窗口有效。
```

**步骤3：创建函数**

随机产生字符串：（同上一章）

```mysql
DELIMITER //
CREATE FUNCTION rand_string(n INT)
	RETURNS VARCHAR(255) #该函数会返回一个字符串
BEGIN
	DECLARE chars_str VARCHAR(100) DEFAULT
'abcdefghijklmnopqrstuvwxyzABCDEFJHIJKLMNOPQRSTUVWXYZ';
	DECLARE return_str VARCHAR(255) DEFAULT '';
    DECLARE i INT DEFAULT 0;
    WHILE i < n DO
    	SET return_str =CONCAT(return_str,SUBSTRING(chars_str,FLOOR(1+RAND()*52),1));
    	SET i = i + 1;
    END WHILE;
    RETURN return_str;
END //
DELIMITER ;

# 测试
SELECT rand_string(10);
```

产生随机数值：（同上一章）

```mysql
DELIMITER //
CREATE FUNCTION rand_num (from_num INT ,to_num INT) RETURNS INT(11)
BEGIN
    DECLARE i INT DEFAULT 0;
    SET i = FLOOR(from_num +RAND()*(to_num - from_num+1)) ;
    RETURN i;
END //
DELIMITER ;

#测试：
SELECT rand_num(10,100);
```

**步骤4：创建存储过程**

```mysql
DELIMITER //
CREATE PROCEDURE insert_stu1( START INT , max_num INT )
BEGIN
DECLARE i INT DEFAULT 0;
    SET autocommit = 0; #设置手动提交事务
    REPEAT #循环
    SET i = i + 1; #赋值
    INSERT INTO student (stuno, NAME ,age ,classId ) VALUES
    ((START+i),rand_string(6),rand_num(10,100),rand_num(10,1000));
    UNTIL i = max_num
    END REPEAT;
    COMMIT; #提交事务
END //
DELIMITER ;
```

**步骤5：调用存储过程**

```mysql
#调用刚刚写好的函数, 4000000条记录,从100001号开始

CALL insert_stu1(100001,4000000);
```

### 测试及分析

**1. 测试**

```mysql
mysql> SELECT * FROM student WHERE stuno = 3455655;
+---------+---------+--------+------+---------+
|   id    |  stuno  |  name  | age  | classId |
+---------+---------+--------+------+---------+
| 3523633 | 3455655 | oQmLUr |  19  |    39   |
+---------+---------+--------+------+---------+
1 row in set (2.09 sec)

mysql> SELECT * FROM student WHERE name = 'oQmLUr';
+---------+---------+--------+------+---------+
|   id    |  stuno  |  name  |  age | classId |
+---------+---------+--------+------+---------+
| 1154002 | 1243200 | OQMlUR | 266  |   28    |
| 1405708 | 1437740 | OQMlUR | 245  |   439   |
| 1748070 | 1680092 | OQMlUR | 240  |   414   |
| 2119892 | 2051914 | oQmLUr | 17   |   32    |
| 2893154 | 2825176 | OQMlUR | 245  |   435   |
| 3523633 | 3455655 | oQmLUr | 19   |   39    |
+---------+---------+--------+------+---------+
6 rows in set (2.39 sec)
```

从上面的结果可以看出来，查询学生编号为“3455655”的学生信息花费时间为2.09秒。查询学生姓名为 “oQmLUr”的学生信息花费时间为2.39秒。已经达到了秒的数量级，说明目前查询效率是比较低的，下面 的小节我们分析一下原因。

**2. 分析**

```mysql
show status like 'slow_queries';
```

<img src="D:\笔记\mysql.assets\image-20220628195650079.png" alt="image-20220628195650079" style="float:left;" />

###  慢查询日志分析工具：mysqldumpslow

在生产环境中，如果要手工分析日志，查找、分析SQL，显然是个体力活，MySQL提供了日志分析工具 `mysqldumpslow` 。

查看mysqldumpslow的帮助信息

```properties
mysqldumpslow --help
```

<img src="D:\笔记\mysql.assets\image-20220628195821440.png" alt="image-20220628195821440" style="float:left;" />

mysqldumpslow 命令的具体参数如下：

* -a: 不将数字抽象成N，字符串抽象成S
* -s: 是表示按照何种方式排序：
	* c: 访问次数 
	* l: 锁定时间 
	* r: 返回记录 
	* t: 查询时间 
	* al:平均锁定时间 
	* ar:平均返回记录数 
	* at:平均查询时间 （默认方式） 
	* ac:平均查询次数
* -t: 即为返回前面多少条的数据；
* -g: 后边搭配一个正则匹配模式，大小写不敏感的；

举例：我们想要按照查询时间排序，查看前五条 SQL 语句，这样写即可：

```properties
mysqldumpslow -s t -t 5 /var/lib/mysql/atguigu01-slow.log
```

```properties
[root@bogon ~]# mysqldumpslow -s t -t 5 /var/lib/mysql/atguigu01-slow.log

Reading mysql slow query log from /var/lib/mysql/atguigu01-slow.log
Count: 1 Time=2.39s (2s) Lock=0.00s (0s) Rows=13.0 (13), root[root]@localhost
SELECT * FROM student WHERE name = 'S'

Count: 1 Time=2.09s (2s) Lock=0.00s (0s) Rows=2.0 (2), root[root]@localhost
SELECT * FROM student WHERE stuno = N

Died at /usr/bin/mysqldumpslow line 162, <> chunk 2.
```

**工作常用参考：**

```properties
#得到返回记录集最多的10个SQL
mysqldumpslow -s r -t 10 /var/lib/mysql/atguigu-slow.log

#得到访问次数最多的10个SQL
mysqldumpslow -s c -t 10 /var/lib/mysql/atguigu-slow.log

#得到按照时间排序的前10条里面含有左连接的查询语句
mysqldumpslow -s t -t 10 -g "left join" /var/lib/mysql/atguigu-slow.log

#另外建议在使用这些命令时结合 | 和more 使用 ，否则有可能出现爆屏情况
mysqldumpslow -s r -t 10 /var/lib/mysql/atguigu-slow.log | more
```

###  关闭慢查询日志

MySQL服务器停止慢查询日志功能有两种方法：

**方式1：永久性方式**

```properties
[mysqld]
slow_query_log=OFF
```

或者，把slow_query_log一项注释掉 或 删除

```properties
[mysqld]
#slow_query_log =OFF
```

重启MySQL服务，执行如下语句查询慢日志功能。

```mysql
SHOW VARIABLES LIKE '%slow%'; #查询慢查询日志所在目录
SHOW VARIABLES LIKE '%long_query_time%'; #查询超时时长
```

**方式2：临时性方式**

使用SET语句来设置。 

（1）停止MySQL慢查询日志功能，具体SQL语句如下。

```mysql
SET GLOBAL slow_query_log=off;
```

（2）**重启MySQL服务**，使用SHOW语句查询慢查询日志功能信息，具体SQL语句如下。

```mysql
SHOW VARIABLES LIKE '%slow%';
#以及
SHOW VARIABLES LIKE '%long_query_time%';
```

### 删除慢查询日志

使用SHOW语句显示慢查询日志信息，具体SQL语句如下。

```mysql
SHOW VARIABLES LIKE `slow_query_log%`;
```

<img src="D:\笔记\mysql.assets\image-20220628203545536.png" alt="image-20220628203545536" style="float:left;" />

从执行结果可以看出，慢查询日志的目录默认为MySQL的数据目录，在该目录下 `手动删除慢查询日志文件` 即可。

使用命令 `mysqladmin flush-logs` 来重新生成查询日志文件，具体命令如下，执行完毕会在数据目录下重新生成慢查询日志文件。

```properties
mysqladmin -uroot -p flush-logs slow
```

> 提示
>
> 慢查询日志都是使用mysqladmin flush-logs命令来删除重建的。使用时一定要注意，一旦执行了这个命令，慢查询日志都只存在新的日志文件中，如果需要旧的查询日志，就必须事先备份。

## 5. 查看 SQL 执行成本：SHOW PROFILE

show profile 在《逻辑架构》章节中讲过，这里作为复习。

show profile 是 MySQL 提供的可以用来分析当前会话中 SQL 都做了什么、执行的资源消耗工具的情况，可用于 sql 调优的测量。`默认情况下处于关闭状态`，并保存最近15次的运行结果。

我们可以在会话级别开启这个功能。

```mysql
mysql > show variables like 'profiling';
```

<img src="D:\笔记\mysql.assets\image-20220628204922556.png" alt="image-20220628204922556" style="float:left;" />

通过设置 profiling='ON' 来开启 show profile:

```mysql
mysql > set profiling = 'ON';
```

<img src="D:\笔记\mysql.assets\image-20220628205029208.png" alt="image-20220628205029208" style="zoom:80%;float:left" />

然后执行相关的查询语句。接着看下当前会话都有哪些 profiles，使用下面这条命令：

```mysql
mysql > show profiles;
```

<img src="D:\笔记\mysql.assets\image-20220628205243769.png" alt="image-20220628205243769" style="zoom:80%;float:left" />

你能看到当前会话一共有 2 个查询。如果我们想要查看最近一次查询的开销，可以使用：

```mysql
mysql > show profile;
```

<img src="D:\笔记\mysql.assets\image-20220628205317257.png" alt="image-20220628205317257" style="float:left;" />

```mysql
mysql> show profile cpu,block io for query 2
```

<img src="D:\笔记\mysql.assets\image-20220628205354230.png" alt="image-20220628205354230" style="float:left;" />

**show profile的常用查询参数： **

① ALL：显示所有的开销信息。 

② BLOCK IO：显示块IO开销。 

③ CONTEXT SWITCHES：上下文切换开销。 

④ CPU：显示CPU开销信息。 

⑤ IPC：显示发送和接收开销信息。

⑥ MEMORY：显示内存开销信 息。 

⑦ PAGE FAULTS：显示页面错误开销信息。 

⑧ SOURCE：显示和Source_function，Source_file， Source_line相关的开销信息。 

⑨ SWAPS：显示交换次数开销信息。

**日常开发需注意的结论：**

① `converting HEAP to MyISAM`: 查询结果太大，内存不够，数据往磁盘上搬了。 

② `Creating tmp table`：创建临时表。先拷贝数据到临时表，用完后再删除临时表。 

③ `Copying to tmp table on disk`：把内存中临时表复制到磁盘上，警惕！ 

④ `locked`。 

如果在show profile诊断结果中出现了以上4条结果中的任何一条，则sql语句需要优化。

**注意：**

不过SHOW PROFILE命令将被启用，我们可以从 information_schema 中的 profiling 数据表进行查看。

##  分析查询语句：EXPLAIN

###  概述

<img src="D:\笔记\mysql.assets\image-20220628210837301.png" alt="image-20220628210837301" style="float:left;" />

**1. 能做什么？**

* 表的读取顺序
* 数据读取操作的操作类型
* 哪些索引可以使用
* 哪些索引被实际使用
* 表之间的引用
* 每张表有多少行被优化器查询

**2. 官网介绍**

https://dev.mysql.com/doc/refman/5.7/en/explain-output.html 

https://dev.mysql.com/doc/refman/8.0/en/explain-output.html

![image-20220628211207436](D:\笔记\mysql.assets\image-20220628211207436.png)

**3. 版本情况**

* MySQL 5.6.3以前只能 EXPLAIN SELECT ；MYSQL 5.6.3以后就可以 EXPLAIN SELECT，UPDATE， DELETE 
* 在5.7以前的版本中，想要显示 partitions 需要使用 explain partitions 命令；想要显示 filtered 需要使用 explain extended 命令。在5.7版本后，默认explain直接显示partitions和 filtered中的信息。

<img src="D:\笔记\mysql.assets\image-20220628211351678.png" alt="image-20220628211351678" style="float:left;" />

### 基本语法

EXPLAIN 或 DESCRIBE语句的语法形式如下：

```mysql
EXPLAIN SELECT select_options
或者
DESCRIBE SELECT select_options
```

如果我们想看看某个查询的执行计划的话，可以在具体的查询语句前边加一个 EXPLAIN ，就像这样：

```mysql
mysql> EXPLAIN SELECT 1;
```

<img src="D:\笔记\mysql.assets\image-20220628212029574.png" alt="image-20220628212029574" style="float:left;" />

EXPLAIN 语句输出的各个列的作用如下：

![image-20220628212049096](D:\笔记\mysql.assets\image-20220628212049096.png)

在这里把它们都列出来知识为了描述一个轮廓，让大家有一个大致的印象。

###  数据准备

**1. 建表**

```mysql
CREATE TABLE s1 (
    id INT AUTO_INCREMENT,
    key1 VARCHAR(100),
    key2 INT,
    key3 VARCHAR(100),
    key_part1 VARCHAR(100),
    key_part2 VARCHAR(100),
    key_part3 VARCHAR(100),
    common_field VARCHAR(100),
    PRIMARY KEY (id),
    INDEX idx_key1 (key1),
    UNIQUE INDEX idx_key2 (key2),
    INDEX idx_key3 (key3),
    INDEX idx_key_part(key_part1, key_part2, key_part3)
) ENGINE=INNODB CHARSET=utf8;
```

```mysql
CREATE TABLE s2 (
    id INT AUTO_INCREMENT,
    key1 VARCHAR(100),
    key2 INT,
    key3 VARCHAR(100),
    key_part1 VARCHAR(100),
    key_part2 VARCHAR(100),
    key_part3 VARCHAR(100),
    common_field VARCHAR(100),
    PRIMARY KEY (id),
    INDEX idx_key1 (key1),
    UNIQUE INDEX idx_key2 (key2),
    INDEX idx_key3 (key3),
    INDEX idx_key_part(key_part1, key_part2, key_part3)
) ENGINE=INNODB CHARSET=utf8;
```

**2. 设置参数 log_bin_trust_function_creators**

创建函数，假如报错，需开启如下命令：允许创建函数设置：

```mysql
set global log_bin_trust_function_creators=1; # 不加global只是当前窗口有效。
```

**3. 创建函数**

```mysql
DELIMITER //
CREATE FUNCTION rand_string1(n INT)
	RETURNS VARCHAR(255) #该函数会返回一个字符串
BEGIN
	DECLARE chars_str VARCHAR(100) DEFAULT
'abcdefghijklmnopqrstuvwxyzABCDEFJHIJKLMNOPQRSTUVWXYZ';
    DECLARE return_str VARCHAR(255) DEFAULT '';
    DECLARE i INT DEFAULT 0;
    WHILE i < n DO
        SET return_str =CONCAT(return_str,SUBSTRING(chars_str,FLOOR(1+RAND()*52),1));
        SET i = i + 1;
    END WHILE;
    RETURN return_str;
END //
DELIMITER ;
```

**4. 创建存储过程**

创建往s1表中插入数据的存储过程：

```mysql
DELIMITER //
CREATE PROCEDURE insert_s1 (IN min_num INT (10),IN max_num INT (10))
BEGIN
    DECLARE i INT DEFAULT 0;
    SET autocommit = 0;
    REPEAT
    SET i = i + 1;
    INSERT INTO s1 VALUES(
        (min_num + i),
        rand_string1(6),
        (min_num + 30 * i + 5),
        rand_string1(6),
        rand_string1(10),
        rand_string1(5),
        rand_string1(10),
        rand_string1(10));
    UNTIL i = max_num
    END REPEAT;
    COMMIT;
END //
DELIMITER ;
```

创建往s2表中插入数据的存储过程：

```mysql
DELIMITER //
CREATE PROCEDURE insert_s2 (IN min_num INT (10),IN max_num INT (10))
BEGIN
    DECLARE i INT DEFAULT 0;
    SET autocommit = 0;
    REPEAT
    SET i = i + 1;
    INSERT INTO s2 VALUES(
        (min_num + i),
        rand_string1(6),
        (min_num + 30 * i + 5),
        rand_string1(6),
        rand_string1(10),
        rand_string1(5),
        rand_string1(10),
        rand_string1(10));
    UNTIL i = max_num
    END REPEAT;
    COMMIT;
END //
DELIMITER ;
```

**5. 调用存储过程**

s1表数据的添加：加入1万条记录：

```mysql
CALL insert_s1(10001,10000);
```

s2表数据的添加：加入1万条记录：

```mysql
CALL insert_s2(10001,10000);
```

###  EXPLAIN各列作用

为了让大家有比较好的体验，我们调整了下 `EXPLAIN` 输出列的顺序。

#### table

不论我们的查询语句有多复杂，里边儿 包含了多少个表 ，到最后也是需要对每个表进行 单表访问 的，所 以MySQL规定EXPLAIN语句输出的每条记录都对应着某个单表的访问方法，该条记录的table列代表着该 表的表名（有时不是真实的表名字，可能是简称）。

```mysql
mysql > EXPLAIN SELECT * FROM s1;
```

![image-20220628221143339](D:\笔记\mysql.assets\image-20220628221143339.png)

这个查询语句只涉及对s1表的单表查询，所以 `EXPLAIN` 输出中只有一条记录，其中的table列的值为s1，表明这条记录是用来说明对s1表的单表访问方法的。

下边我们看一个连接查询的执行计划

```mysql
mysql > EXPLAIN SELECT * FROM s1 INNER JOIN s2;
```

![image-20220628221414097](D:\笔记\mysql.assets\image-20220628221414097.png)

可以看出这个连接查询的执行计划中有两条记录，这两条记录的table列分别是s1和s2，这两条记录用来分别说明对s1表和s2表的访问方法是什么。

####  id

我们写的查询语句一般都以 SELECT 关键字开头，比较简单的查询语句里只有一个 SELECT 关键字，比 如下边这个查询语句：

```mysql
SELECT * FROM s1 WHERE key1 = 'a';
```

稍微复杂一点的连接查询中也只有一个 SELECT 关键字，比如：

```mysql
SELECT * FROM s1 INNER JOIN s2
ON s1.key1 = s2.key1
WHERE s1.common_field = 'a';
```

但是下边两种情况下在一条查询语句中会出现多个SELECT关键字：

<img src="D:\笔记\mysql.assets\image-20220628221948512.png" alt="image-20220628221948512" style="float:left;" />

```mysql
mysql > EXPLAIN SELECT * FROM s1 WHERE key1 = 'a';
```

![image-20220628222055716](D:\笔记\mysql.assets\image-20220628222055716.png)

对于连接查询来说，一个SELECT关键字后边的FROM字句中可以跟随多个表，所以在连接查询的执行计划中，每个表都会对应一条记录，但是这些记录的id值都是相同的，比如：

```mysql
mysql> EXPLAIN SELECT * FROM s1 INNER JOIN s2;
```

![image-20220628222251309](D:\笔记\mysql.assets\image-20220628222251309.png)

可以看到，上述连接查询中参与连接的s1和s2表分别对应一条记录，但是这两条记录对应的`id`都是1。这里需要大家记住的是，**在连接查询的执行计划中，每个表都会对应一条记录，这些记录的id列的值是相同的**，出现在前边的表表示`驱动表`，出现在后面的表表示`被驱动表`。所以从上边的EXPLAIN输出中我们可以看到，查询优化器准备让s1表作为驱动表，让s2表作为被驱动表来执行查询。

对于包含子查询的查询语句来说，就可能涉及多个`SELECT`关键字，所以在**包含子查询的查询语句的执行计划中，每个`SELECT`关键字都会对应一个唯一的id值，比如这样：

```mysql
mysql> EXPLAIN SELECT * FROM s1 WHERE key1 IN (SELECT key1 FROM s2) OR key3 = 'a';
```

![image-20220629165122837](D:\笔记\mysql.assets\image-20220629165122837.png)

<img src="D:\笔记\mysql.assets\image-20220629170848349.png" alt="image-20220629170848349" style="float:left;" />

```mysql
# 查询优化器可能对涉及子查询的查询语句进行重写，转变为多表查询的操作。  
mysql> EXPLAIN SELECT * FROM s1 WHERE key1 IN (SELECT key2 FROM s2 WHERE common_field = 'a');
```

![image-20220629165603072](D:\笔记\mysql.assets\image-20220629165603072.png)

可以看到，虽然我们的查询语句是一个子查询，但是执行计划中s1和s2表对应的记录的`id`值全部是1，这就表明`查询优化器将子查询转换为了连接查询`。

对于包含`UNION`子句的查询语句来说，每个`SELECT`关键字对应一个`id`值也是没错的，不过还是有点儿特别的东西，比方说下边的查询：

```mysql
# Union去重
mysql> EXPLAIN SELECT * FROM s1 UNION SELECT * FROM s2;
```

![image-20220629165909340](D:\笔记\mysql.assets\image-20220629165909340.png)

<img src="D:\笔记\mysql.assets\image-20220629171104375.png" alt="image-20220629171104375" style="float:left;" />

```mysql
mysql> EXPLAIN SELECT * FROM s1 UNION ALL SELECT * FROM s2;
```

![image-20220629171138065](D:\笔记\mysql.assets\image-20220629171138065.png)

**小结:**

* id如果相同，可以认为是一组，从上往下顺序执行 
* 在所有组中，id值越大，优先级越高，越先执行 
* 关注点：id号每个号码，表示一趟独立的查询, 一个sql的查询趟数越少越好

#### select_type

<img src="D:\笔记\mysql.assets\image-20220629171611716.png" alt="image-20220629171611716" style="float:left;" />

![image-20220629171442624](D:\笔记\mysql.assets\image-20220629171442624.png)

具体分析如下：

* SIMPLE

	查询语句中不包含`UNION`或者子查询的查询都算作是`SIMPLE`类型，比方说下边这个单表查询`select_type`的值就是`SIMPLE`:

	```mysql
	mysql> EXPLAIN SELECT * FROM s1;
	```

![image-20220629171840300](D:\笔记\mysql.assets\image-20220629171840300.png)

​        当然，连接查询也算是 SIMPLE 类型，比如：

```mysql
mysql> EXPLAIN SELECT * FROM s1 INNER JOIN s2;
```

![image-20220629171904912](D:\笔记\mysql.assets\image-20220629171904912.png)

* PRIMARY

	对于包含`UNION、UNION ALL`或者子查询的大查询来说，它是由几个小查询组成的，其中最左边的那个查询的`select_type`的值就是`PRIMARY`,比方说：

	```mysql
	mysql> EXPLAIN SELECT * FROM s1 UNION SELECT * FROM s2;
	```

	![image-20220629171929924](D:\笔记\mysql.assets\image-20220629171929924.png)

	从结果中可以看到，最左边的小查询`SELECT * FROM s1`对应的是执行计划中的第一条记录，它的`select_type`的值就是`PRIMARY`。

* UNION

	对于包含`UNION`或者`UNION ALL`的大查询来说，它是由几个小查询组成的，其中除了最左边的那个小查询意外，其余的小查询的`select_type`值就是UNION，可以对比上一个例子的效果。

* UNION RESULT

	MySQL 选择使用临时表来完成`UNION`查询的去重工作，针对该临时表的查询的`select_type`就是`UNION RESULT`, 例子上边有。

* SUBQUERY

	如果包含子查询的查询语句不能够转为对应的`semi-join`的形式，并且该子查询是不相关子查询，并且查询优化器决定采用将该子查询物化的方案来执行该子查询时，该子查询的第一个`SELECT`关键字代表的那个查询的`select_type`就是`SUBQUERY`，比如下边这个查询：

	```mysql
	mysql> EXPLAIN SELECT * FROM s1 WHERE key1 IN (SELECT key1 FROM s2) OR key3 = 'a';
	```

	![image-20220629172449267](D:\笔记\mysql.assets\image-20220629172449267.png)

* DEPENDENT SUBQUERY

	```mysql
	mysql> EXPLAIN SELECT * FROM s1 WHERE key1 IN (SELECT key1 FROM s2 WHERE s1.key2 = s2.key2) OR key3 = 'a';
	```

	![image-20220629172525236](D:\笔记\mysql.assets\image-20220629172525236.png)

* DEPENDENT UNION

	```mysql
	mysql> EXPLAIN SELECT * FROM s1 WHERE key1 IN (SELECT key1 FROM s2 WHERE key1 = 'a' UNION SELECT key1 FROM s1 WHERE key1 = 'b');
	```

	![image-20220629172555603](D:\笔记\mysql.assets\image-20220629172555603.png)

* DERIVED

	```mysql
	mysql> EXPLAIN SELECT * FROM (SELECT key1, count(*) as c FROM s1 GROUP BY key1) AS derived_s1 where c > 1;
	```

	![image-20220629172622893](D:\笔记\mysql.assets\image-20220629172622893.png)

	从执行计划中可以看出，id为2的记录就代表子查询的执行方式，它的select_type是DERIVED, 说明该子查询是以物化的方式执行的。id为1的记录代表外层查询，大家注意看它的table列显示的是derived2，表示该查询时针对将派生表物化之后的表进行查询的。

* MATERIALIZED

	当查询优化器在执行包含子查询的语句时，选择将子查询物化之后的外层查询进行连接查询时，该子查询对应的`select_type`属性就是DERIVED，比如下边这个查询：

	```mysql
	mysql> EXPLAIN SELECT * FROM s1 WHERE key1 IN (SELECT key1 FROM s2);
	```

	![image-20220629172646367](D:\笔记\mysql.assets\image-20220629172646367.png)

* UNCACHEABLE SUBQUERY

	不常用，就不多说了。

* UNCACHEABLE UNION

	不常用，就不多说了。

####   partitions (可略)

* 代表分区表中的命中情况，非分区表，该项为`NULL`。一般情况下我们的额查询语句的执行计划的`partitions`列的值为`NULL`。
* <a>https://dev.mysql.com/doc/refman/5.7/en/alter-table-partition-operations.html</a>
* 如果想详细了解，可以如下方式测试。创建分区表：

```mysql
-- 创建分区表，
-- 按照id分区，id<100 p0分区，其他p1分区
CREATE TABLE user_partitions (id INT auto_increment,
NAME VARCHAR(12),PRIMARY KEY(id))
PARTITION BY RANGE(id)(
PARTITION p0 VALUES less than(100),
PARTITION p1 VALUES less than MAXVALUE
);
```

<img src="D:\笔记\mysql.assets\image-20220629190304966.png" alt="image-20220629190304966" style="float:left;" />

```mysql
DESC SELECT * FROM user_partitions WHERE id>200;
```

查询id大于200（200>100，p1分区）的记录，查看执行计划，partitions是p1，符合我们的分区规则

<img src="D:\笔记\mysql.assets\image-20220629190335371.png" alt="image-20220629190335371" style="float:left;" />

####  type ☆

执行计划的一条记录就代表着MySQL对某个表的 `执行查询时的访问方法` , 又称“访问类型”，其中的 `type` 列就表明了这个访问方法是啥，是较为重要的一个指标。比如，看到`type`列的值是`ref`，表明`MySQL`即将使用`ref`访问方法来执行对`s1`表的查询。

完整的访问方法如下： `system ， const ， eq_ref ， ref ， fulltext ， ref_or_null ， index_merge ， unique_subquery ， index_subquery ， range ， index ， ALL` 。

我们详细解释一下：

* `system`

	当表中`只有一条记录`并且该表使用的存储引擎的统计数据是精确的，比如MyISAM、Memory，那么对该表的访问方法就是`system`。比方说我们新建一个`MyISAM`表，并为其插入一条记录：

	```mysql
	mysql> CREATE TABLE t(i int) Engine=MyISAM;
	Query OK, 0 rows affected (0.05 sec)
	
	mysql> INSERT INTO t VALUES(1);
	Query OK, 1 row affected (0.01 sec)
	```

	然后我们看一下查询这个表的执行计划：

	```mysql
	mysql> EXPLAIN SELECT * FROM t;
	```

	<img src="D:\笔记\mysql.assets\image-20220630164434315.png" alt="image-20220630164434315" style="float:left;" />

	可以看到`type`列的值就是`system`了，

	> 测试，可以把表改成使用InnoDB存储引擎，试试看执行计划的`type`列是什么。ALL

* `const`

	当我们根据主键或者唯一二级索引列与常数进行等值匹配时，对单表的访问方法就是`const`, 比如：

	```mysql
	mysql> EXPLAIN SELECT * FROM s1 WHERE id = 10005;
	```

	<img src="D:\笔记\mysql.assets\image-20220630164724548.png" alt="image-20220630164724548" style="float:left;" />

* `eq_ref`

	在连接查询时，如果被驱动表是通过主键或者唯一二级索引列等值匹配的方式进行访问的（如果该主键或者唯一二级索引是联合索引的话，所有的索引列都必须进行等值比较）。则对该被驱动表的访问方法就是`eq_ref`，比方说：

	```mysql
	mysql> EXPLAIN SELECT * FROM s1 INNER JOIN s2 ON s1.id = s2.id;
	```

	<img src="D:\笔记\mysql.assets\image-20220630164802559.png" alt="image-20220630164802559" style="float:left;" />

	从执行计划的结果中可以看出，MySQL打算将s2作为驱动表，s1作为被驱动表，重点关注s1的访问 方法是 `eq_ref` ，表明在访问s1表的时候可以 `通过主键的等值匹配` 来进行访问。

* `ref`

	当通过普通的二级索引列与常量进行等值匹配时来查询某个表，那么对该表的访问方法就可能是`ref`，比方说下边这个查询：

	```mysql
	mysql> EXPLAIN SELECT * FROM s1 WHERE key1 = 'a';
	```

	<img src="D:\笔记\mysql.assets\image-20220630164930020.png" alt="image-20220630164930020" style="float:left;" />

* `fulltext`

	全文索引

* `ref_or_null`

	当对普通二级索引进行等值匹配查询，该索引列的值也可以是`NULL`值时，那么对该表的访问方法就可能是`ref_or_null`，比如说：

	```mysql
	mysql> EXPLAIN SELECT * FROM s1 WHERE key1 = 'a' OR key1 IS NULL;
	```

	<img src="D:\笔记\mysql.assets\image-20220630175133920.png" alt="image-20220630175133920" style="float:left;" />

* `index_merge`

	一般情况下对于某个表的查询只能使用到一个索引，但单表访问方法时在某些场景下可以使用`Interseation、union、Sort-Union`这三种索引合并的方式来执行查询。我们看一下执行计划中是怎么体现MySQL使用索引合并的方式来对某个表执行查询的：

	```mysql
	mysql> EXPLAIN SELECT * FROM s1 WHERE key1 = 'a' OR key3 = 'a';
	```

	<img src="D:\笔记\mysql.assets\image-20220630175511644.png" alt="image-20220630175511644" style="float:left;" />

	从执行计划的 `type` 列的值是 `index_merge` 就可以看出，MySQL 打算使用索引合并的方式来执行 对 s1 表的查询。

* `unique_subquery`

	类似于两表连接中被驱动表的`eq_ref`访问方法，`unique_subquery`是针对在一些包含`IN`子查询的查询语句中，如果查询优化器决定将`IN`子查询转换为`EXISTS`子查询，而且子查询可以使用到主键进行等值匹配的话，那么该子查询执行计划的`type`列的值就是`unique_subquery`，比如下边的这个查询语句：

	```mysql
	mysql> EXPLAIN SELECT * FROM s1 WHERE key2 IN (SELECT id FROM s2 where s1.key1 = s2.key1) OR key3 = 'a';
	```

	<img src="D:\笔记\mysql.assets\image-20220630180123913.png" alt="image-20220630180123913" style="float:left;" />

+ `index_subquery`

	`index_subquery` 与 `unique_subquery` 类似，只不过访问子查询中的表时使用的是普通的索引，比如这样：

	```mysql
	mysql> EXPLAIN SELECT * FROM s1 WHERE common_field IN (SELECT key3 FROM s2 where s1.key1 = s2.key1) OR key3 = 'a';
	```

![image-20220703214407225](D:\笔记\mysql.assets\image-20220703214407225.png)

* `range`

	```mysql
	mysql> EXPLAIN SELECT * FROM s1 WHERE key1 IN ('a', 'b', 'c');
	```

	![image-20220703214633338](D:\笔记\mysql.assets\image-20220703214633338.png)

	或者：

	```mysql
	mysql> EXPLAIN SELECT * FROM s1 WHERE key1 > 'a' AND key1 < 'b';
	```

	![image-20220703214657251](D:\笔记\mysql.assets\image-20220703214657251.png)

* `index`

	当我们可以使用索引覆盖，但需要扫描全部的索引记录时，该表的访问方法就是`index`，比如这样：

	```mysql
	mysql> EXPLAIN SELECT key_part2 FROM s1 WHERE key_part3 = 'a';
	```

	![image-20220703214844885](D:\笔记\mysql.assets\image-20220703214844885.png)

	上述查询中的所有列表中只有key_part2 一个列，而且搜索条件中也只有 key_part3 一个列，这两个列又恰好包含在idx_key_part这个索引中，可是搜索条件key_part3不能直接使用该索引进行`ref`和`range`方式的访问，只能扫描整个`idx_key_part`索引的记录，所以查询计划的`type`列的值就是`index`。

	> 再一次强调，对于使用InnoDB存储引擎的表来说，二级索引的记录只包含索引列和主键列的值，而聚簇索引中包含用户定义的全部列以及一些隐藏列，所以扫描二级索引的代价比直接全表扫描，也就是扫描聚簇索引的代价更低一些。

* `ALL`

	最熟悉的全表扫描，就不多说了，直接看例子：

	```mysql
	mysql> EXPLAIN SELECT * FROM s1;
	```

	![image-20220703215958374](D:\笔记\mysql.assets\image-20220703215958374.png)

**小结: **

**结果值从最好到最坏依次是： **

**system > const > eq_ref > ref** > fulltext > ref_or_null > index_merge > unique_subquery > index_subquery > range > index > ALL 

**其中比较重要的几个提取出来（见上图中的粗体）。SQL 性能优化的目标：至少要达到 range 级别，要求是 ref 级别，最好是 consts级别。（阿里巴巴 开发手册要求）**

####   possible_keys和key

在EXPLAIN语句输出的执行计划中，`possible_keys`列表示在某个查询语句中，对某个列执行`单表查询时可能用到的索引`有哪些。一般查询涉及到的字段上若存在索引，则该索引将被列出，但不一定被查询使用。`key`列表示`实际用到的索引`有哪些，如果为NULL，则没有使用索引。比方说下面这个查询：

```mysql
mysql> EXPLAIN SELECT * FROM s1 WHERE key1 > 'z' AND key3 = 'a';
```

![image-20220703220724964](D:\笔记\mysql.assets\image-20220703220724964.png)

上述执行计划的`possible_keys`列的值是`idx_key1, idx_key3`，表示该查询可能使用到`idx_key1, idx_key3`两个索引，然后`key`列的值是`idx_key3`，表示经过查询优化器计算使用不同索引的成本后，最后决定采用`idx_key3`。

#### 7. key_len ☆

实际使用到的索引长度 (即：字节数)

帮你检查`是否充分的利用了索引`，`值越大越好`，主要针对于联合索引，有一定的参考意义。

```mysql
mysql> EXPLAIN SELECT * FROM s1 WHERE id = 10005;
```

![image-20220704130030692](D:\笔记\mysql.assets\image-20220704130030692.png)

> int 占用 4 个字节

```mysql
mysql> EXPLAIN SELECT * FROM s1 WHERE key2 = 10126;
```

![image-20220704130138204](D:\笔记\mysql.assets\image-20220704130138204.png)

> key2上有一个唯一性约束，是否为NULL占用一个字节，那么就是5个字节

```mysql
mysql> EXPLAIN SELECT * FROM s1 WHERE key1 = 'a';
```

![image-20220704130214482](D:\笔记\mysql.assets\image-20220704130214482.png)

> key1 VARCHAR(100) 一个字符占3个字节，100*3，是否为NULL占用一个字节，varchar的长度信息占两个字节。

```mysql
mysql> EXPLAIN SELECT * FROM s1 WHERE key_part1 = 'a';
```

![image-20220704130442095](D:\笔记\mysql.assets\image-20220704130442095.png)

```mysql
mysql> EXPLAIN SELECT * FROM s1 WHERE key_part1 = 'a' AND key_part2 = 'b';
```

![image-20220704130515031](D:\笔记\mysql.assets\image-20220704130515031.png)

> 联合索引中可以比较，key_len=606的好于key_len=303

**练习： **

key_len的长度计算公式：

```mysql
varchar(10)变长字段且允许NULL = 10 * ( character set：utf8=3,gbk=2,latin1=1)+1(NULL)+2(变长字段)

varchar(10)变长字段且不允许NULL = 10 * ( character set：utf8=3,gbk=2,latin1=1)+2(变长字段)

char(10)固定字段且允许NULL = 10 * ( character set：utf8=3,gbk=2,latin1=1)+1(NULL)

char(10)固定字段且不允许NULL = 10 * ( character set：utf8=3,gbk=2,latin1=1)
```

####  ref

<img src="D:\笔记\mysql.assets\image-20220704131759630.png" alt="image-20220704131759630" style="float:left;" />

```mysql
mysql> EXPLAIN SELECT * FROM s1 WHERE key1 = 'a';
```

![image-20220704130837498](D:\笔记\mysql.assets\image-20220704130837498.png)

可以看到`ref`列的值是`const`，表明在使用`idx_key1`索引执行查询时，与`key1`列作等值匹配的对象是一个常数，当然有时候更复杂一点:

```mysql
mysql> EXPLAIN SELECT * FROM s1 INNER JOIN s2 ON s1.id = s2.id;
```

![image-20220704130925426](D:\笔记\mysql.assets\image-20220704130925426.png)

```mysql
mysql> EXPLAIN SELECT * FROM s1 INNER JOIN s2 ON s2.key1 = UPPER(s1.key1);
```

![image-20220704130957359](D:\笔记\mysql.assets\image-20220704130957359.png)

####  rows ☆

预估的需要读取 的记录条数，`值越小越好`。

```mysql
mysql> EXPLAIN SELECT * FROM s1 WHERE key1 > 'z';
```

![image-20220704131050496](D:\笔记\mysql.assets\image-20220704131050496.png)

####  filtered

某个表经过搜索条件过滤后剩余记录条数的百分比

如果使用的是索引执行的单表扫描，那么计算时需要估计出满足除使用到对应索引的搜索条件外的其他搜索条件的记录有多少条。

```mysql
mysql> EXPLAIN SELECT * FROM s1 WHERE key1 > 'z' AND common_field = 'a';
```

![image-20220704131323242](D:\笔记\mysql.assets\image-20220704131323242.png)

对于单表查询来说，这个filtered的值没有什么意义，我们`更关注在连接查询中驱动表对应的执行计划记录的filtered值`，它决定了被驱动表要执行的次数 (即: rows * filtered)

```mysql
mysql> EXPLAIN SELECT * FROM s1 INNER JOIN s2 ON s1.key1 = s2.key1 WHERE s1.common_field = 'a';
```

![image-20220704131644615](D:\笔记\mysql.assets\image-20220704131644615.png)

从执行计划中可以看出来，查询优化器打算把`s1`作为驱动表，`s2`当做被驱动表。我们可以看到驱动表`s1`表的执行计划的`rows`列为`9688`，filtered列为`10.00`，这意味着驱动表`s1`的扇出值就是`9688 x 10.00% = 968.8`，这说明还要对被驱动表执行大约`968`次查询。

####  Extra ☆

顾名思义，`Extra`列是用来说明一些额外信息的，包含不适合在其他列中显示但十分重要的额外信息。我们可以通过这些额外信息来`更准确的理解MySQL到底将如何执行给定的查询语句`。MySQL提供的额外信息有好几十个，我们就不一个一个介绍了，所以我们只挑选比较重要的额外信息介绍给大家。

* `No tables used`

	当查询语句没有`FROM`子句时将会提示该额外信息，比如：

	```mysql
	mysql> EXPLAIN SELECT 1;
	```

	![image-20220704132345383](D:\笔记\mysql.assets\image-20220704132345383.png)

* `Impossible WHERE`

	当查询语句的`WHERE`子句永远为`FALSE`时将会提示该额外信息

	```mysql
	mysql> EXPLAIN SELECT * FROM s1 WHERE 1 != 1;
	```

	![image-20220704132458978](D:\笔记\mysql.assets\image-20220704132458978.png)

* `Using where`

	<img src="D:\笔记\mysql.assets\image-20220704140148163.png" alt="image-20220704140148163" style="float:left;" />

	```mysql
	mysql> EXPLAIN SELECT * FROM s1 WHERE common_field = 'a';
	```

	![image-20220704132655342](D:\笔记\mysql.assets\image-20220704132655342.png)

	<img src="D:\笔记\mysql.assets\image-20220704140212813.png" alt="image-20220704140212813" style="float:left;" />

	```mysql
	mysql> EXPLAIN SELECT * FROM s1 WHERE key1 = 'a' AND common_field = 'a';
	```

	![image-20220704133130515](D:\笔记\mysql.assets\image-20220704133130515.png)

* `No matching min/max row`

	当查询列表处有`MIN`或者`MAX`聚合函数，但是并没有符合`WHERE`子句中的搜索条件的记录时。

	```mysql
	mysql> EXPLAIN SELECT MIN(key1) FROM s1 WHERE key1 = 'abcdefg';
	```

	![image-20220704134324354](D:\笔记\mysql.assets\image-20220704134324354.png)

* `Using index`

	当我们的查询列表以及搜索条件中只包含属于某个索引的列，也就是在可以使用覆盖索引的情况下，在`Extra`列将会提示该额外信息。比方说下边这个查询中只需要用到`idx_key1`而不需要回表操作:

	```mysql
	mysql> EXPLAIN SELECT key1 FROM s1 WHERE key1 = 'a';
	```

	![image-20220704134931220](D:\笔记\mysql.assets\image-20220704134931220.png)

* `Using index condition`

	有些搜索条件中虽然出现了索引列，但却不能使用到索引，比如下边这个查询：

	```mysql
	SELECT * FROM s1 WHERE key1 > 'z' AND key1 LIKE '%a';
	```

	<img src="D:\笔记\mysql.assets\image-20220704140344015.png" alt="image-20220704140344015" style="float:left;" />

	<img src="D:\笔记\mysql.assets\image-20220704140411033.png" alt="image-20220704140411033" style="float:left;" />

	```mysql
	mysql> EXPLAIN SELECT * FROM s1 WHERE key1 > 'z' AND key1 LIKE '%b';
	```

	![image-20220704140441702](D:\笔记\mysql.assets\image-20220704140441702.png)

* `Using join buffer (Block Nested Loop)`

	在连接查询执行过程中，当被驱动表不能有效的利用索引加快访问速度，MySQL一般会为其分配一块名叫`join buffer`的内存块来加快查询速度，也就是我们所讲的`基于块的嵌套循环算法`。

	```mysql
	mysql> EXPLAIN SELECT * FROM s1 INNER JOIN s2 ON s1.common_field = s2.common_field;
	```

	![image-20220704140815955](D:\笔记\mysql.assets\image-20220704140815955.png)

* `Not exists`

	当我们使用左(外)连接时，如果`WHERE`子句中包含要求被驱动表的某个列等于`NULL`值的搜索条件，而且那个列是不允许存储`NULL`值的，那么在该表的执行计划的Extra列就会提示这个信息：

	```mysql
	mysql> EXPLAIN SELECT * FROM s1 LEFT JOIN s2 ON s1.key1 = s2.key1 WHERE s2.id IS NULL;
	```

	![image-20220704142059555](D:\笔记\mysql.assets\image-20220704142059555.png)

* `Using intersect(...) 、 Using union(...) 和 Using sort_union(...)`

	如果执行计划的`Extra`列出现了`Using intersect(...)`提示，说明准备使用`Intersect`索引合并的方式执行查询，括号中的`...`表示需要进行索引合并的索引名称；

	如果出现`Using union(...)`提示，说明准备使用`Union`索引合并的方式执行查询;

	如果出现`Using sort_union(...)`提示，说明准备使用`Sort-Union`索引合并的方式执行查询。

	```mysql
	mysql> EXPLAIN SELECT * FROM s1 WHERE key1 = 'a' OR key3 = 'a';
	```

	![image-20220704142552890](D:\笔记\mysql.assets\image-20220704142552890.png)

* `Zero limit`

	当我们的`LIMIT`子句的参数为`0`时，表示压根儿不打算从表中读取任何记录，将会提示该额外信息

	```mysql
	mysql> EXPLAIN SELECT * FROM s1 LIMIT 0;
	```

	![image-20220704142754394](D:\笔记\mysql.assets\image-20220704142754394.png)

* `Using filesort`

	有一些情况下对结果集中的记录进行排序是可以使用到索引的。

	```mysql
	mysql> EXPLAIN SELECT * FROM s1 ORDER BY key1 LIMIT 10;
	```

	![image-20220704142901857](D:\笔记\mysql.assets\image-20220704142901857.png)

	<img src="D:\笔记\mysql.assets\image-20220704145143170.png" alt="image-20220704145143170" style="float:left;" />

	```mysql
	mysql> EXPLAIN SELECT * FROM s1 ORDER BY common_field LIMIT 10;
	```

	![image-20220704143518857](D:\笔记\mysql.assets\image-20220704143518857.png)

	需要注意的是，如果查询中需要使用`filesort`的方式进行排序的记录非常多，那么这个过程是很耗费性能的，我们最好想办法`将使用文件排序的执行方式改为索引进行排序`。

* `Using temporary`

	<img src="D:\笔记\mysql.assets\image-20220704145924130.png" alt="image-20220704145924130" style="float:left;" />

	```mysql
	mysql> EXPLAIN SELECT DISTINCT common_field FROM s1;
	```

	![image-20220704150030005](D:\笔记\mysql.assets\image-20220704150030005.png)

	再比如：

	```mysql
	mysql> EXPLAIN SELECT common_field, COUNT(*) AS amount FROM s1 GROUP BY common_field;
	```

	![image-20220704150156416](D:\笔记\mysql.assets\image-20220704150156416.png)

	执行计划中出现`Using temporary`并不是一个好的征兆，因为建立与维护临时表要付出很大的成本的，所以我们`最好能使用索引来替代掉使用临时表`，比方说下边这个包含`GROUP BY`子句的查询就不需要使用临时表：

	```mysql
	mysql> EXPLAIN SELECT key1, COUNT(*) AS amount FROM s1 GROUP BY key1;
	```

	![image-20220704150308189](D:\笔记\mysql.assets\image-20220704150308189.png)

	从 `Extra` 的 `Using index` 的提示里我们可以看出，上述查询只需要扫描 `idx_key1` 索引就可以搞 定了，不再需要临时表了。

* 其他

	其它特殊情况这里省略。

####  小结

* EXPLAIN不考虑各种Cache 
* EXPLAIN不能显示MySQL在执行查询时所作的优化工作 
* EXPLAIN不会告诉你关于触发器、存储过程的信息或用户自定义函数对查询的影响情况 
* 部分统计信息是估算的，并非精确值

##  EXPLAIN的进一步使用

### EXPLAIN四种输出格式

这里谈谈EXPLAIN的输出格式。EXPLAIN可以输出四种格式： `传统格式` ，`JSON格式` ， `TREE格式` 以及 `可视化输出` 。用户可以根据需要选择适用于自己的格式。

####  传统格式

传统格式简单明了，输出是一个表格形式，概要说明查询计划。

```mysql
mysql> EXPLAIN SELECT s1.key1, s2.key1 FROM s1 LEFT JOIN s2 ON s1.key1 = s2.key1 WHERE s2.common_field IS NOT NULL;
```

![image-20220704161702384](D:\笔记\mysql.assets\image-20220704161702384.png)

#### JSON格式

第1种格式中介绍的`EXPLAIN`语句输出中缺少了一个衡量执行好坏的重要属性 —— `成本`。而JSON格式是四种格式里面输出`信息最详尽`的格式，里面包含了执行的成本信息。

* JSON格式：在EXPLAIN单词和真正的查询语句中间加上 FORMAT=JSON 。

```mysql
EXPLAIN FORMAT=JSON SELECT ....
```

* EXPLAIN的Column与JSON的对应关系：(来源于MySQL 5.7文档)

![image-20220704164236909](D:\笔记\mysql.assets\image-20220704164236909.png)

这样我们就可以得到一个json格式的执行计划，里面包含该计划花费的成本。比如这样：

```mysql
mysql> EXPLAIN FORMAT=JSON SELECT * FROM s1 INNER JOIN s2 ON s1.key1 = s2.key2 WHERE s1.common_field = 'a'\G
```

![image-20220704172833362](D:\笔记\mysql.assets\image-20220704172833362.png)

![image-20220704172920158](D:\笔记\mysql.assets\image-20220704172920158.png)

![image-20220704173012413](D:\笔记\mysql.assets\image-20220704173012413.png)

![image-20220704173045190](D:\笔记\mysql.assets\image-20220704173045190.png)

![image-20220704173108888](D:\笔记\mysql.assets\image-20220704173108888.png)

我们使用 # 后边跟随注释的形式为大家解释了 `EXPLAIN FORMAT=JSON` 语句的输出内容，但是大家可能 有疑问 "`cost_info`" 里边的成本看着怪怪的，它们是怎么计算出来的？先看 s1 表的 "`cost_info`" 部 分：

```
"cost_info": {
    "read_cost": "1840.84",
    "eval_cost": "193.76",
    "prefix_cost": "2034.60",
    "data_read_per_join": "1M"
}
```

* `read_cost` 是由下边这两部分组成的：

	* IO 成本
	* 检测 rows × (1 - filter) 条记录的 CPU 成本

	> 小贴士： rows和filter都是我们前边介绍执行计划的输出列，在JSON格式的执行计划中，rows 相当于rows_examined_per_scan，filtered名称不变。

+ `eval_cost` 是这样计算的：

	检测 rows × filter 条记录的成本。

+ `prefix_cost` 就是单独查询 s1 表的成本，也就是：

	`read_cost + eval_cost`

+ `data_read_per_join` 表示在此次查询中需要读取的数据量。

对于 `s2` 表的 "`cost_info`" 部分是这样的：

```
"cost_info": {
    "read_cost": "968.80",
    "eval_cost": "193.76",
    "prefix_cost": "3197.16",
    "data_read_per_join": "1M"
}
```

由于 `s2` 表是被驱动表，所以可能被读取多次，这里的`read_cost` 和 `eval_cost` 是访问多次 `s2` 表后累加起来的值，大家主要关注里边儿的 `prefix_cost` 的值代表的是整个连接查询预计的成本，也就是单次查询 `s1` 表和多次查询 `s2` 表后的成本的和，也就是：

```
968.80 + 193.76 + 2034.60 = 3197.16
```

####  TREE格式

TREE格式是8.0.16版本之后引入的新格式，主要根据查询的 `各个部分之间的关系` 和 `各部分的执行顺序` 来描述如何查询。

```mysql
mysql> EXPLAIN FORMAT=tree SELECT * FROM s1 INNER JOIN s2 ON s1.key1 = s2.key2 WHERE
s1.common_field = 'a'\G
*************************** 1. row ***************************
EXPLAIN: -> Nested loop inner join (cost=1360.08 rows=990)
-> Filter: ((s1.common_field = 'a') and (s1.key1 is not null)) (cost=1013.75
rows=990)
-> Table scan on s1 (cost=1013.75 rows=9895)
-> Single-row index lookup on s2 using idx_key2 (key2=s1.key1), with index
condition: (cast(s1.key1 as double) = cast(s2.key2 as double)) (cost=0.25 rows=1)
1 row in set, 1 warning (0.00 sec)
```

#### 可视化输出

可视化输出，可以通过MySQL Workbench可视化查看MySQL的执行计划。通过点击Workbench的放大镜图标，即可生成可视化的查询计划。 

![image-20220704174401970](D:\笔记\mysql.assets\image-20220704174401970.png)

上图按从左到右的连接顺序显示表。红色框表示 `全表扫描` ，而绿色框表示使用 `索引查找` 。对于每个表， 显示使用的索引。还要注意的是，每个表格的框上方是每个表访问所发现的行数的估计值以及访问该表的成本。

### SHOW WARNINGS的使用

在我们使用`EXPLAIN`语句查看了某个查询的执行计划后，紧接着还可以使用`SHOW WARNINGS`语句查看与这个查询的执行计划有关的一些扩展信息，比如这样：

```mysql
mysql> EXPLAIN SELECT s1.key1, s2.key1 FROM s1 LEFT JOIN s2 ON s1.key1 = s2.key1 WHERE s2.common_field IS NOT NULL;
```

![image-20220704174543663](D:\笔记\mysql.assets\image-20220704174543663.png)

```mysql
mysql> SHOW WARNINGS\G
*************************** 1. row ***************************
    Level: Note
     Code: 1003
Message: /* select#1 */ select `atguigu`.`s1`.`key1` AS `key1`,`atguigu`.`s2`.`key1`
AS `key1` from `atguigu`.`s1` join `atguigu`.`s2` where ((`atguigu`.`s1`.`key1` =
`atguigu`.`s2`.`key1`) and (`atguigu`.`s2`.`common_field` is not null))
1 row in set (0.00 sec)
```

大家可以看到`SHOW WARNINGS`展示出来的信息有三个字段，分别是`Level、Code、Message`。我们最常见的就是Code为1003的信息，当Code值为1003时，`Message`字段展示的信息类似于查询优化器将我们的查询语句重写后的语句。比如我们上边的查询本来是一个左(外)连接查询，但是有一个s2.common_field IS NOT NULL的条件，这就会导致查询优化器把左(外)连接查询优化为内连接查询，从`SHOW WARNINGS`的`Message`字段也可以看出来，原本的LEFE JOIN已经变成了JOIN。

但是大家一定要注意，我们说`Message`字段展示的信息类似于查询优化器将我们的查询语句`重写后的语句`，并不是等价于，也就是说`Message`字段展示的信息并不是标准的查询语句，在很多情况下并不能直接拿到黑框框中运行，它只能作为帮助我们理解MySQL将如何执行查询语句的一个参考依据而已。

## 分析优化器执行计划：trace

<img src="D:\笔记\mysql.assets\image-20220704175711800.png" alt="image-20220704175711800" style="float:left;" />

```mysql
SET optimizer_trace="enabled=on",end_markers_in_json=on;
set optimizer_trace_max_mem_size=1000000;
```

开启后，可分析如下语句： 

* SELECT 
* INSERT 
* REPLACE
* UPDATE 
* DELETE 
* EXPLAIN 
* SET 
* DECLARE 
* CASE 
* IF 
* RETURN 
* CALL

测试：执行如下SQL语句

```mysql
select * from student where id < 10;
```

最后， 查询 information_schema.optimizer_trace 就可以知道MySQL是如何执行SQL的 ：

```mysql
select * from information_schema.optimizer_trace\G
```

```mysql
*************************** 1. row ***************************
//第1部分：查询语句
QUERY: select * from student where id < 10
//第2部分：QUERY字段对应语句的跟踪信息
TRACE: {
"steps": [
{
    "join_preparation": { //预备工作
        "select#": 1,
        "steps": [
            {
            "expanded_query": "/* select#1 */ select `student`.`id` AS
            `id`,`student`.`stuno` AS `stuno`,`student`.`name` AS `name`,`student`.`age` AS
            `age`,`student`.`classId` AS `classId` from `student` where (`student`.`id` < 10)"
            }
        ] /* steps */
    } /* join_preparation */
},
{
    "join_optimization": { //进行优化
    "select#": 1,
    "steps": [
        {
        "condition_processing": { //条件处理
        "condition": "WHERE",
        "original_condition": "(`student`.`id` < 10)",
        "steps": [
        {
            "transformation": "equality_propagation",
            "resulting_condition": "(`student`.`id` < 10)"
        },
        {
            "transformation": "constant_propagation",
            "resulting_condition": "(`student`.`id` < 10)"
        },
        {
            "transformation": "trivial_condition_removal",
            "resulting_condition": "(`student`.`id` < 10)"
        }
        ] /* steps */
    } /* condition_processing */
    },
    {
        "substitute_generated_columns": { //替换生成的列
        } /* substitute_generated_columns */
    },
    {
        "table_dependencies": [ //表的依赖关系
        {
            "table": "`student`",
            "row_may_be_null": false,
            "map_bit": 0,
            "depends_on_map_bits": [
            ] /* depends_on_map_bits */
        }
    ] /* table_dependencies */
    },
    {
    "ref_optimizer_key_uses": [ //使用键
        ] /* ref_optimizer_key_uses */
        },
    {
        "rows_estimation": [ //行判断
        {
            "table": "`student`",
            "range_analysis": {
                "table_scan": {
                    "rows": 3973767,
                    "cost": 408558
            } /* table_scan */, //扫描表
            "potential_range_indexes": [ //潜在的范围索引
                {
                    "index": "PRIMARY",
                    "usable": true,
                    "key_parts": [
                    "id"
                    ] /* key_parts */
                }
            ] /* potential_range_indexes */,
        "setup_range_conditions": [ //设置范围条件
        ] /* setup_range_conditions */,
        "group_index_range": {
            "chosen": false,
            "cause": "not_group_by_or_distinct"
        } /* group_index_range */,
            "skip_scan_range": {
                "potential_skip_scan_indexes": [
                    {
                        "index": "PRIMARY",
                        "usable": false,
                        "cause": "query_references_nonkey_column"
                    }
                ] /* potential_skip_scan_indexes */
            } /* skip_scan_range */,
        "analyzing_range_alternatives": { //分析范围选项
            "range_scan_alternatives": [
                {
                "index": "PRIMARY",
                    "ranges": [
                        "id < 10"
                    ] /* ranges */,
                "index_dives_for_eq_ranges": true,
                "rowid_ordered": true,
                "using_mrr": false,
                "index_only": false,
                "rows": 9,
                "cost": 1.91986,
                "chosen": true
                }
            ] /* range_scan_alternatives */,
        "analyzing_roworder_intersect": {
            "usable": false,
            "cause": "too_few_roworder_scans"
        	} /* analyzing_roworder_intersect */
        } /* analyzing_range_alternatives */,
        "chosen_range_access_summary": { //选择范围访问摘要
            "range_access_plan": {
                "type": "range_scan",
                "index": "PRIMARY",
                "rows": 9,
                "ranges": [
                "id < 10"
                ] /* ranges */
                } /* range_access_plan */,
                "rows_for_plan": 9,
                "cost_for_plan": 1.91986,
                "chosen": true
                } /* chosen_range_access_summary */
                } /* range_analysis */
            }
        ] /* rows_estimation */
    },
    {
    "considered_execution_plans": [ //考虑执行计划
    {
    "plan_prefix": [
    ] /* plan_prefix */,
        "table": "`student`",
        "best_access_path": { //最佳访问路径
        "considered_access_paths": [
        {
            "rows_to_scan": 9,
            "access_type": "range",
            "range_details": {
            "used_index": "PRIMARY"
        } /* range_details */,
        "resulting_rows": 9,
        "cost": 2.81986,
        "chosen": true
    }
    ] /* considered_access_paths */
    } /* best_access_path */,
        "condition_filtering_pct": 100, //行过滤百分比
        "rows_for_plan": 9,
        "cost_for_plan": 2.81986,
        "chosen": true
    }
    ] /* considered_execution_plans */
    },
    {
        "attaching_conditions_to_tables": { //将条件附加到表上
        "original_condition": "(`student`.`id` < 10)",
        "attached_conditions_computation": [
        ] /* attached_conditions_computation */,
        "attached_conditions_summary": [ //附加条件概要
    {
        "table": "`student`",
        "attached": "(`student`.`id` < 10)"
    }
    ] /* attached_conditions_summary */
    } /* attaching_conditions_to_tables */
    },
    {
    "finalizing_table_conditions": [
    {
        "table": "`student`",
        "original_table_condition": "(`student`.`id` < 10)",
        "final_table_condition ": "(`student`.`id` < 10)"
    }
    ] /* finalizing_table_conditions */
    },
    {
    "refine_plan": [ //精简计划
    {
    	"table": "`student`"
    }
    ] /* refine_plan */
    }
    ] /* steps */
    } /* join_optimization */
},
	{
        "join_execution": { //执行
            "select#": 1,
            "steps": [
            ] /* steps */
        	} /* join_execution */
        }
    ] /* steps */
}
//第3部分：跟踪信息过长时，被截断的跟踪信息的字节数。
MISSING_BYTES_BEYOND_MAX_MEM_SIZE: 0 //丢失的超出最大容量的字节
//第4部分：执行跟踪语句的用户是否有查看对象的权限。当不具有权限时，该列信息为1且TRACE字段为空，一般在
调用带有SQL SECURITY DEFINER的视图或者是存储过程的情况下，会出现此问题。
INSUFFICIENT_PRIVILEGES: 0 //缺失权限
1 row in set (0.00 sec)
```

## 9. MySQL监控分析视图-sys schema

<img src="D:\笔记\mysql.assets\image-20220704190726180.png" alt="image-20220704190726180" style="float:left;" />

### Sys schema视图摘要

1. **主机相关**：以host_summary开头，主要汇总了IO延迟的信息。 
2. **Innodb相关**：以innodb开头，汇总了innodb buffer信息和事务等待innodb锁的信息。 
3. **I/o相关**：以io开头，汇总了等待I/O、I/O使用量情况。 
4. **内存使用情况**：以memory开头，从主机、线程、事件等角度展示内存的使用情况 
5. **连接与会话信息**：processlist和session相关视图，总结了会话相关信息。 
6. **表相关**：以schema_table开头的视图，展示了表的统计信息。 
7. **索引信息**：统计了索引的使用情况，包含冗余索引和未使用的索引情况。 
8. **语句相关**：以statement开头，包含执行全表扫描、使用临时表、排序等的语句信息。 
9. **用户相关**：以user开头的视图，统计了用户使用的文件I/O、执行语句统计信息。 
10. **等待事件相关信息**：以wait开头，展示等待事件的延迟情况。

###  Sys schema视图使用场景

索引情况

```mysql
#1. 查询冗余索引
select * from sys.schema_redundant_indexes;
#2. 查询未使用过的索引
select * from sys.schema_unused_indexes;
#3. 查询索引的使用情况
select index_name,rows_selected,rows_inserted,rows_updated,rows_deleted
from sys.schema_index_statistics where table_schema='dbname';
```

表相关

```mysql
# 1. 查询表的访问量
select table_schema,table_name,sum(io_read_requests+io_write_requests) as io from
sys.schema_table_statistics group by table_schema,table_name order by io desc;
# 2. 查询占用bufferpool较多的表
select object_schema,object_name,allocated,data
from sys.innodb_buffer_stats_by_table order by allocated limit 10;
# 3. 查看表的全表扫描情况
select * from sys.statements_with_full_table_scans where db='dbname';
```

语句相关

```mysql
#1. 监控SQL执行的频率
select db,exec_count,query from sys.statement_analysis
order by exec_count desc;
#2. 监控使用了排序的SQL
select db,exec_count,first_seen,last_seen,query
from sys.statements_with_sorting limit 1;
#3. 监控使用了临时表或者磁盘临时表的SQL
select db,exec_count,tmp_tables,tmp_disk_tables,query
from sys.statement_analysis where tmp_tables>0 or tmp_disk_tables >0
order by (tmp_tables+tmp_disk_tables) desc;
```

IO相关

```mysql
#1. 查看消耗磁盘IO的文件
select file,avg_read,avg_write,avg_read+avg_write as avg_io
from sys.io_global_by_file_by_bytes order by avg_read limit 10;
```

Innodb 相关

```mysql
#1. 行锁阻塞情况
select * from sys.innodb_lock_waits;
```

<img src="D:\笔记\mysql.assets\image-20220704192020603.png" alt="image-20220704192020603" style="float:left;" />

# 索引优化与查询优化

都有哪些维度可以进行数据库调优？简言之：

* 索引失效、没有充分利用到索引——建立索引
* 关联查询太多JOIN（设计缺陷或不得已的需求）——SQL优化
* 服务器调优及各个参数设置（缓冲、线程数等）——调整my.cnf
* 数据过多——分库分表

关于数据库调优的知识非常分散。不同的DBMS，不同的公司，不同的职位，不同的项目遇到的问题都不尽相同。这里我们分为三个章节进行细致讲解。

虽然SQL查询优化的技术有很多，但是大方向上完全可以分成`物理查询优化`和`逻辑查询优化`两大块。

* 物理查询优化是通过`索引`和`表连接方式`等技术来进行优化，这里重点需要掌握索引的使用。
* 逻辑查询优化就是通过SQL`等价变换`提升查询效率，直白一点就是说，换一种查询写法效率可能更高。

## 数据准备

`学员表` 插 `50万` 条，` 班级表` 插 `1万` 条。

```mysql
CREATE DATABASE atguigudb2;
USE atguigudb2;
```

**步骤1：建表**

```mysql
CREATE TABLE `class` (
    `id` INT(11) NOT NULL AUTO_INCREMENT,
    `className` VARCHAR(30) DEFAULT NULL,
    `address` VARCHAR(40) DEFAULT NULL,
    `monitor` INT NULL ,
    PRIMARY KEY (`id`)
) ENGINE=INNODB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;

CREATE TABLE `student` (
    `id` INT(11) NOT NULL AUTO_INCREMENT,
    `stuno` INT NOT NULL ,
    `name` VARCHAR(20) DEFAULT NULL,
    `age` INT(3) DEFAULT NULL,
    `classId` INT(11) DEFAULT NULL,
    PRIMARY KEY (`id`)
    #CONSTRAINT `fk_class_id` FOREIGN KEY (`classId`) REFERENCES `t_class` (`id`)
) ENGINE=INNODB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;
```

**步骤2：设置参数**

* 命令开启：允许创建函数设置：

```mysql
set global log_bin_trust_function_creators=1; # 不加global只是当前窗口有效。
```

**步骤3：创建函数**

保证每条数据都不同。

```mysql
#随机产生字符串
DELIMITER //
CREATE FUNCTION rand_string(n INT) RETURNS VARCHAR(255)
BEGIN
DECLARE chars_str VARCHAR(100) DEFAULT
'abcdefghijklmnopqrstuvwxyzABCDEFJHIJKLMNOPQRSTUVWXYZ';
DECLARE return_str VARCHAR(255) DEFAULT '';
DECLARE i INT DEFAULT 0;
WHILE i < n DO
SET return_str =CONCAT(return_str,SUBSTRING(chars_str,FLOOR(1+RAND()*52),1));
SET i = i + 1;
END WHILE;
RETURN return_str;
END //
DELIMITER ;
#假如要删除
#drop function rand_string;
```

随机产生班级编号

```mysql
#用于随机产生多少到多少的编号
DELIMITER //
CREATE FUNCTION rand_num (from_num INT ,to_num INT) RETURNS INT(11)
BEGIN
DECLARE i INT DEFAULT 0;
SET i = FLOOR(from_num +RAND()*(to_num - from_num+1)) ;
RETURN i;
END //
DELIMITER ;
#假如要删除
#drop function rand_num;
```

**步骤4：创建存储过程**

```mysql
#创建往stu表中插入数据的存储过程
DELIMITER //
CREATE PROCEDURE insert_stu( START INT , max_num INT )
BEGIN
DECLARE i INT DEFAULT 0;
SET autocommit = 0; #设置手动提交事务
REPEAT #循环
SET i = i + 1; #赋值
INSERT INTO student (stuno, name ,age ,classId ) VALUES
((START+i),rand_string(6),rand_num(1,50),rand_num(1,1000));
UNTIL i = max_num
END REPEAT;
COMMIT; #提交事务
END //
DELIMITER ;
#假如要删除
#drop PROCEDURE insert_stu;
```

创建往class表中插入数据的存储过程

```mysql
#执行存储过程，往class表添加随机数据
DELIMITER //
CREATE PROCEDURE `insert_class`( max_num INT )
BEGIN
DECLARE i INT DEFAULT 0;
SET autocommit = 0;
REPEAT
SET i = i + 1;
INSERT INTO class ( classname,address,monitor ) VALUES
(rand_string(8),rand_string(10),rand_num(1,100000));
UNTIL i = max_num
END REPEAT;
COMMIT;
END //
DELIMITER ;
#假如要删除
#drop PROCEDURE insert_class;
```

**步骤5：调用存储过程**

class

```mysql
#执行存储过程，往class表添加1万条数据
CALL insert_class(10000);
```

stu

```mysql
#执行存储过程，往stu表添加50万条数据
CALL insert_stu(100000,500000);
```

**步骤6：删除某表上的索引**

创建存储过程

```mysql
DELIMITER //
CREATE PROCEDURE `proc_drop_index`(dbname VARCHAR(200),tablename VARCHAR(200))
BEGIN
        DECLARE done INT DEFAULT 0;
        DECLARE ct INT DEFAULT 0;
        DECLARE _index VARCHAR(200) DEFAULT '';
        DECLARE _cur CURSOR FOR SELECT index_name FROM
information_schema.STATISTICS WHERE table_schema=dbname AND table_name=tablename AND
seq_in_index=1 AND index_name <>'PRIMARY' ;
#每个游标必须使用不同的declare continue handler for not found set done=1来控制游标的结束
		DECLARE CONTINUE HANDLER FOR NOT FOUND set done=2 ;
#若没有数据返回,程序继续,并将变量done设为2
        OPEN _cur;
        FETCH _cur INTO _index;
        WHILE _index<>'' DO
            SET @str = CONCAT("drop index " , _index , " on " , tablename );
            PREPARE sql_str FROM @str ;
            EXECUTE sql_str;
            DEALLOCATE PREPARE sql_str;
            SET _index='';
            FETCH _cur INTO _index;
        END WHILE;
    CLOSE _cur;
END //
DELIMITER ;
```

执行存储过程

```mysql
CALL proc_drop_index("dbname","tablename");
```

##  索引失效案例

<img src="D:\笔记\mysql.assets\image-20220704202453482.png" alt="image-20220704202453482" style="float:left;" />

###  全值匹配我最爱

系统中经常出现的sql语句如  下：

```mysql
EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE age=30;
EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE age=30 AND classId=4;
EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE age=30 AND classId=4 AND name = 'abcd';
```

建立索引前执行：（关注执行时间）

```mysql
mysql> SELECT SQL_NO_CACHE * FROM student WHERE age=30 AND classId=4 AND name = 'abcd';
Empty set, 1 warning (0.28 sec)
```

**建立索引**

```mysql
CREATE INDEX idx_age ON student(age);
CREATE INDEX idx_age_classid ON student(age,classId);
CREATE INDEX idx_age_classid_name ON student(age,classId,name);
```

建立索引后执行：

```mysql
mysql> SELECT SQL_NO_CACHE * FROM student WHERE age=30 AND classId=4 AND name = 'abcd';
Empty set, 1 warning (0.01 sec)
```

<img src="D:\笔记\mysql.assets\image-20220704204140589.png" alt="image-20220704204140589" style="float:left;" />

###  最佳左前缀法则

在MySQL建立联合索引时会遵守最佳左前缀原则，即最左优先，在检索数据时从联合索引的最左边开始匹配。

举例1：

```mysql
EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE student.age=30 AND student.name = 'abcd';
```

举例2：

```mysql
EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE student.classId=1 AND student.name = 'abcd';
```

举例3：索引`idx_age_classid_name`还能否正常使用？

```mysql
EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE student.classId=4 AND student.age=30 AND student.name = 'abcd';
```

如果索引了多列，要遵守最左前缀法则。指的是查询从索引的最左前列开始并且不跳过索引中的列。

```mysql
mysql> EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE student.age=30 AND student.name = 'abcd';
```

![image-20220704211116351](D:\笔记\mysql.assets\image-20220704211116351.png)

虽然可以正常使用，但是只有部分被使用到了。

```mysql
mysql> EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE student.classId=1 AND student.name = 'abcd';
```

![image-20220704211254581](D:\笔记\mysql.assets\image-20220704211254581.png)

完全没有使用上索引。

结论：MySQL可以为多个字段创建索引，一个索引可以包含16个字段。对于多列索引，**过滤条件要使用索引必须按照索引建立时的顺序，依次满足，一旦跳过某个字段，索引后面的字段都无法被使用**。如果查询条件中没有用这些字段中第一个字段时，多列（或联合）索引不会被使用。

> 拓展：Alibaba《Java开发手册》 
>
> 索引文件具有 B-Tree 的最左前缀匹配特性，如果左边的值未确定，那么无法使用此索引。

###  主键插入顺序

<img src="D:\笔记\mysql.assets\image-20220704212354041.png" alt="image-20220704212354041" style="float:left;" />

如果此时再插入一条主键值为 9 的记录，那它插入的位置就如下图：

![image-20220704212428607](D:\笔记\mysql.assets\image-20220704212428607.png)

可这个数据页已经满了，再插进来咋办呢？我们需要把当前 `页面分裂` 成两个页面，把本页中的一些记录移动到新创建的这个页中。页面分裂和记录移位意味着什么？意味着： `性能损耗` ！所以如果我们想尽量避免这样无谓的性能损耗，最好让插入的记录的 `主键值依次递增` ，这样就不会发生这样的性能损耗了。 所以我们建议：让主键具有 `AUTO_INCREMENT` ，让存储引擎自己为表生成主键，而不是我们手动插入 ， 比如： `person_info` 表：

```mysql
CREATE TABLE person_info(
    id INT UNSIGNED NOT NULL AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    birthday DATE NOT NULL,
    phone_number CHAR(11) NOT NULL,
    country varchar(100) NOT NULL,
    PRIMARY KEY (id),
    KEY idx_name_birthday_phone_number (name(10), birthday, phone_number)
);
```

我们自定义的主键列 `id` 拥有 `AUTO_INCREMENT` 属性，在插入记录时存储引擎会自动为我们填入自增的主键值。这样的主键占用空间小，顺序写入，减少页分裂。

### 计算、函数、类型转换(自动或手动)导致索引失效

1. 这两条sql哪种写法更好

	```mysql
	EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE student.name LIKE 'abc%';
	```

	```mysql
	EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE LEFT(student.name,3) = 'abc';
	```

2. 创建索引

	```mysql
	CREATE INDEX idx_name ON student(NAME);
	```

3. 第一种：索引优化生效

	```mysql
	mysql> EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE student.name LIKE 'abc%';
	```

	```mysql
	mysql> SELECT SQL_NO_CACHE * FROM student WHERE student.name LIKE 'abc%';
	+---------+---------+--------+------+---------+
	| id | stuno | name | age | classId |
	+---------+---------+--------+------+---------+
	| 5301379 | 1233401 | AbCHEa | 164 | 259 |
	| 7170042 | 3102064 | ABcHeB | 199 | 161 |
	| 1901614 | 1833636 | ABcHeC | 226 | 275 |
	| 5195021 | 1127043 | abchEC | 486 | 72 |
	| 4047089 | 3810031 | AbCHFd | 268 | 210 |
	| 4917074 | 849096 | ABcHfD | 264 | 442 |
	| 1540859 | 141979 | abchFF | 119 | 140 |
	| 5121801 | 1053823 | AbCHFg | 412 | 327 |
	| 2441254 | 2373276 | abchFJ | 170 | 362 |
	| 7039146 | 2971168 | ABcHgI | 502 | 465 |
	| 1636826 | 1580286 | ABcHgK | 71 | 262 |
	| 374344 | 474345 | abchHL | 367 | 212 |
	| 1596534 | 169191 | AbCHHl | 102 | 146 |
	...
	| 5266837 | 1198859 | abclXe | 292 | 298 |
	| 8126968 | 4058990 | aBClxE | 316 | 150 |
	| 4298305 | 399962 | AbCLXF | 72 | 423 |
	| 5813628 | 1745650 | aBClxF | 356 | 323 |
	| 6980448 | 2912470 | AbCLXF | 107 | 78 |
	| 7881979 | 3814001 | AbCLXF | 89 | 497 |
	| 4955576 | 887598 | ABcLxg | 121 | 385 |
	| 3653460 | 3585482 | AbCLXJ | 130 | 174 |
	| 1231990 | 1283439 | AbCLYH | 189 | 429 |
	| 6110615 | 2042637 | ABcLyh | 157 | 40 |
	+---------+---------+--------+------+---------+
	401 rows in set, 1 warning (0.01 sec)
	```

4. 第二种：索引优化失效

	```mysql
	mysql> EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE LEFT(student.name,3) = 'abc';
	```

	![image-20220704214905412](D:\笔记\mysql.assets\image-20220704214905412.png)

	```mysql
	mysql> SELECT SQL_NO_CACHE * FROM student WHERE LEFT(student.name,3) = 'abc';
	+---------+---------+--------+------+---------+
	| id | stuno | name | age | classId |
	+---------+---------+--------+------+---------+
	| 5301379 | 1233401 | AbCHEa | 164 | 259 |
	| 7170042 | 3102064 | ABcHeB | 199 | 161 |
	| 1901614 | 1833636 | ABcHeC | 226 | 275 |
	| 5195021 | 1127043 | abchEC | 486 | 72 |
	| 4047089 | 3810031 | AbCHFd | 268 | 210 |
	| 4917074 | 849096 | ABcHfD | 264 | 442 |
	| 1540859 | 141979 | abchFF | 119 | 140 |
	| 5121801 | 1053823 | AbCHFg | 412 | 327 |
	| 2441254 | 2373276 | abchFJ | 170 | 362 |
	| 7039146 | 2971168 | ABcHgI | 502 | 465 |
	| 1636826 | 1580286 | ABcHgK | 71 | 262 |
	| 374344 | 474345 | abchHL | 367 | 212 |
	| 1596534 | 169191 | AbCHHl | 102 | 146 |
	...
	| 5266837 | 1198859 | abclXe | 292 | 298 |
	| 8126968 | 4058990 | aBClxE | 316 | 150 |
	| 4298305 | 399962 | AbCLXF | 72 | 423 |
	| 5813628 | 1745650 | aBClxF | 356 | 323 |
	| 6980448 | 2912470 | AbCLXF | 107 | 78 |
	| 7881979 | 3814001 | AbCLXF | 89 | 497 |
	| 4955576 | 887598 | ABcLxg | 121 | 385 |
	| 3653460 | 3585482 | AbCLXJ | 130 | 174 |
	| 1231990 | 1283439 | AbCLYH | 189 | 429 |
	| 6110615 | 2042637 | ABcLyh | 157 | 40 |
	+---------+---------+--------+------+---------+
	401 rows in set, 1 warning (3.62 sec)
	```

	type为“ALL”，表示没有使用到索引，查询时间为 3.62 秒，查询效率较之前低很多。

**再举例：**

* student表的字段stuno上设置有索引

	```mysql
	CREATE INDEX idx_sno ON student(stuno);
	```

* 索引优化失效：（假设：student表的字段stuno上设置有索引）

	```mysql
	EXPLAIN SELECT SQL_NO_CACHE id, stuno, NAME FROM student WHERE stuno+1 = 900001;
	```

运行结果：

![image-20220704215159768](D:\笔记\mysql.assets\image-20220704215159768.png)

* 索引优化生效：

	```mysql
	EXPLAIN SELECT SQL_NO_CACHE id, stuno, NAME FROM student WHERE stuno = 900000;
	```

**再举例：**

* student表的字段name上设置有索引

	```mysql
	CREATE INDEX idx_name ON student(NAME);
	```

	```mysql
	EXPLAIN SELECT id, stuno, name FROM student WHERE SUBSTRING(name, 1,3)='abc';
	```

	![image-20220704215533871](D:\笔记\mysql.assets\image-20220704215533871.png)

* 索引优化生效

	```mysql
	EXPLAIN SELECT id, stuno, NAME FROM student WHERE NAME LIKE 'abc%';
	```

	![image-20220704215600507](D:\笔记\mysql.assets\image-20220704215600507.png)

### 类型转换导致索引失效

下列哪个sql语句可以用到索引。（假设name字段上设置有索引）

```mysql
# 未使用到索引
EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE name=123;
```

![image-20220704215658526](D:\笔记\mysql.assets\image-20220704215658526.png)

```mysql
# 使用到索引
EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE name='123';
```

![image-20220704215721216](D:\笔记\mysql.assets\image-20220704215721216.png)

name=123发生类型转换，索引失效。

### 范围条件右边的列索引失效

1. 系统经常出现的sql如下：

```mysql
ALTER TABLE student DROP INDEX idx_name;
ALTER TABLE student DROP INDEX idx_age;
ALTER TABLE student DROP INDEX idx_age_classid;

EXPLAIN SELECT SQL_NO_CACHE * FROM student
WHERE student.age=30 AND student.classId>20 AND student.name = 'abc' ;
```

![image-20220704220123647](D:\笔记\mysql.assets\image-20220704220123647.png)

2. 那么索引 idx_age_classId_name 这个索引还能正常使用么？

* 不能，范围右边的列不能使用。比如：(<) (<=) (>) (>=) 和 between 等
* 如果这种sql出现较多，应该建立：

```mysql
create index idx_age_name_classId on student(age,name,classId);
```

* 将范围查询条件放置语句最后：

```mysql
EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE student.age=30 AND student.name = 'abc' AND student.classId>20;
```

> 应用开发中范围查询，例如：金额查询，日期查询往往都是范围查询。应将查询条件放置where语句最后。（创建的联合索引中，务必把范围涉及到的字段写在最后）

3. 效果

![image-20220704223211981](D:\笔记\mysql.assets\image-20220704223211981.png)

###  不等于(!= 或者<>)索引失效

* 为name字段创建索引

```mysql
CREATE INDEX idx_name ON student(NAME);
```

* 查看索引是否失效

```mysql
EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE student.name <> 'abc';
```

![image-20220704224552374](D:\笔记\mysql.assets\image-20220704224552374.png)

或者

```mysql
EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE student.name != 'abc';
```

![image-20220704224916117](D:\笔记\mysql.assets\image-20220704224916117.png)

场景举例：用户提出需求，将财务数据，产品利润金额不等于0的都统计出来。

###   is null可以使用索引，is not null无法使用索引

* IS NULL: 可以触发索引

```mysql
EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE age IS NULL;
```

* IS NOT NULL: 无法触发索引

```mysql
EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE age IS NOT NULL;
```

![image-20220704225333199](D:\笔记\mysql.assets\image-20220704225333199.png)

> 结论：最好在设计数据库的时候就将`字段设置为 NOT NULL 约束`，比如你可以将 INT 类型的字段，默认值设置为0。将字符类型的默认值 设置为空字符串('')。
>
> 扩展：同理，在查询中使用`not like`也无法使用索引，导致全表扫描。

###  like以通配符%开头索引失效

在使用LIKE关键字进行查询的查询语句中，如果匹配字符串的第一个字符为'%'，索引就不会起作用。只有'%'不在第一个位置，索引才会起作用。

* 使用到索引

```mysql
EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE name LIKE 'ab%';
```

![image-20220705131643304](D:\笔记\mysql.assets\image-20220705131643304.png)

* 未使用到索引

```mysql
EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE name LIKE '%ab%';
```

![image-20220705131717329](D:\笔记\mysql.assets\image-20220705131717329.png)

> 拓展：Alibaba《Java开发手册》 
>
> 【强制】页面搜索严禁左模糊或者全模糊，如果需要请走搜索引擎来解决。

###  OR 前后存在非索引的列，索引失效

在WHERE子句中，如果在OR前的条件列进行了索引，而在OR后的条件列没有进行索引，那么索引会失效。也就是说，**OR前后的两个条件中的列都是索引时，查询中才使用索引。**

因为OR的含义就是两个只要满足一个即可，因此`只有一个条件列进行了索引是没有意义的`，只要有条件列没有进行索引，就会进行`全表扫描`，因此所以的条件列也会失效。

查询语句使用OR关键字的情况：

```mysql
# 未使用到索引
EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE age = 10 OR classid = 100;
```

![image-20220705132221045](D:\笔记\mysql.assets\image-20220705132221045.png)

因为classId字段上没有索引，所以上述查询语句没有使用索引。

```mysql
#使用到索引
EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE age = 10 OR name = 'Abel';
```

![image-20220705132239232](D:\笔记\mysql.assets\image-20220705132239232.png)

因为age字段和name字段上都有索引，所以查询中使用了索引。你能看到这里使用到了`index_merge`，简单来说index_merge就是对age和name分别进行了扫描，然后将这两个结果集进行了合并。这样做的好处就是`避免了全表扫描`。

###  数据库和表的字符集统一使用utf8mb4

统一使用utf8mb4( 5.5.3版本以上支持)兼容性更好，统一字符集可以避免由于字符集转换产生的乱码。不 同的 `字符集` 进行比较前需要进行 `转换` 会造成索引失效。

###  练习及一般性建议

**练习：**假设：index(a,b,c)

![image-20220705145225852](D:\笔记\mysql.assets\image-20220705145225852.png)

**一般性建议**

* 对于单列索引，尽量选择针对当前query过滤性更好的索引
* 在选择组合索引的时候，当前query中过滤性最好的字段在索引字段顺序中，位置越靠前越好。
* 在选择组合索引的时候，尽量选择能够当前query中where子句中更多的索引。
* 在选择组合索引的时候，如果某个字段可能出现范围查询时，尽量把这个字段放在索引次序的最后面。

**总之，书写SQL语句时，尽量避免造成索引失效的情况**

##  关联查询优化

###  数据准备

```mysql
# 分类
CREATE TABLE IF NOT EXISTS `type` (
`id` INT(10) UNSIGNED NOT NULL AUTO_INCREMENT,
`card` INT(10) UNSIGNED NOT NULL,
PRIMARY KEY (`id`)
);
#图书
CREATE TABLE IF NOT EXISTS `book` (
`bookid` INT(10) UNSIGNED NOT NULL AUTO_INCREMENT,
`card` INT(10) UNSIGNED NOT NULL,
PRIMARY KEY (`bookid`)
);

#向分类表中添加20条记录
INSERT INTO `type`(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO `type`(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO `type`(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO `type`(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO `type`(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO `type`(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO `type`(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO `type`(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO `type`(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO `type`(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO `type`(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO `type`(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO `type`(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO `type`(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO `type`(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO `type`(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO `type`(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO `type`(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO `type`(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO `type`(card) VALUES(FLOOR(1 + (RAND() * 20)));

#向图书表中添加20条记录
INSERT INTO book(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO book(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO book(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO book(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO book(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO book(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO book(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO book(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO book(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO book(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO book(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO book(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO book(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO book(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO book(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO book(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO book(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO book(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO book(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO book(card) VALUES(FLOOR(1 + (RAND() * 20)));
```

###  采用左外连接

下面开始 EXPLAIN 分析

```mysql
EXPLAIN SELECT SQL_NO_CACHE * FROM `type` LEFT JOIN book ON type.card = book.card;
```

![image-20220705160504018](D:\笔记\mysql.assets\image-20220705160504018.png)

结论：type 有All

添加索引优化

```mysql
ALTER TABLE book ADD INDEX Y ( card); #【被驱动表】，可以避免全表扫描
EXPLAIN SELECT SQL_NO_CACHE * FROM `type` LEFT JOIN book ON type.card = book.card;
```

![image-20220705160935109](D:\笔记\mysql.assets\image-20220705160935109.png)

可以看到第二行的 type 变为了 ref，rows 也变成了优化比较明显。这是由左连接特性决定的。LEFT JOIN 条件用于确定如何从右表搜索行，左边一定都有，所以 `右边是我们的关键点,一定需要建立索引` 。

```mysql
ALTER TABLE `type` ADD INDEX X (card); #【驱动表】，无法避免全表扫描
EXPLAIN SELECT SQL_NO_CACHE * FROM `type` LEFT JOIN book ON type.card = book.card;
```

![image-20220705161243838](D:\笔记\mysql.assets\image-20220705161243838.png)

接着：

```mysql
DROP INDEX Y ON book;
EXPLAIN SELECT SQL_NO_CACHE * FROM `type` LEFT JOIN book ON type.card = book.card;
```

![image-20220705161515545](D:\笔记\mysql.assets\image-20220705161515545.png)

###  采用内连接

```mysql
drop index X on type;
drop index Y on book;（如果已经删除了可以不用再执行该操作）
```

换成 inner join（MySQL自动选择驱动表）

```mysql
EXPLAIN SELECT SQL_NO_CACHE * FROM type INNER JOIN book ON type.card=book.card;
```

![image-20220705161602362](D:\笔记\mysql.assets\image-20220705161602362.png)

添加索引优化

```mysql
ALTER TABLE book ADD INDEX Y (card);
EXPLAIN SELECT SQL_NO_CACHE * FROM type INNER JOIN book ON type.card=book.card;
```

![image-20220705161746184](D:\笔记\mysql.assets\image-20220705161746184.png)

```mysql
ALTER TABLE type ADD INDEX X (card);
EXPLAIN SELECT SQL_NO_CACHE * FROM type INNER JOIN book ON type.card=book.card;
```

![image-20220705161843558](D:\笔记\mysql.assets\image-20220705161843558.png)

对于内连接来说，查询优化器可以决定谁作为驱动表，谁作为被驱动表出现的

接着：

```mysql
DROP INDEX X ON `type`;
EXPLAIN SELECT SQL_NO_CACHE * FROM TYPE INNER JOIN book ON type.card=book.card;
```

![image-20220705161929544](D:\笔记\mysql.assets\image-20220705161929544.png)

接着：

```mysql
ALTER TABLE `type` ADD INDEX X (card);
EXPLAIN SELECT SQL_NO_CACHE * FROM `type` INNER JOIN book ON type.card=book.card;
```

![image-20220705162009145](D:\笔记\mysql.assets\image-20220705162009145.png)

接着：

```mysql
#向图书表中添加20条记录
INSERT INTO book(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO book(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO book(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO book(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO book(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO book(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO book(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO book(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO book(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO book(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO book(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO book(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO book(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO book(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO book(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO book(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO book(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO book(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO book(card) VALUES(FLOOR(1 + (RAND() * 20)));
INSERT INTO book(card) VALUES(FLOOR(1 + (RAND() * 20)));

ALTER TABLE book ADD INDEX Y (card);
EXPLAIN SELECT SQL_NO_CACHE * FROM `type` INNER JOIN book ON `type`.card = book.card;
```

![image-20220705163833445](D:\笔记\mysql.assets\image-20220705163833445.png)

图中发现，由于type表数据大于book表数据，MySQL选择将type作为被驱动表。

###  join语句原理

join方式连接多个表，本质就是各个表之间数据的循环匹配。MySQL5.5版本之前，MySQL只支持一种表间关联方式，就是嵌套循环(Nested Loop Join)。如果关联表的数据量很大，则join关联的执行时间会很长。在MySQL5.5以后的版本中，MySQL通过引入BNLJ算法来优化嵌套执行。

####  驱动表和被驱动表

驱动表就是主表，被驱动表就是从表、非驱动表。

* 对于内连接来说：

```mysql
SELECT * FROM A JOIN B ON ...
```

A一定是驱动表吗？不一定，优化器会根据你查询语句做优化，决定先查哪张表。先查询的那张表就是驱动表，反之就是被驱动表。通过explain关键字可以查看。

* 对于外连接来说：

```mysql
SELECT * FROM A LEFT JOIN B ON ...
# 或
SELECT * FROM B RIGHT JOIN A ON ... 
```

通常，大家会认为A就是驱动表，B就是被驱动表。但也未必。测试如下：

```mysql
CREATE TABLE a(f1 INT, f2 INT, INDEX(f1)) ENGINE=INNODB;
CREATE TABLE b(f1 INT, f2 INT) ENGINE=INNODB;

INSERT INTO a VALUES(1,1),(2,2),(3,3),(4,4),(5,5),(6,6);
INSERT INTO b VALUES(3,3),(4,4),(5,5),(6,6),(7,7),(8,8);

SELECT * FROM b;

# 测试1
EXPLAIN SELECT * FROM a LEFT JOIN b ON(a.f1=b.f1) WHERE (a.f2=b.f2);

# 测试2
EXPLAIN SELECT * FROM a LEFT JOIN b ON(a.f1=b.f1) AND (a.f2=b.f2);
```

####  Simple Nested-Loop Join (简单嵌套循环连接)

算法相当简单，从表A中取出一条数据1，遍历表B，将匹配到的数据放到result.. 以此类推，驱动表A中的每一条记录与被驱动表B的记录进行判断：

![image-20220705165559127](D:\笔记\mysql.assets\image-20220705165559127.png)

可以看到这种方式效率是非常低的，以上述表A数据100条，表B数据1000条计算，则A*B=10万次。开销统计如下:

![image-20220705165646252](D:\笔记\mysql.assets\image-20220705165646252.png)

当然mysql肯定不会这么粗暴的去进行表的连接，所以就出现了后面的两种对Nested-Loop Join优化算法。

####  Index Nested-Loop Join （索引嵌套循环连接）

Index Nested-Loop Join其优化的思路主要是为了`减少内存表数据的匹配次数`，所以要求被驱动表上必须`有索引`才行。通过外层表匹配条件直接与内层表索引进行匹配，避免和内存表的每条记录去进行比较，这样极大的减少了对内存表的匹配次数。

![image-20220705172315554](D:\笔记\mysql.assets\image-20220705172315554.png)

驱动表中的每条记录通过被驱动表的索引进行访问，因为索引查询的成本是比较固定的，故mysql优化器都倾向于使用记录数少的表作为驱动表（外表）。

![image-20220705172650749](D:\笔记\mysql.assets\image-20220705172650749.png)

如果被驱动表加索引，效率是非常高的，但如果索引不是主键索引，所以还得进行一次回表查询。相比，被驱动表的索引是主键索引，效率会更高。

####  Block Nested-Loop Join（块嵌套循环连接）

<img src="D:\笔记\mysql.assets\image-20220705173047234.png" alt="image-20220705173047234" style="float:left;" />

> 注意：
>
> 这里缓存的不只是关联表的列，select后面的列也会缓存起来。
>
> 在一个有N个join关联的sql中会分配N-1个join buffer。所以查询的时候尽量减少不必要的字段，可以让join buffer中可以存放更多的列。

![image-20220705174005280](D:\笔记\mysql.assets\image-20220705174005280.png)

![image-20220705174250551](D:\笔记\mysql.assets\image-20220705174250551.png)

参数设置：

* block_nested_loop

通过`show variables like '%optimizer_switch%` 查看 `block_nested_loop`状态。默认是开启的。

* join_buffer_size

驱动表能不能一次加载完，要看join buffer能不能存储所有的数据，默认情况下`join_buffer_size=256k`。

```mysql
mysql> show variables like '%join_buffer%';
```

join_buffer_size的最大值在32位操作系统可以申请4G，而在64位操作系统下可以申请大于4G的Join Buffer空间（64位Windows除外，其大值会被截断为4GB并发出警告）。

#### Join小结

1、**整体效率比较：INLJ > BNLJ > SNLJ**

2、永远用小结果集驱动大结果集（其本质就是减少外层循环的数据数量）（小的度量单位指的是表行数 * 每行大小）

```mysql
select t1.b,t2.* from t1 straight_join t2 on (t1.b=t2.b) where t2.id<=100; # 推荐
select t1.b,t2.* from t2 straight_join t1 on (t1.b=t2.b) where t2.id<=100; # 不推荐
```

3、为被驱动表匹配的条件增加索引(减少内存表的循环匹配次数)

4、增大join buffer size的大小（一次索引的数据越多，那么内层包的扫描次数就越少）

5、减少驱动表不必要的字段查询（字段越少，join buffer所缓存的数据就越多）

####  Hash Join

**从MySQL的8.0.20版本开始将废弃BNLJ，因为从MySQL8.0.18版本开始就加入了hash join默认都会使用hash join**

* Nested Loop:

	对于被连接的数据子集较小的情况，Nested Loop是个较好的选择。

* Hash Join是做`大数据集连接`时的常用方式，优化器使用两个表中较小（相对较小）的表利用Join Key在内存中建立`散列表`，然后扫描较大的表并探测散列表，找出与Hash表匹配的行。

	* 这种方式适合于较小的表完全可以放于内存中的情况，这样总成本就是访问两个表的成本之和。
	* 在表很大的情况下并不能完全放入内存，这时优化器会将它分割成`若干不同的分区`，不能放入内存的部分就把该分区写入磁盘的临时段，此时要求有较大的临时段从而尽量提高I/O的性能。
	* 它能够很好的工作于没有索引的大表和并行查询的环境中，并提供最好的性能。大多数人都说它是Join的重型升降机。Hash Join只能应用于等值连接（如WHERE A.COL1 = B.COL2），这是由Hash的特点决定的。

![image-20220705205050280](D:\笔记\mysql.assets\image-20220705205050280.png)

###  小结

* 保证被驱动表的JOIN字段已经创建了索引 
* 需要JOIN 的字段，数据类型保持绝对一致。 
* LEFT JOIN 时，选择小表作为驱动表， 大表作为被驱动表 。减少外层循环的次数。 
* INNER JOIN 时，MySQL会自动将 小结果集的表选为驱动表 。选择相信MySQL优化策略。 
* 能够直接多表关联的尽量直接关联，不用子查询。(减少查询的趟数) 
* 不建议使用子查询，建议将子查询SQL拆开结合程序多次查询，或使用 JOIN 来代替子查询。 
* 衍生表建不了索引

##  子查询优化

MySQL从4.1版本开始支持子查询，使用子查询可以进行SELECT语句的嵌套查询，即一个SELECT查询的结 果作为另一个SELECT语句的条件。 `子查询可以一次性完成很多逻辑上需要多个步骤才能完成的SQL操作` 。

**子查询是 MySQL 的一项重要的功能，可以帮助我们通过一个 SQL 语句实现比较复杂的查询。但是，子 查询的执行效率不高。**原因：

① 执行子查询时，MySQL需要为内层查询语句的查询结果 建立一个临时表 ，然后外层查询语句从临时表 中查询记录。查询完毕后，再 撤销这些临时表 。这样会消耗过多的CPU和IO资源，产生大量的慢查询。

② 子查询的结果集存储的临时表，不论是内存临时表还是磁盘临时表都 不会存在索引 ，所以查询性能会 受到一定的影响。

③ 对于返回结果集比较大的子查询，其对查询性能的影响也就越大。

**在MySQL中，可以使用连接（JOIN）查询来替代子查询。**连接查询 `不需要建立临时表` ，其 `速度比子查询` 要快 ，如果查询中使用索引的话，性能就会更好。

举例1：查询学生表中是班长的学生信息

* 使用子查询

```mysql
# 创建班级表中班长的索引
CREATE INDEX idx_monitor ON class(monitor);

EXPLAIN SELECT * FROM student stu1
WHERE stu1.`stuno` IN (
SELECT monitor
FROM class c
WHERE monitor IS NOT NULL
)
```

* 推荐使用多表查询

```mysql
EXPLAIN SELECT stu1.* FROM student stu1 JOIN class c
ON stu1.`stuno` = c.`monitor`
WHERE c.`monitor` is NOT NULL;
```

举例2：取所有不为班长的同学

* 不推荐

```mysql
EXPLAIN SELECT SQL_NO_CACHE a.*
FROM student a
WHERE a.stuno NOT IN (
	SELECT monitor FROM class b
    WHERE monitor IS NOT NULL
);
```

执行结果如下：

![image-20220705210708343](D:\笔记\mysql.assets\image-20220705210708343.png)

* 推荐：

```mysql
EXPLAIN SELECT SQL_NO_CACHE a.*
FROM student a LEFT OUTER JOIN class b
ON a.stuno = b.monitor
WHERE b.monitor IS NULL;
```

![image-20220705210839437](D:\笔记\mysql.assets\image-20220705210839437.png)

> 结论：尽量不要使用NOT IN或者NOT EXISTS，用LEFT JOIN xxx ON xx WHERE xx IS NULL替代

## 5. 排序优化

###  排序优化

**问题**：在 WHERE 条件字段上加索引，但是为什么在 ORDER BY 字段上还要加索引呢？

**回答：**

在MySQL中，支持两种排序方式，分别是 `FileSort` 和 `Index` 排序。

* Index 排序中，索引可以保证数据的有序性，不需要再进行排序，`效率更高`。
* FileSort 排序则一般在 `内存中` 进行排序，占用`CPU较多`。如果待排结果较大，会产生临时文件 I/O 到磁盘进行排序的情况，效率较低。

**优化建议：**

1. SQL 中，可以在 WHERE 子句和 ORDER BY 子句中使用索引，目的是在 WHERE 子句中 `避免全表扫描` ，在 ORDER BY 子句 `避免使用 FileSort 排序` 。当然，某些情况下全表扫描，或者 FileSort 排序不一定比索引慢。但总的来说，我们还是要避免，以提高查询效率。 
2. 尽量使用 Index 完成 ORDER BY 排序。如果 WHERE 和 ORDER BY 后面是相同的列就使用单索引列； 如果不同就使用联合索引。 
3. 无法使用 Index 时，需要对 FileSort 方式进行调优。

### 测试

删除student表和class表中已创建的索引。

```mysql
# 方式1
DROP INDEX idx_monitor ON class;
DROP INDEX idx_cid ON student;
DROP INDEX idx_age ON student;
DROP INDEX idx_name ON student;
DROP INDEX idx_age_name_classId ON student;
DROP INDEX idx_age_classId_name ON student;

# 方式2
call proc_drop_index('atguigudb2','student';)
```

以下是否能使用到索引，`能否去掉using filesort`

**过程一：**

![image-20220705215436102](D:\笔记\mysql.assets\image-20220705215436102.png)

**过程二： order by 时不limit,索引失效**

![image-20220705215909350](D:\笔记\mysql.assets\image-20220705215909350.png)

**过程三：order by 时顺序错误，索引失效**

<img src="D:\笔记\mysql.assets\image-20220705220033520.png" alt="image-20220705220033520" style="zoom:80%;float:left" />

**过程四：order by 时规则不一致，索引失效（顺序错，不索引；方向反，不索引）**

<img src="D:\笔记\mysql.assets\image-20220705220404802.png" alt="image-20220705220404802" style="zoom:80%;float:left" />

> 结论：ORDER BY 子句，尽量使用 Index 方式排序，避免使用 FileSort 方式排序

**过程五：无过滤，不索引**

<img src="D:\笔记\mysql.assets\image-20220705221212879.png" alt="image-20220705221212879" style="zoom:80%;float:left" />

**小结**

```mysql
INDEX a_b_c(a,b,c)
order by 能使用索引最左前缀
- ORDER BY a
- ORDER BY a,b
- ORDER BY a,b,c
- ORDER BY a DESC,b DESC,c DESC
如果WHERE使用索引的最左前缀定义为常量，则order by 能使用索引
- WHERE a = const ORDER BY b,c
- WHERE a = const AND b = const ORDER BY c
- WHERE a = const ORDER BY b,c
- WHERE a = const AND b > const ORDER BY b,c
不能使用索引进行排序
- ORDER BY a ASC,b DESC,c DESC /* 排序不一致 */
- WHERE g = const ORDER BY b,c /*丢失a索引*/
- WHERE a = const ORDER BY c /*丢失b索引*/
- WHERE a = const ORDER BY a,d /*d不是索引的一部分*/
- WHERE a in (...) ORDER BY b,c /*对于排序来说，多个相等条件也是范围查询*/
```

###  案例实战

ORDER BY子句，尽量使用Index方式排序，避免使用FileSort方式排序。 

执行案例前先清除student上的索引，只留主键：

```mysql
DROP INDEX idx_age ON student;
DROP INDEX idx_age_classid_stuno ON student;
DROP INDEX idx_age_classid_name ON student;

#或者
call proc_drop_index('atguigudb2','student');
```

**场景:查询年龄为30岁的，且学生编号小于101000的学生，按用户名称排序**

```mysql
EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE age = 30 AND stuno <101000 ORDER BY NAME ;
```

![image-20220705222027812](D:\笔记\mysql.assets\image-20220705222027812.png)

查询结果如下：

```mysql
mysql> SELECT SQL_NO_CACHE * FROM student WHERE age = 30 AND stuno <101000 ORDER BY NAME;
+---------+--------+--------+------+---------+
| id      | stuno  |  name  | age  | classId |
+---------+--------+--------+------+---------+
| 922     | 100923 | elTLXD | 30   | 249     |
| 3723263 | 100412 | hKcjLb | 30   | 59      |
| 3724152 | 100827 | iHLJmh | 30   | 387     |
| 3724030 | 100776 | LgxWoD | 30   | 253     |
| 30      | 100031 | LZMOIa | 30   | 97      |
| 3722887 | 100237 | QzbJdx | 30   | 440     |
| 609     | 100610 | vbRimN | 30   | 481     |
| 139     | 100140 | ZqFbuR | 30   | 351     |
+---------+--------+--------+------+---------+
8 rows in set, 1 warning (3.16 sec)
```

> 结论：type 是 ALL，即最坏的情况。Extra 里还出现了 Using filesort,也是最坏的情况。优化是必须的。

**方案一: 为了去掉filesort我们可以把索引建成**

```mysql
#创建新索引
CREATE INDEX idx_age_name ON student(age,NAME);
```

```mysql
EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE age = 30 AND stuno <101000 ORDER BY NAME;
```

![image-20220705222912521](D:\笔记\mysql.assets\image-20220705222912521.png)

这样我们优化掉了 using filesort

查询结果如下：

<img src="D:\笔记\mysql.assets\image-20220705222954971.png" alt="image-20220705222954971" style="float:left;" />

**方案二：尽量让where的过滤条件和排序使用上索引**

建一个三个字段的组合索引：

```mysql
DROP INDEX idx_age_name ON student;
CREATE INDEX idx_age_stuno_name ON student (age,stuno,NAME);
EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE age = 30 AND stuno <101000 ORDER BY NAME;
```

![image-20220705223111883](D:\笔记\mysql.assets\image-20220705223111883.png)

我们发现using filesort依然存在，所以name并没有用到索引，而且type还是range光看名字其实并不美好。原因是，因为`stuno是一个范围过滤`，所以索引后面的字段不会在使用索引了 。

结果如下：

```mysql
mysql> SELECT SQL_NO_CACHE * FROM student
-> WHERE age = 30 AND stuno <101000 ORDER BY NAME ;
+-----+--------+--------+------+---------+
| id | stuno | name | age | classId |
+-----+--------+--------+------+---------+
| 167 | 100168 | AClxEF | 30 | 319 |
| 323 | 100324 | bwbTpQ | 30 | 654 |
| 651 | 100652 | DRwIac | 30 | 997 |
| 517 | 100518 | HNSYqJ | 30 | 256 |
| 344 | 100345 | JuepiX | 30 | 329 |
| 905 | 100906 | JuWALd | 30 | 892 |
| 574 | 100575 | kbyqjX | 30 | 260 |
| 703 | 100704 | KJbprS | 30 | 594 |
| 723 | 100724 | OTdJkY | 30 | 236 |
| 656 | 100657 | Pfgqmj | 30 | 600 |
| 982 | 100983 | qywLqw | 30 | 837 |
| 468 | 100469 | sLEKQW | 30 | 346 |
| 988 | 100989 | UBYqJl | 30 | 457 |
| 173 | 100174 | UltkTN | 30 | 830 |
| 332 | 100333 | YjWiZw | 30 | 824 |
+-----+--------+--------+------+---------+
15 rows in set, 1 warning (0.00 sec)
```

结果竟然有 filesort的 sql 运行速度， 超过了已经优化掉 filesort的 sql ，而且快了很多，几乎一瞬间就出现了结果。

原因：

<img src="D:\笔记\mysql.assets\image-20220705223329164.png" alt="image-20220705223329164" style="zoom:80%;float:left" />

> 结论：
>
> 1. 两个索引同时存在，mysql自动选择最优的方案。（对于这个例子，mysql选择 idx_age_stuno_name）。但是， `随着数据量的变化，选择的索引也会随之变化的 `。 
> 2. **当【范围条件】和【group by 或者 order by】的字段出现二选一时，优先观察条件字段的过 滤数量，如果过滤的数据足够多，而需要排序的数据并不多时，优先把索引放在范围字段上。反之，亦然。**

思考：这里我们使用如下索引，是否可行？

```mysql
DROP INDEX idx_age_stuno_name ON student;

CREATE INDEX idx_age_stuno ON student(age,stuno);
```

当然可以。

###  filesort算法：双路排序和单路排序

排序的字段若不在索引列上，则filesort会有两种算法：双路排序和单路排序

**双路排序 （慢）**

* `MySQL 4.1之前是使用双路排序 `，字面意思就是两次扫描磁盘，最终得到数据， 读取行指针和` order by列` ，对他们进行排序，然后扫描已经排序好的列表，按照列表中的值重新从列表中读取对应的数据输出 
* 从磁盘取排序字段，在buffer进行排序，再从磁盘取其他字段 。

取一批数据，要对磁盘进行两次扫描，众所周知，IO是很耗时的，所以在mysql4.1之后，出现了第二种 改进的算法，就是单路排序。

**单路排序 （快）**

从磁盘读取查询需要的` 所有列` ，按照order by列在buffer对它们进行排序，然后扫描排序后的列表进行输出， 它的效率更快一些，避免了第二次读取数据。并且把随机IO变成了顺序IO，但是它会使用更多的空间， 因为它把每一行都保存在内存中了。

**结论及引申出的问题**

* 由于单路是后出的，总体而言好过双路 
* 但是用单路有问题
	* 在sort_buffer中，单路要比多路多占用很多空间，因为单路是把所有字段都取出，所以有可能取出的数据的总大小超出了`sort_buffer`的容量，导致每次只能取`sort_buffer`容量大小的数据，进行排序（创建tmp文件，多路合并），排完再取sort_buffer容量大小，再排...从而多次I/O。
	* 单路本来想省一次I/O操作，反而导致了大量的I/O操作，反而得不偿失。

**优化策略**

**1. 尝试提高 sort_buffer_size**

<img src="D:\笔记\mysql.assets\image-20220705224410340.png" alt="image-20220705224410340" style="zoom:80%;float:left" />

**2. 尝试提高 max_length_for_sort_data**

<img src="D:\笔记\mysql.assets\image-20220705224505668.png" alt="image-20220705224505668" style="zoom:80%;float:left" />

**3. Order by 时select * 是一个大忌。最好只Query需要的字段。**

<img src="D:\笔记\mysql.assets\image-20220705224551104.png" alt="image-20220705224551104" style="float:left;" />

##  GROUP BY优化

* group by 使用索引的原则几乎跟order by一致 ，group by 即使没有过滤条件用到索引，也可以直接使用索引。 
* group by 先排序再分组，遵照索引建的最佳左前缀法则 
* 当无法使用索引列，增大 max_length_for_sort_data 和 sort_buffer_size 参数的设置 
* where效率高于having，能写在where限定的条件就不要写在having中了 
* 减少使用order by，和业务沟通能不排序就不排序，或将排序放到程序端去做。Order by、group by、distinct这些语句较为耗费CPU，数据库的CPU资源是极其宝贵的。 
* 包含了order by、group by、distinct这些查询的语句，where条件过滤出来的结果集请保持在1000行 以内，否则SQL会很慢。

##  优化分页查询

<img src="D:\笔记\mysql.assets\image-20220705225329130.png" alt="image-20220705225329130" style="float:left;" />

**优化思路一**

在索引上完成排序分页操作，最后根据主键关联回原表查询所需要的其他列内容。

```mysql
EXPLAIN SELECT * FROM student t,(SELECT id FROM student ORDER BY id LIMIT 2000000,10) a WHERE t.id = a.id;
```

![image-20220705225625166](D:\笔记\mysql.assets\image-20220705225625166.png)

**优化思路二**

该方案适用于主键自增的表，可以把Limit 查询转换成某个位置的查询 。

```mysql
EXPLAIN SELECT * FROM student WHERE id > 2000000 LIMIT 10;
```

![image-20220705225654124](D:\笔记\mysql.assets\image-20220705225654124.png)

##  优先考虑覆盖索引

###  什么是覆盖索引？

**理解方式一**：索引是高效找到行的一个方法，但是一般数据库也能使用索引找到一个列的数据，因此它不必读取整个行。毕竟索引叶子节点存储了它们索引的数据；当能通过读取索引就可以得到想要的数据，那就不需要读取行了。**一个索引包含了满足查询结果的数据就叫做覆盖索引**。

**理解方式二**：非聚簇复合索引的一种形式，它包括在查询里的SELECT、JOIN和WHERE子句用到的所有列 （即建索引的字段正好是覆盖查询条件中所涉及的字段）。

简单说就是， `索引列+主键` 包含 `SELECT 到 FROM之间查询的列` 。

**举例一：**

```mysql
# 删除之前的索引
DROP INDEX idx_age_stuno ON student;
CREATE INDEX idx_age_name ON student(age, NAME);
EXPLAIN SELECT * FROM student WHERE age <> 20;
```

![image-20220706124528680](D:\笔记\mysql.assets\image-20220706124528680.png)

**举例二：**

```mysql
EXPLAIN SELECT * FROM student WHERE NAME LIKE '%abc';
```

![image-20220706124612180](D:\笔记\mysql.assets\image-20220706124612180.png)

```mysql
CREATE INDEX idx_age_name ON student(age, NAME);
EXPLAIN SELECT id,age,NAME FROM student WHERE NAME LIKE '%abc';
```

![image-20220706125113658](D:\笔记\mysql.assets\image-20220706125113658.png)

上述都使用到了声明的索引，下面的情况则不然，查询列依然多了classId,结果是未使用到索引：

```mysql
EXPLAIN SELECT id,age,NAME,classId FROM student WHERE NAME LIKE '%abc';
```

![image-20220706125351116](D:\笔记\mysql.assets\image-20220706125351116.png)

###  覆盖索引的利弊

<img src="D:\笔记\mysql.assets\image-20220706125943936.png" alt="image-20220706125943936" style="zoom:80%;float:left" />

##  如何给字符串添加索引

有一张教师表，表定义如下：

```mysql
create table teacher(
ID bigint unsigned primary key,
email varchar(64),
...
)engine=innodb;
```

讲师要使用邮箱登录，所以业务代码中一定会出现类似于这样的语句：

```mysql
mysql> select col1, col2 from teacher where email='xxx';
```

如果email这个字段上没有索引，那么这个语句就只能做 `全表扫描` 。

### 前缀索引

MySQL是支持前缀索引的。默认地，如果你创建索引的语句不指定前缀长度，那么索引就会包含整个字 符串。

```mysql
mysql> alter table teacher add index index1(email);
#或
mysql> alter table teacher add index index2(email(6));
```

这两种不同的定义在数据结构和存储上有什么区别呢？下图就是这两个索引的示意图。

![image-20220706130901307](D:\笔记\mysql.assets\image-20220706130901307.png)

以及

<img src="D:\笔记\mysql.assets\image-20220706130921934.png" alt="image-20220706130921934" style="zoom:70%;" />

**如果使用的是index1**（即email整个字符串的索引结构），执行顺序是这样的：

1. 从index1索引树找到满足索引值是’ zhangssxyz@xxx.com’的这条记录，取得ID2的值； 
2. 到主键上查到主键值是ID2的行，判断email的值是正确的，将这行记录加入结果集； 
3. 取index1索引树上刚刚查到的位置的下一条记录，发现已经不满足email=' zhangssxyz@xxx.com ’的 条件了，循环结束。

这个过程中，只需要回主键索引取一次数据，所以系统认为只扫描了一行。

**如果使用的是index2**（即email(6)索引结构），执行顺序是这样的：

1. 从index2索引树找到满足索引值是’zhangs’的记录，找到的第一个是ID1； 
2. 到主键上查到主键值是ID1的行，判断出email的值不是’ zhangssxyz@xxx.com ’，这行记录丢弃； 
3. 取index2上刚刚查到的位置的下一条记录，发现仍然是’zhangs’，取出ID2，再到ID索引上取整行然 后判断，这次值对了，将这行记录加入结果集； 
4. 重复上一步，直到在idxe2上取到的值不是’zhangs’时，循环结束。

也就是说**使用前缀索引，定义好长度，就可以做到既节省空间，又不用额外增加太多的查询成本。**前面 已经讲过区分度，区分度越高越好。因为区分度越高，意味着重复的键值越少。

### 前缀索引对覆盖索引的影响

> 结论： 使用前缀索引就用不上覆盖索引对查询性能的优化了，这也是你在选择是否使用前缀索引时需要考虑的一个因素。

##  索引下推

###  使用前后对比

Index Condition Pushdown(ICP)是MySQL 5.6中新特性，是一种在存储引擎层使用索引过滤数据的一种优化方式。

<img src="D:\笔记\mysql.assets\image-20220706131320477.png" alt="image-20220706131320477" style="zoom:80%;float:left" />

###  ICP的开启/关闭

* 默认情况下启动索引条件下推。可以通过设置系统变量`optimizer_switch`控制：`index_condition_pushdown`

```mysql
# 打开索引下推
SET optimizer_switch = 'index_condition_pushdown=on';

# 关闭索引下推
SET optimizer_switch = 'index_condition_pushdown=off';
```

* 当使用索引条件下推是，`EXPLAIN`语句输出结果中`Extra`列内容显示为`Using index condition`。

###  ICP使用案例

<img src="D:\笔记\mysql.assets\image-20220706135436316.png" alt="image-20220706135436316" style="zoom:80%;float:left" />

<img src="D:\笔记\mysql.assets\image-20220706135506409.png" alt="image-20220706135506409" style="zoom:80%;float:left" />

* 主键索引 (简图)

![image-20220706135633814](D:\笔记\mysql.assets\image-20220706135633814.png)

二级索引zip_last_first (简图，这里省略了数据页等信息)

![image-20220706135701187](D:\笔记\mysql.assets\image-20220706135701187.png)

<img src="D:\笔记\mysql.assets\image-20220706135723203.png" alt="image-20220706135723203" style="zoom:80%;float:left" />

### 开启和关闭ICP性能对比

<img src="D:\笔记\mysql.assets\image-20220706135904713.png" alt="image-20220706135904713" style="zoom:80%;float:left" />

<img src="D:\笔记\mysql.assets\image-20220706140213382.png" alt="image-20220706140213382" style="zoom:80%;float:left" />

###  ICP的使用条件

1. 如果表的访问类型为 range 、 ref 、 eq_ref 或者 ref_or_null 可以使用ICP。
2. ICP可以使用`InnDB`和`MyISAM`表，包括分区表`InnoDB`和`MyISAM`表
3. 对于`InnoDB`表，ICP仅用于`二级索引`。ICP的目标是减少全行读取次数，从而减少I/O操作。
4. 当SQL使用覆盖索引时，不支持ICP优化方法。因为这种情况下使用ICP不会减少I/O。
5. 相关子查询的条件不能使用ICP

##  普通索引 vs 唯一索引

从性能的角度考虑，你选择唯一索引还是普通索引呢？选择的依据是什么呢？

假设，我们有一个主键列为ID的表，表中有字段k，并且在k上有索引，假设字段 k 上的值都不重复。

这个表的建表语句是：

```mysql
mysql> create table test(
id int primary key,
k int not null,
name varchar(16),
index (k)
)engine=InnoDB;
```

表中R1~R5的(ID,k)值分别为(100,1)、(200,2)、(300,3)、(500,5)和(600,6)。

### 查询过程

假设，执行查询的语句是 select id from test where k=5。

* 对于普通索引来说，查找到满足条件的第一个记录(5,500)后，需要查找下一个记录，直到碰到第一 个不满足k=5条件的记录。 
* 对于唯一索引来说，由于索引定义了唯一性，查找到第一个满足条件的记录后，就会停止继续检 索。

那么，这个不同带来的性能差距会有多少呢？答案是， 微乎其微 。

###  更新过程

为了说明普通索引和唯一索引对更新语句性能的影响这个问题，介绍一下change buffer。

当需要更新一个数据页时，如果数据页在内存中就直接更新，而如果这个数据页还没有在内存中的话， 在不影响数据一致性的前提下， `InooDB会将这些更新操作缓存在change buffer中` ，这样就不需要从磁盘中读入这个数据页了。在下次查询需要访问这个数据页的时候，将数据页读入内存，然后执行change buffer中与这个页有关的操作。通过这种方式就能保证这个数据逻辑的正确性。

将change buffer中的操作应用到原数据页，得到最新结果的过程称为 merge 。除了 `访问这个数据页` 会触 发merge外，系统有 `后台线程会定期` merge。在 `数据库正常关闭（shutdown）` 的过程中，也会执行merge 操作。

如果能够将更新操作先记录在change buffer， `减少读磁盘` ，语句的执行速度会得到明显的提升。而且， 数据读入内存是需要占用 buffer pool 的，所以这种方式还能够 `避免占用内存 `，提高内存利用率。

`唯一索引的更新就不能使用change buffer` ，实际上也只有普通索引可以使用。

如果要在这张表中插入一个新记录(4,400)的话，InnoDB的处理流程是怎样的？

###  change buffer的使用场景

1. 普通索引和唯一索引应该怎么选择？其实，这两类索引在查询能力上是没差别的，主要考虑的是 对 更新性能 的影响。所以，建议你 尽量选择普通索引 。 
2. 在实际使用中会发现， 普通索引 和 change buffer 的配合使用，对于 数据量大 的表的更新优化 还是很明显的。 
3. 如果所有的更新后面，都马上 伴随着对这个记录的查询 ，那么你应该 关闭change buffer 。而在 其他情况下，change buffer都能提升更新性能。 
4. 由于唯一索引用不上change buffer的优化机制，因此如果 业务可以接受 ，从性能角度出发建议优 先考虑非唯一索引。但是如果"业务可能无法确保"的情况下，怎么处理呢？ 
	* 首先， 业务正确性优先 。我们的前提是“业务代码已经保证不会写入重复数据”的情况下，讨论性能 问题。如果业务不能保证，或者业务就是要求数据库来做约束，那么没得选，必须创建唯一索引。 这种情况下，本节的意义在于，如果碰上了大量插入数据慢、内存命中率低的时候，给你多提供一 个排查思路。 
	* 然后，在一些“ 归档库 ”的场景，你是可以考虑使用唯一索引的。比如，线上数据只需要保留半年， 然后历史数据保存在归档库。这时候，归档数据已经是确保没有唯一键冲突了。要提高归档效率， 可以考虑把表里面的唯一索引改成普通索引。

## 其它查询优化策略

###  EXISTS 和 IN 的区分

**问题：**

不太理解哪种情况下应该使用 EXISTS，哪种情况应该用 IN。选择的标准是看能否使用表的索引吗？

**回答：**

<img src="D:\笔记\mysql.assets\image-20220706141957185.png" alt="image-20220706141957185" style="zoom:80%;float:left" />

### COUNT(*)与COUNT(具体字段)效率

问：在 MySQL 中统计数据表的行数，可以使用三种方式： SELECT COUNT(*) 、 SELECT COUNT(1) 和 SELECT COUNT(具体字段) ，使用这三者之间的查询效率是怎样的？

答：

<img src="D:\笔记\mysql.assets\image-20220706142648452.png" alt="image-20220706142648452" style="zoom:80%;float:left" />

###  关于SELECT(*)

在表查询中，建议明确字段，不要使用 * 作为查询的字段列表，推荐使用SELECT <字段列表> 查询。原因： 

① MySQL 在解析的过程中，会通过查询数据字典 将"*"按序转换成所有列名，这会大大的耗费资源和时间。 

② 无法使用 覆盖索引

### LIMIT 1 对优化的影响

针对的是会扫描全表的 SQL 语句，如果你可以确定结果集只有一条，那么加上 LIMIT 1 的时候，当找到一条结果的时候就不会继续扫描了，这样会加快查询速度。

 如果数据表已经对字段建立了唯一索引，那么可以通过索引进行查询，不会全表扫描的话，就不需要加上 LIMIT 1 了。

###  多使用COMMIT

只要有可能，在程序中尽量多使用 COMMIT，这样程序的性能得到提高，需求也会因为 COMMIT 所释放 的资源而减少。

COMMIT 所释放的资源： 

* 回滚段上用于恢复数据的信息 
* 被程序语句获得的锁 
* redo / undo log buffer 中的空间 
* 管理上述 3 种资源中的内部花费

## 淘宝数据库，主键如何设计的？

聊一个实际问题：淘宝的数据库，主键是如何设计的？

某些错的离谱的答案还在网上年复一年的流传着，甚至还成为了所谓的MySQL军规。其中，一个最明显的错误就是关于MySQL的主键设计。

大部分人的回答如此自信：用8字节的 BIGINT 做主键，而不要用INT。 `错 `！

这样的回答，只站在了数据库这一层，而没有 `从业务的角度` 思考主键。主键就是一个自增ID吗？站在 2022年的新年档口，用自增做主键，架构设计上可能 `连及格都拿不到` 。

### 自增ID的问题

自增ID做主键，简单易懂，几乎所有数据库都支持自增类型，只是实现上各自有所不同而已。自增ID除 了简单，其他都是缺点，总体来看存在以下几方面的问题：

1. **可靠性不高**

	存在自增ID回溯的问题，这个问题直到最新版本的MySQL 8.0才修复。 

2. **安全性不高 **

	对外暴露的接口可以非常容易猜测对应的信息。比如：/User/1/这样的接口，可以非常容易猜测用户ID的 值为多少，总用户数量有多少，也可以非常容易地通过接口进行数据的爬取。 

3. **性能差** 

	自增ID的性能较差，需要在数据库服务器端生成。 

4. **交互多** 

	业务还需要额外执行一次类似 last_insert_id() 的函数才能知道刚才插入的自增值，这需要多一次的 网络交互。在海量并发的系统中，多1条SQL，就多一次性能上的开销。 

5. **局部唯一性 **

	最重要的一点，自增ID是局部唯一，只在当前数据库实例中唯一，而不是全局唯一，在任意服务器间都 是唯一的。对于目前分布式系统来说，这简直就是噩梦。

###  业务字段做主键

为了能够唯一地标识一个会员的信息，需要为 会员信息表 设置一个主键。那么，怎么为这个表设置主 键，才能达到我们理想的目标呢？ 这里我们考虑业务字段做主键。

表数据如下：

![image-20220706151506580](D:\笔记\mysql.assets\image-20220706151506580.png)

在这个表里，哪个字段比较合适呢？

* **选择卡号（cardno）**

会员卡号（cardno）看起来比较合适，因为会员卡号不能为空，而且有唯一性，可以用来 标识一条会员 记录。

```mysql
mysql> CREATE TABLE demo.membermaster
-> (
-> cardno CHAR(8) PRIMARY KEY, -- 会员卡号为主键
-> membername TEXT,
-> memberphone TEXT,
-> memberpid TEXT,
-> memberaddress TEXT,
-> sex TEXT,
-> birthday DATETIME
-> );
Query OK, 0 rows affected (0.06 sec)
```

不同的会员卡号对应不同的会员，字段“cardno”唯一地标识某一个会员。如果都是这样，会员卡号与会 员一一对应，系统是可以正常运行的。

但实际情况是， 会员卡号可能存在重复使用 的情况。比如，张三因为工作变动搬离了原来的地址，不再 到商家的门店消费了 （退还了会员卡），于是张三就不再是这个商家门店的会员了。但是，商家不想让 这个会 员卡空着，就把卡号是“10000001”的会员卡发给了王五。

从系统设计的角度看，这个变化只是修改了会员信息表中的卡号是“10000001”这个会员 信息，并不会影 响到数据一致性。也就是说，修改会员卡号是“10000001”的会员信息， 系统的各个模块，都会获取到修 改后的会员信息，不会出现“有的模块获取到修改之前的会员信息，有的模块获取到修改后的会员信息， 而导致系统内部数据不一致”的情况。因此，从 信息系统层面 上看是没问题的。

但是从使用 系统的业务层面 来看，就有很大的问题 了，会对商家造成影响。

比如，我们有一个销售流水表（trans），记录了所有的销售流水明细。2020 年 12 月 01 日，张三在门店 购买了一本书，消费了 89 元。那么，系统中就有了张三买书的流水记录，如下所示：

![image-20220706151715106](D:\笔记\mysql.assets\image-20220706151715106.png)

接着，我们查询一下 2020 年 12 月 01 日的会员销售记录：

```mysql
mysql> SELECT b.membername,c.goodsname,a.quantity,a.salesvalue,a.transdate
-> FROM demo.trans AS a
-> JOIN demo.membermaster AS b
-> JOIN demo.goodsmaster AS c
-> ON (a.cardno = b.cardno AND a.itemnumber=c.itemnumber);
+------------+-----------+----------+------------+---------------------+
| membername | goodsname | quantity | salesvalue | transdate |
+------------+-----------+----------+------------+---------------------+
|     张三   | 书         | 1.000    | 89.00      | 2020-12-01 00:00:00 |
+------------+-----------+----------+------------+---------------------+
1 row in set (0.00 sec)
```

如果会员卡“10000001”又发给了王五，我们会更改会员信息表。导致查询时：

```mysql
mysql> SELECT b.membername,c.goodsname,a.quantity,a.salesvalue,a.transdate
-> FROM demo.trans AS a
-> JOIN demo.membermaster AS b
-> JOIN demo.goodsmaster AS c
-> ON (a.cardno = b.cardno AND a.itemnumber=c.itemnumber);
+------------+-----------+----------+------------+---------------------+
| membername | goodsname | quantity | salesvalue | transdate |
+------------+-----------+----------+------------+---------------------+
| 王五        | 书        | 1.000    | 89.00      | 2020-12-01 00:00:00 |
+------------+-----------+----------+------------+---------------------+
1 row in set (0.01 sec)
```

这次得到的结果是：王五在 2020 年 12 月 01 日，买了一本书，消费 89 元。显然是错误的！结论：千万 不能把会员卡号当做主键。

* **选择会员电话 或 身份证号**

会员电话可以做主键吗？不行的。在实际操作中，手机号也存在 被运营商收回 ，重新发给别人用的情况。

那身份证号行不行呢？好像可以。因为身份证决不会重复，身份证号与一个人存在一一对 应的关系。可 问题是，身份证号属于 个人隐私 ，顾客不一定愿意给你。要是强制要求会员必须登记身份证号，会把很 多客人赶跑的。其实，客户电话也有这个问题，这也是我们在设计会员信息表的时候，允许身份证号和 电话都为空的原因。

**所以，建议尽量不要用跟业务有关的字段做主键。毕竟，作为项目设计的技术人员，我们谁也无法预测 在项目的整个生命周期中，哪个业务字段会因为项目的业务需求而有重复，或者重用之类的情况出现。**

> 经验： 刚开始使用 MySQL 时，很多人都很容易犯的错误是喜欢用业务字段做主键，想当然地认为了解业 务需求，但实际情况往往出乎意料，而更改主键设置的成本非常高。

###  淘宝的主键设计

在淘宝的电商业务中，订单服务是一个核心业务。请问， 订单表的主键 淘宝是如何设计的呢？是自增ID 吗？

打开淘宝，看一下订单信息：

![image-20220706161436920](D:\笔记\mysql.assets\image-20220706161436920.png)

从上图可以发现，订单号不是自增ID！我们详细看下上述4个订单号：

```mysql
1550672064762308113
1481195847180308113
1431156171142308113
1431146631521308113
```

订单号是19位的长度，且订单的最后5位都是一样的，都是08113。且订单号的前面14位部分是单调递增的。

大胆猜测，淘宝的订单ID设计应该是：

```mysql
订单ID = 时间 + 去重字段 + 用户ID后6位尾号
```

这样的设计能做到全局唯一，且对分布式系统查询及其友好。

### 推荐的主键设计

**非核心业务** ：对应表的主键自增ID，如告警、日志、监控等信息。

**核心业务** ：`主键设计至少应该是全局唯一且是单调递增`。全局唯一保证在各系统之间都是唯一的，单调 递增是希望插入时不影响数据库性能。

这里推荐最简单的一种主键设计：UUID。

**UUID的特点：**

全局唯一，占用36字节，数据无序，插入性能差。

**认识UUID：**

* 为什么UUID是全局唯一的？ 
* 为什么UUID占用36个字节？ 
* 为什么UUID是无序的？

MySQL数据库的UUID组成如下所示：

```mysql
UUID = 时间+UUID版本（16字节）- 时钟序列（4字节） - MAC地址（12字节）
```

我们以UUID值e0ea12d4-6473-11eb-943c-00155dbaa39d举例：

![image-20220706162131362](D:\笔记\mysql.assets\image-20220706162131362.png)

`为什么UUID是全局唯一的？`

在UUID中时间部分占用60位，存储的类似TIMESTAMP的时间戳，但表示的是从1582-10-15 00：00：00.00 到现在的100ns的计数。可以看到UUID存储的时间精度比TIMESTAMPE更高，时间维度发生重复的概率降 低到1/100ns。

时钟序列是为了避免时钟被回拨导致产生时间重复的可能性。MAC地址用于全局唯一。

`为什么UUID占用36个字节？`

UUID根据字符串进行存储，设计时还带有无用"-"字符串，因此总共需要36个字节。

`为什么UUID是随机无序的呢？`

因为UUID的设计中，将时间低位放在最前面，而这部分的数据是一直在变化的，并且是无序。

**改造UUID**

若将时间高低位互换，则时间就是单调递增的了，也就变得单调递增了。MySQL 8.0可以更换时间低位和时间高位的存储方式，这样UUID就是有序的UUID了。

MySQL 8.0还解决了UUID存在的空间占用的问题，除去了UUID字符串中无意义的"-"字符串，并且将字符串用二进制类型保存，这样存储空间降低为了16字节。

可以通过MySQL8.0提供的uuid_to_bin函数实现上述功能，同样的，MySQL也提供了bin_to_uuid函数进行转化：

```mysql
SET @uuid = UUID();
SELECT @uuid,uuid_to_bin(@uuid),uuid_to_bin(@uuid,TRUE);
```

![image-20220706162657448](D:\笔记\mysql.assets\image-20220706162657448.png)

**通过函数uuid_to_bin(@uuid,true)将UUID转化为有序UUID了**。全局唯一 + 单调递增，这不就是我们想要的主键！

**有序UUID性能测试**

16字节的有序UUID，相比之前8字节的自增ID，性能和存储空间对比究竟如何呢？

我们来做一个测试，插入1亿条数据，每条数据占用500字节，含有3个二级索引，最终的结果如下所示：

<img src="D:\笔记\mysql.assets\image-20220706162947613.png" alt="image-20220706162947613" style="zoom:67%;" />

从上图可以看到插入1亿条数据有序UUID是最快的，而且在实际业务使用中有序UUID在 `业务端就可以生成` 。还可以进一步减少SQL的交互次数。

另外，虽然有序UUID相比自增ID多了8个字节，但实际只增大了3G的存储空间，还可以接受。

> 在当今的互联网环境中，非常不推荐自增ID作为主键的数据库设计。更推荐类似有序UUID的全局 唯一的实现。 
>
> 另外在真实的业务系统中，主键还可以加入业务和系统属性，如用户的尾号，机房的信息等。这样 的主键设计就更为考验架构师的水平了。

**如果不是MySQL8.0 肿么办？**

手动赋值字段做主键！

比如，设计各个分店的会员表的主键，因为如果每台机器各自产生的数据需要合并，就可能会出现主键重复的问题。

可以在总部 MySQL 数据库中，有一个管理信息表，在这个表中添加一个字段，专门用来记录当前会员编号的最大值。

门店在添加会员的时候，先到总部 MySQL 数据库中获取这个最大值，在这个基础上加 1，然后用这个值 作为新会员的“id”，同时，更新总部 MySQL 数据库管理信息表中的当前会员编号的最大值。

这样一来，各个门店添加会员的时候，都对同一个总部 MySQL 数据库中的数据表字段进行操作，就解 决了各门店添加会员时会员编号冲突的问题。

# 数据库的设计规范

##  为什么需要数据库设计

<img src="D:\笔记\mysql.assets\image-20220706164201695.png" alt="image-20220706164201695" style="zoom:80%;float:left" />

<img src="D:\笔记\mysql.assets\image-20220706164359539.png" alt="image-20220706164359539" style="zoom:80%;float:left" />

##  范 式

###  范式简介

在关系型数据库中，关于数据表设计的基本原则、规则就称为范式。可以理解为，一张数据表的设计结 构需要满足的某种设计标准的 级别 。要想设计一个结构合理的关系型数据库，必须满足一定的范式。

###  范式都包括哪些

目前关系型数据库有六种常见范式，按照范式级别，从低到高分别是：第一范式（1NF）、第二范式 （2NF）、第三范式（3NF）、巴斯-科德范式（BCNF）、第四范式(4NF）和第五范式（5NF，又称完美范式）。

数据库的范式设计越高阶，夯余度就越低，同时高阶的范式一定符合低阶范式的要求，满足最低要求的范式是第一范式（1NF）。在第一范式的基础上进一步满足更多规范的要求称为第二范式（2NF），其余范式以此类推。

一般来说，在关系型数据库设计中，最高也就遵循到`BCNF`, 普遍还是`3NF`。但也不绝对，有时候为了提高某些查询性能，我们还需要破坏范式规则，也就是`反规范化`。

![image-20220706165020939](D:\笔记\mysql.assets\image-20220706165020939.png)

### 键和相关属性的概念

<img src="D:\笔记\mysql.assets\image-20220706165231022.png" alt="image-20220706165231022" style="float:left;" />

**举例:**

这里有两个表：

`球员表(player)` ：球员编号 | 姓名 | 身份证号 | 年龄 | 球队编号 

`球队表(team) `：球队编号 | 主教练 | 球队所在地

* 超键 ：对于球员表来说，超键就是包括球员编号或者身份证号的任意组合，比如（球员编号） （球员编号，姓名）（身份证号，年龄）等。 
* 候选键 ：就是最小的超键，对于球员表来说，候选键就是（球员编号）或者（身份证号）。 
* 主键 ：我们自己选定，也就是从候选键中选择一个，比如（球员编号）。 
* 外键 ：球员表中的球队编号。 
* 主属性 、 非主属性 ：在球员表中，主属性是（球员编号）（身份证号），其他的属性（姓名） （年龄）（球队编号）都是非主属性。

###  第一范式(1st NF)

第一范式主要确保数据库中每个字段的值必须具有`原子性`，也就是说数据表中每个字段的值为`不可再次拆分`的最小数据单元。

我们在设计某个字段的时候，对于字段X来说，不能把字段X拆分成字段X-1和字段X-2。事实上，任何的DBMS都会满足第一范式的要求，不会将字段进行拆分。

**举例1：**

假设一家公司要存储员工的姓名和联系方式。它创建一个如下表：

![image-20220706171057270](D:\笔记\mysql.assets\image-20220706171057270.png)

该表不符合 1NF ，因为规则说“表的每个属性必须具有原子（单个）值”，lisi和zhaoliu员工的 emp_mobile 值违反了该规则。为了使表符合 1NF ，我们应该有如下表数据：

![image-20220706171130851](D:\笔记\mysql.assets\image-20220706171130851.png)

**举例2：**

user 表的设计不符合第一范式

![image-20220706171225292](D:\笔记\mysql.assets\image-20220706171225292.png)

其中，user_info字段为用户信息，可以进一步拆分成更小粒度的字段，不符合数据库设计对第一范式的 要求。将user_info拆分后如下：

![image-20220706171242455](D:\笔记\mysql.assets\image-20220706171242455.png)

**举例3：**

属性的原子性是 主观的 。例如，Employees关系中雇员姓名应当使用1个（fullname）、2个（firstname 和lastname）还是3个（firstname、middlename和lastname）属性表示呢？答案取决于应用程序。如果应 用程序需要分别处理雇员的姓名部分（如：用于搜索目的），则有必要把它们分开。否则，不需要。

表1：

![image-20220706171442919](D:\笔记\mysql.assets\image-20220706171442919.png)

表2：

![image-20220706171456873](D:\笔记\mysql.assets\image-20220706171456873.png)

###  第二范式(2nd NF)

第二范式要求，在满足第一范式的基础上，还要**满足数据库里的每一条数据记录，都是可唯一标识的。而且所有非主键字段，都必须完全依赖主键，不能只依赖主键的一部分**。如果知道主键的所有属性的值，就可以检索到任何元组（行）的任何属性的任何值。（要求中的主键，其实可以扩展替换为候选键）。

**举例1：**

`成绩表` （学号，课程号，成绩）关系中，（学号，课程号）可以决定成绩，但是学号不能决定成绩，课 程号也不能决定成绩，所以“（学号，课程号）→成绩”就是 `完全依赖关系` 。

**举例2：**

`比赛表 player_game` ，里面包含球员编号、姓名、年龄、比赛编号、比赛时间和比赛场地等属性，这 里候选键和主键都为（球员编号，比赛编号），我们可以通过候选键（或主键）来决定如下的关系：

```mysql
(球员编号, 比赛编号) → (姓名, 年龄, 比赛时间, 比赛场地，得分)
```

但是这个数据表不满足第二范式，因为数据表中的字段之间还存在着如下的对应关系：

```mysql
(球员编号) → (姓名，年龄)

(比赛编号) → (比赛时间, 比赛场地)
```

对于非主属性来说，并非完全依赖候选键。这样会产生怎样的问题呢？

1. `数据冗余` ：如果一个球员可以参加 m 场比赛，那么球员的姓名和年龄就重复了 m-1 次。一个比赛 也可能会有 n 个球员参加，比赛的时间和地点就重复了 n-1 次。 
2. `插入异常` ：如果我们想要添加一场新的比赛，但是这时还没有确定参加的球员都有谁，那么就没法插入。 
3. `删除异常` ：如果我要删除某个球员编号，如果没有单独保存比赛表的话，就会同时把比赛信息删 除掉。 
4. `更新异常` ：如果我们调整了某个比赛的时间，那么数据表中所有这个比赛的时间都需要进行调 整，否则就会出现一场比赛时间不同的情况。

为了避免出现上述的情况，我们可以把球员比赛表设计为下面的三张表。

![image-20220707122639894](D:\笔记\mysql.assets\image-20220707122639894.png)

这样的话，每张数据表都符合第二范式，也就避免了异常情况的发生。

> 1NF 告诉我们字段属性需要是原子性的，而 2NF 告诉我们一张表就是一个独立的对象，一张表只表达一个意思。

**举例3：**

定义了一个名为 Orders 的关系，表示订单和订单行的信息：

![image-20220707123038469](D:\笔记\mysql.assets\image-20220707123038469.png)

违反了第二范式，因为有非主键属性仅依赖于候选键（或主键）的一部分。例如，可以仅通过orderid找 到订单的 orderdate，以及 customerid 和 companyname，而没有必要再去使用productid。

修改：

Orders表和OrderDetails表如下，此时符合第二范式。

![image-20220707123104009](D:\笔记\mysql.assets\image-20220707123104009.png)

> 小结：第二范式（2NF）要求实体的属性完全依赖主关键字。如果存在不完全依赖，那么这个属性和主关键字的这一部分应该分离出来形成一个新的实体，新实体与元实体之间是一对多的关系。

###  第三范式(3rd NF)

第三范式是在第二范式的基础上，确保数据表中的每一个非主键字段都和主键字段直接相关，也就是说，**要求数据表中的所有非主键字段不能依赖于其他非主键字段**。（即，不能存在非主属性A依赖于非主属性B，非主属性B依赖于主键C的情况，即存在“A->B->C"的决定关系）通俗地讲，该规则的意思是所有`非主键属性`之间不能由依赖关系，必须`相互独立`。

这里的主键可以扩展为候选键。

**举例1：**

`部门信息表` ：每个部门有部门编号（dept_id）、部门名称、部门简介等信息。

`员工信息表 `：每个员工有员工编号、姓名、部门编号。列出部门编号后就不能再将部门名称、部门简介 等与部门有关的信息再加入员工信息表中。

如果不存在部门信息表，则根据第三范式（3NF）也应该构建它，否则就会有大量的数据冗余。

**举例2：**

![image-20220707124011654](D:\笔记\mysql.assets\image-20220707124011654.png)

商品类别名称依赖于商品类别编号，不符合第三范式。

修改：

表1：符合第三范式的 `商品类别表` 的设计

![image-20220707124040899](D:\笔记\mysql.assets\image-20220707124040899.png)

表2：符合第三范式的 `商品表` 的设计

![image-20220707124058174](D:\笔记\mysql.assets\image-20220707124058174.png)

商品表goods通过商品类别id字段（category_id）与商品类别表goods_category进行关联。

**举例3：**

`球员player表` ：球员编号、姓名、球队名称和球队主教练。现在，我们把属性之间的依赖关系画出来，如下图所示:

![image-20220707124136228](D:\笔记\mysql.assets\image-20220707124136228.png)

你能看到球员编号决定了球队名称，同时球队名称决定了球队主教练，非主属性球队主教练就会传递依 赖于球员编号，因此不符合 3NF 的要求。

如果要达到 3NF 的要求，需要把数据表拆成下面这样：

![image-20220707124152312](D:\笔记\mysql.assets\image-20220707124152312.png)

**举例4：**

修改第二范式中的举例3。

此时的Orders关系包含 orderid、orderdate、customerid 和 companyname 属性，主键定义为 orderid。 customerid 和companyname均依赖于主键——orderid。例如，你需要通过orderid主键来查找代表订单中 客户的customerid，同样，你需要通过 orderid 主键查找订单中客户的公司名称（companyname）。然 而， customerid和companyname也是互相依靠的。为满足第三范式，可以改写如下：

![image-20220707124212114](D:\笔记\mysql.assets\image-20220707124212114.png)

> 符合3NF后的数据模型通俗地讲，2NF和3NF通常以这句话概括：“每个非键属性依赖于键，依赖于 整个键，并且除了键别无他物”。

### 小结

<img src="D:\笔记\mysql.assets\image-20220707124343085.png" alt="image-20220707124343085" style="zoom:80%;float:left" />

## 反范式化

###  概述

<img src="D:\笔记\mysql.assets\image-20220707124741675.png" alt="image-20220707124741675" style="zoom:80%;float:left" />

**规范化 vs 性能**

> 1. 为满足某种商业目标 , 数据库性能比规范化数据库更重要 
> 2. 在数据规范化的同时 , 要综合考虑数据库的性能 
> 3. 通过在给定的表中添加额外的字段，以大量减少需要从中搜索信息所需的时间 
> 4. 通过在给定的表中插入计算列，以方便查询

### 应用举例

**举例1：**

员工的信息存储在 `employees 表` 中，部门信息存储在 `departments 表` 中。通过 employees 表中的 department_id字段与 departments 表建立关联关系。如果要查询一个员工所在部门的名称：

```mysql
select employee_id,department_name
from employees e join departments d
on e.department_id = d.department_id;
```

如果经常需要进行这个操作，连接查询就会浪费很多时间。可以在 employees 表中增加一个冗余字段 department_name，这样就不用每次都进行连接操作了。

**举例2：**

反范式化的 `goods商品信息表` 设计如下：

![image-20220707125118808](D:\笔记\mysql.assets\image-20220707125118808.png)

**举例3：**

我们有 2 个表，分别是 `商品流水表（atguigu.trans ）`和 `商品信息表 （atguigu.goodsinfo）` 。商品流水表里有 400 万条流水记录，商品信息表里有 2000 条商品记录。

商品流水表：

![image-20220707125401029](D:\笔记\mysql.assets\image-20220707125401029.png)

商品信息表：

![image-20220707125447317](D:\笔记\mysql.assets\image-20220707125447317.png)

新的商品流水表如下所示：

![image-20220707125500378](D:\笔记\mysql.assets\image-20220707125500378.png)

**举例4：**

`课程评论表 class_comment` ，对应的字段名称及含义如下：

![image-20220707125531172](D:\笔记\mysql.assets\image-20220707125531172.png)

`学生表 student` ，对应的字段名称及含义如下：

<img src="D:\笔记\mysql.assets\image-20220707125545891.png" alt="image-20220707125545891" style="zoom:80%;" />

在实际应用中，我们在显示课程评论的时候，通常会显示这个学生的昵称，而不是学生 ID，因此当我们 想要查询某个课程的前 1000 条评论时，需要关联 class_comment 和 student这两张表来进行查询。

**实验数据：模拟两张百万量级的数据表**

为了更好地进行 SQL 优化实验，我们需要给学生表和课程评论表随机模拟出百万量级的数据。我们可以 通过存储过程来实现模拟数据。

**反范式优化实验对比**

如果我们想要查询课程 ID 为 10001 的前 1000 条评论，需要写成下面这样：

```mysql
SELECT p.comment_text, p.comment_time, stu.stu_name
FROM class_comment AS p LEFT JOIN student AS stu
ON p.stu_id = stu.stu_id
WHERE p.class_id = 10001
ORDER BY p.comment_id DESC
LIMIT 1000;
```

运行结果（1000 条数据行）：

<img src="D:\笔记\mysql.assets\image-20220707125642908.png" alt="image-20220707125642908" style="zoom:80%;" />

运行时长为 0.395 秒，对于网站的响应来说，这已经很慢了，用户体验会非常差。

如果我们想要提升查询的效率，可以允许适当的数据冗余，也就是在商品评论表中增加用户昵称字段， 在 class_comment 数据表的基础上增加 stu_name 字段，就得到了 class_comment2 数据表。

这样一来，只需单表查询就可以得到数据集结果：

```mysql
SELECT comment_text, comment_time, stu_name
FROM class_comment2
WHERE class_id = 10001
ORDER BY class_id DESC LIMIT 1000;
```

运行结果（1000 条数据）：

<img src="D:\笔记\mysql.assets\image-20220707125718469.png" alt="image-20220707125718469" style="zoom:80%;" />

优化之后只需要扫描一次聚集索引即可，运行时间为 0.039 秒，查询时间是之前的 1/10。 你能看到， 在数据量大的情况下，查询效率会有显著的提升。

###  反范式的新问题

* 存储` 空间变大`了 
* 一个表中字段做了修改，另一个表中冗余的字段也需要做同步修改，否则 `数据不一致 `
* 若采用存储过程来支持数据的更新、删除等额外操作，如果更新频繁，会非常 `消耗系统资源 `
* 在 `数据量小` 的情况下，反范式不能体现性能的优势，可能还会让数据库的设计更加复杂

### 反范式的适用场景

当冗余信息有价值或者能 `大幅度提高查询效率` 的时候，我们才会采取反范式的优化。

####  增加冗余字段的建议

增加冗余字段一定要符合如下两个条件。只要满足这两个条件，才可以考虑增加夯余字段。

1）这个冗余字段`不需要经常进行修改`。

2）这个冗余字段`查询的时候不可或缺`。

####  历史快照、历史数据的需要

在现实生活中，我们经常需要一些冗余信息，比如订单中的收货人信息，包括姓名、电话和地址等。每 次发生的 `订单收货信息` 都属于 `历史快照` ，需要进行保存，但用户可以随时修改自己的信息，这时保存这 些冗余信息是非常有必要的。

反范式优化也常用在 `数据仓库` 的设计中，因为数据仓库通常`存储历史数据` ，对增删改的实时性要求不 强，对历史数据的分析需求强。这时适当允许数据的冗余度，更方便进行数据分析。

我简单总结下数据仓库和数据库在使用上的区别：

1. 数据库设计的目的在于`捕捉数据`，而数据仓库设计的目的在于`分析数据`。
2. 数据库对数据的`增删改实时性`要求强，需要存储在线的用户数据，而数据仓库存储的一般是`历史数据`。
3. 数据库设计需要`尽量避免冗余`，但为了提高查询效率也允许一定的`冗余度`，而数据仓库在设计上更偏向采用反范式设计，

##  BCNF(巴斯范式)

人们在3NF的基础上进行了改进，提出了巴斯范式（BCNF），页脚巴斯 - 科德范式（Boyce - Codd Normal Form）。BCNF被认为没有新的设计规范加入，只是对第三范式中设计规范要求更强，使得数据库冗余度更小。所以，称为是`修正的第三范式`，或`扩充的第三范式`，BCNF不被称为第四范式。

若一个关系达到了第三范式，并且它只有一个候选键，或者它的每个候选键都是单属性，则该关系自然达到BC范式。

一般来说，一个数据库设符合3NF或者BCNF就可以了。

**1. 案例**

我们分析如下表的范式情况：

<img src="D:\笔记\mysql.assets\image-20220707131428597.png" alt="image-20220707131428597" style="zoom:80%;" />

在这个表中，一个仓库只有一个管理员，同时一个管理员也只管理一个仓库。我们先来梳理下这些属性之间的依赖关系。

仓库名决定了管理员，管理员也决定了仓库名，同时（仓库名，物品名）的属性集合可以决定数量这个 属性。这样，我们就可以找到数据表的候选键。

`候选键 `：是（管理员，物品名）和（仓库名，物品名），然后我们从候选键中选择一个作为主键 ，比 如（仓库名，物品名）。

`主属性` ：包含在任一候选键中的属性，也就是仓库名，管理员和物品名。

`非主属性` ：数量这个属性。

**2. 是否符合三范式**

如何判断一张表的范式呢？我们需要根据范式的等级，从低到高来进行判断。

首先，数据表每个属性都是原子性的，符合 1NF 的要求；

其次，数据表中非主属性”数量“都与候选键全部依赖，（仓库名，物品名）决定数量，（管理员，物品 名）决定数量。因此，数据表符合 2NF 的要求；

最后，数据表中的非主属性，不传递依赖于候选键。因此符合 3NF 的要求。

**3. 存在的问题**

既然数据表已经符合了 3NF 的要求，是不是就不存在问题了呢？我们来看下面的情况：

1. 增加一个仓库，但是还没有存放任何物品。根据数据表实体完整性的要求，主键不能有空值，因 此会出现 插入异常 ；
2. 如果仓库更换了管理员，我们就可能会修改数据表中的多条记录 ；
3. 如果仓库里的商品都卖空了，那么此时仓库名称和相应的管理员名称也会随之被删除。

你能看到，即便数据表符合 3NF 的要求，同样可能存在插入，更新和删除数据的异常情况。

**4. 问题解决**

首先我们需要确认造成异常的原因：主属性仓库名对于候选键（管理员，物品名）是部分依赖的关系， 这样就有可能导致上面的异常情况。因此引入BCNF，**它在 3NF 的基础上消除了主属性对候选键的部分依赖或者传递依赖关系**。

* 如果在关系R中，U为主键，A属性是主键的一个属性，若存在A->Y，Y为主属性，则该关系不属于 BCNF。

根据 BCNF 的要求，我们需要把仓库管理关系 warehouse_keeper 表拆分成下面这样：

`仓库表` ：（仓库名，管理员）

`库存表 `：（仓库名，物品名，数量）

这样就不存在主属性对于候选键的部分依赖或传递依赖，上面数据表的设计就符合 BCNF。

再举例：

有一个 `学生导师表` ，其中包含字段：学生ID，专业，导师，专业GPA，这其中学生ID和专业是联合主键。

![image-20220707132038425](D:\笔记\mysql.assets\image-20220707132038425.png)

这个表的设计满足三范式，但是这里存在另一个依赖关系，“专业”依赖于“导师”，也就是说每个导师只做一个专业方面的导师，只要知道了是哪个导师，我们自然就知道是哪个专业的了。

所以这个表的部分主键Major依赖于非主键属性Advisor，那么我们可以进行以下的调整，拆分成2个表：

学生导师表：

![image-20220707132344634](D:\笔记\mysql.assets\image-20220707132344634.png)

导师表：

![image-20220707132355841](D:\笔记\mysql.assets\image-20220707132355841.png)

## 第四范式

多值依赖的概念：

* `多值依赖`即属性之间的一对多关系，记为K—>—>A。
* `函数依赖`事实上是单值依赖，所以不能表达属性值之间的一对多关系。
* `平凡的多值依赖`：全集U=K+A，一个K可以对应于多个A，即K—>—>A。此时整个表就是一组一对多关系。
* `非平凡的多值依赖`：全集U=K+A+B，一个K可以对应于多个A，也可以对应于多个B，A与B相互独立，即K—>—>A，K—>—>B。整个表有多组一对多关系，且有："一"部分是相同的属性集合，“多”部分是相互独立的属性集合。

第四范式即在满足巴斯 - 科德范式（BCNF）的基础上，消除非平凡且非函数依赖的多值依赖（即把同一表的多对多关系删除）。

**举例1：**职工表(职工编号，职工孩子姓名，职工选修课程)。

在这个表中，同一个职工可能会有多个职工孩子姓名。同样，同一个职工也可能会有多个职工选修课程，即这里存在着多值事实，不符合第四范式。

如果要符合第四范式，只需要将上表分为两个表，使它们只有一个多值事实，例如： `职工表一` (职工编 号，职工孩子姓名)， `职工表二`(职工编号，职工选修课程)，两个表都只有一个多值事实，所以符合第四范式。

**举例2：**

比如我们建立课程、教师、教材的模型。我们规定，每门课程有对应的一组教师，每门课程也有对应的一组教材，一门课程使用的教材和教师没有关系。我们建立的关系表如下：

课程ID，教师ID，教材ID；这三列作为联合主键。

为了表述方便，我们用Name代替ID，这样更容易看懂：

![image-20220707133830721](D:\笔记\mysql.assets\image-20220707133830721.png)

这个表除了主键，就没有其他字段了，所以肯定满足BC范式，但是却存在 `多值依赖` 导致的异常。

假如我们下学期想采用一本新的英版高数教材，但是还没确定具体哪个老师来教，那么我们就无法在这 个表中维护Course高数和Book英版高数教材的的关系。

解决办法是我们把这个多值依赖的表拆解成2个表，分别建立关系。这是我们拆分后的表：

![image-20220707134028730](D:\笔记\mysql.assets\image-20220707134028730.png)

以及

![image-20220707134220820](D:\笔记\mysql.assets\image-20220707134220820.png)

##  第五范式、域键范式

除了第四范式外，我们还有更高级的第五范式（又称完美范式）和域键范式（DKNF）。

在满足第四范式（4NF）的基础上，消除不是由候选键所蕴含的连接依赖。**如果关系模式R中的每一个连 接依赖均由R的候选键所隐含**，则称此关系模式符合第五范式。

函数依赖是多值依赖的一种特殊的情况，而多值依赖实际上是连接依赖的一种特殊情况。但连接依赖不 像函数依赖和多值依赖可以由 `语义直接导出` ，而是在 `关系连接运算` 时才反映出来。存在连接依赖的关系 模式仍可能遇到数据冗余及插入、修改、删除异常等问题。

第五范式处理的是 `无损连接问题` ，这个范式基本 `没有实际意义` ，因为无损连接很少出现，而且难以察觉。而域键范式试图定义一个 `终极范式` ，该范式考虑所有的依赖和约束类型，但是实用价值也是最小的，只存在理论研究中。

##  实战案例

商超进货系统中的`进货单表`进行剖析：

进货单表：

![image-20220707134636225](D:\笔记\mysql.assets\image-20220707134636225.png)

这个表中的字段很多，表里的数据量也很惊人。大量重复导致表变得庞大，效率极低。如何改造？

> 在实际工作场景中，这种由于数据表结构设计不合理，而导致的数据重复的现象并不少见。往往是系统虽然能够运行，承载能力却很差，稍微有点流量，就会出现内存不足、CPU使用率飙升的情况，甚至会导致整个项目失败。

###  迭代1次：考虑1NF

第一范式要求：**所有的字段都是基本数据类型，不可进行拆分**。这里需要确认，所有的列中，每个字段只包含一种数据。

这张表里，我们把“property"这一字段，拆分成”specification (规格)" 和 "unit (单位)"，这两个字段如下：

![image-20220707154400580](D:\笔记\mysql.assets\image-20220707154400580.png)

###  迭代2次：考虑2NF

第二范式要求，在满足第一范式的基础上，**还要满足数据表里的每一条数据记录，都是可唯一标识的。而且所有字段，都必须完全依赖主键，不能只依赖主键的一部分**。

第1步，就是要确定这个表的主键。通过观察发现，字段“listnumber（单号）"+"barcode（条码）"可以唯一标识每一条记录，可以作为主键。

第2步，确定好了主键以后，判断哪些字段完全依赖主键，哪些字段只依赖于主键的一部分。把只依赖于主键一部分的字段拆出去，形成新的数据表。

首先，进货单明细表里面的"goodsname(名称)""specification(规格)""unit(单位)"这些信息是商品的属性，只依赖于"batcode(条码)"，不完全依赖主键，可以拆分出去。我们把这3个字段加上它们所依赖的字段"barcode(条码)"，拆分形成新的数据表"商品信息表"。

这样一来，原来的数据表就被拆分成了两个表。

商品信息表：

<img src="D:\笔记\mysql.assets\image-20220707163807205.png" alt="image-20220707163807205" style="float:left;" />

进货单表：

<img src="D:\笔记\mysql.assets\image-20220707163828614.png" alt="image-20220707163828614" style="float:left;" />

此外，字段"supplierid(供应商编号)""suppliername(供应商名称)""stock(仓库)“只依赖于"listnumber(单号)"，不完全依赖于主键，所以，我们可以把"supplierid""suppliername""stock"这3个字段拆出去，再加上它们依赖的字段"listnumber(单号)"，就形成了一个新的表"进货单头表"。剩下的字段，会组成新的表，我们叫它"进货单明细表"。

原来的数据表就拆分成了3个表。

进货单头表：

![image-20220707164128704](D:\笔记\mysql.assets\image-20220707164128704.png)

进货单明细表：

![image-20220707164146216](D:\笔记\mysql.assets\image-20220707164146216.png)

商品信息表：

![image-20220707164227845](D:\笔记\mysql.assets\image-20220707164227845.png)

现在，我们再来分析一下拆分后的3个表，保证这3个表都满足第二范式的要求。

第3步，在“商品信息表”中，字段“barcode"是有`可能存在重复`的，比如，用户门店可能有散装称重商品和自产商品，会存在条码共用的情况。所以，所有的字段都不能唯一标识表里的记录。这个时候，我们必须给这个表加上一个主键，比如说是`自增字段"itemnumber"`。

###  迭代3次：考虑3NF

我们的进货单头表，还有数据冗余的可能。因为"suppliername"依赖"supplierid"，那么就可以按照第三范式的原则进行拆分了。我们就进一步拆分进货单头表，把它拆解陈供货商表和进货单头表。

供货商表：

<img src="D:\笔记\mysql.assets\image-20220707165011050.png" alt="image-20220707165011050" style="float:left;" />

进货单头表：

<img src="D:\笔记\mysql.assets\image-20220707165038108.png" alt="image-20220707165038108" style="float:left;" />

这2个表都满足第三范式的要求了。

###  反范式化：业务优先的原则

<img src="D:\笔记\mysql.assets\image-20220707165459547.png" alt="image-20220707165459547" style="zoom:80%;float:left" />

因此，最后我们可以把进货单表拆分成下面的4个表：

供货商表：

<img src="D:\笔记\mysql.assets\image-20220707165011050.png" alt="image-20220707165011050" style="float:left;" />

进货单头表：

<img src="D:\笔记\mysql.assets\image-20220707165038108.png" alt="image-20220707165038108" style="float:left;" />

进货单明细表：

<img src="D:\笔记\mysql.assets\image-20220707164146216.png" alt="image-20220707164146216" style="zoom:80%;float:left" />

商品信息表：

<img src="D:\笔记\mysql.assets\image-20220707164227845.png" alt="image-20220707164227845" style="zoom:80%;float:left" />

这样一来，我们就避免了冗余数据，而且还能够满足业务的需求，这样的数据库设计，才是合格的设计。

##  ER模型

<img src="D:\笔记\mysql.assets\image-20220707170027637.png" alt="image-20220707170027637" style="zoom:80%;float:left" />

### ER模型包括哪些要素？

**ER 模型中有三个要素，分别是实体、属性和关系。**

`实体` ，可以看做是数据对象，往往对应于现实生活中的真实存在的个体。在 ER 模型中，用 矩形 来表 示。实体分为两类，分别是 强实体 和 弱实体 。强实体是指不依赖于其他实体的实体；弱实体是指对另 一个实体有很强的依赖关系的实体。

`属性` ，则是指实体的特性。比如超市的地址、联系电话、员工数等。在 ER 模型中用 椭圆形 来表示。

`关系` ，则是指实体之间的联系。比如超市把商品卖给顾客，就是一种超市与顾客之间的联系。在 ER 模 型中用 菱形 来表示。

注意：实体和属性不容易区分。这里提供一个原则：我们要从系统整体的角度出发去看，**可以独立存在的是实体，不可再分的是属性**。也就是说，属性不能包含其他属性。

### 关系的类型

在 ER 模型的 3 个要素中，关系又可以分为 3 种类型，分别是 一对一、一对多、多对多。

`一对一` ：指实体之间的关系是一一对应的，比如个人与身份证信息之间的关系就是一对一的关系。一个人只能有一个身份证信息，一个身份证信息也只属于一个人。

`一对多` ：指一边的实体通过关系，可以对应多个另外一边的实体。相反，另外一边的实体通过这个关系，则只能对应唯一的一边的实体。比如说，我们新建一个班级表，而每个班级都有多个学生，每个学 生则对应一个班级，班级对学生就是一对多的关系。

`多对多` ：指关系两边的实体都可以通过关系对应多个对方的实体。比如在进货模块中，供货商与超市之 间的关系就是多对多的关系，一个供货商可以给多个超市供货，一个超市也可以从多个供货商那里采购 商品。再比如一个选课表，有许多科目，每个科目有很多学生选，而每个学生又可以选择多个科目，这 就是多对多的关系。

###  建模分析

ER 模型看起来比较麻烦，但是对我们把控项目整体非常重要。如果你只是开发一个小应用，或许简单设 计几个表够用了，一旦要设计有一定规模的应用，在项目的初始阶段，建立完整的 ER 模型就非常关键 了。开发应用项目的实质，其实就是 建模 。

我们设计的案例是 电商业务 ，由于电商业务太过庞大且复杂，所以我们做了业务简化，比如针对 SKU（StockKeepingUnit，库存量单位）和SPU（Standard Product Unit，标准化产品单元）的含义上，我 们直接使用了SKU，并没有提及SPU的概念。本次电商业务设计总共有8个实体，如下所示。

* 地址实体 
* 用户实体 
* 购物车实体 
* 评论实体 
* 商品实体 
* 商品分类实体 
* 订单实体 
* 订单详情实体

其中， 用户 和 商品分类 是强实体，因为它们不需要依赖其他任何实体。而其他属于弱实体，因为它们 虽然都可以独立存在，但是它们都依赖用户这个实体，因此都是弱实体。知道了这些要素，我们就可以 给电商业务创建 ER 模型了，如图：

![image-20220707170608782](D:\笔记\mysql.assets\image-20220707170608782.png)

在这个图中，地址和用户之间的添加关系，是一对多的关系，而商品和商品详情示一对1的关系，商品和 订单是多对多的关系。 这个 ER 模型，包括了 8个实体之间的 8种关系。

（1）用户可以在电商平台添加多个地址； 

（2）用户只能拥有一个购物车； 

（3）用户可以生成多个订单； 

（4）用户可以发表多条评论； 

（5）一件商品可以有多条评论； 

（6）每一个商品分类包含多种商品；

（7）一个订单可以包含多个商品，一个商品可以在多个订单里。 

（8）订单中又包含多个订单详情，因为一个订单中可能包含不同种类的商品

###  ER 模型的细化

有了这个 ER 模型，我们就可以从整体上 理解 电商的业务了。刚刚的 ER 模型展示了电商业务的框架， 但是只包括了订单，地址，用户，购物车，评论，商品，商品分类和订单详情这八个实体，以及它们之 间的关系，还不能对应到具体的表，以及表与表之间的关联。我们需要把 属性加上 ，用 椭圆 来表示， 这样我们得到的 ER 模型就更加完整了。

因此，我们需要进一步去设计一下这个 ER 模型的各个局部，也就是细化下电商的具体业务流程，然后把 它们综合到一起，形成一个完整的 ER 模型。这样可以帮助我们理清数据库的设计思路。

接下来，我们再分析一下各个实体都有哪些属性，如下所示。

（1） `地址实体` 包括用户编号、省、市、地区、收件人、联系电话、是否是默认地址。 

（2） `用户实体` 包括用户编号、用户名称、昵称、用户密码、手机号、邮箱、头像、用户级别。

（3） `购物车实体` 包括购物车编号、用户编号、商品编号、商品数量、图片文件url。

（4） `订单实体` 包括订单编号、收货人、收件人电话、总金额、用户编号、付款方式、送货地址、下单 时间。 

（5） `订单详情实体` 包括订单详情编号、订单编号、商品名称、商品编号、商品数量。 

（6） `商品实体` 包括商品编号、价格、商品名称、分类编号、是否销售，规格、颜色。 

（7） `评论实体` 包括评论id、评论内容、评论时间、用户编号、商品编号 

（8） `商品分类实体` 包括类别编号、类别名称、父类别编号

这样细分之后，我们就可以重新设计电商业务了，ER 模型如图：

![image-20220707171022246](D:\笔记\mysql.assets\image-20220707171022246.png)

###  ER 模型图转换成数据表

通过绘制 ER 模型，我们已经理清了业务逻辑，现在，我们就要进行非常重要的一步了：把绘制好的 ER 模型，转换成具体的数据表，下面介绍下转换的原则：

（1）一个 实体 通常转换成一个 数据表 ； 

（2）一个 多对多的关系 ，通常也转换成一个 数据表 ； 

（3）一个 1 对 1 ，或者 1 对多 的关系，往往通过表的 外键 来表达，而不是设计一个新的数据表； 

（4） 属性 转换成表的 字段 。

下面结合前面的ER模型，具体讲解一下怎么运用这些转换的原则，把 ER 模型转换成具体的数据表，从 而把抽象出来的数据模型，落实到具体的数据库设计当中。

####  一个实体转换成一个数据库

**先来看一下强实体转换成数据表:**

`用户实体`转换成用户表(user_info)的代码如下所示。

<img src="D:\笔记\mysql.assets\image-20220707171335255.png" alt="image-20220707171335255" style="float:left;" />

<img src="D:\笔记\mysql.assets\image-20220707171412363.png" alt="image-20220707171412363" style="float:left;" />

<img src="D:\笔记\mysql.assets\image-20220707171915637.png" alt="image-20220707171915637" style="float:left;" />

**下面我们再把弱实体转换成数据表：**

<img src="D:\笔记\mysql.assets\image-20220707172033399.png" alt="image-20220707172033399" style="float:left;" />

<img src="D:\笔记\mysql.assets\image-20220707172052236.png" alt="image-20220707172052236" style="float:left;" />

<img src="D:\笔记\mysql.assets\image-20220707172143793.png" alt="image-20220707172143793" style="float:left;" />

<img src="D:\笔记\mysql.assets\image-20220707172217772.png" alt="image-20220707172217772" style="float:left;" />

<img src="D:\笔记\mysql.assets\image-20220707172236606.png" alt="image-20220707172236606" style="float:left;" />

<img src="D:\笔记\mysql.assets\image-20220707172259143.png" alt="image-20220707172259143" style="float:left;" />

#### 一个多对多的关系转换成一个数据表

<img src="D:\笔记\mysql.assets\image-20220707172350226.png" alt="image-20220707172350226" style="float:left;" />

#### 通过外键来表达1对多的关系

<img src="D:\笔记\mysql.assets\image-20220707172609833.png" alt="image-20220707172609833" style="float:left;" />

#### 把属性转换成表的字段

<img src="D:\笔记\mysql.assets\image-20220707172819174.png" alt="image-20220707172819174" style="float:left;" />

![image-20220707172918017](D:\笔记\mysql.assets\image-20220707172918017.png)

## 数据表的设计原则

综合以上内容，总结出数据表设计的一般原则："三少一多"

**1. 数据表的个数越少越好**

<img src="D:\笔记\mysql.assets\image-20220707173028203.png" alt="image-20220707173028203" style="float:left;" />

**2. 数据表中的字段个数越少越好**

<img src="D:\笔记\mysql.assets\image-20220707173402491.png" alt="image-20220707173402491" style="float:left;" />

**3. 数据表中联合主键的字段个数越少越好**

<img src="D:\笔记\mysql.assets\image-20220707173522971.png" alt="image-20220707173522971" style="float:left;" />

**4. 使用主键和外键越多越好**

<img src="D:\笔记\mysql.assets\image-20220707173557568.png" alt="image-20220707173557568" style="float:left;" />

##  数据库对象编写建议

###  关于库

1. 【强制】库的名称必须控制在32个字符以内，只能使用英文字母、数字和下划线，建议以英文字 母开头。 
2. 【强制】库名中英文 一律小写 ，不同单词采用 下划线 分割。须见名知意。 
3. 【强制】库的名称格式：业务系统名称_子系统名。
4. 【强制】库名禁止使用关键字（如type,order等）。
5. 【强制】创建数据库时必须 显式指定字符集 ，并且字符集只能是utf8或者utf8mb4。 创建数据库SQL举例：CREATE DATABASE crm_fund DEFAULT CHARACTER SET 'utf8' ; 
6. 【建议】对于程序连接数据库账号，遵循 权限最小原则 使用数据库账号只能在一个DB下使用，不准跨库。程序使用的账号 原则上不准有drop权限 。 
7. 【建议】临时库以 tmp_ 为前缀，并以日期为后缀； 备份库以 bak_ 为前缀，并以日期为后缀。

### 关于表、列

1. 【强制】表和列的名称必须控制在32个字符以内，表名只能使用英文字母、数字和下划线，建议 以 英文字母开头 。 

2. 【强制】 表名、列名一律小写 ，不同单词采用下划线分割。须见名知意。 

3. 【强制】表名要求有模块名强相关，同一模块的表名尽量使用 统一前缀 。比如：crm_fund_item 

4. 【强制】创建表时必须 显式指定字符集 为utf8或utf8mb4。 

5. 【强制】表名、列名禁止使用关键字（如type,order等）。 

6. 【强制】创建表时必须 显式指定表存储引擎 类型。如无特殊需求，一律为InnoDB。 

7. 【强制】建表必须有comment。 

8. 【强制】字段命名应尽可能使用表达实际含义的英文单词或 缩写 。如：公司 ID，不要使用 corporation_id, 而用corp_id 即可。 

9. 【强制】布尔值类型的字段命名为 is_描述 。如member表上表示是否为enabled的会员的字段命 名为 is_enabled。 

10. 【强制】禁止在数据库中存储图片、文件等大的二进制数据 通常文件很大，短时间内造成数据量快速增长，数据库进行数据库读取时，通常会进行大量的随 机IO操作，文件很大时，IO操作很耗时。通常存储于文件服务器，数据库只存储文件地址信息。 

11. 【建议】建表时关于主键： 表必须有主键

	 (1)强制要求主键为id，类型为int或bigint，且为 auto_increment 建议使用unsigned无符号型。

	 (2)标识表里每一行主体的字段不要设为主键，建议 设为其他字段如user_id，order_id等，并建立unique key索引。因为如果设为主键且主键值为随机 插入，则会导致innodb内部页分裂和大量随机I/O，性能下降。 

12. 【建议】核心表（如用户表）必须有行数据的 创建时间字段 （create_time）和 最后更新时间字段 （update_time），便于查问题。 

13. 【建议】表中所有字段尽量都是 NOT NULL 属性，业务可以根据需要定义 DEFAULT值 。 因为使用 NULL值会存在每一行都会占用额外存储空间、数据迁移容易出错、聚合函数计算结果偏差等问 题。 

14. 【建议】所有存储相同数据的 列名和列类型必须一致 （一般作为关联列，如果查询时关联列类型 不一致会自动进行数据类型隐式转换，会造成列上的索引失效，导致查询效率降低）。 

15. 【建议】中间表（或临时表）用于保留中间结果集，名称以 tmp_ 开头。 备份表用于备份或抓取源表快照，名称以 bak_ 开头。中间表和备份表定期清理。 1

16. 【示范】一个较为规范的建表语句：

```mysql
CREATE TABLE user_info (
`id` int unsigned NOT NULL AUTO_INCREMENT COMMENT '自增主键',
`user_id` bigint(11) NOT NULL COMMENT '用户id',
`username` varchar(45) NOT NULL COMMENT '真实姓名',
`email` varchar(30) NOT NULL COMMENT '用户邮箱',
`nickname` varchar(45) NOT NULL COMMENT '昵称',
`birthday` date NOT NULL COMMENT '生日',
`sex` tinyint(4) DEFAULT '0' COMMENT '性别',
`short_introduce` varchar(150) DEFAULT NULL COMMENT '一句话介绍自己，最多50个汉字',
`user_resume` varchar(300) NOT NULL COMMENT '用户提交的简历存放地址',
`user_register_ip` int NOT NULL COMMENT '用户注册时的源ip',
`create_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
`update_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE
CURRENT_TIMESTAMP COMMENT '修改时间',
`user_review_status` tinyint NOT NULL COMMENT '用户资料审核状态，1为通过，2为审核中，3为未
通过，4为还未提交审核',
PRIMARY KEY (`id`),
UNIQUE KEY `uniq_user_id` (`user_id`),
KEY `idx_username`(`username`),
KEY `idx_create_time_status`(`create_time`,`user_review_status`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT='网站用户基本信息
```

17. 【建议】创建表时，可以使用可视化工具。这样可以确保表、字段相关的约定都能设置上。

实际上，我们通常很少自己写 DDL 语句，可以使用一些可视化工具来创建和操作数据库和数据表。

可视化工具除了方便，还能直接帮我们将数据库的结构定义转化成 SQL 语言，方便数据库和数据表结构的导出和导入。

###  关于索引

1. 【强制】InnoDB表必须主键为id int/bigint auto_increment，且主键值 禁止被更新 。 
2. 【强制】InnoDB和MyISAM存储引擎表，索引类型必须为 BTREE 。 
3. 【建议】主键的名称以 pk_ 开头，唯一键以 uni_ 或 uk_ 开头，普通索引以 idx_ 开头，一律使用小写格式，以字段的名称或缩写作为后缀。 
4. 【建议】多单词组成的columnname，取前几个单词首字母，加末单词组成column_name。如: sample 表 member_id 上的索引：idx_sample_mid。 
5. 【建议】单个表上的索引个数 不能超过6个 。 
6. 【建议】在建立索引时，多考虑建立 联合索引 ，并把区分度最高的字段放在最前面。 
7. 【建议】在多表 JOIN 的SQL里，保证被驱动表的连接列上有索引，这样JOIN 执行效率最高。 
8. 【建议】建表或加索引时，保证表里互相不存在 冗余索引 。 比如：如果表里已经存在key(a,b)， 则key(a)为冗余索引，需要删除。

###  SQL编写

1. 【强制】程序端SELECT语句必须指定具体字段名称，禁止写成 *。 
2. 【建议】程序端insert语句指定具体字段名称，不要写成INSERT INTO t1 VALUES(…)。 
3. 【建议】除静态表或小表（100行以内），DML语句必须有WHERE条件，且使用索引查找。 
4. 【建议】INSERT INTO…VALUES(XX),(XX),(XX).. 这里XX的值不要超过5000个。 值过多虽然上线很 快，但会引起主从同步延迟。 
5. 【建议】SELECT语句不要使用UNION，推荐使用UNION ALL，并且UNION子句个数限制在5个以 内。 
6. 【建议】线上环境，多表 JOIN 不要超过5个表。 
7. 【建议】减少使用ORDER BY，和业务沟通能不排序就不排序，或将排序放到程序端去做。ORDER BY、GROUP BY、DISTINCT 这些语句较为耗费CPU，数据库的CPU资源是极其宝贵的。 
8. 【建议】包含了ORDER BY、GROUP BY、DISTINCT 这些查询的语句，WHERE 条件过滤出来的结果 集请保持在1000行以内，否则SQL会很慢。 
9. 【建议】对单表的多次alter操作必须合并为一次 对于超过100W行的大表进行alter table，必须经过DBA审核，并在业务低峰期执行，多个alter需整 合在一起。 因为alter table会产生 表锁 ，期间阻塞对于该表的所有写入，对于业务可能会产生极 大影响。 
10. 【建议】批量操作数据时，需要控制事务处理间隔时间，进行必要的sleep。 
11. 【建议】事务里包含SQL不超过5个。 因为过长的事务会导致锁数据较久，MySQL内部缓存、连接消耗过多等问题。 
12. 【建议】事务里更新语句尽量基于主键或UNIQUE KEY，如UPDATE… WHERE id=XX; 否则会产生间隙锁，内部扩大锁定范围，导致系统性能下降，产生死锁。

## PowerDesigner的使用

PowerDesigner是一款开发人员常用的数据库建模工具，用户利用该软件可以方便地制作 `数据流程图` 、 `概念数据模型` 、 `物理数据模型` ，它几乎包括了数据库模型设计的全过程，是Sybase公司为企业建模和设 计提供的一套完整的集成化企业级建模解决方案。

###  开始界面

当前使用的PowerDesigner版本是16.5的。打开软件即是此页面，可选择Create Model,也可以选择Do Not Show page Again,自行在打开软件后创建也可以！完全看个人的喜好，在此我在后面的学习中不在显示此页面。

<img src="D:\笔记\mysql.assets\image-20220707175250944.png" alt="image-20220707175250944" style="zoom:80%;float:left" />

“Create Model”的作用类似于普通的一个文件，该文件可以单独存放也可以归类存放。

 “Create Project”的作用类似于文件夹，负责把有关联关系的文件集中归类存放。

###  概念数据模型

常用的模型有4种，分别是 `概念模型(CDM Conceptual Data Model)` ， `物理模型（PDM,Physical Data Model）` ， `面向对象的模型（OOM Objcet Oriented Model）` 和 `业务模型（BPM Business Process Model）` ，我们先创建概念数据模型。

<img src="D:\笔记\mysql.assets\image-20220707175350250.png" alt="image-20220707175350250" style="float:left;" />

点击上面的ok，即可出现下图左边的概念模型1，可以自定义概念模型的名字，在概念模型中使用最多的 就是如图所示的Entity(实体),Relationship(关系)

<img src="D:\笔记\mysql.assets\image-20220707175604026.png" alt="image-20220707175604026" style="float:left;" />

**Entity实体**

选中右边框中Entity这个功能，即可出现下面这个方框，需要注意的是书写name的时候，code自行补全，name可以是英文的也可以是中文的，但是code必须是英文的。

<img src="D:\笔记\mysql.assets\image-20220707175653689.png" alt="image-20220707175653689" style="float:left;" />

**填充实体字段**

General中的name和code填好后，就可以点击Attributes（属性）来设置name（名字），code(在数据库中 的字段名)，Data Type(数据类型) ，length(数据类型的长度)

* Name: 实体名字一般为中文，如论坛用户 
* Code: 实体代号，一般用英文，如XXXUser 
* Comment:注释，对此实体详细说明 
* Code属性：代号，一般用英文UID DataType 
* Domain域，表示属性取值范围如可以创建10个字符的地址域 
* M:Mandatory强制属性，表示该属性必填。不能为空 
* P:Primary Identifer是否是主标识符，表示实体唯一标识符 
* D:Displayed显示出来，默认全部勾选

<img src="D:\笔记\mysql.assets\image-20220707175805226.png" alt="image-20220707175805226" style="float:left;" />

在此上图说明name和code的起名方法

<img src="D:\笔记\mysql.assets\image-20220707175827417.png" alt="image-20220707175827417" style="float:left;" />

**设置主标识符**

如果不希望系统自动生成标识符而是手动设置的话，那么切换到Identifiers选项卡，添加一行Identifier， 然后单击左上角的“属性”按钮，然后弹出的标识属性设置对话框中单击“添加行”按钮，选择该标识中使用的属性。例如将学号设置为学生实体的标识。

<img src="D:\笔记\mysql.assets\image-20220707175858031.png" alt="image-20220707175858031" style="float:left;" />

**放大模型**

创建好概念数据模型如图所示，但是创建好的字体很小，读者可以按着ctrl键同时滑动鼠标的可滑动按钮 即可放大缩写字体，同时也可以看到主标识符有一个*号的标志，同时也显示出来了，name,Data type和 length这些可见的属性

<img src="D:\笔记\mysql.assets\image-20220707175925155.png" alt="image-20220707175925155" style="float:left;" />

**实体关系**

同理创建一个班级的实体（需要特别注意的是，点击完右边功能的按钮后需要点击鼠标指针状态的按钮 或者右击鼠标即可，不然很容易乱操作，这点注意一下就可以了），然后使用Relationship（关系）这个 按钮可以连接学生和班级之间的关系，发生一对多（班级对学生）或者多对一（学生对班级）的关系。 

如图所示

<img src="D:\笔记\mysql.assets\image-20220707175954634.png" alt="image-20220707175954634" style="float:left;" />

需要注意的是点击Relationship这个按钮，就把班级和学生联系起来了，就是一条线，然后双击这条线进 行编辑，在General这块起name和code

<img src="D:\笔记\mysql.assets\image-20220707180021612.png" alt="image-20220707180021612" style="float:left;" />

上面的name和code起好后就可以在Cardinalities这块查看班级和学生的关系，可以看到班级的一端是一 条线，学生的一端是三条，代表班级对学生是一对多的关系即one对many的关系，点击应用，然后确定 即可

<img src="D:\笔记\mysql.assets\image-20220707180044291.png" alt="image-20220707180044291" style="float:left;" />

一对多和多对一练习完还有多对多的练习，如下图操作所示，老师实体和上面介绍的一样，自己将 name，data type等等修改成自己需要的即可，满足项目开发需求即可。（comment是解释说明，自己可以写相关的介绍和说明）

<img src="D:\笔记\mysql.assets\image-20220707180113532.png" alt="image-20220707180113532" style="float:left;" />

多对多需要注意的是自己可以手动点击按钮将关系调整称为多对多的关系many对many的关系，然后点击应用和确定即可

<img src="D:\笔记\mysql.assets\image-20220707180159184.png" alt="image-20220707180159184" style="float:left;" />

综上即可完成最简单的学生，班级，教师这种概念数据模型的设计，需要考虑数据的类型和主标识码， 是否为空。关系是一对一还是一对多还是多对多的关系，自己需要先规划好再设计，然后就ok了。

![image-20220707180254510](D:\笔记\mysql.assets\image-20220707180254510.png)

### 物理数据模型

上面是概念数据模型，下面介绍一下物理数据模型，以后 经常使用 的就是物理数据模型。打开 PowerDesigner，然后点击File-->New Model然后选择如下图所示的物理数据模型，物理数据模型的名字自己起，然后选择自己所使用的数据库即可。

<img src="D:\笔记\mysql.assets\image-20220707180327712.png" alt="image-20220707180327712" style="float:left;" />

创建好主页面如图所示，但是右边的按钮和概念模型略有差别，物理模型最常用的三个是 `table(表)` ， `view(视图)`， `reference(关系) `；

<img src="D:\笔记\mysql.assets\image-20220707180418090.png" alt="image-20220707180418090" style="float:left;" />

鼠标先点击右边table这个按钮然后在新建的物理模型点一下，即可新建一个表，然后双击新建如下图所示，在General的name和code填上自己需要的，点击应用即可），如下图：

<img src="D:\笔记\mysql.assets\image-20220707180449212.png" alt="image-20220707180449212" style="float:left;" />

然后点击Columns,如下图设置，非常简单，需要注意的就是P（primary主键） , F （foreign key外键） , M（mandatory强制性的，代表不可为空） 这三个。

<img src="D:\笔记\mysql.assets\image-20220707180537251.png" alt="image-20220707180537251" style="float:left;" />

在此设置学号的自增（MYSQL里面的自增是这个AUTO_INCREMENT），班级编号同理，不多赘述！

<img src="D:\笔记\mysql.assets\image-20220707180556645.png" alt="image-20220707180556645" style="float:left;" />

在下面的这个点上对号即可，就设置好了自增

<img src="D:\笔记\mysql.assets\image-20220707180619440.png" alt="image-20220707180619440" style="float:left;" />

全部完成后如下图所示。

<img src="D:\笔记\mysql.assets\image-20220707180643107.png" alt="image-20220707180643107" style="float:left;" />

班级物理模型同理如下图所示创建即可

<img src="D:\笔记\mysql.assets\image-20220707180723698.png" alt="image-20220707180723698" style="float:left;" />

<img src="D:\笔记\mysql.assets\image-20220707180744600.png" alt="image-20220707180744600" style="float:left;" />

完成后如下图所示

<img src="D:\笔记\mysql.assets\image-20220707180806150.png" alt="image-20220707180806150" style="float:left;" />

上面的设置好如上图所示，然后下面是关键的地方，点击右边按钮Reference这个按钮，因为是班级对学 生是一对多的，所以鼠标从学生拉到班级如下图所示，学生表将发生变化，学生表里面增加了一行，这 行是班级表的主键作为学生表的外键，将班级表和学生表联系起来。（仔细观察即可看到区别。）

<img src="D:\笔记\mysql.assets\image-20220707180828164.png" alt="image-20220707180828164" style="float:left;" />

做完上面的操作，就可以双击中间的一条线，显示如下图，修改name和code即可

<img src="D:\笔记\mysql.assets\image-20220707183743297.png" alt="image-20220707183743297" style="float:left;" />

但是需要注意的是，修改完毕后显示的结果却如下图所示，并没有办法直接像概念模型那样，修改过后 显示在中间的那条线上面，自己明白即可。

<img src="D:\笔记\mysql.assets\image-20220707193816176.png" alt="image-20220707193816176" style="float:left;" />

学习了多对一或者一对多的关系，接下来学习多对对的关系，同理自己建好老师表，这里不在叙述，记得老师编号自增，建好如下图所示

<img src="D:\笔记\mysql.assets\image-20220707193932694.png" alt="image-20220707193932694" style="float:left;" />

下面是多对多关系的关键，由于物理模型多对多的关系需要一个中间表来连接，如下图，只设置一个字 段，主键，自增

<img src="D:\笔记\mysql.assets\image-20220707193957629.png" alt="image-20220707193957629" style="float:left;" />

点击应用，然后设置Columns，只添加一个字段

<img src="D:\笔记\mysql.assets\image-20220707194048843.png" alt="image-20220707194048843" style="float:left;" />

这是设置字段递增，前面已经叙述过好几次

<img src="D:\笔记\mysql.assets\image-20220707194111885.png" alt="image-20220707194111885" style="float:left;" />

设置好后如下图所示，需要注意的是有箭头的一方是一，无箭头的一方是多，即一对多的多对一的关系 需要搞清楚，学生也可以有很多老师，老师也可以有很多学生，所以学生和老师都可以是主体；

<img src="D:\笔记\mysql.assets\image-20220707194138137.png" alt="image-20220707194138137" style="float:left;" />

可以看到添加关系以后学生和教师的关系表前后发生的变化

<img src="D:\笔记\mysql.assets\image-20220707194158936.png" alt="image-20220707194158936" style="float:left;" />

### 概念模型转为物理模型

1：如下图所示先打开概念模型图，然后点击Tool,如下图所示

![image-20220707194228064](D:\笔记\mysql.assets\image-20220707194228064.png)

点开的页面如下所示，name和code已经从概念模型1改成物理模型1了

![image-20220707194248236](D:\笔记\mysql.assets\image-20220707194248236.png)

完成后如下图所示，将自行打开修改的物理模型，需要注意的是这些表的数据类型已经自行改变了，而 且中间表出现两个主键，即双主键

![image-20220707194308595](D:\笔记\mysql.assets\image-20220707194308595.png)

### 物理模型转为概念模型

上面介绍了概念模型转物理模型，下面介绍一下物理模型转概念模型（如下图点击操作即可）

![image-20220707194405358](D:\笔记\mysql.assets\image-20220707194405358.png)

然后出现如下图所示界面，然后将物理修改为概念 ，点击应用确认即可

![image-20220707194419360](D:\笔记\mysql.assets\image-20220707194419360.png)

点击确认后将自行打开如下图所示的页面，自己观察有何变化，如果转换为oracle的，数据类型会发生变 化，比如Varchar2等等）；

![image-20220707194433407](D:\笔记\mysql.assets\image-20220707194433407.png)

###  11.6 物理模型导出SQL语句

![image-20220707194544714](D:\笔记\mysql.assets\image-20220707194544714.png)

打开之后如图所示，修改好存在sql语句的位置和生成文件的名称即可

![image-20220707194557554](D:\笔记\mysql.assets\image-20220707194557554.png)

在Selection中选择需要导出的表，然后点击应用和确认即可

![image-20220707194637242](D:\笔记\mysql.assets\image-20220707194637242.png)

完成以后出现如下图所示，可以点击Edit或者close按钮

![image-20220707194727849](D:\笔记\mysql.assets\image-20220707194727849.png)

自此，就完成了导出sql语句，就可以到自己指定的位置查看导出的sql语句了；PowerDesigner在以后在 项目开发过程中用来做需求分析和数据库的设计非常的方便和快捷。
