# JavaScript 高级程序设计（第三版）

## 第一章：在HTML中使用JavaScript

### 1.1 \<script>元素

#### 1.1.1 定义

使用 \<script> 元素向HTML页面中插入JavaScript。

#### 1.1.2 \<script>的属性

HTML为 \<script> 定义了下列属性：

- async：可选。表示应该立即下载脚本，但不妨碍页面中的其他操作。标记为 async 的脚本并不保证按照指定它们的先后顺序执行（异步）。
- charset：可选。表示通过 scr 属性指定的代码的字符集。
- defer：可选。表示脚本可以延迟到文档完全被解析和显示之后再执行。相当于告诉浏览器 \<script> 元素立即下载，但延迟执行。

- sec：可选。表示包含要执行代码的外部文件。
- type：可选，表示编写代码使用的脚本语言的内容类型。

### 1.1.3 标签的位置

- 所有 \<script> 元素都应该放在页面的 \<head> 元素中。这样意味着，只有执行完全部的 \<script> 后才能开始呈现页面的内容。这会导致页面显示的延迟。
- 为了避免延迟的问题，现代Web应用程序一般都把全部的 JavaScript 引用放在 \<body> 元素中页面内容的后面。

### 1.2 嵌入代码与外部文件

在 HTML 中嵌入 JavaScript 代码虽然没有问题，但一般认为最好的做法还是尽可能使用外部文件来包含 JavaScript 代码。这样做会有以下优点：

- 可维护性：在不同 HTML 文件里的 JavaScript 会造成维护问题。但把所有的 JavaScript 文件都放在一个文件夹中，维护起来会轻松很多。
- 可缓存：浏览器能够根据具体的设置缓存链接的外部 JavaScript 文件，即如果两个或多个 HTML 页面都使用了同一个 JavaScript 文件，则浏览器只需缓存一次。
- 适应未来：HTML 和 XHTML 包含外部文件的语法时相同的。

### 1.3 \<noscript> 元素

当浏览器不支持脚本或者浏览器支持脚本，但脚本被禁用时，我们可以使用 \<noscript> 元素来提示用户。

```js
<noscript>
   <p>本页面需要浏览器支持Javascript<\p>
<\noscript>
```

这个提示在脚本无效的情况下会向用户显示一条消息，而在能正常使用 JavaScript 脚本的浏览器中，用户可能永远不会看到他。

## 第二章：基本概念
### 2.1 语法

#### 2.1.1 区分大小写

ECMAScript 中的一切（变量、函数名和操作符）都区分大小写。

#### 2.1.2 标识符

标识符，就是指变量、函数、属性的名字，或者函数的参数。标识符可以是按照以下格式规则组合起来的一个或多个字符：

- 第一个字符必须是一个字母、下划线（_）或者一个美元符号（$）；
- 其他标识符可以是字母，下划线，数字和美元符号。

按照惯例，ECMAScript 标识符采用驼峰大小写格式，即第一个字母小写，剩下的每个有意义的单词的第一个字母大写。如：doSomethingImportant。

#### 2.1.3 注释

```js
// 单行注释
/*
*   块级注释（多行注释）
*
*/   
```

#### 2.1.4 严格模式

ECMAScript 的语法较为宽松，一般情况下不会报错，但在严格模式下，ECMAScript 中的一些不确定的行为将得到处理，且对于某些不安全的操作也会抛出错误。要在整个脚本中启用严格模式，可以在顶部添加如下代码：

```js
"use strict"   //启用严格模式
```

如果只是需要指定某个函数在严格模式下执行。则可以在函数内部的上方加入如上指令：

```js
function dosomething(){
    "use strict";
    函数体
}                //为函数启用严格模式
```

#### 2.1.5 语句

ECMAScript 中的语句以一个分号结尾；如果省略分号，也不会报错，而是由解析器自行确定语句的结尾。省略分号虽然不会报错，但在写代码时加上分号，会减少很多不必要的麻烦。

### 2.2 关键字和保留字

- 关键字：是指 JS本身已经使用了的字，不能再用它们充当变量名、方法名。
  包括：break、case、catch、continue、default、delete、do、else、finally、for、function、if、in、instanceof、new、return、switch、this、throw、try、typeof、var、void、while、with 等。
