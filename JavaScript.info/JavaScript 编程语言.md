# 基础知识

## 脚本引入

内嵌脚本

```html
<html>
	<head></head>
	<body>
		<script>
	    alert('Hello, world!');
	  </script>
	</body>
</html>
```

在HTML5中，type和language已无需指定

外部脚本

```html
<script src="<https://cdnjs.cloudflare.com/ajax/libs/lodash.js/3.2.0/lodash.js>"></script>
```

使用独立文件的好处是浏览器会下载它，然后将它保存到浏览器的缓存中。这可以节省流量，并使得页面（加载）更快。

**总结：临时的简单逻辑片段可以使用内嵌脚本，复杂逻辑使用外部脚本独立管理。**



## 自动分号插入

JavaScript具有自动插入分号的能力，开发者可以选择不写分号，在多数情况下代码可以正确运行。然而，存在JavaScript无法确认是否需要自动插入分号的情况，这种情况下将导致难以排查的代码错误。

```jsx
alert("There will be an error")

[1, 2].forEach(alert)
```

JavaScript 并不会在方括号 [...] 前添加一个隐式的分号。因此代码将被解析为👇这样

```jsx
alert("There will be an error")[1, 2].forEach(alert)
```

**总结：主动书写分号，避免引擎无法确认是否需要自动插入分号的情况。**



## 严格模式

```jsx
"use strict";
```

向引擎指明JavaScript代码需要在严格的条件下运行。严格模式做了什么？

- 消除代码运行的一些不安全之处，保证代码运行的安全；比如，不允许使用未声明变量，这在默认的松散模式下是被允许的。https://zhuanlan.zhihu.com/p/122193624
- 提高编译器效率，增加运行速度；
- 为未来新版本的JavaScript做好铺垫。比如，新增关键字：let/static/public......

浏览器兼容性：Internet Explorer 10 +、 Firefox 4+ Chrome 13+、 Safari 5.1+、 Opera 12+。

**总结：对于现代开发，建议使用严格模式启动脚本。**



## 变量和常量

- 使用`let`声明变量，一行声明一个变量可读性相对更好。
- 使用`const`声明常量，硬编码常量使用大写命名，需在运行时计算的常量使用小写。
- 变量命名仅包含字母，数字，$和_，字母包括任何语言字符（比如中文，阿拉伯文），但不推荐使用。
- 变量命名不允许使用数字开头。

```jsx
let color;
let length = 10;

color = "red";
length = 20;
const SIZE_SMALL = 12;
const SIZE_MIDDLE = 14;
const SIZE_LARGE = 16;

const size = getSize();
```



## 数据类型

### 八大基础数据类型

#### number

- 整数、浮点数、特殊值（Infinity, -Infinity, NaN）

- 安全范围

  ```javascript
  const biggestInt  = Number.MAX_SAFE_INTEGER  //  (253 - 1) =>  9007199254740991
  const smallestInt = Number.MIN_SAFE_INTEGER  // -(253 - 1) => -9007199254740991
  ```

  超出安全范围的数据无法正确算术运算，无法通过JSON parser正确转换为对应数值。

#### bigInt

- 任意长度的整数

#### string

- 字符串

#### boolean

- true/false

#### null

- 空值或未知值，不代表空的object

#### undefined

- 未定义值，一般是变量已声明但未定义的情况

#### symbol

- 唯一标识

#### object

- 其他几个数据类型称为“原始类型”，因为值只包含一个单独的内容。object则用于存储复杂的数据结构。

### 查询数据类型

```javascript
var str = "abc"
typeof str; // "string"
typeof(str); // "string"
```

- `typeof x` 或者 `typeof(x)`
- 以字符串形式返回类型名称，比如"string"
- `typeof null` 会返回 `"object"` —— 这是 JavaScript 编程语言的一个错误，实际上它并不是一个 object。



## 数据类型转换

#### 字符串转换

- 发生在输出内容时
- 显式转换：`String(value)`

#### 数字型转换

- 发生在算术操作时

- 显式转换：`Number(value)`

  ```javascript
  Number(undefined); // NaN
  Number(null); // 0
  Number(true); // 1
  Number(false); // 0
  Number(" 123 "); // 123
  Number("4px"); // NaN
  Number(""); // 0
  
  // 忽略字符串的首尾处的空格字符。从左到右读取，遇到无法转换为数字的字符输出NaN。空字符串转换为0。
  ```

#### 布尔型转换

- 发生在逻辑操作时
- 显式转换：`Boolean(value)`
  - 0, null, undefined, NaN, "" -> false
  - 其他 -> true



## 运算符

