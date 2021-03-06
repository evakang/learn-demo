[TOC]

# 第 1 章 JavaScript 简介
> 讲述了 JavaScript 的起源：因何而生，如何发展，现状如何。涉及的概念主要有 JavaScript 与 ECMAScript 之间的关系、DOM（Document Object Model，文档对象模型）、BOM（Browser Object Model，浏览器对象模型）。此外，还将讨论 ECMA（European Computer Manufacturer's Association，欧洲计算机制造商协会）和 W3C（World Wide Web Consortium，万维网联盟）指定的一些相关标准。

1、JavaScript 的含义比 ECMS-262 中规定的要多得多

- 核心（ECMAScript）
- 文档对象模型（DOM）
- 浏览器对象模型（BOM）

2、由 ECMA-262 定义的 ECMAScript 与 Web 浏览器没有依赖关系。Web 浏览器只是 ECMAScript 实现可能的宿主环境之一。其他宿主环境包括 Node（一种服务端 JavaScript 平台）和 Adobe Flash。

3、ECMA-262 的最近一版是第 5 版，发布于 2009 年。

4、文档对象模型（DOM，Document Object Model）是针对 XML 但经过扩展用于 HTML 的应用程序编程接口（API，Application Programming Interface）。

5、DOM1级（DOM Level 1）与 1998 年 10 月成为 W3C 的推荐标准
[关于 DOM 的发展历程](https://www.w3.org/TR/?tag=dom)

6、而 BOM 真正与众不同的地方（也是经常会导致问题的地方），还是它作为 JavaScript 实现的一部分但却没有相关的标准。这个问题在 HTML5 中得到了解决，HTML5 致力于把很多 BOM 功能写入正式规范

7、由于没有 BOM 标准可以遵循，因此每个浏览器都有自己的实现。虽然也存在一些事实标准，例如要有 window 对象和 navigator 对象等，但每个浏览器都会为这两个对象乃至其他对象定义自己的属性和方法。

## 第 1 章小结
1、DOM，提供访问和操作网页内容的方法和接口；BOM，提供与浏览器交互的方法和接口。
2、所有浏览器对 ECMAScript 第 3 版的支持大体上都还不错，而对 ECMAScript5 的支持程度越来越高，但对 DOM 的支持则彼此相差比较多。对已经正式纳入 HTML5 标准的 BOM 来说，尽管各浏览器都实现了某些众所周知的共同特性，但其他特性还是会因浏览器而异。

# 第 2 章 在 HTML 中使用 JavaScript
> 在 HTML 中使用 JavaScript 创建动态网页。这一章不仅展示了在网页中嵌入 JavaScript 的各种方式，还讨论了 JavaScript 内容类型（content-type）及其与 `<script>` 元素的关系。

1、HTML 4.0.1为 `<script>` 定义了下列 6 个属性：

- async：可选。表示应该立即下载脚本，但不应妨碍页面中的其他操作，比如下载其他资源或等待加载其他脚本。只对外部脚本文件有效。
- charset：可选。表示通过 src 属性执行的代码的字符集。由于大多数浏览器会忽略它的值，因此这个属性很少有人用。
- defer：可选。表示脚本可以延迟到文档完全被解析和显示之后再执行。只对外部脚本文件有效。
- language：已废弃。
- src：可选。表示包含要执行代码的外部文件。
- type：可选。可以看成是 language 的替代属性；表示编写代码使用的脚本语言的内容类型（也称为MIME类型）。实际上，服务器在传送 JavaScript 文件时使用的 MIME 类型通常是 application/x-javascript，但在 type 中设置这个值却可能导致脚本被忽略。目前 type 属性的值依旧还是 text/javascript。不过，这个属性并不是必需的，如果没有指定这个属性，则其默认值仍为 text/javascript。

2、在解释器对 `<script>` 元素内部的所有代码求值完毕以前，页面中的其余内容都不会被浏览器加载或显示。

3、`<script src="a.js"></script>` 标签之间不应该再包含额外的 JavaScript 代码。如果包含了嵌入的代码，则只会下载并执行外部脚本文件，嵌入的代码会被忽略。

4、另外，通过 `<script>` 元素的 src 属性还可以包含来自外部域的 JavaScript 文件。这一点既让 `<script>` 元素倍显强大，又让它备受争议。在这一点上，`<script>` 与 `<img>` 元素非常相似，即它的 src 属性可以是指向当前 HTML 页面所在域之外的某个域中的完整 URL。

4-5、延迟脚本 defer，脚本会被延迟到整个页面都解析完毕后再运行。因此，在 `<script>` 元素中设置 defer 属性，相当于告诉浏览器立即下载，但延迟执行。

4-5、异步脚本 async，同样与 defer 类似，async 只适用于外部脚本文件，并告诉浏览器立即下载文件。但与 defer 不同的是，标记为 async 的脚本并不保证按照指定它们的先后顺序执行。

5、无论如何包含代码，只要不存在 defer 和 async 属性，浏览器都会按照 `<script>` 元素在页面中出现的先后顺序对它们依次进行解析。换句话说，在第一个 `<script>` 元素包含的代码解析完成后，第二个 `<script>` 包含的代码才会被解析，然后才是第三个、第四个...

6、传统的做法，所有 `<script>` 元素都应该放在页面的 `<head>` 元素中，可是，在文档的 `<head>` 元素中包含所有 JavaScript 文件，意味着必须等到全部 JavaScript 代码都被下载、解析和执行完成以后，才能开始呈现页面的内容（浏览器在遇到 `<body>` 标签时才开始呈现内容）。对于那些需要很多 JavaScript 代码的页面来说，这无疑会导致浏览器在呈现页面时出现明显的延迟，而延迟期间的浏览器窗口中将是一片空白。为了避免这个问题，现在 Web 应用程序一般都把全部 JavaScript 引用放在 `<body>` 元素中页面内容的后面。
```
<body>
<!-- 这里放内容 -->
<script src="example1.js"></script>
</body>
```

7、HTML 4.0.1 为 `<script>` 标签定义了 defer 属性。这个属性的用途是表明脚本在执行时不会影响页面的构造。也就是说，脚本会被延迟到整个页面都解析完毕后再运行。因此，在 `<script>` 元素中设置 defer 属性，相当于告诉浏览器立即下载，但延迟执行。

# 第 3 章 基本概念

1、严格模式是为 JavaScript 定义了一种不同的解析与执行模型。在严格模式下，ECMAScript 中的一些不确定的行为将得到处理，而且对某些不安全的操作也会抛出错误。
在函数内部的上方包含这条编译指示，也可以指定函数在严格模式下执行：
```
function doSomething() {
  "use strict";
  //函数体
}
```

2、`var message;` 像这样未经过初始化的变量，会保存一个特殊的值 —— undefined。

3、`var message = "hi"; message = 100;` 虽然我们不建议修改变量所保存值的类型，但这种操作在 ECMAScript 中完全有效。

4、
```
function test() {
  message = "h1"; //全局变量
}
test();
alert(message); // "hi"
```
这里省略了 var 操作符，因而 message 就成了全局变量。这样，只要调用过一次 test() 函数，这个变量就有了定义，就可以在函数外部的任何地方被访问到。

> 当然，这种做法是不推荐的，给未经声明的变量赋值在严格模式下会导致抛出ReferenceError错误。

5、对一个值使用 typeof 操作符可能返回下列某个字符串：
"undefined"、"boolean"、"string"、"number"、
"object" - 如果这个值是对象或null
"function" - 如果这个值是函数

6、注意、typeof 是一个操作符而不是函数，因此例子中的圆括号尽管可以使用，但不是必需的。
`alert(typeof message);` 即可。

7、【从技术角度讲，函数在 ECMAScript 中是对象，不是一种数据类型，然后，函数也确实有一些特殊的属性，因此通过 typeof 操作符来区分函数和其他对象是有必要的。】

8、未初始化的变量会自动被赋予undefined值，未声明的变量使用typeof检测，也是undefined。

9、Null 类型是第二个只有一个值的数据类型，这个特殊的值是null。从逻辑角度来看，null值表示一个空对象指针，而这也正是使用typeof操作符检测null值时会返回"object"的原因。

10、如果定义的变量准备在将来用于保存对象，那么最好将该变量初始化为null而不是其他值。

11、虽然 Boolean 类型的字面值只有两个，但 ECMAScript 中所有类型的值都有与这两个 Boolean 值等价的值。要将一个值转换为其对应的 Boolean 值，可以调用转型函数 Boolean() 。

12、下面是各种数据类型以及对应的转换规则：
数据类型  转换为true的值   转换为false的值
String    任何非空字符串   ""(空字符串)
Number    任何非零数字值(包括无穷大)    0和NaN
Object    任何对象        null
Undefined  n/a          undefined

n/a(或N/A)，是 not applicable 的缩写，意思是“不适用”。

我们在使用控制语句的时候，`if()` 括号里面的内容会被自动转换成对应的 Boolean 值，所以了解上面的转换规则，至关重要。

13、最基本的数值字面量格式是十进制整数。八进制字面值的第一位必须是零(0)，然后是八进制数字序列（0~7）。如果字面值中的数值超出了范围，那么前导零将被忽略，后面的数值将被当做十进制数值解析。

14、八进制字面量在严格模式下是无效的，会导致支持的 JavaScript 引擎抛出错误。

15、在进行算术计算时，所有以八进制和十六进制表示的数据最终都将被转换成十进制数值。

16、js中数值范围是有限制的，如果某次计算返回了正或负的 Infinity 值，那么该值将无法继续参与下一次的计算，因为 Infinity 不是能够参与计算的数值。要想确定一个数值是不是有穷的（换句话说，是不是位于最小和最大的数值之间），可以使用 isFinite() 函数。这个函数在参数位于最小与最大数值之间时会返回true.

17、NaN本身有两个非同寻常的特定。首先，任何涉及NaN的操作（例如NaN/10）都会返回NaN，其次，NaN与任何值都不相等，包括NaN本身。

18、在ECMAScript 3 JavaScript引擎中，"070"被当成八进制字面量，因此转换后的值是十进制的 56。而在 ECMAScript 5 JavaScript 引擎中，parseInt() 已经不具有解析八进制值的能力，因此前导的零会被认为无效，从而将这个值当成"70"，结果就得到十进制的70。在ECMAScript 5中，即使是在非严格模式下也会如此。为了消除在使用parseInt()函数时可能导致的上述困惑，可以为这个函数提供第二个参数：转换时使用的基数（即多少进制）。

19、不指定基数意味着让parseInt()决定如何解析输入的字符串，因此为了避免错误的解析，我们建议无论在什么情况下都明确指定基数。多数情况下，我们要解析的都是十进制数值，因此始终将10作为第二个参数是非常必要的。

20、parseFloat(),十六进制格式的字符串则始终会被转换为0。由于parseFloat()只解析十进制值，因此它没有用第二个参数指定基数的用法。最后还要注意一点：parseInt()的结果并不都是浮点型。如果字符串包含的是一个可解析为整数的数（没有小数点，或者小数点后都是零），parseFloat()会返回整数。

21、要把一个值转换为一个字符串有两种方式。第一种是使用几乎每个值都有的toString()方法。这个方法唯一要做的就是返回相应值的字符串表现。

22、数值、布尔值、对象和字符串值（没错，每个字符串也都有一个toString()方法，该方法返回字符串的一个副本）都有toString()。但null和undefined值没有这个方法。

23、在调用数值的 toString() 方法时，可以传递一个参数：输出数值的基数。

24、在不知道要转换的值是不是null或undefined的情况下，还可以使用转型函数String()，这个函数能够将任何类型的值转换为字符串。String()函数遵循下列转换规则：
++如果值有toString()方法，则调用该方法（没有参数）并返回相应的结果
++如果值是null，则返回"null"
++如果值是undefined，则返回"undefined"

25、【要把某个值转换为字符串，可以使用加号操作符把它与一个字符串("")加在一起。】

26、
```
var o = new Object();
var o = new Object;
```
仅仅创建Object的实例并没有什么用处，但关键是要理解一个重要的思想：即在ECMAScript中，Object类型是所有它的实例的基础。换句话说，Object类型所具有的任何属性和方法也同样存在于更具体的对象中。
Object的每个实例都具有下列属性和方法。
constructor：保存着用于创建当前对象的函数。对于前面的例子而言，构造函数(constructor)就是Object()。
hasOwnProperty(propertyName)
isPrototypeOf(object)
propertyIsEnumerable(propertyName)：用于检查给定的属性是否能够使用for-in语句来枚举。
toLocaleString()
toString()
valueOf()

27、【从技术角度讲，ECMA-262中对象的行为不一定适用于 JavaScript 中的其他对象。浏览器环境中的对象，比如DOM和DOM中的对象，都属于宿主对象，因为它们是由宿主实现提供和定义的。ECMA-262不负责定义宿主对象，因此宿主对象可能会，也可能不会继承Object】

28、不过，在对非数值应用一元加操作符时，该操作符会像 Number() 转型函数一样对这个值执行转换。

29、在应用于非数值时，一元减操作符遵循与一元加操作符相同的规则，最后再将得到的数值转换为负数。

30、负数同样以二进制码存储，但使用的格式是二进制补码。计算一个数值的二进制补码。需要经过下列3个步骤：
求这个数值绝对值的二进制码
求二进制反码，即将0替换为1，将1替换为0
得到的二进制反码加1

31、逻辑与（&&）
逻辑与操作可以应用于任何类型的操作数，而不仅仅是布尔值。在有一个操作数不是布尔值的情况下，逻辑与操作就不一定返回布尔值，此时，它遵循下列规则：
如果第一个操作数是对象，则返回第二个操作数。

32、关系操作符（ >,<,>=,<=）
如果两个操作数都是字符串，则比较两个字符串对应的字符编码值。
由于大写字母的字符编码全部小于小写字母的字符编码，因此我们就会看到如下所示的奇怪现象：
```
var result = "Brick" < "alphabet";  // true
```

33、在用于赋值时，逗号操作符总会返回表达式中的最后一项。

34、for-in语句是一种精准的迭代语句，可以用来枚举对象的属性。
```
for(var propName in window) {
    document.write(propName);
}
```
ECMAScript 对象的属性没有顺序。因此，通过 for-in 循环输出的属性名的顺序是不可预测的。具体来讲，所有属性都会被返回一次，但返回的先后次序可能会因浏览器而异。

35、break和continue语句都可以与label语句联合使用，从而返回代码中特定的位置。这种联合使用的情况多发生在循环嵌套的情况下，如下面的例子所示：
```
var num = 0;
outermost:
for(var i=0; i<10; i++) {
    for(var j=0; j<10; j++) {
        if(i == 5 && j == 5) {
            break outermost;
        }
        num++
    }
}
alert(num);  // 55
```
这个内部循环中的break语句带另一个参数：要返回到的标签。添加这个标签的结果将导致break语句不仅会退出内部的for语句，而且也会退出外部的for语句。所以，当i和j都等于5时，num的值正好是55。而且break退出了全部循环。
如果没有这个`outermost`，结果是95。

同样，continue语句也可以像这样与label语句联用，来看例子：
```
var num = 0;
outermost:
for(var i=0; i<10; i++) {
    for(var j=0; j<10; j++) {
        if(i==5 && j==5) {
            continue outermost;
        }
        num++
    }
}
alert(num);  // 95
```
如果没有这个`outermost`，结果是99。

36、【没有重载】
ECMAScript函数不能像传统意义上那样实现重载。但是，可以通过检查传入函数中参数的类型和数量并作出不同的反应，可以模仿方法的重载。

37、ECMAScript 中所有函数的参数都是按值传递的。也就是说，把函数外部的值复制给函数内部的参数，就和把值从一个变量复制到另一个变量一样。基本类型值的传递如同基本类型变量的复制一样，而引用类型值的传递，则如同引用类型变量的【复制】(理解这个复制的意思很重要)一样。有不少开发人员在这一点上可能会感到困惑，因为访问变量有按值和按引用两种方式，而参数只能按值传递。

38、我们来看一个函数传递引用类型参数的例子：

```javascript
function setName(obj) {
  obj.name = 'yu'
}
var person = new Object()
setName(person)
alert(person.name)  // yu
```

person变量传递到setName()函数中之后就被【复制】给了obj。在这个函数内部，obj和person引用的是同一个对象。于是，当在函数内部位obj添加name属性后，函数外部的person也将有所反映。

【那么有人就会觉得这个是按引用传递了。我们用一个例子来反推】

```javascript
function setName(obj) {
    obj.name = 'yu'
    obj = new Object()
    obj.name = 'yyf'
}
var person = new Object()
setName(person)
alert(person.name)  // yu
```

如果person是按引用传递的，那么person就会自动被修改为指向其name属性值为“yyf“的新对象。但是，当接下来再访问person.name时，显示的值仍然是“yu”。这说明即使再函数内部修改了参数的值，但原始的引用仍然保持不变。实际上，当在函数内部重写obj时，这个变量引用的就是一个局部对象了。而这个局部对象会在函数执行完毕后立即被销毁。

【可以把ECMAScript函数的参数想象成局部变量】

39、延长作用域链

Try-catch语句的catch块

with语句

```
function buildUrl() {
    var qs = '?debug=true';
    with(location) {
        var url = href + qs;
    }
    return url
}
```

我们可以看到在with外面是可以return url 的。



# 第 4 章 变量、作用域和内存问题



# 第 5 章 引用类型

# 第 6 章 面向对象的程序设计

# 第 7 章 函数表达式

# 第 8 章 BOM

# 第 9 章 客户端检测

# 第 10 章 DOM

# 第 11 章 DOM 扩展

# 第 12 章 DOM2 和 DOM3

# 第 13 章 事件

# 第 14 章 表单脚本

# 第 15 章 使用 Canvas 绘图

# 第 16 章 HTML5 脚本编程

# 第 17 章 错误处理与调试

# 第 18 章 JavaScript 与 XML

# 第 19 章 E4X

# 第 20 章 JSON

# 第 21 章 Ajax 与 Comet

# 第 22 章 高级技巧

# 第 23 章 离线应用与客户端存储

# 第 24 章 最佳实践

# 第 25 章 新兴的 API