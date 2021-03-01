---
layout: post
title: 09-JavaScript 语法基础
date: 2021-02-08
description:
tags: JavaScript 前端
---

## 1. JavaScript 学习工具

学 JavaScript 可以结合 html 来学，我使用的编辑器是 VScode，界面美观，插件丰富：

<img src="/images/posts/blogImages/image-20210108163524900.png" width=800>


/home/yin/workspace/Git/git-jhy.github.io

### 1.1 好用的插件

主题插件：`Atom One Dark Theme`、`Material Icon Theme`

代码补全：`JavaScript(ES6) code snippets`、`Beauty`

实时预览效果：`Live Server`





### 1.2 相关设置

- Linux系统上安装 VScode 后，快捷键 Ctrl + E 会自动打开 VScode，终端输入代码可解决：

  xdg-mime default dde-file-manager.desktop inode/directory

- 键盘放大代码比较麻烦，可以设置鼠标滚轮放大缩小 [blog](https://blog.csdn.net/qq_34492122/article/details/102989330)；

- 代码主题和图标主题安装后需要在 settings 中设置选择对应的主题；

- 安装 `Live Server`后，建议重新设置快捷键(File - Preference - Keyboard Shortcuts 搜索Live Server)；

- VScode 输入 `html:5` 按下 Enter 可以生成 HTML 模板

---

## 2. 使用 JavaScript

### 2.1 内部脚本

将 JavaScript 代码放在一对 `<script></script>` 标签之间：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <script>
        alert("Hello world!");
    </script>
</body>
</html>
```





### 2.2 外部脚本

在 HTML 中引用外部引用：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script src="test.js"></script>
</head>
<body>
</body>
</html>
```

下面是 `test.js`文件内容：

```javascript
console.log("Hello world!")
```


### 2.3 strict

脚本文件的**顶部**使用 `"use strict";` 或者 `'use strict';`可以使整个脚本文件都将以“现代”模式进行工作。

```js
"use strict";

// 代码以现代模式工作
```

一旦进入严格模式，就无法取消了！

---

## 3. 交互

#### alert

`alert` 函数可以让浏览器窗口弹出一个小窗口，叫做模态窗，等待用户处理：

```js
alert("Hello")
```

#### prompt

语法：

```js
result = prompt(title, [default]);
```

`prompt` 可以接受两个参数：

- title：显示给用户的文本
- default：可选的参数，指定 input 框的默认值，`[...]` 表示方括号内的参数是可选）

浏览器会显示一个带有文本消息的模态窗口，还有 input 框和确定 / 取消按钮。



举个栗子：

```js
let year = prompt('Which year are you born in?', 2021);
alert(`You are ${(new Date()).getFullYear() - year} years old now!`);
```

#### confirm

语法：

```js
result = confirm(question);
```

`confirm` 函数显示带有 `question` 以及确定和取消按钮的模态窗口，返回 `true` 或 `false`

举个栗子：

```js
let isGirl = confirm("Are you a Girl?");
alert(isGirl);
```

---

## 4. 变量和数据类型

### 4.1 变量

**变量名**

1. 变量名只能包含字母、数字、$ 和 _
2. 首字母非数字

**声明变量**

用 `let` 声明变量：

```js
let message = "Hello";
alert(message);
```



### 4.2 常量

用 `const` 声明常量：

```js
const PI = 3.14;
const RED = '#F00';
const ORANGE = '#FF7F00';
```



### 4.3 关键字的选择

可以使用 `var`、`let` 或 `const` 声明变量：

- `let` — 现代的变量声明方式
- `var` — 老旧的变量声明方式，一般情况下不使用
- `const` — 只声明常量



### 4.4 数据类型

JavaScript 的一个变量可以存储多种不同的数据类型数据，因为 JavaScript 是一种 “动态类型” (dynamically typed) 语言。如：

```js
let num = 5;
num = "hahaha";
```

JavaScript 有 **8 种数据类型**（一种为引用类型）。

```js
let n = 123;		// Number
let str = 'this';	// String
let checked = true;	// Boolean
let age = null;		// null
let age;			// undefined
alert(age);			
let arr = [1, 2];	// obect
```

字符串可以使用 `''`、`""` 或 `` 包裹，使用反单引号时可以内嵌表达式：

```js
let name = "Tom";
alert(`name: ${name}`);		// name: Tom
alert(`3 + 5 = ${3 + 5}`);	// 3 + 5 = 8
```

`typeof` 运算符可以查看数据的类型

```js
let n = 5;
console.log(typeof n);	// 作为运算符
console.log(typeof(n));	// 作为函数
```

**引用类型**

引用类型通常叫做类，类的对象是引用类型的，通过 `new` 创建对象：

```js
let a = new Number(5);
```



### 4.5 类型转换

多数情况下，运算符和函数会自动将接受到的数据转换到正确的类型，但有时需要进行显式的转换。



##### 字符串转换

```js
"use strict";

let value = true;
console.log(typeof value);	// boolean
value = String(true); 		// "true"
console.log(typeof value);	// string
```



##### 数字转换

`Number()` 可以将纯数字字符串转为数字、`undefined` 和非数字字符串转为 `NaN`、`null` 转为 0、`true` 和 `false` 转为 1 和 0：

```js
console.log("6" / "2"); 	// 3
let name = "tom";
name = Number(name);		// NaN
console.log(name); 			// Number
```

注：使用一元运算符 `+` 可以实现其他类型转换为数字类型。



##### 布尔型转换

Boolean() 将 null、空字符串、0、undefined 和 NaN 转为 `false`，其他转为 `true`：

```js
alert(Boolean(""));			// false
alert(Boolean(2));			// true
```

---

## 5. 运算符

### 5.1 数学运算符

下面的栗子数学表达式为：$2^3$ 对 $5$ 取余

```js
console.log(2 ** 3 % 5);	// 3
```



### 5.2 加号

**连接字符串**

加号的运算顺序为从左往右，遇到字符串时，数字会转换为字符串，再进行连接。

举个栗子：

```js
console.log(1 + 2 + "3");	// "33"
```

若为减法，则字符串转换为数字，再进行运算：

```js
console.log("5" - 2);		// 3
```

**数字转化**

通过 `prompt` 接收两个数字，再相加：

```js
let a = prompt("Input the value of a", '');
let b = prompt("Input the value of b", '');

console.log(a + b);     // 36
```

得到的结果为 36，原因是 a 和 b 都是字符串，可以这样处理以获得正确结果：

```js
console.log(+a + +b);	// 9
```



### 5.3 自增

```js
let a = 5;
let b = a++;	// 5
let c = ++a;	// 7
```



### 5.4 逗号运算符

逗号运算符可以使用 `,` 分隔语句，只有最后一个语句结果会被返回，如：

```js
let a = (1 + 1, 2 + 2);	// 4
for (let i = 0, j = 2; i < 5; i++) {
	...
}
```



### 5.5 比较运算符

数字和字符串比较时，字符串会转化为数字：

```js
console.log("01" == 1);		// true
```

`==` 用来比较值相同，`===` 为严格相等，除了值相等也要求类型相同：

```js
console.log(0 === false);	// false
```



### 5.6 逻辑运算符

在 JavaScript 中，逻辑运算符更加灵活强大。

**或运算符**

`||`运算符寻找第一个真值并返回

- 从左往右计算操作数（均转化为 boolean）
- 出现真值则返回操作数的初始值
- 若无真值出现则返回最后一个操作数

举个栗子：从左往右出现 5 为真值，因此 a = 5

```js
let a = 0 || 5 || 2;
console.log(a); 	// 5
alert("" || null || "a" || 0);	// "a"
```

再看个例子：

```js
alert(alert(1) || 2 || alert(3));
```

执行上面的语句，会出现 2 个弹窗，首先弹出 1，由于 `alert` 函数无返回值，2 是真值，因此 alert(3) 未执行，直接弹窗显示 2



**与运算符**

`&&` 寻找第一个假值

- 从左往右一次计算操作数
- 所有操作数转化为 boolean，出现 false 则返回操作数的初始值
- 无 false 则返回最后一个操作数的初始值

举个栗子：

```js
let a = 1 && 2 && null && 0;
console.log(a);		// null
let b = 1 && 2 && 3;
console.log(b);		// 3
```

再看个栗子：

```js
alert(alert(1) && alert(2));
```

执行上述代码后，首先弹窗显示 1，由于 `alert` 函数无返回值，因此第一个操作数为 `undefined`，为假，不执行后面的语句，因此直接弹窗 undefined 后结束执行。

---

## 6. 流程控制

### 6.1 if - else

if else 的结构如下：

```js
let hour = (new Date).getHours();

if (hour < 12) {
    alert("Good morning!");
} else if (hour < 18) {
    alert("Good afternon!");
} else {
    alert("Good evening!");
}
```

可以使用条件运算符 `?` 实现同样的功能：

```js
let hour = (new Date).getHours();

time = (hour < 12) ? "morning" :
    (hour < 18) ? "afternoon" :
    "evening!";
```



### 6.2 while

**while** 循环的结构如下：

```js
let i = 3;
while (i) {
    console.log(i--);
}
```

上面的代码执行了三次，打印了 3 2 和 1

等价于 `do-while`循环：

```js
let i = 3;
do {
    console.log(i--);
} while (i);
```



### 6.3 for

**for** 循环的结构如下：

```js
for (begin; condition; step){
    // body
    if (condition2)	break;
    if (condition1)	continue;
}
```

执行步骤为：

- 判断`condition` 是否为真
- 为真则执行 `body`，接着运行 `step`，回到第一步
- 为假则退出循环

举个栗子：

```js
for (let i=0; i<5; i++){
    console.log(i);
}
```

其中 `i`是在循环中声明的，在循环外不可见！



### 6.4 switch

语法：

```js
switch (x) {
    case val1:	// if (x == val1)
        ...
        [break]
    case val2:
        ...
        [break]
    default:
        ...
        [break]
}
```

举个栗子：

```js
let num = 23;
switch (num % 2) {
    case 0:
        alert("Even");
        break;
    case 1:
        alert("Odd");
        break;
    default:
        alert("An error occured!");
}
```

**case 分组**

```js
let day = (new Date()).getDay();

switch (day) {
    case 1: // 下面这 5 个 case 被分在一组
    case 2:
    case 3:
    case 4:
    case 5:
        alert("work time");
        break;
    case 6:	// 下面这 5 个 case 被分在一组
    case 0:
        alert("relax time");
        break;
}
```

---

## 7. 函数

### 7.1 函数声明

函数的声明方式如下：

```js
function Name(parameters) {
    // body
}
```

**变量**

函数内定义的变量为`局部变量`，作用域为函数内部；

函数外定义的变量为`外部变量`，作用域为全局环境；



**函数参数**

- 函数调用时会创建副本，函数内对参数的修改对函数外无效！
- 允许对参数赋默认值

函数在定义时允许指定默认参数：

```js
function add(a=0, b=0){
    return a + b;
}
```

**返回值**

函数的返回值可有可无，无返回值的函数也可以使用`return` 来结束函数。

```js
function printStars(num){
    if (num < 1) return;
    while (num--){
        console.log("*");
    }
}
```



### 7.2 函数表达式

在 JavaScript 中，函数也可以作为值赋给一个变量，这种创建函数的语法称为`函数表达式`，是表达式上下文中的函数。

语法：

```js
let name = function(arg1, arg2, ..., argN) {
    // body
};
```

注：函数表达式不是代码块而是一个赋值语句，因此末尾需要加上一个分号 `;`。



函数表达式类似一个数值，可以赋给其他变量，下面的代码可以打印函数的定义：

```js
let add = function(a, b) {
    return a + b;
}

console.log(add);		// 函数定义代码
console.log(add(2, 3));	// 5
```



**函数表达式 VS 函数声明**

- 函数声明在函数调用的后面
- 函数表达式只有在创建之后才能够使用



### 7.3 回调函数

一个函数表达式可以作为另一个函数的参数，称为回调函数。举个栗子：

```js
function ask(question, yes, no) {
    if (confirm(question)) {
        yes();
    } else {
        no();
    }
}

function yes() {
    alert("You agreed!")
}

function no() {
    alert("You disagreed!")
}

ask("Are you handsome?", yes, no)
```

当用户点击确认时，会执行 `yes()` 函数，否则执行 `no()` 函数。



### 7.4 箭头函数

使用下面的方法也可以创建函数，而且更简洁高效：

```js
let func = (arg1, arg2, ..., argN) => return expression
let func = (arg1, arg2, ..., argN) => {
    // body
}
```

等价于：

```js
let func = function (arg1, arg2, ..., argN){
    return expression;
}
```

举个栗子：

```js
let power = (a, b) => a ** b;
alert(power(2, 3));		// 8
```

当函数体有多行时，用 `{}` 括起来：

```js
let max = (a, b) => {
    if (a > b) {
        return a;
    } else {
        return b;
    }
};

alert(max(2, 3));	// 3
```

应用例子：

上面的回调函数可以写作：

```js
let ask = function(question, yes, no) {
    if (confirm(question)) yes();
    else no();
};

ask("You are handsome?",
    () => { alert("You agreed!") },
    () => { alert("You disagreed!") }
);
```



### 7.5 调度

可以通过 `setTimeout` 和 `setInterval`来指定时间调用函数：

- `setTimeout`：将函数推迟一段时间后再执行
- `setInterval`：设置重复执行的间隔



#### setTimeout

语法：

```js
let timerId = setTimeout(func|code, [delay], [arg1], [arg2], ...)
```

`func|code` 是函数表达式，`delay` 是延迟时间，`arg` 是函数的参数。



举个例子： 1000 ms 后弹窗一次

```js
setTimeout((name) => alert('Hello! ' + name), 1000, "Jerry");
```

**取消调度**

```js
let timerID = setTimeout();
clearTimeout(timerID);
```



#### setInterval

它的用法与 `setTimeout` 一样：

```js
let timer = setInterval(function() {
    console.log((new Date()).getSeconds());
}, 1000);
```



**嵌套**

嵌套的 `setTimeout` 可以精确地设置时间间隔：

```js
let delay = 5000;
let timerID = setTimeout(function request() {
    if (failed == true) {
        delay *= 2;
    }
    timerID = setTimeout(request, delay);
}, delay);
```



---

## 8. 类与继承

前面我们学习了使用构造器和 `new` 创建对象的方法：

```js
function Cat(name, color = "brown") {
    this.name = name;
    this.color = color;
}

let tom = new Cat("tom");
console.log(tom.name);
```

现代 JavaScript 中，还有一个更高级的“类（`class`）”构造方式，它引入许多非常棒的新功能，这些功能对于面向对象编程很有用。



### 8.1 class

语法：

```js
class className {
    // method
    constructor() {...}
    method1() {...}
    method2() {...}
    ...
}
```

然后使用 `new className` 来创建类的对象，`new` 会自动调用 `constructor()` 方法，因此可以在构造方法中初始化对象。



下面的类定义了矩形，包含了长和宽两个属性，有计算面积的属性：

```js
class Rect {
    constructor(width, height) {
        this.width = width;
        this.height = height;
    }

    getSquare() {
        return this.width * this.height;
    }
}

let t = new Rect(3, 4);
console.log(t.getSquare());		// 12

console.log(typeof Rect);		// function
```

`constructor` 函数被调用后为对象分配 `this.width` 和 `this.width`。

最后一行代码打印了类的本质 —— function



### 8.2 类表达式

可以像函数函数表达式一样定义一个类表达式：

```js
let Rect = class {
    constructor(width, height) {
        this.width = width;
        this.height = height;
    }

    getSquare() {
        return this.width * this.height;
    }
}

console.log(new Rect(3, 4).getSquare());
```



### 8.3 getters 和 setters

就像对象字面量，类可能包括 getters/setters

举个栗子：

```js
class User {
    constructor(name) {
        this.name = name;
    }

    set name(value) {
        if (value.length < 4) {
            alert("Too short!");
            return;
        }
        this.name = value;
    }
}

let user = new User('n');   // alert
```



### 8.4 继承

使用 `extend` 继承父类，子类可以使用 `super()` 调用父类的构造函数，继承后子类获得父类的所有属性和方法：

```js
class Point {
    constructor(x, y) {
        this.x = x;
        this.y = y;
    }
}

class Circle extends Point {
    constructor(x, y, radius) {
        super(x, y);
        this.radius = radius;
    }

    getSquare() {
        return Math.PI * this.radius ** 2;
    }
}

let c = new Circle(0, 0, 3);
console.log(c.getSquare());
```




参考内容：[现代 JavaScript 教程](https://zh.javascript.info/)