- 保留字：实际上就是预留的“关键字”，意思是现在虽然还不是关键字，但是未来可能会成为关键字，同样不能使用它们当变量名或方法名。
  包括：boolean、byte、char、class、const、debugger、double、enum、export、extends、fimal、float、goto、implements、import、int、interface、long、mative、package、private、protected、public、short、static、super、synchronized、throws、transient、volatile 等。

注意：如果将保留字用作变量名或函数名，那么除非将来的浏览器实现了该保留字，否则很可能收不到任何错误消息。当浏览器将其实现后，该单词将被看做关键字，如此将出现关键字错误
### 2.3 变量

ECMAScript 中的变量时松散类型的，可以用来保存任何类型的数据。

```js
var message; //定义了一个名字为message的变量，该变量可以用来保存任何类型的数据。

//可以使用一条语句定义多个变量，如：
var message = "hi";
	found = false;
	age = 18;
```

### 2.4 数据类型

#### 2.4.1 typeof 操作符

typeof 是用来检测变量的数据类型的操作符。对一个值使用 typeof 操作符可能返回以下结果：

- "undefine"：表示这个值未定义；
- "boolean"：表示这个值是布尔值；
- "string"：表示这个值是字符串；
- "number"：表示这个值是数字；
- "object"：表示这个值是一个对象或者 null ；
- "function"：表示这个值是个函数。

#### 2.4.2 Undefined 类型

Undeefined 类型只有一个值，即特殊的 undefined 。在使用 var 声明变量但未对其加以初始化时，这个变量的值就是 undefined 。

#### 2.4.3 Null 类型

Null 类型是第二个只有一个值的数据类型，这个特殊的值是null ，从逻辑角度来看，null 值表示一个空对象指针，所以用 typeof 操作符对 null 值进行检测是会显示为 ”object“ 。

#### 2.4.4 Boolean 类型

Boolean 类型只有两个值：true 和 false 。true 不一定为1，而 false 也不一定为0。

注意：这里的 true 和 false 是区分大小写的，即 True 和 Flase 都不是 boolean 值，只是普通的标识符。 

#### 2.4.5 Number 类型

Number 类型就是数字类型，用来表示整数和浮点数值。

- 整数

```js
var intNum = 55;		//十进制整数
var octalNum = 070;		//八进制的56，八进制字面值的第一位必须为0，如果字面值中的数值超出了0~7的范围，则前导零会被忽略，后面的数值当作十进制数组解析
var hexNum = 0xA;		//十六进制的10，十六进制的前两位必须是0x。
//在进行数值运算时，所有的八进制和十六进制的数值最后都会被转换成十进制的数值。
```

- 浮点数：数值中必须包含一个小数点，且小数点的后面必须至少有一位数字。

```js
var floatNum = 1.1
```

1. 保存浮点数值需要的内存空间为整数值的两倍，因此 ECMAScript 会检测数值，如果小数点后面没有数字或者数字为 0，则会将其转换成整数来存储。

2. 对于极大或者极小的数值，可以用 e 表示法表示浮点数值。（浮点数值的最高精度为 17 位小数）

   ```js
   var floatNum = 3.125e7;			//等于31250000
   ```

- 数值范围：由于内存的限制，ECMAScript 并不能保存所有的数值，如果某次计算中的结果是一个超出 JavaScript 数值范围的值，则这个数值将被自动转换为特殊的 Infinity 值。

- NaN

  - NaN 表示非数值，是一个特殊的数值。这个数值用来表示一个本来要返回数值的操作数未返回数值的情况（这样就不会因为报错而导致代码停止运行）。

  - NaN 值不与任何值相等，包括 NuN本身。

  - isNaN () 是一个判断数值是否为 NaN 的函数。isNaN () 在接受到一个值之后，会尝试将这个值转换成数值，只有不能被转换成数值的值该函数才会返回 true 。

    ```js
    alert(isNaN(NaN));			//true
    aleet(isNaN(10));			//false,10是一个数值
    aleet(isNaN("10"));			//false,可以转换成数值10
    aleet(isNaN("blue"));		//true
    aleet(isNaN(true));			//false,可以转换成数值1
    ```

    