- 进行算术操作时，会对运算元进行数字类型转换
- 二元 运算符 "+" 具有字符串类型转换和连接字符串的特性：如果其中一个运算元是字符串，运算元将被转换为字符串连接。例如："4" + 2 // "42"
- 一元运算符 "+" "-" 具有数字类型转换特性，例如 +"4" // 4，-"4" // -4
- 自增/自减运算符：自增自减只能应用于变量。前置先做自增自减再做其他操作，后置先做其他操作再自增自减



## 值的比较

- 值的比较返回布尔值

- 同类型数据比较

  - 字符串比较逐个比较字符的Unicode编码值，例如`"apple" < "pineapple"`

- 不同类型数据比较

  - 转换为数字类型再进行比较，例如`"1" > false`

  - **特殊的：**相等比较时，null和undefined不会进行类型转换，因此

    ```javascript
    null == undefined // true
    null == 0 // false
    ```

  - **再特殊的：**严格相等比较时，不会进行类型转换么，因此

    ```javascript
    null === undefined // false
    ```



## 逻辑运算符

- || 串联：寻找第一个真值，返回初始值，**并不是**返回转换后的布尔值
- && 串联：寻找第一个假值，返回初始值，**并不是**返回转换后的布尔值
- 运算符优先级：! > && > ||



## 函数

- 函数表达式在代码执行到达时被创建，并且仅从那一刻起可用。
- **在函数声明被定义之前，它就可以被调用。**这是内部算法的原故。当 JavaScript **准备** 运行脚本时，首先会在脚本中寻找全局函数声明，并创建这些函数。我们可以将其视为“初始化阶段”。
- 严格模式下，当一个函数声明在一个代码块内时，它在该代码块内的任何位置都是可见的。但在代码块外不可见。



# Object（对象）：基础知识

## 对象基础知识

- 通常创建对象来表示真实世界中的实体，实体具有特性（对象属性）和行为（对象方法）。
- 对象属性名（key）必须是字符串或 Symbol。其它类型将被自动地转化为字符串。
- 对象字面量中属性名可用方括号包裹的表达式表示，称之为计算属性。
- 使用 `delete` 删除属性，使用 `in` 检查属性是否存在，使用 `for..in` 遍历对象中的所有键。
- 对象排序：整数键按升序排序，其他键按创建顺序排序。
- 对象和原始类型的区别是，对象通过引用存储和复制，即变量存储的是内存中的地址而非对象本身。
- 被 `const` 修饰的对象是可以被修改。 `const user = { name: "John" }; user.age = 25;` 关键字 const 只保护变量本身不被改变，变量存储的是对一个对象的引用，对象的内容改变并不会改变对象的内存地址。
- 对象的复制，使用 `Object.assign` 进行浅拷贝，`_.cloneDeep(obj)` 进行深拷贝。

## 垃圾回收

JavaScript 中主要的内存管理概念是 **可达性**。

简而言之，“可达”值是那些以某种方式可访问或可用的值。它们一定是存储在内存中的。

1. 这里列出固有的可达值的基本集合，这些值明显不能被释放。

   比方说：

   - 当前函数的局部变量和参数。
   - 嵌套调用时，当前调用链上所有函数的变量与参数。
   - 全局变量。
   - （还有一些内部的）

   这些值被称作 **根（roots）**。

2. 如果一个值可以通过引用或引用链从根访问任何其他值，则认为该值是可达的。

3. 如果局部变量中有一个对象，并且该对象有一个属性引用了另一个对象，则该对象被认为是可达的。而且它引用的内容也是可达的。。

在 JavaScript 引擎中有一个被称作垃圾回收器的东西在后台执行。它监控着所有对象的状态，并删除掉那些已经不可达的。

JavaScript 引擎做了许多优化，使垃圾回收运行速度更快，并且不影响正常代码运行。

一些优化建议：

- **分代收集（Generational collection）**—— 对象被分成两组：“新的”和“旧的”。许多对象出现，完成他们的工作并很快死去，他们可以很快被清理。那些长期存活的对象会变得“老旧”，而且被检查的频次也会减少。
- **增量收集（Incremental collection）**—— 如果有许多对象，并且我们试图一次遍历并标记整个对象集，则可能需要一些时间，并在执行过程中带来明显的延迟。所以引擎试图将垃圾收集工作分成几部分来做。然后将这几部分会逐一进行处理。这需要他们之间有额外的标记来追踪变化，但是这样会有许多微小的延迟而不是一个大的延迟。（类似于分片+追踪变化）
- **闲时收集（Idle-time collection）**—— 垃圾收集器只会在 CPU 空闲时尝试运行，以减少可能对代码执行的影响。

## Symbol

