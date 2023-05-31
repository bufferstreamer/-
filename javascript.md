# JavaScript

基本同java

JavaScript的实现包括以下3个部分：

| [ECMAScript](https://so.csdn.net/so/search?q=ECMAScript&spm=1001.2101.3001.7020)(核心) | 描述了JS的语法和基本对象。 |
| ------------------------------------------------------------ | -------------------------- |
| 文档对象模型 （DOM）                                         | 处理网页内容的方法和接口   |
| 浏览器对象模型（BOM）                                        | 与浏览器交互的方法和接口   |

DOM 是为了操作文档出现的 API，document 是其的一个对象；
		BOM 是为了操作浏览器出现的 API，window 是其的一个对象。

BOM是浏览器对象模型，DOM是[文档对象模型](https://www.baidu.com/s?wd=文档对象模型&tn=44039180_cpr&fenlei=mv6quAkxTZn0IZRqIHcvrjTdrH00T1Y4mvn3mWKWmWT4nW99myRv0ZwV5Hcvrjm3rH6sPfKWUMw85HfYnjn4nH6sgvPsT6KdThsqpZwYTjCEQLGCpyw9Uz4Bmy-bIi4WUvYETgN-TLwGUv3EnHTvPH6Ln1DzPH0snWR1P1fvn0)，前者是对浏览器本身进行操作，而后者是对浏览器（可看成容器）内的内容进行操作

## 规则

var定义变量，该变量为全局作用域，重复定义时会覆盖前面的变量

let定义变量，改变量为局部作用域，重复定义会报错

const定义静态变量，当定义的是基本类型时，该变量无法被改变。当定义的是引用类型的时候，该变量只是指向该对象的地址，对象内部的数据可以被改变。当对象内部不想被改变时，可以使用Object.freeze（对象名）来冻结

用{}来表示一个对象

引用类型默认值为null 

基本类型默认值为undefined  当函数和形参没有被定义的时候也会返回undefined

for in 遍历的是键

for of 遍历的是值

``可以在字符串拼接时起到很好的作用

如<img src="C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220218135223470.png" alt="image-20220218135223470" style="zoom:50%;" />



### ...语法

可以使数量不定得多个元素变得可以用一个变量来接受



#### 聚合

...在等号左边时相当于聚合功能

![image-20220225155701726](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220225155701726.png)

![image-20220225155718455](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220225155718455.png)

![image-20220225155740405](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220225155740405.png)

![image-20220225155757605](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220225155757605.png)

![image-20220225160340169](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220225160340169.png)

![image-20220225160406153](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220225160406153.png)

![image-20220225160655184](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220225160655184.png)

![image-20220225160707565](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220225160707565.png)

#### 分散

...在等号右边时相当于分散功能

![image-20220225161534186](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220225161534186.png)

![image-20220225161555098](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220225161555098.png)



### 比较和转换

数组或者对象在转换为布尔类型时会被转换为真

但是在与布尔类型比较时，内部有一个元素会转换为真，没有元素会转换为假，多个元素会转换为NaN

## 函数

定义：var 函数名 = function （）{}

			或者  function 函数名 （）{}

其中函数参数定义个数和实际传入的参数个数可以不相等，js提供了一个关键字arguments数组来获取形参个数。还引入了rest来表示<img src="C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220115145920259.png" alt="image-20220115145920259" style="zoom:67%;" />

## 字符串

String.length

String.toUpperCase()	转为大写

String.toLowerCase()	转为小写

String.trim()	删除空白

### 字符串截取函数

String.slice(number start,number end)	 start为负数时，从后往前截取

String.sbustring(number start,number end)	start为负数时，视为0

String.substr(number start,number mount)	start为负数时，从后往前截取

### 字符串检索

String.indexOf(string character,number start_location)	找不到返回-1 

String.lastIndexOf(string character,number start_location)	找不到返回-1  从右侧开始查

String.includes(string character,number start_location)	找不到返回FALSE

String.startsWith(string character)

String.endsWith(string character)

### 字符串分割



#### split

`split` 方法用于将字符串分割成数组，类似`join`方法的反函数。

```javascript
let price = "99,78,68";
console.log(price.split(",")); //["99", "78", "68"]
```

####

## 数组

### 管理元素

#### push和unshift

push从数组后面添加元素

unshift从数组前面添加元素

#### pop和shift

pop从数组后面弹出元素

shift从数组前面弹出元素

#### fill

给数组填充元素

Array.fill("填充得内容"，开始位置，结束位置)

#### slice和splice

Array.slice(开始位置，结束位置)返回原数组被截断的数组，不改变原数组

Array.splice(开始位置，截取个数，替换的元素)原数组被改变

#### 清空数组

将数组值修改为`[]`可以清空数组，如果有多个引用时数组在内存中存在被其他变量引用。

```javascript
let user = [{ name: "hdcms" }, { name: "后盾人" }];
let cms = user;
user = [];
console.log(user);
console.log(cms);
```

将数组`length`设置为0也可以清空数组

```javascript
let user = [{ name: "hdcms" }, { name: "后盾人" }];
user.length = 0;
console.log(user);
```

使用`splice`方法删除所有数组元素

```javascript
let user = [{ name: "hdcms" }, { name: "后盾人" }];
user.splice(0, user.length);
console.log(user);
```

使用`pop/shift`删除所有元素，来清空数组

```javascript
let user = [{ name: "hdcms" }, { name: "后盾人" }];
while (user.pop()) {}
console.log(user);
```

### 合并拆分

#### join

Array.join("连接得符号") 将数组各个元素以指定得符号进行连接，返回一个连接后得字符串

#### concat

`concat`方法用于连接两个或多个数组，元素是值类型的是复制操作，如果是引用类型还是指向同一对象

```javascript
let array = ["hdcms", "houdunren"];
let hd = [1, 2];
let cms = [3, 4];
console.log(array.concat(hd, cms)); //["hdcms", "houdunren", 1, 2, 3, 4]
```

也可以使用扩展语法实现连接

```javascript
console.log([...array, ...hd, ...cms]);
```

#### copyWithin

使用 `copyWithin` 从数组中复制一部分到同数组中的另外位置。

语法说明

```javascript
array.copyWithin(target, start, end)
```

参数说明

| 参数     | 描述                                                         |
| :------- | :----------------------------------------------------------- |
| *target* | 必需。复制到指定目标索引位置。                               |
| *start*  | 可选。元素复制的起始位置。                                   |
| *end*    | 可选。停止复制的索引位置 (默认为 *array*.length)。如果为负值，表示倒数。 |

```javascript
const arr = [1, 2, 3, 4];
console.log(arr.copyWithin(2, 0, 2)); //[1, 2, 1, 2]
```

### 查找元素

#### indexOf

使用 `indexOf` 从前向后查找元素出现的位置，如果找不到返回 `-1`。

```javascript
let arr = [7, 3, 2, 8, 2, 6];
console.log(arr.indexOf(2)); // 2 从前面查找2出现的位置
```

如下面代码一下，使用 `indexOf` 查找字符串将找不到，因为`indexOf` 类似于`===`是严格类型约束。

```javascript
let arr = [7, 3, 2, '8', 2, 6];
console.log(arr.indexOf(8)); // -1
```

第二个参数用于指定查找开始位置

```javascript
let arr = [7, 3, 2, 8, 2, 6];
//从第二个元素开始向后查找
console.log(arr.indexOf(2, 3)); //4
```

#### lastIndexOf

使用 `lastIndexOf` 从后向前查找元素出现的位置，如果找不到返回 `-1`。

```javascript
let arr = [7, 3, 2, 8, 2, 6];
console.log(arr.lastIndexOf(2)); // 4 从后查找2出现的位置
```

第二个参数用于指定查找开始位置

```javascript
let arr = [7, 3, 2, 8, 2, 6];
//从第五个元素向前查找
console.log(arr.lastIndexOf(2, 5));

//从最后一个字符向前查找
console.log(arr.lastIndexOf(2, -2));
```

#### includes

使用 `includes` 查找字符串返回值是布尔类型更方便判断

```javascript
let arr = [7, 3, 2, 6];
console.log(arr.includes(6)); //true
```

我们来实现一个自已经的`includes`函数，来加深对`includes`方法的了解

```javascript
function includes(array, item) {
  for (const value of array)
    if (item === value) return true;
  return false;
}

console.log(includes([1, 2, 3, 4], 3)); //true
```

#### find

find 方法找到后会把值返回出来

- 如果找不到返回值为`undefined`

返回第一次找到的值，不继续查找

```javascript
let arr = ["hdcms", "houdunren", "hdcms"];

let find = arr.find(function(item) {
  return item == "hdcms";
});

console.log(find); //hdcms
```

使用`includes`等不能查找引用类型，因为它们的内存地址是不相等的

```javascript
const user = [{ name: "李四" }, { name: "张三" }, { name: "后盾人" }];
const find = user.includes({ name: "后盾人" });
console.log(find);
```

`find` 可以方便的查找引用类型

```javascript
const user = [{ name: "李四" }, { name: "张三" }, { name: "后盾人" }];
const find = user.find(user => (user.name = "后盾人"));
console.log(find);
```

#### findIndex

`findIndex` 与 `find` 的区别是返回索引值，参数也是 : 当前值，索引，操作数组。

- 查找不到时返回 `-1`

```javascript
let arr = [7, 3, 2, '8', 2, 6];

console.log(arr.findIndex(function (v) {
	return v == 8;
})); //3
```

#### find原理

下面使用自定义函数

```javascript
let arr = [1, 2, 3, 4, 5];
function find(array, callback) {
  for (const value of array) {
    if (callback(value) === true) return value;
  }
  return undefined;
}
let res = find(arr, function(item) {
  return item == 23;
});
console.log(res);
```

下面添加原型方法实现

```javascript
Array.prototype.findValue = function(callback) {
  for (const value of this) {
    if (callback(value) === true) return value;
  }
  return undefined;
};

let re = arr.findValue(function(item) {
  return item == 2;
});
console.log(re);

```

### 循环遍历

#### for

根据数组长度结合`for` 循环来遍历数组

```text
let lessons = [
	{title: '媒体查询响应式布局',category: 'css'},
  {title: 'FLEX 弹性盒模型',category: 'css'},
	{title: 'MYSQL多表查询随意操作',category: 'mysql'}
];

for (let i = 0; i < lessons.length; i++) {
  lessons[i] = `后盾人: ${lessons[i].title}`;
}
console.log(lessons);
```

#### forEach

`forEach`使函数作用在每个数组元素上，但是没有返回值。

下面例子是截取标签的五个字符。

```text
let lessons = [
	{title: '媒体查询响应式布局',category: 'css'},
  {title: 'FLEX 弹性盒模型',category: 'css'},
	{title: 'MYSQL多表查询随意操作',category: 'mysql'}
];

lessons.forEach((item, index, array) => {
    item.title = item.title.substr(0, 5);
});
console.log(lessons);
```

#### for/in

遍历时的 key 值为数组的索引

```text
let lessons = [
	{title: '媒体查询响应式布局',category: 'css'},
  {title: 'FLEX 弹性盒模型',category: 'css'},
	{title: 'MYSQL多表查询随意操作',category: 'mysql'}
];

for (const key in lessons) {
    console.log(`标题: ${lessons[key].title}`);
}
```

#### for/of

与 `for/in` 不同的是 `for/of` 每次循环取其中的值而不是索引。

```javascript
let lessons = [
	{title: '媒体查询响应式布局',category: 'css'},
  {title: 'FLEX 弹性盒模型',category: 'css'},
	{title: 'MYSQL多表查询随意操作',category: 'mysql'}
];

for (const item of lessons) {
  console.log(`
    标题: ${item.title}
    栏目: ${item.category == "css" ? "前端" : "数据库"}
  `);
}
```

使用数组的迭代对象遍历获取索引与值（有关迭代器知识后面章节会讲到）

```js
const hd = ['houdunren', 'hdcms'];
const iterator = hd.entries();
console.log(iterator.next()); //value:{0:0,1:'houdunren'}
console.log(iterator.next()); //value:{0:1,1:'hdcms'}
```

这样就可以使用解构特性与 `for/of` 遍历并获取索引与值了

```javascript
const hd = ["hdcms", "houdunren"];

for (const [key, value] of hd.entries()) {
  console.log(key, value); //这样就可以遍历了
}
```

取数组中的最大值

```js
function arrayMax(array) {
  let max = array[0];
  for (const elem of array) {
    max = max > elem ? max : elem;
  }
  return max;
}

console.log(arrayMax([1, 3, 2, 9]));
```

### 迭代器

#### keys

通过迭代对象获取索引

```javascript
const hd = ["houdunren", "hdcms"];
const keys = hd.keys();
console.log(keys.next());
console.log(keys.next());
```

获取数组所有键

```javascript
"use strict";
const arr = ["a", "b", "c", "后盾人"];

for (const key of arr.keys()) {
  console.log(key);
}
```

使用while遍历

```javascript
let arr = ["hdcms", "houdunren"];
while (({ value, done } = values.keys()) && done === false) {
	console.log(value);
}
```

####  values

通过迭代对象获取值

```javascript
const hd = ["houdunren", "hdcms"];
const values = hd.values();
console.log(values.next());
console.log(values.next());
console.log(values.next());
```

获取数组的所有值

```javascript
"use strict";
const arr = ["a", "b", "c", "后盾人"];

for (const value of arr.values()) {
  console.log(value);
}
```

#### entries

返回数组所有键值对，下面使用解构语法循环

```javascript
const arr = ["a", "b", "c", "后盾人"];
for (const [key, value] of arr.entries()) {
  console.log(key, value);
}
```

解构获取内容（对象章节会详细讲解）

```js
const hd = ["houdunren", "hdcms"];
const iterator = hd.entries();

let {done,value: [k, v]} = iterator.next();

console.log(v);
```

### 扩展方法



#### every

`every` 用于递归的检测元素，要所有元素操作都要返回真结果才为真。

查看班级中同学的JS成绩是否都及格

```js
const user = [
  { name: "李四", js: 89 },
  { name: "马六", js: 55 },
  { name: "张三", js: 78 }
];
const resust = user.every(user => user.js >= 60);
console.log(resust);
```

标题的关键词检查

```js
let words = ['后盾', '北京', '培训'];
let title = '后盾人不断分享技术教程';

let state = words.every(function (item, index, array) {
  return title.indexOf(item) >= 0;
});

if (state == false) console.log('标题必须包含所有关键词');
```

#### some

使用 `some` 函数可以递归的检测元素，如果有一个返回true，表达式结果就是真。第一个参数为元素，第二个参数为索引，第三个参数为原数组。

下面是使用 `some` 检测规则关键词的示例，如果匹配到一个词就提示违规。

```js
let words = ['后盾', '北京', '武汉'];
let title = '后盾人不断分享技术教程'

let state = words.some(function (item, index, array) {
	return title.indexOf(item) >= 0;
});

if (state) console.log('标题含有违规关键词');
```

#### filter

使用 `filter` 可以过滤数据中元素，下面是获取所有在CSS栏目的课程。

```js
let lessons = [
  {title: '媒体查询响应式布局',category: 'css'},
  {title: 'FLEX 弹性盒模型',category: 'css'},
  {title: 'MYSQL多表查询随意操作',category: 'mysql'}
];

let cssLessons = lessons.filter(function (item, index, array) {
  if (item.category.toLowerCase() == 'css') {
    return true;
  }
});

console.log(cssLessons);
```

我们来写一个过滤元素的方法来加深些技术

```js
function except(array, excepts) {
  const newArray = [];
  for (const elem of array)
    if (!excepts.includes(elem)) newArray.push(elem);
  return newArray;
}

const array = [1, 2, 3, 4];
console.log(except(array, [2, 3])); //[1,4]
```

map

map(function(value,index,arr)) 对数组进行二次操作。

#### reduce

使用 `reduce` 与 `reduceRight` 函数可以迭代数组的所有元素，`reduce` 从前开始 `reduceRight` 从后面开始。下面通过函数计算课程点击数的和。

第一个参数是执行函数，第二个参数为初始值

- 传入第二个参数时将所有元素循环一遍
- 不传第二个参数时从第二个元素开始循环,不传入参数时prev第一次为数组第一个值，从第二次开始是函数的返回值

函数参数说明如下

| 参数  | 说明                       |
| ----- | -------------------------- |
| prev  | 上次调用回调函数返回的结果 |
| cur   | 当前的元素值               |
| index | 当前的索引                 |
| array | 原数组                     |

## symbol

Symbol用于防止属性名冲突而产生的，比如向第三方对象中添加属性时。

Symbol 的值是唯一的，独一无二的不会重复的

### 基础知识

#### Symbol

```js
let hd = Symbol();
let edu = Symbol();
console.log(hd); //symbol
console.log(hd == edu); //false
```

Symbol 不可以添加属性

```js
let hd = Symbol();
hd.name = "后盾人";
console.log(hd.name);
```

#### 描述参数

可传入字符串用于描述Symbol，方便在控制台分辨Symbol

```js
let hd = Symbol("is name");
let edu = Symbol("这是一个测试");

console.log(hd); //Symbol(is name)
console.log(edu.toString()); //Symbol(这是一个测试)
```

传入相同参数Symbol也是独立唯一的，因为参数只是描述而已，但使用 `Symbol.for`则不会

```js
let hd = Symbol("后盾人");
let edu = Symbol("后盾人");
console.log(hd == edu); //false
```

使用`description`可以获取传入的描述参数

```js
let hd = Symbol("后盾人");
console.log(hd.description); //后盾人
```

#### Symbol.for

根据描述获取Symbol，如果不存在则新建一个Symbol

- 使用Symbol.for会在系统中将Symbol登记
- 使用Symbol则不会登记

```js
let hd = Symbol.for("后盾人");
let edu = Symbol.for("后盾人");
console.log(hd == edu); //true
```

#### Symbol.keyFor

`Symbol.keyFor` 根据使用`Symbol.for`登记的Symbol返回描述，如果找不到返回undefined 。

```js
let hd = Symbol.for("后盾人");
console.log(Symbol.keyFor(hd)); //后盾人

let edu = Symbol("houdunren");
console.log(Symbol.keyFor(edu)); //undefined
```

#### 对象属性

Symbol 是独一无二的所以可以保证对象属性的唯一。

- Symbol 声明和访问使用 `[]`（变量）形式操作
- 也不能使用 `.` 语法因为 `.`语法是操作字符串属性的。

下面写法是错误的，会将`symbol` 当成字符串`symbol`处理

```js
let symbol = Symbol("后盾人");
let obj = {
  symbol: "hdcms.com"
};
console.log(obj);
```

正确写法是以`[]` 变量形式声明和访问

```js
let symbol = Symbol("后盾人");
let obj = {
  [symbol]: "houdunren.com"
};
console.log(obj[symbol]); //houdunren.com
```

### 实例操作

#### 缓存操作

使用`Symbol`可以解决在保存数据时由于名称相同造成的耦合覆盖问题。

```js
class Cache {
  static data = {};
  static set(name, value) {
    this.data[name] = value;
  }
  static get(name) {
    return this.data[name];
  }
}

let user = {
  name: "后盾人",
  key: Symbol("缓存")
};

let cart = {
  name: "购物车",
  key: Symbol("购物车")
};

Cache.set(user.key, user);
Cache.set(cart.key, cart);
console.log(Cache.get(user.key));
```

#### 遍历属性

Symbol 不能使用 `for/in`、`for/of` 遍历操作

```js
let symbol = Symbol("后盾人");
let obj = {
  name: "hdcms.com",
  [symbol]: "houdunren.com"
};

for (const key in obj) {
  console.log(key); //name
}

for (const key of Object.keys(obj)) {
  console.log(key); //name
}
```

可以使用 `Object.getOwnPropertySymbols` 获取所有`Symbol`属性

```js
...
for (const key of Object.getOwnPropertySymbols(obj)) {
  console.log(key);
}
```

也可以使用 `Reflect.ownKeys(obj)` 获取所有属性包括`Symbol`

```js
...
for (const key of Reflect.ownKeys(obj)) {
  console.log(key);
}
...
```

如果对象属性不想被遍历，可以使用`Symbol`保护

```js
const site = Symbol("网站名称");
class User {
  constructor(name) {
    this[site] = "后盾人";
    this.name = name;
  }
  getName() {
    return `${this[site]}-${this.name}`;
  }
}
const hd = new User("向军大叔");
console.log(hd.getName());
for (const key in hd) {
  console.log(key);
}
```

## set

用于存储任何类型的唯一值，无论是基本类型还是对象引用。

- 只能保存值没有键名
- 严格类型检测如字符串数字不等于数值型数字
- 值是唯一的
- 遍历顺序是添加的顺序，方便保存回调函数

### 基本使用

对象可以属性最终都会转为字符串

```js
let obj = { 1: "hdcms", "1": "houdunren" };
console.table(obj); //{1:"houdunren"}
```

使用对象做为键名时，会将对象转为字符串后使用

```js
let obj = { 1: "hdcms", "1": "houdunren" };
console.table(obj);

let hd = { [obj]: "后盾人" };
console.table(hd);

console.log(hd[obj.toString()]);
console.log(hd["[object Object]"]);
```

使用数组做初始数据

```js
let hd = new Set(['后盾人', 'hdcms']);
console.log(hd.values()); //{"后盾人", "hdcms"}
```

Set 中是严格类型约束的，下面的数值`1`与字符串`1`属于两个不同的值

```js
let set = new Set();
set.add(1);
set.add("1");
console.log(set); //Set(2) {1, "1"}
```

使用 `add` 添加元素，不允许重复添加`hdcms`值

```js
let hd = new Set();

hd.add('houdunren');
hd.add('hdcms');
hd.add('hdcms')

console.log(hd.values()); //SetIterator {"houdunren", "hdcms"}
```

### 获取数量

获取元素数量

```js
let hd = new Set(['后盾人', 'hdcms']);
console.log(hd.size); //2
```

### 元素检测

检测元素是否存在

```js
let hd = new Set();
hd.add('hdcms');
console.log(hd.has('hdcms'));//true
```

### 删除元素

使用 `delete` 方法删除单个元素，返回值为`boolean`类型

```js
let hd = new Set();
hd.add("hdcms");
hd.add("houdunren");

console.log(hd.delete("hdcms")); //true

console.log(hd.values());
console.log(hd.has("hdcms")); //false
```

使用 `clear` 删除所有元素

```js
let hd = new Set();
hd.add('hdcms');
hd.add('houdunren');
hd.clear();
console.log(hd.values());
```

### 数组转换

可以使用`点语法` 或 `Array.form` 静态方法将Set类型转为数组，这样就可以使用数组处理函数了

```js
const set = new Set(["hdcms", "houdunren"]);
console.log([...set]); //["hdcms", "houdunren"]
console.log(Array.from(set)); //["hdcms", "houdunren"]
```

移除Set中大于5的数值

```js
let hd = new Set("123456789");
hd = new Set([...hd].filter(item => item < 5));
console.log(hd);
```

### 去除重复

去除字符串重复

```js
console.log([...new Set("houdunren")].join(""));//houdnre
```

去除数组重复

```js
const arr = [1, 2, 3, 5, 2, 3];
console.log(...new Set(arr)); // 1,2,4,5
```

### 遍历数据

使用 `keys()/values()/entries()` 都可以返回迭代对象，因为`set`类型只有值所以 `keys与values` 方法结果一致。

```js
const hd = new Set(["hdcms", "houdunren"]);
console.log(hd.values()); //SetIterator {"hdcms", "houdunren"}
console.log(hd.keys()); //SetIterator {"hdcms", "houdunren"}
console.log(hd.entries()); //SetIterator {"hdcms" => "hdcms", "houdunren" => "houdunren"}
```

可以使用 `forEach` 遍历Set数据，默认使用 `values` 方法创建迭代器。

为了保持和遍历数组参数统一，函数中的value与key是一样的。

```js
let arr = [7, 6, 2, 8, 2, 6];
let set = new Set(arr);
//使用forEach遍历
set.forEach((item,key) => console.log(item,key));
```

也可以使用 `forof` 遍历Set数据，默认使用 `values` 方法创建迭代器

```js
//使用for/of遍历
let set = new Set([7, 6, 2, 8, 2, 6]);

for (const iterator of set) {
	console.log(iterator);
}
```

## weakSet

WeakSet结构同样不会存储重复的值，它的成员必须只能是对象类型的值。

- 垃圾回收不考虑WeakSet，即被WeakSet引用时引用计数器不加一，所以对象不被引用时不管WeakSet是否在使用都将删除
- 因为WeakSet 是弱引用，由于其他地方操作成员可能会不存在，所以不可以进行`forEach( )`遍历等操作
- 也是因为弱引用，WeakSet 结构没有keys( )，values( )，entries( )等方法和size属性
- 因为是弱引用所以当外部引用删除时，希望自动删除数据时使用 `WeakMap`

### 声明定义

以下操作由于数据不是对象类型将产生错误

```js
new WeakSet(["hdcms", "houdunren"]); //Invalid value used in weak set

new WeakSet("hdcms"); //Invalid value used in weak set
```

WeakSet的值必须为对象类型

```js
new WeakSet([["hdcms"], ["houdunren"]]);
```

将DOM节点保存到`WeakSet`

```js
document.querySelectorAll("button").forEach(item => Wset.add(item));
```

### 基本操作

下面是WeakSet的常用指令

```js
const hd = new WeakSet();
const arr = ["hdcms"];
//添加操作
hd.add(arr);
console.log(hd.has(arr));

//删除操作
hd.delete(arr);

//检索判断
console.log(hd.has(arr));
```

### 垃圾回收

WeaSet保存的对象不会增加引用计数器，如果一个对象不被引用了会自动删除。

- 下例中的数组被 `arr` 引用了，引用计数器+1
- 数据又添加到了 hd 的WeaSet中，引用计数还是1
- 当 `arr` 设置为null时，引用计数-1 此时对象引用为0
- 当垃圾回收时对象被删除，这时WakeSet也就没有记录了

```js
const hd = new WeakSet();
let arr = ["hdcms"];
hd.add(arr);
console.log(hd.has(arr));

arr = null;
console.log(hd); //WeakSet {Array(1)}

setTimeout(() => {
  console.log(hd); //WeakSet {}
}, 1000);
```

## BOM

![image-20220314194412799](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220314194412799.png)

![image-20220314194446140](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220314194446140.png)

## DOM

![image-20220314194610319](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220314194610319.png)



## 作用域

var定义的作用域同java类似

## 方法

<img src="C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220115152204588.png" alt="image-20220115152204588" style="zoom:50%;" />

this指向调用方法的对象，任何函数都有一个apply方法来修改this指向的对象

<img src="C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220115152533411.png" alt="image-20220115152533411" style="zoom:50%;" />

## 正则表达式

### 表达式全集

|     字符     | 描述                                                         |
| :----------: | :----------------------------------------------------------- |
|      \       | 将下一个字符标记为一个特殊字符、或一个原义字符、或一个向后引用、或一个八进制转义符。例如，“`n`”匹配字符“`n`”。“`\n`”匹配一个换行符。串行“`\\`”匹配“`\`”而“`\(`”则匹配“`(`”。 |
|      ^       | 匹配输入字符串的开始位置。如果设置了RegExp对象的Multiline属性，^也匹配“`\n`”或“`\r`”之后的位置。 |
|      $       | 匹配输入字符串的结束位置。如果设置了RegExp对象的Multiline属性，$也匹配“`\n`”或“`\r`”之前的位置。 |
|      *       | 匹配前面的子表达式零次或多次。例如，zo*能匹配“`z`”以及“`zoo`”。*等价于{0,}。 |
|      +       | 匹配前面的子表达式一次或多次。例如，“`zo+`”能匹配“`zo`”以及“`zoo`”，但不能匹配“`z`”。+等价于{1,}。 |
|      ?       | 匹配前面的子表达式零次或一次。例如，“`do(es)?`”可以匹配“`does`”或“`does`”中的“`do`”。?等价于{0,1}。 |
|    {*n*}     | *n*是一个非负整数。匹配确定的*n*次。例如，“`o{2}`”不能匹配“`Bob`”中的“`o`”，但是能匹配“`food`”中的两个o。 |
|    {*n*,}    | *n*是一个非负整数。至少匹配*n*次。例如，“`o{2,}`”不能匹配“`Bob`”中的“`o`”，但能匹配“`foooood`”中的所有o。“`o{1,}`”等价于“`o+`”。“`o{0,}`”则等价于“`o*`”。 |
|  {*n*,*m*}   | *m*和*n*均为非负整数，其中*n*<=*m*。最少匹配*n*次且最多匹配*m*次。例如，“`o{1,3}`”将匹配“`fooooood`”中的前三个o。“`o{0,1}`”等价于“`o?`”。请注意在逗号和两个数之间不能有空格。 |
|      ?       | 当该字符紧跟在任何一个其他限制符（*,+,?，{*n*}，{*n*,}，{*n*,*m*}）后面时，匹配模式是非贪婪的。非贪婪模式尽可能少的匹配所搜索的字符串，而默认的贪婪模式则尽可能多的匹配所搜索的字符串。例如，对于字符串“`oooo`”，“`o+?`”将匹配单个“`o`”，而“`o+`”将匹配所有“`o`”。 |
|      .       | 匹配除“`\`*`n`*”之外的任何单个字符。要匹配包括“`\`*`n`*”在内的任何字符，请使用像“`(.|\n)`”的模式。 |
|  (pattern)   | 匹配pattern并获取这一匹配。所获取的匹配可以从产生的Matches集合得到，在VBScript中使用SubMatches集合，在JScript中则使用$0…$9属性。要匹配圆括号字符，请使用“`\(`”或“`\)`”。 |
| (?:pattern)  | 匹配pattern但不获取匹配结果，也就是说这是一个非获取匹配，不进行存储供以后使用。这在使用或字符“`(|)`”来组合一个模式的各个部分是很有用。例如“`industr(?:y|ies)`”就是一个比“`industry|industries`”更简略的表达式。 |
| (?=pattern)  | 正向肯定预查，在任何匹配pattern的字符串开始处匹配查找字符串。这是一个非获取匹配，也就是说，该匹配不需要获取供以后使用。例如，“`Windows(?=95|98|NT|2000)`”能匹配“`Windows2000`”中的“`Windows`”，但不能匹配“`Windows3.1`”中的“`Windows`”。预查不消耗字符，也就是说，在一个匹配发生后，在最后一次匹配之后立即开始下一次匹配的搜索，而不是从包含预查的字符之后开始。 |
| (?!pattern)  | 正向否定预查，在任何不匹配pattern的字符串开始处匹配查找字符串。这是一个非获取匹配，也就是说，该匹配不需要获取供以后使用。例如“`Windows(?!95|98|NT|2000)`”能匹配“`Windows3.1`”中的“`Windows`”，但不能匹配“`Windows2000`”中的“`Windows`”。预查不消耗字符，也就是说，在一个匹配发生后，在最后一次匹配之后立即开始下一次匹配的搜索，而不是从包含预查的字符之后开始 |
| (?<=pattern) | 反向肯定预查，与正向肯定预查类拟，只是方向相反。例如，“`(?<=95|98|NT|2000)Windows`”能匹配“`2000Windows`”中的“`Windows`”，但不能匹配“`3.1Windows`”中的“`Windows`”。 |
| (?<!pattern) | 反向否定预查，与正向否定预查类拟，只是方向相反。例如“`(?<!95|98|NT|2000)Windows`”能匹配“`3.1Windows`”中的“`Windows`”，但不能匹配“`2000Windows`”中的“`Windows`”。 |
|     x\|y     | 匹配x或y。例如，“`z|food`”能匹配“`z`”或“`food`”。“`(z|f)ood`”则匹配“`zood`”或“`food`”。 |
|    [xyz]     | 字符集合。匹配所包含的任意一个字符。例如，“`[abc]`”可以匹配“`plain`”中的“`a`”。 |
|    [^xyz]    | 负值字符集合。匹配未包含的任意字符。例如，“`[^abc]`”可以匹配“`plain`”中的“`p`”。 |
|    [a-z]     | 字符范围。匹配指定范围内的任意字符。例如，“`[a-z]`”可以匹配“`a`”到“`z`”范围内的任意小写字母字符。 |
|    [^a-z]    | 负值字符范围。匹配任何不在指定范围内的任意字符。例如，“`[^a-z]`”可以匹配任何不在“`a`”到“`z`”范围内的任意字符。 |
|      \b      | 匹配一个单词边界，也就是指单词和空格间的位置。例如，“`er\b`”可以匹配“`never`”中的“`er`”，但不能匹配“`verb`”中的“`er`”。 |
|      \B      | 匹配非单词边界。“`er\B`”能匹配“`verb`”中的“`er`”，但不能匹配“`never`”中的“`er`”。 |
|     \cx      | 匹配由x指明的控制字符。例如，\cM匹配一个Control-M或回车符。x的值必须为A-Z或a-z之一。否则，将c视为一个原义的“`c`”字符。 |
|      \d      | 匹配一个数字字符。等价于[0-9]。                              |
|      \D      | 匹配一个非数字字符。等价于[^0-9]。                           |
|      \f      | 匹配一个换页符。等价于\x0c和\cL。                            |
|      \n      | 匹配一个换行符。等价于\x0a和\cJ。                            |
|      \r      | 匹配一个回车符。等价于\x0d和\cM。                            |
|      \s      | 匹配任何空白字符，包括空格、制表符、换页符等等。等价于[ \f\n\r\t\v]。 |
|      \S      | 匹配任何非空白字符。等价于[^ \f\n\r\t\v]。                   |
|      \t      | 匹配一个制表符。等价于\x09和\cI。                            |
|      \v      | 匹配一个垂直制表符。等价于\x0b和\cK。                        |
|      \w      | 匹配包括下划线的任何单词字符。等价于“`[A-Za-z0-9_]`”。       |
|      \W      | 匹配任何非单词字符。等价于“`[^A-Za-z0-9_]`”。                |
|    \x*n*     | 匹配*n*，其中*n*为十六进制转义值。十六进制转义值必须为确定的两个数字长。例如，“`\x41`”匹配“`A`”。“`\x041`”则等价于“`\x04&1`”。正则表达式中可以使用ASCII编码。. |
|    \*num*    | 匹配*num*，其中*num*是一个正整数。对所获取的匹配的引用。例如，“`(.)\1`”匹配两个连续的相同字符。 |
|     \*n*     | 标识一个八进制转义值或一个向后引用。如果\*n*之前至少*n*个获取的子表达式，则*n*为向后引用。否则，如果*n*为八进制数字（0-7），则*n*为一个八进制转义值。 |
|    \*nm*     | 标识一个八进制转义值或一个向后引用。如果\*nm*之前至少有*nm*个获得子表达式，则*nm*为向后引用。如果\*nm*之前至少有*n*个获取，则*n*为一个后跟文字*m*的向后引用。如果前面的条件都不满足，若*n*和*m*均为八进制数字（0-7），则\*nm*将匹配八进制转义值*nm*。 |
|    \*nml*    | 如果*n*为八进制数字（0-3），且*m和l*均为八进制数字（0-7），则匹配八进制转义值*nm*l。 |
|    \u*n*     | 匹配*n*，其中*n*是一个用四个十六进制数字表示的Unicode字符。例如，\u00A9匹配版权符号（©）。 |

### 常用正则表达式



|         用户名          | /^[a-z0-9_-]{3,16}$/                                         |
| :---------------------: | :----------------------------------------------------------- |
|          密码           | /^[a-z0-9_-]{6,18}$/                                         |
|       十六进制值        | /^#?([a-f0-9]{6}\|[a-f0-9]{3})$/                             |
|        电子邮箱         | /^([a-z0-9_\.-]+)@([\da-z\.-]+)\.([a-z\.]{2,6})\$/                                                                                /^[a-z\d]+(\.[a-z\d]+)*@([\da-z](-[\da-z])?)+(\.{1,2}[a-z]+)+$/ |
|           URL           | /^(https?:\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?$/ |
|         IP 地址         | /((2[0-4]\d\|25[0-5]\|[01]?\d\d?)\.){3}(2[0-4]\d\|25[0-5]\|[01]?\d\d?)/                                              /^(?:(?:25[0-5]\|2\[0-4][0-9]\|[01]?[0-9][0-9]?)\.){3}(?:25[0-5]\|2\[0-4][0-9]\|[01]?\[0-9][0-9]?)$/ |
|        HTML 标签        | /^<([a-z]+)([^<]+)*(?:>(.*)<\/\1>\|\s+\/>)$/                 |
|     删除代码\\注释      | (?<!http:\|\S)//.*$                                          |
| Unicode编码中的汉字范围 | /^[\u2E80-\u9FFF]+$/                                         |



## jQuery

![image-20220116145243398](C:\Users\LN\AppData\Roaming\Typora\typora-user-images\image-20220116145243398.png)

需要引入

```html
<script src="https://code.jquery.com/jquery-3.1.1.min.js"></script>
```

<script src="https://code.jquery.com/jquery-3.1.1.min.js"></script>

### 选择器

id选择器：$(#id名)

类选择器：$(.类名)

标签选择器：$(标签名)

name选择器：$(：name名)

### 常用函数

$(select).click()	鼠标点击

$(select).text()	文本内容 text无参数获取选择器的文本，有参数设置文本

$(select).html()	html内容 用法同上

$(select).val()	获取表单的内容 单选框多选框的赋值等 即value

$(select).attr()	设置获取属性的值	一个形参为获取该属性的值 两个为设置

$(select).prop()	同上

## ES6

### 简介

ECMAScript 6.0（以下简称 ES6，ECMAScript 是一种由 Ecma 国际(前身为欧洲计算机制造商
协会,英文名称是 European Computer Manufacturers Association)通过 ECMA-262标准化的脚本
程序设计语言）是 JavaScript 语言的下一代标准，已经在 2015 年 6 月正式发布了，并且
从 ECMAScript 6 开始，开始采用年号来做版本。即 ECMAScript 2015，就是 ECMAScript6。
它的目标，是使得 JavaScript 语言可以用来编写复杂的大型应用程序，成为企业级开发语言。
每年一个新版本。

### let

```js
// var 声明的变量往往会越域
// let 声明的变量有严格局部作用域
{
var a = 1;
let b = 2;
}
console.log(a); // 1
console.log(b); // ReferenceError: b is not defined
// var 可以声明多次
// let 只能声明一次
var m = 1
var m = 2
let n = 3
// let n = 4
console.log(m) // 2
console.log(n) // Identifier 'n' has already been declared
// var 会变量提升
// let 不存在变量提升
console.log(x); // undefined
var x = 10;
console.log(y); //ReferenceError: y is not defined
let y = 20;
```

### const 声明常量（只读变量）

```js
// 1. 声明之后不允许改变
// 2. 一但声明必须初始化，否则会报错
const a = 1;
a = 3; //Uncaught TypeError: Assignment to constant variable.
```

### 数组解构

```js
let arr = [1,2,3];
//以前我们想获取其中的值，只能通过角标。ES6 可以这样：
const [x,y,z] = arr;// x，y，z 将与 arr 中的每个位置对应来取值
// 然后打印
console.log(x,y,z);
```

### 对象解构

```js
const person = {
name: "jack",
age: 21,
language: ['java', 'js', 'css']
}
// 解构表达式获取值，将 person 里面每一个属性和左边对应赋值
const { name, age, language } = person;
// 等价于下面
// const name = person.name;
// const age = person.age;
// const language = person.language;
// 可以分别打印
console.log(name);
console.log(age);
console.log(language);
//扩展：如果想要将 name 的值赋值给其他变量，可以如下,nn 是新的变量名
const { name: nn, age, language } = person;
console.log(nn);
console.log(age);
console.log(language);
```

### 函数参数默认值

```js
//在 ES6 以前，我们无法给一个函数参数设置默认值，只能采用变通写法：
function add(a, b) {
// 判断 b 是否为空，为空就给默认值 1
b = b || 1;
return a + b;
}
// 传一个参数
console.log(add(10));
//现在可以这么写：直接给参数写上默认值，没传就会自动使用默认值
function add2(a , b = 1) {
return a + b;
}
// 传一个参数
console.log(add2(10));

```

### 、实战：箭头函数结合解构表达式

```js
//需求，声明一个对象，hello 方法需要对象的个别属性
//以前的方式：
const person = {
name: "jack",
age: 21,
language: ['java', 'js', 'css']
}
function hello(person) {
console.log("hello," + person.name)}
//现在的方式
var hello2 = ({ name }) => { console.log("hello," + name) };
//测试
hello2(person);
```

### 声明对象简写

```js
const age = 23
const name = "张三"// 传统
const person1 = { age: age, name: name }
console.log(person1)
// ES6：属性名和属性值变量名一样，可以省略
const person2 = { age, name }
console.log(person2) //{age: 23, name: "张三"}
```

### 对象的函数属性简写

```js
let person = {
name: "jack",
// 以前：
eat: function (food) {
console.log(this.name + "在吃" + food);
},
// 箭头函数版：这里拿不到 this
eat2: food => console.log(person.name + "在吃" + food),
// 简写版：
eat3(food) {
console.log(this.name + "在吃" + food);
}
}
person.eat("apple");
```

### 对象拓展运算符

```js
// 1、拷贝对象（深拷贝）
let person1 = { name: "Amy", age: 15 }
let someone = { ...person1 }
console.log(someone) //{name: "Amy", age: 15}
// 2、合并对象
let age = { age: 15 }
let name = { name: "Amy" }
let person2 = { ...age, ...name } //如果两个对象的字段名重复，后面对象字
段值会覆盖前面对象的字段值
console.log(person2) //{age: 15, name: "Amy"}
```

### map 和 reduce

```js
let arr = ['1', '20', '-5', '3'];
console.log(arr)
arr = arr.map(s => parseInt(s));
console.log(arr)
//map()：接收一个函数，将原数组中的所有元素用这个函数处理后放入新数组返回。
```

语法：
`arr.reduce(callback,[initialValue])`
`reduce `为数组中的每一个元素依次执行回调函数，不包括数组中被删除或从未被赋值的元素，接受四个参数：

初始值（或者上一次回调函数的返回值），当前元素值，当前索引，调用 reduce 的数组。callback （执行数组中每个值的函数，包含四个参数）
1、previousValue （上一次调用回调返回的值，或者是提供的初始值（initialValue））
2、currentValue （数组中当前被处理的元素）
3、index （当前元素在数组中的索引）
4、array （调用 reduce 的数组）
initialValue （作为第一次调用 callback 的第一个参数。）

```js
const arr = [1,20,-5,3];
//没有初始值：
console.log(arr.reduce((a,b)=>a+b));//19
console.log(arr.reduce((a,b)=>a*b));//-300//指定初始值：
console.log(arr.reduce((a,b)=>a+b,1));//20
console.log(arr.reduce((a,b)=>a*b,0));//-0
```

### Promise 语法

```js
const promise = new Promise(function (resolve, reject) {
// 执行异步操作
if (/* 异步操作成功 */) {
resolve(value);// 调用 resolve，代表 Promise 将返回成功的结果
} else {
reject(error);// 调用 reject，代表 Promise 会返回失败结果
}
});
//使用箭头函数可以简写为：
const promise = new Promise((resolve, reject) =>{
// 执行异步操作
if (/* 异步操作成功 */) {
resolve(value);// 调用 resolve，代表 Promise 将返回成功的结果
} else {
reject(error);// 调用 reject，代表 Promise 会返回失败结果
}
});
```

## 模块化

模块化就是把代码进行拆分，方便重复利用。类似 java 中的导包：要使用一个包，必须先导包。而 JS 中没有包的概念，换来的是 模块。模块功能主要由两个命令构成：`export`和`import`。

- `export`命令用于规定模块的对外接口。
- `import`命令用于导入其他模块提供的功能。

比如我定义一个 js 文件:hello.js，里面有一个对象

```js
const util = {
sum(a,b){
return a + b;
}
}
```

我可以使用 export 将这个对象导出：

```js
const util = {
sum(a,b){return a + b;
}
}
export {util};
```


当然，也可以简写为：

```js
export const util = {
sum(a,b){
return a + b;
}
}
```

`export`不仅可以导出对象，一切 JS 变量都可以导出。比如：基本类型变量、函数、数组、对象。
当要导出多个值时，还可以简写。比如我有一个文件：user.js：

```js
var name = "jack"
var age = 21
export {name,age}
```

上面的导出代码中，都明确指定了导出的变量名，这样其它人在导入使用时就必须准确写出变量名，否则就会出错。
因此 js 提供了`default`关键字，可以对导出的变量名进行省略
例如：

```js
// 无需声明对象的名字
export default {
sum(a,b){
return a + b;
}
}
这样，当使用者导入时，可以任意起名字
```

使用`export`命令定义了模块的对外接口以后，其他 JS 文件就可以通过`import`命令加载这
个模块。

```js
// 导入 utilimport util from 'hello.js'
// 调用 util 中的属性
util.sum(1,2)
//要批量导入前面导出的 name 和 age：
import {name, age} from 'user.js'
console.log(name + " , 今年"+ age +"岁了")
```

## nodejs

前端开发，少不了 node.js；Node.js 是一个基于 Chrome V8 引擎的 JavaScript 运行环境。

http://nodejs.cn/api/
我们关注与 node.js 的 npm 功能就行；
NPM 是随同 NodeJS 一起安装的包管理工具，JavaScript-NPM，Java-Maven；
1）、官网下载安装 node.js，并使用` node -v` 检查版本
2）、配置 npm 使用淘宝镜像
`npm config set registry http://registry.npm.taobao.org/`
3）、大家如果 `npm install` 安装依赖出现 chromedriver 之类问题，先在项目里运行下面命令
`npm install chromedriver --chromedriver_cdnurl=http://cdn.npm.taobao.org/dist/chromedriver`
然后再运行 `npm install`