- 通过 `Symbol(name)` 创建唯一的标识符，name是可选的描述。
- `Symbol` 不会自动转换为字符串，因此直接输出会抛出类型错误。如果真的想要显示一个 `Symbol`，可以使用 `toString()` 方法。或者通过 `symbol.description` 获取描述。
- 在对象中， `Symbol` 是“隐藏符号属性”
  - 代码的任何其他部分都不能意外访问或重写这些属性
  - 隐藏符号属性不参与 for..in 循环
  - 在对象复制时，隐藏符号属性会被复制
- 通过全局注册表获取和创建全局 `Symbol`
  - `Symbol.for(key)` 返回（如果需要的话则创建）一个以 key 作为名字的全局标识符
  - `Symbol.keyFor(symbol)` 返回全局标识符对应的描述
- 从技术上说，Symbol 不是 100% 隐藏的。有一个内置方法 Object.getOwnPropertySymbols(obj) 允许我们获取所有的 Symbol。还有一个名为 Reflect.ownKeys(obj) 的方法可以返回一个对象的 所有 键，包括 Symbol。所以它们并不是真正的隐藏。但是大多数库、内置方法和语法结构都没有使用这些方法。

## this

- `this` 的值是在代码运行时计算出来的，它取决于代码上下文。
  - 以对象方法的语法调用函数时：`object.method()` ，`this` 的值是 `object`
  - 在全局作用域，`this` 的值是 `Window`
  - 在普通函数中，严格模式下，`this` 的值是 `undefined` ；非严格模式下， `this` 的值是 `Window`
  - 作为构造函数调用函数时，`this` 指向实例化的对象
  - 使用`call`/ `apply`调用时，`this` 指向参数指定上下文
- 箭头函数本身没有this，导致以下三种现象：
  - 根据外层（函数或者全局）作用域来决定 `this`
  - 箭头函数不能作为构造函数使用
  - 不能使用`call`, `apply`手动修改`this`
- 方法调用的内部原理：引用类型。
  - 当以对象方法调用函数时：`object.method()`，点符号返回一个引用类型值 —— `(base, name, strict)`，`base`是对象，`name`是属性名，`strict`是`boolean`值严格模式下为`true`。
  - 引用类型是一种特殊的“中间”内部类型，用于将信息从点符号 . 传递到调用括号 ()。
  - 像赋值 `hi = user.hi` 等其他的操作，将引用类型作为一个整体丢弃，只获取 `user.hi`（一个函数）的值进行传递。因此，任何进一步的操作都会“失去” `this`。

## 对象原始值转换

对象到原始值的转换，是由许多期望以原始值作为值的内建函数和运算符自动调用的。

这里有三种类型（hint）：

- `"string"`（对于 `alert` 和其他需要字符串的操作）
- `"number"`（多数算术运算，大小比较）
- `"default"`（二元加分，相等比较）

规范明确描述了哪个运算符使用哪个 hint。很少有运算符“不知道期望什么”并使用 `"default"` hint。通常对于内建对象，`"default"` hint 的处理方式与 `"number"` 相同，因此在实践中，最后两个 hint 常常合并在一起。

转换算法是：

1. 调用 `obj[Symbol.toPrimitive](hint)` ，
2. 否则，如果 hint 是 `"string"`。尝试 `obj.toString()` 和 `obj.valueOf()`，无论哪个存在。
3. 否则，如果 hint 是 `"number"` 或者 `"default"`。尝试 `obj.valueOf()` 和 `obj.toString()`，无论哪个存在。

在实践中，为了便于进行日志记录或调试，对于所有能够返回一种“可读性好”的对象的表达形式的转换，只实现以 `obj.toString()` 作为全能转换的方法就够了。

## 构造函数

- 构造函数，又称构造器。本质是常规函数，和常规函数的区别在于

  - 构造函数约定首字母大写
  - 只能通过 `new` 来调用

- 使用 `new` 调用构造函数时，实质上是创建了空的 `this`，并在最后返回填充了值的 `this`。类似于下面的代码

  ```jsx
  function User(name) {
    // this = {};（隐式创建）
  
    // 添加属性到 this
    this.name = name;
    this.isAdmin = false;
  
    // return this;（隐式返回）
  }
  ```

- 在函数中使用 `new.target` 可知其是否被 `new` 调用。在一些库中会使用该特性使构造语法更加灵活

  ```jsx
  function User(name) {
    if (!new.target) { // 如果你没有通过 new 运行我
      return new User(name); // ……我会给你添加 new
    }
  
    this.name = name;
  }
  
  let john = User("John"); // 将调用重定向到新用户
  alert(john.name); // John
  ```

- 构造函数中， `return` 对象时返回该对象，在所有其他情况下返回 `this`

- 构造函数用于复用对象创建逻辑，以及聚合复杂对象创建逻辑





# 数据类型

## 原始类型

现实情况，存在两个需求：

- 原始类型本身只包含一个原始值，非常轻量。
- 原始类型可以执行操作，但是需要保持轻量。

解决方案，使用“对象包装器”：

1. 在访问原始类型的属性或方法时，JavaScript内部创建一个对象包装器，承载原始值和对象上的方法。
2. 对象访问属性或者执行操作
3. 销毁对象包装器，留下原始值

JavaScript 引擎高度优化了这个过程。它甚至可能跳过创建额外的对象。但是它仍然必须遵守规范，并且表现得好像它创造了一样。

思考下面的代码：

```jsx
let str = "Hello";

str.test = 5;

alert(str.test);
```

你怎么想的呢，它会工作吗？会得到什么样的结果？

- Answer
  1. `undefined`（非严格模式）
  2. 报错（严格模式）。



## 数字类型

### 科学计数法

可以用科学技术法来省略0，如 `123e6`，`1e-10`



### Number 转换为 String

`String(number)` 将数字转换为字符串，但无法指定进制。



`number.toString(base)`  将数字转换为基于 `base` 进制的字符串，如 `255..toString(16) //ff`  

P.S.这里的..是为了解决小数点和方法调用语法冲突，也可以使用 `(255).toString(16)`。



### String 转换为 Number

`Number(string)` 逐个读取字符，遇到无法转换为数字的字符返回 `NaN`



`parseInt(str, base) / parseFloat(str)` ，它从字符串中读取一个数字，然后返回错误发生前可以读取的值。如 `parseInt("123e", 10) // 123`  `parseFloat("123.1.2") // 123.1`



### 小数精度损失

JavaScript中的数字以双精度浮点数存储，因此存在小数点精度损失的问题。

比如0.1，0.2这类无法使用二进制有限位数保存的数字，在使用时会有误差，`0.1 + 0.2 ≠ 0.3` `0.155.toFixed(2) == 0.15`

遇到这类问题，可以先将小数放大为整数或者其他无精度损失的数值，再进行计算。



### 小数舍入

| 方法               | 说明                  |
| ------------------ | --------------------- |
| Math.floor(number) | 向下取整              |
| Math.round(number) | 四舍五入为整数        |
| Math.ceil(number)  | 向上取整              |
| Math.trunc(number) | 直接截取整数          |
| number.toFixed(n)  | 四舍五入到小数点后n位 |



### 随机数

`Math.random()` - 返回[0, 1)区间内数字

获取指定范围内随机整数，并要求获取概率一致。

如果直接使用 `Math.round(Math.random() * (max - min)) + min` 获取，概率并不一致。

```
min...min+0.5...min+1...min+1.5   ...    max-0.5....max
└───┬───┘└────────┬───────┘└───── ... ─────┘└───┬──┘   ← Math.round()
   min          min+1                          max
```

解法一：选择在前后补上0.5

```jsx
function randomInteger(min, max) {
  // now rand is from  (min-0.5) to (max+0.5)
  let rand = min - 0.5 + Math.random() * (max - min + 1);
  return Math.round(rand);
}
```

解法二：在[min, max + 1)之间向下取整

```jsx
function getRandomIntInclusive(min, max) {
  min = Math.ceil(min);
  max = Math.floor(max);
  return Math.floor(Math.random() * (max - min + 1)) + min; //The maximum is inclusive and the minimum is inclusive 
}

function getRandomInt(min, max) {
  min = Math.ceil(min);
  max = Math.floor(max);
  return Math.floor(Math.random() * (max - min)) + min; //The maximum is exclusive and the minimum is inclusive
}
```



## 字符串

### 字面量

有 3 种类型的引号。反引号允许字符串跨越多行并可以使用 `${…}` 在字符串中嵌入表达式。



### 字符串存储

JavaScript 中的字符串使用的是 UTF-16 编码。UTF-16是字符编码表的一种实现方式，把Unicode字符集的抽象码位映射为16位长的整数（即码元）的序列，用于数据存储或传递。



所有常用的字符都是一个 2 字节的代码，但 2 字节只允许 65536 个组合，这对于表示每个可能的符号是不够的。所以稀有的符号被称为“代理对”的一对 2 字节的符号编码。这些符号的长度是 2。

```
alert( '𝒳'.length ); // 2，大写数学符号 X
alert( '😂'.length ); // 2，笑哭表情
alert( '𩷶'.length ); // 2，罕见的中国象形文字
```

`charCodeAt` 总是返回一个小于 65536 的值。这是因为高位编码单元使用一对低位编码代理伪字符来表示，从而构成一个真正的字符。因此，为了查看或复制65536 及以上编码字符的完整字符，需要在获取 `charCodeAt(i)` 的值的同时获取 `charCodeAt(i+1)` 的值，或者改为获取 `codePointAt(i)` 的值。

Source: https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/charCodeAt

`chartCodeAt/fromCharCode` 是基于 UTF-16 的获取和编码，`codePointAt/fromCodePoint` 是基于UTF-32的获取和编码。

```jsx
"𠮷".charCodeAt(0).toString(16);//d842
"𠮷".charCodeAt(1).toString(16);//dfb7

"𠮷".codePointAt(0);//20bb7
"𠮷".codePointAt(1);//dfb7

console.log("\\ud842\\udfb7");//𠮷, an example of hexadecimal digits
console.log("\\u20bb7\\udfb7");//₻7�
console.log("\\u{20bb7}");//𠮷 an unicode code point escapes the "\\ud842\\udfb7"
```

Source: https://stackoverflow.com/questions/36527642/difference-between-codepointat-and-charcodeat



### 字符串操作

#### 获取字符

- 使用 `[]`，查找不到字符时返回undefined
-  `str.charAt(pos)` ，查找不到字符时返回空字符串



#### 获取子字符串

| 方法                  | 范围                              | 说明                      |
| --------------------- | --------------------------------- | ------------------------- |
| slice(start, end)     | [start, end)                      | 支持负数，负数从尾部倒数  |
| substring(start, end) | [min(start, end), max(start,end)) | 负数相当于0               |
| substr(start, length) | [start, start+length)             | 从start开始取length个字符 |



### 查找字符串

- `indexOf(str, pos)`
- `includes(str, pos)`
- `startsWith(str)`
- `endsWith(str)`
- `includes(str) === ~indexOf(str)`



### 大/小写转换

字符串的大/小写转换，使用：`toLowerCase/toUpperCase`。



### 字符串比较

如果直接使用值比较，将根据字符在字符编码表中对应的编号比较。除此之外，可以通过 `str.localeCompare(str1)` 根据语言规则比较。

调用 [str.localeCompare(str2)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/String/localeCompare) 会根据语言规则返回一个整数，这个整数能表明 `str` 是否在 `str2` 前，后或者等于它：

- 如果 `str` 小于 `str2` 则返回负数。
- 如果 `str` 大于 `str2` 则返回正数。
- 如果它们相等则返回 `0`。



## 数组

数组用于存储有序集合，JavaScript 引擎尝试把这些元素存储在连续的内存区域，并针对数组有所优化。数组的本质是对象，但如果按使用对象的方式使用数组，破坏其连续的特性，引擎对数组的优化将不再适用。

- 存储有序数据集合
- 连续内存区域
- 引擎对数组有所优化
- 本质是对象



### 类数组对象

- 索引
- length属性



### 双端队列数据结构

队列：`push/shift`和

栈：`push/pop`

`push/pop`的效率要高于`shift/unshift`，因为末端操作不需要更改其他元素的索引。

| 操作              | 返回值       | 说明                 |
| ----------------- | ------------ | -------------------- |
| push(...items)    | arr.length   | 向数组末端添加元素   |
| pop()             | 最后一个元素 | 从末端移除并返回元素 |
| unshift(...items) | arr.length   | 向数组首端添加元素   |
| shift()           | 第一个元素   | 从首端移除并返回元素 |



### length

- `length` 是一个自动计算值，其值为数组最大索引+1。
- 有趣的是，`length` 同时是一个可写值， `length` 被手动缩短时，数组将被截断。



### 数组遍历

- `for (let i = 0; i < arr.length; i++)` 运行得最快，可兼容旧版本浏览器。
- `for (let item of arr)` 现代语法，只能访问value无法访问索引
- `for (let i in arr)` 请不要使用该方法遍历数组。
  - `for..in` 是针对对象的遍历方法，会遍历所有属性，处理类数组对象时，“额外”属性被遍历将会导致问题。
  - `for..in` 循环适用于普通对象，并且做了对应的优化。但是不适用于数组，因此速度要慢 10-100 倍。
- `forEach` 数组方法， 但是无法中断循环



### 数组方法

#### 方法集合

| 操作                                                         | 返回值                   | 说明                                                         |
| ------------------------------------------------------------ | ------------------------ | ------------------------------------------------------------ |
| push(...items)                                               | arr.length               | 增                                                           |
| pop()                                                        | 最后一个元素             | 删                                                           |
| unshift(...items)                                            | arr.length               | 增                                                           |
| shift()                                                      | 第一个元素               | 删                                                           |
| splice(index[, deleteCount, elem1, ..., elemN])              | 被删除元素的数组         | 增删改，index为负数时从末尾倒数                              |
| slice(start, end)                                            | [start, ... , end-1]     | 获取数组副本，[start, end)                                   |
| concat(arg1, arg2...)                                        | 包含arg1, arg2元素的数组 | 拼接数组<br />只复制数组中的元素<br />其他对象如果具有Symbol.isConcatSpreadable属性，会被当作数组处理，否则会被视为一个整体 |
| arr.forEach(function(item, index, array){})                  | undefined                | 遍历数组                                                     |
| arr.indexOf(item, from)                                      | 索引                     | 查找元素所在第一个位置的索引，找不到为-1                     |
| arr.lastIndexOf(item, from)                                  | 索引                     | 从右向左查找元素所在第一个位置的索引                         |
| arr.includes(item, from)                                     | true/false               | 查找元素是否在数组中，能正确处理NaN                          |
| arr.find(function(item, index, array){})                     | 数组元素/undefined       | 查找数组中符合特定条件的第一个元素                           |
| arr.findIndex(function(item, index, array){})                | 元素索引/-1              | 查找数组中符合特定条件的第一个元素的索引                     |
| arr.filter(function(item, index, array) {});                 | [elem1, ...]/[]          | 查找数组中符合特定条件的所有元素                             |
| arr.map(function(item, index, array) {})                     | [elem1....]              | 对数组中每个元素都做处理                                     |
| arr.sort(function(a,b){})                                    | sorted arr               | 原位排序                                                     |
| arr.reverse()                                                | reversed arr             | 原位倒转                                                     |
| arr.join(glue)                                               | string                   | elements + glue -> string<br />和string.split(glue)互相转换  |
| arr.reduce(function(accumulator, item, index, array) {   // ... }, [initial]); | result                   | 带着结果遍历                                                 |
| arr.reduceRight(function(accumulator, item, index, array) {   // ... }, [initial]); | result                   | 带着结果从右向左遍历                                         |
| arr.some(fn)                                                 | true/false               | 检查数组中是否存在满足条件的元素                             |
| arr.every(fn)                                                | true/false               | 检查数组中每一个元素是否都满足条件                           |
|                                                              |                          |                                                              |
|                                                              |                          |                                                              |



#### sort

`sort()` 方法的语法为 `arr.sort([compareFunction])`

- 如果没有指明 `compareFunction`，那么元素会被按照转换为的字符串的诸个字符的 Unicode 编码进行排序
- 如果指明了 `compareFunction`，那么数组会按照调用该函数的返回值排序。

a 和 b 是两个将要被比较的元素：

- 如果 `compareFunction(a, b)` 小于 `0`，那么 `a` 会被排列到 `b` 之前；
- 如果 `compareFunction(a, b)` 等于 `0`，那么 `a` 和 `b` 的相对位置不变。
- 如果 `compareFunction(a, b)` 大于 `0`，那么 `b` 会被排列到 `a` 之前。



#### thisArg

几乎所有调用函数的数组方法 – 比如 `find`，`filter`，`map`，除了 `sort` 是一个特例，都接受一个可选的附加参数 `thisArg`。传递上下文给调用函数。



#### Array.isArray()

判断变量是否是数组

## 可迭代对象

### 可迭代对象

**可迭代（Iterable）** 对象是数组的泛化。这个概念是说任何对象都可以被定制为可在 `for..of` 循环中使用的对象。

- `for...of`启动时调用`Symbol.iterator`方法得到迭代器（**iterator**）—— 一个有`next`方法的的对象
- `for...of`循环迭代时调用`next()`
- `next()`返回`{done:.., value: ...}`对象

``` javascript
range[Symbol.iterator] = function() {

  // ……它返回迭代器对象（iterator object）：
  // 2. 接下来，for..of 仅与此迭代器一起工作，要求它提供下一个值
  return {
    current: this.from,
    last: this.to,

    // 3. next() 在 for..of 的每一轮循环迭代中被调用
    next() {
      // 4. 它将会返回 {done:.., value :...} 格式的对象
      if (this.current <= this.last) {
        return { done: false, value: this.current++ };
      } else {
        return { done: true };
      }
    }
  };
};
```

### 内置可迭代对象

数组和字符串

### Array.from

通过可迭代或类数组，获取一个“真正的”数组，然后我们就可以对其调用数组方法了。

本质是通过`for...of`对可迭代对象进行遍历，放入数组。

`Array.from(obj[, mapFn, thisArg])`

-  `mapFn` 可以是一个函数，该函数会在对象中的元素被添加到数组前，被应用于每个元素
-  `thisArg` 允许我们为该函数设置 `this`



## Map

### 特征

- Map允许任何类型的键（key），Object在处理键时会进行字符串转换

### 方法

- `new Map(entries)` - 初始化
- `map.set(key, valye)` - 设置键值，返回map本身，因此可进行链式调用
- `map.get(key)` - 根据键返回值，不存在返回`undefined`
- `map.has(key)` - 如果`key`存在返回`true`，否则返回`false`
- `map.delete(key)`  - 删除指定键的值
- `map.clear()` - 清空map
- `map.size` - 返回当前元素的个数

### 迭代

- `map.keys()` - 返回所有键的集合，**是可迭代对象（MapIterator），并不是数组**，
- `map.values()` - 返回所有值的集合，**是可迭代对象（MapIterator），并不是数组**
- `map.entries()` - 返回所有实体`[[key, value]...]`, **是可迭代对象（MapIterator），并不是数组**，默认情况下使用`for...of`
- `map.forEach((value, key, map) => {})`

### 普通对象 => Map`

`Object.entries(obj)` => `[[key, value], [key1, value1]]`

`new Map(Object.entries(obj))`

### Map => 普通对象

`Object.fromEntries(iterable object)` => `{key: value, key1: value1}`

`Object.fromEntries(map.entries())`

`Object.fromEntries(map)`



## Set

### 特征

- 值的集合，每一个值只会出现一次

### 方法

- `new Set(iterable)` - 从可迭代对象复制值到set
- `set.add(value)` - 增加一个值，返回set本身
- `set.delete(value)` - 删除值，确实删除返回`true`，否则返回`false`
- `set.has(value)` - 如何`value`在set中返回`true`
- `set.clear()` - 清空set
- `set.size` - 返回元素个数

`Set` 内部对唯一性检查进行了更好的优化。

### 迭代

- `set.keys()` - 返回所有值，**是可迭代对象（SetIterator），并不是数组**
- `set.values()` - 返回所有值，**是可迭代对象（SetIterator），并不是数组**
- `set.entries()` - 返回所有实体`[[value, value]]`，**是可迭代对象（SetIterator），并不是数组**
- `set.forEach((value, valueAgain, set) => {})` 



## WeakMap & WeakSet

### 特征

- 键只能是对象
- 对象不再被引用时，集合中的键和值会被清除，不会阻止垃圾回收进行
- 不可迭代，不可获取所有值，只能做单个操作。这与JavaScript引擎内部实现有关，垃圾回收的时机不确定，和所有值相关的操作也具有不确定性

### 使用场景

存储和对象共存亡的数据，比如缓存数据或者临时额外数据



## 普通对象迭代

- [Object.keys(obj)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Object/keys) —— 返回一个包含该对象所有的键的数组。
- [Object.values(obj)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Object/values) —— 返回一个包含该对象所有的值的数组。
- [Object.entries(obj)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Object/entries) —— 返回一个包含该对象所有 [key, value] 键值对的数组。
- `Object.fromEntries([[key, value]])` —— 转换回对象



## 解构赋值

### 定义

将可迭代对象中的属性浅拷贝到变量中的快速方法

### 数组赋值

``` javascript
let [item1, item2 = "default", ...rest] = array;
```

### 对象赋值

``` javascript
let { id, name = "", old : age = 20, ...rest } = object;
```



## Date

### 创建日期对象

```javascript
new Date // 创建当前日期
new Date(milliseconds) // 传人毫秒数，创建指定时间戳的日期
new Date(year, month, date, hours, minutes, seconds, milliseconds) // year, month必须指定，其他默认值为0
new Date(dateString) // 和Date.parse使用相同算法
```

### 获取日期对象值

```javascript
date.getFullYear() // 四位数
date.getMonth() // 0 - 11
date.getDate()
date.getHours() 
date.getMinutes()
date.getSeconds()
date.getMilliseconds()
date.getDay() // 一周第几天，星期几，从周日开始计。0 - 周日， 6 - 周六

// 上述方法都有UTC变体

date.getTime() // 获取时间戳
date.getTimezoneOffset() // 获取UTC和本地时区的时差。分钟表示
```

### 设置日期对象

**⚠️会改变Date对象原始值，返回值是时间戳，并不是Date对象**

```javascript
date.setFullYear(year, [month], [date])
date.setMonth(month, [date])
date.setDate(date)
date.setHours(hour, [min], [sec], [ms])
date.setMinutes(min, [sec], [ms])
date.setSeconds(sec, [ms])
date.setMilliseconds(ms)

date.setTime(ms) // 传入时间戳设置整个对象
```

### 自动校准

设置超出范围的日期，Date对象会自动校准。

常用于获取指定偏差的日期对象

常用于获取一个月的最后一天

```jsx
// 获取当月最后一天
const date = new Date();
new Date(date.getFullYear(), date.getMonth() + 1, 0);
```

### Date.now

使用 Date.now() 可以更快地获取当前时间的时间戳。

不会创建中间的Date对象，更快；而且不会对垃圾处理产生额外的压力

### Date.parse

Date.parse(dateString) 方法从一个字符串中读取时间戳。

dateString需要满足`YYYY-MM-DDTHH:mm:ss.sssZ`

简短格式支持`YYYY-MM-DD`, `YYYY-MM`, `YYYY`

#### 度量测试

- 做度量测试时，为了避免外部环境对测试的干扰，需要运行多次
- 现代的 JavaScript 引擎的先进优化策略只对执行很多次的 “hot code” 有效（对于执行很少次数的代码没有必要优化）。因此，第一次执行的优化程度不高。需要增加一个升温步骤

## JSON

### JSON

#### 定义

JSON（JavaScript Object Notation）是表示值和对象的通用格式。在 RFC 4627 标准中有对其的描述。最初它是为 JavaScript 而创建的，但许多其他编程语言也有用于处理它的库。

#### 操作

`JSON.stringify` 将对象序列化为JSON字符串

`JSON.parse` 将JSON字符串反序列化为对象

#### 特征

- JSON 支持 object，array，string，number，boolean 和 null。
- JSON不支持注释

### JSON.stringify

`JSON.stringify(value[, replacer, space])`

- 对象中不得有循环引用
- JSON 是语言无关的纯数据规范，因此一些特定于 JavaScript 的对象属性会被 `JSON.stringify` 跳过。
  - 函数属性（方法）。
  - Symbol 类型的属性。
  - 存储 `undefined` 的属性。
- replacer会遍历所有键值对，从上到下递归调用
- space用于日志记录和输出美化，指定空格个数
- 对象可以自定义 `toJSON` 方法， `JSON.stringify` 会自动调用它

### JSON.parse

```javascript
let str = '{"title":"Conference","date":"2017-11-30T12:00:00.000Z"}';

let meetup = JSON.parse(str, function(key, value) {
  if (key == 'date') return new Date(value);
  return value;
});
```



# 代码质量

## 在Chrome中调试

https://developers.google.com/web/tools/chrome-devtools

## 代码风格

一些受欢迎的代码风格指南

- [Google JavaScript 风格指南](https://google.github.io/styleguide/javascriptguide.xml)
- [Airbnb JavaScript 风格指南](https://github.com/airbnb/javascript)
- [Idiomatic.JS](https://github.com/rwaldron/idiomatic.js)
- [StandardJS](https://standardjs.com/)

自动检查器（Linters）可以进行代码风格自动检查。常用的代码检查工具[ESLint](https://eslint.org/docs/user-guide/getting-started)

## 注释

如果代码不够清晰以至于需要一个注释，那么或许它应该被重写。应该避免解释性注释，可以使用函数来代替代码片段描述一种行为。

**注释以下这些内容：**

- 整体架构，高层次的观点。
- 函数的用法。（使用JSDoc）
- 重要的解决方案，特别是在不是很明显时。（为什么使用这种方法实现，避免后续维护躺坑）

**避免注释以下这些内容：**

- 描述“代码如何工作”和“代码做了什么”。
- 避免在代码已经足够简单或代码有很好的自描述性而不需要注释的情况下，还写些没必要的注释。

## 自动化测试

BDD（行为驱动测试）：规范在先，实现在后。

测试框架

- Mocha，核心框架，提供describe和it函数以及测试运行主函数。
- Chai，提供断言（assert）支持
- Sinon，监视函数、模拟内置函数...

```jsx
describe("Raises x to power n", function() { // 分组，描述当前在测试的功能
  it("5 in the power of 1 equals 5", function() { // 单个测试用例，当 assert 触发一个错误时，it 代码块会立即终止。因此，一个测试用例对应一个断言会便于调试。
    assert.equal(pow(5, 1), 5);
  });

  it("5 in the power of 2 equals 25", function() {
    assert.equal(pow(5, 2), 25);
  });

  it("5 in the power of 3 equals 125", function() {
    assert.equal(pow(5, 3), 125);
  });
});
```

## Polyfill

不同的JavaScript引擎对语言特性的支持状态是不一致的，可以通过https://kangax.github.io/compat-table/es6/查看。

新的语言特性包括新的内建函数和语法结构。transpiler会将新的语法结构转换为旧的语法结构。polyfill是用于添加/更新新函数的脚本。polyfill指路[core-js](https://github.com/zloirock/core-js)。

Babel支持transpiler和polyfill，是解决引擎对语言特性支持问题的良药。