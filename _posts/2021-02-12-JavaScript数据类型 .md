---
layout: post
title: 11-JavaScript 数据类型
date: 2021-02-12
description:
tags: JavaScript 前端
---

# 1. 原始类型

JavaScript 提供了 7 种原始类型：`string`，`number`，`boolean`，`bigint`，`symbol`，`null` 和 `undefined`。

与对象不同的是，原始类型是轻量型的，它比对象“轻”，不许要占用太多空间，举个栗子：

```js
let str = "hhh";
alert(str.toUpperCase());	// HELLO
```

在这个过程中，创建了原始值 `str`，它不属于对象，但它可以像对象一样使用，在调用函数 `toUpperCase`时经历了下面的步骤：

1. 创建一个包含字符串面值的特殊对象，具有特定的方法；
2. 调用方法返回新的字符串；
3. 销毁特殊对象


**构造器**

我们也可以使用 `new` 来创建原始类型的对象，但**不推荐**，这种方法只供内部使用，否则可能会出现问题，如：

```js
if (new Number(0)) {
    alert("valid!");	// 会执行到这一步
}
```

---

## 2. 数字类型

### 2.1 数字的表示

数字可以用十进制、科学计数、十六进制、二进制和八进制来表示：

```js
let billion = 1e9;
let ns = 1e-9;
alert(0xFF); 		// 十六进制
alert(0b11111111);	// 二进制
alert(0o377);		// 八进制
alert(0o377 == 0b11111111); // true
alert(isFinite(num));	// 判断数字是否为有限值
```


### 2.2 toString(base)

`toString(base)`可以在给定 `base` 进制数字系统中的 `num` 的字符串形式：

```js
let num = 255;
alert(num.toString(16));	// ff
alert(num.toString(2));		// 11111111
```


### 2.3 舍入

```js
let num = 6.256;
alert(Math.floor(num));		// 6
alert(Math.ceil(num));		// 7
alert(Math.round(num));		// 6

let num1 = num.toFixed(2);	// 6.25
alert(num1);
alert(typeof num1);			// string
```


### 2.4 精确度

看下面的例子：

```js
alert(0.1 + 0.2 == 0.3);    // false
alert( 0.1 + 0.2 ); // 0.30000000000000004
```

可以通过下面的方法得到正确的结果（`toFixed` 返回的是字符串，通过 `+`得到数字）：

```js
let sum = +(0.1 + 0.2).toFixed(2);
alert(sum);
```

**应用实例**

`toFixed` 和 `round` 都会舍入到最近的数，存在进度问题，如：

```js
alert(6.35.toFixed(20));
alert(6.35.toFixed(1));		// 6.3

alert(1.35.toFixed(20));
alert(1.35.toFixed(1));		// 1.4
```

在舍入前现将数字靠近整数：

```js
alert(6.35.toFixed(1)); 	// 6.3
alert(Math.round(6.35 * 10) / 10);  // 6.4
```


### 2.5 parse

`parseInt`和`parseFloat`可以从字符串中解析出数字，遇到非数字停止；若首个字符不是数字，则返回`NAN`。

```js
alert(parseInt('120.5cm')); // 120
alert(parseFloat('120.5cm')); // 120.5
alert(parseInt("a100")); // NAN

alert(parseInt('0xff', 16)); // 指定基数
```


### 2.6 数学函数

```js
random(): number	// Returns a pseudorandom number between 0 and 1.

let a = Math.random();		// 0-1 的随机数
let b = Math.max(5, 0, -5, 9);	// 9
let c = Math.pow(2, 3);			// 8
```

自定义一个随机函数，random(min, max)，生成 min 到 max 之间的随机数：

```js
let random = (min, max) => {
    let a = Math.random();
    return min + a * (max - min);
};

alert(random(2, 8));
```

---

## 3. 字符串

### 3.1 访问字符串

获取长度：

```js
let str = "this is a string";
alert(str.length)
```

访问字符：

```js
let str = "string";
console.log(str[0]);
console.log(str.charAt(0));

for (let char of str) {
    console.log(char);
}
```

注意：JavaScript 字符串不可更改！只能创建新字符串。


### 3.2 常用字符串函数

**大小写**

```js
let str = "string";
console.log(str.toUpperCase());
```

**startsWith，endsWith， includes**

```js
alert("string".includes("tr"));		// true
alert("string".startsWith("str"));	// true
alert("string".endsWith("ing"));	// true
```

**子字符串**

`slice` 比较常用：

```js
slice(start?: number, end?: number): string;
```

以下函数都可以截取子字符串：

```js
alert("string".substr(1, 2));   // tr
alert("string".substring(1, 2));// t
alert("string".slice(1, 2));    // t
```

| 方法                    | 选择方式……                                            | 负值参数            |
| :---------------------- | :---------------------------------------------------- | :------------------ |
| `slice(start, end)`     | 从 `start` 到 `end`（不含 `end`）                     | 允许                |
| `substring(start, end)` | `start` 与 `end` 之间（包括 `start`，但不包括 `end`） | 负值代表 `0`        |
| `substr(start, length)` | 从 `start` 开始获取长为 `length` 的字符串             | 允许 `start` 为负数 |


**trim**

`trim` 函数可以删除字符串前后的空格：

```js
let str = " this is a line. ";
console.log(str.length);
console.log(str.trim().length);
```

**repete**

`str.repeat(n)` —— 重复字符串 `n` 次。


**应用举例**

创建一个函数，该函数能够将字符串首字母大写：

```js
let func = (str) => str[0].toUpperCase() + str.slice(1);

console.log(func("tom"));   // "Tom"
```


## 4. 数组

### 4.1 声明

有下面两种方法声明数组：

```js
let arr = [];
let arr = new Array();
```

注意：尽量不要用 `new Array()`，因为它存在问题：

```js
let len = 5;
let arr = new Array(5);	// 此时创建了一个长为 5 的数组，但所有项都是 undefined
```

数组是一个特殊的对象，可以通过下标来访问元素：

```js
let fruits = ['banana', 'apple', 'orange'];
for (let i = 0; i < fruits.length; i++) {
    console.log(fruits[i]);
}
```

修改和添加元素：

```js
fruits[0] = "strawberry";
fruits[3] = "watermelon";
```

数组可以存储任何类型的数据，甚至是函数！


**应用例子**

数组就是一个对象，当调用对象函数时，`this` 指向对象：

```js
let arr = [1, 2, 3];
arr[3] = function() {
    alert(this);
}
arr[3]();
```


### 4.2 常用数组方法方法

**pop & push**

`pop` 弹出尾部的一个元素（删除最后一个元素，length 减 1）；`push`在尾部插入若干元素（插入元素和 key，更新 length）；

```js
let arr = [1, 2, 3, 4];

arr.push(5, 6);
arr.pop();

alert(arr);	// 1, 2, 3, 4, 5, 6
```

**shift & unshift**

`shift`取出首部元素并返回（移出后所有元素左移，所有索引减 1，更新 length）；`unshift`在首端添加元素（插入后所有元素后移，索引和 length 随之更新）；

```js
let arr = [1, 2, 3, 4];

arr.unshift(-1, 0);
arr.shift();

alert(arr);	// 0, 1, 2, 3, 4
```

**splice**

`splice`可以根据值删除一个元素：

```js
splice(start: number, deleteCount?: number): number[]
```

举个栗子

```js
let arr = [1, 2, 3, 4];
// start=1, deleteCount=2, 插入 10 和 100
arr.splice(1, 2, 10, 100);
for (let elem of arr) {
    console.log(elem);
}
```

等价于：

```js
arr.splice(1, 2);
arr.splice(1, 0, 10, 100);
```

**slice**

`slice(start, end)` 可以复制数组的一部分：

```js
let arr = [0, 1, 2, 3, 4, 5];

let newArr = arr.slice(1, 4);
for (let elem of newArr) {
    console.log(elem);
}
```

**forEach**

向 `forEach` 传入一个函数表达式，通过回调函数遍历所有函数：

```js
let arr = [1, 2, 3, 4, 5];
arr.forEach((element, index, array) => {
    console.log(element ** 2);
});
arr.forEach(function(element, index) {
    console.log("index = " + index + ", value = " + element);
});
```

**sort(fn)**

`arr.sort` 方法可以对数组进行<font color=red>原味排序</font>，默认将所有元素转化为字符串，再进行排序，可以自定义排序：

```js
let arr = [1, 3, 12, 2];
arr.sort(); // 1 12 2 3
arr.forEach(element => console.log(element));

arr.sort(function compare(a, b) {
    if (a > b) return 1; // 第一个值比第二个值大
    if (a == b) return 0;
    if (a < b) return -1;
});     // 1 2 3 12
arr.forEach(element => console.log(element));
```

更简单的办法：

```js
let arr = [1, 3, 12, 2];
arr.sort((a, b) => a - b);
arr.forEach(element => console.log(element));	// 1 2, 3, 12
```

应用：按年龄排序

```js
let john = { name: "John", age: 25 };
let pete = { name: "Pete", age: 30 };
let mary = { name: "Mary", age: 28 };

let arr = [pete, john, mary];

arr.sort((a, b) => a.age - b.age);
```

随机排序：

```js
arr.sort(()=>Math.random()-0.5);
```


**map**

`map(func)` —— 根据对每个元素调用 `func` 的结果创建一个新数组

```js
let arr = [1, 2, 3, 4, 5];
let square = arr.map(element => element ** 2);
square.forEach(element => console.log(element));
```


**reduce**

语法：

```js
let value = arr.reduce(function(accumulator, item, index, array) {
  // ...
}, [initial]);
```

第一个参数是一个函数表达式，accumulator 是上一次调用的结果，初始值为 initial，item 是当前数组元素，index 是当前索引，arr 是数组本身。

举个栗子：计算数组的和

```js
let arr = [1, 5, 2, 3, 7];
let sum = arr.reduce((sum, curr) => sum + curr, 0);
```


### 4.3 循环

**for**

```js
for (let i=0; i<arr.length; i++){

}
```

**for ... of**：不能获取索引，只能获取元素值

```js
for (let elem of arr){
    console.log(elem);
}
```

**for ... in**：适用于一般对象，但不适用于数组，速度比较慢（<font color=red>永远不要用这个</font>）

```js
for (let val in arr){
    console.log(val);
}
```

**应用**

下面的例子计算了输入数字的和：

```js
let arr = [];
while (true) {
    let num = +prompt("number = ", '0');
    if (!isFinite(num) || num == "" || num == null) break;
    arr.push(num);
}

let sum = 0;
for (let elem of arr) {
    sum += elem;
}
console.log(sum);
```


### 4.4 多维数组

```js
let arr = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
];

for (let i = 0; i < arr.length; i++) {
    for (let j = 0; j < arr[i].length; j++) {
        console.log(arr[i][j]);
    }
    console.log("");
}
```


### 4.5 传引用

因为数组是特殊的对象，因此 `arr1` 和 `arr` 是同一数组的引用：

```js
let arr = [1, 2, 3];
let arr1 = arr;
arr1.push(4);
console.log(arr.length);	// 4
```


**应用**

求一个数组的最大连续子数组，使得子数组和最大：

```js
function getMax(arr) {
    let maxSum = 0;     // 当前记录到的最大值
    let currSum = 0;    // 当前子数组的和
    for (let elem of arr) {
        currSum += elem;
        // 若当前子数组和小于零，则往后重新选子数组
        if (currSum < 0) currSum = 0;
        maxSum = Math.max(maxSum, currSum);
    }
    return maxSum;
}

arr = [1, 5, 3, -5, 2, 4, -3, 8, 6, -10, 2, -6];
alert(getMax(arr));
```

---


## 5. Map 和 Set

### 5.1 Map

`Map` 是一个带键的数据项极核，与 `object` 一样，但 `Map`允许任意类型的 `key`。

常用方法有：

```js
new Map()	// 创建 map
map.set(key, value)		// 添加键值对
map.get(key)	// 根据 key 获取元素
map.has(key)	// 查找元素是否存在
map.delete(key)	// 删除元素
map.size		// 查看元素个数属性
```

举个栗子：

```js
let map = new Map();
map.set(5, "as");
map.set(1, "sfd");
map.set(2, "rfre");

alert(map.has(5));
alert(map.size)
```


**迭代**

可以通过 `keys()`, `values()` 和 `for...of` 或 `map.entries()` 来遍历元素，得到的顺序与插入顺序一致：

```js
let m = new Map([
    ['ccc', 50],
    ['aaa', 80],
    ['bbb', 20]
]);

// 遍历 keys
for (let key of m.keys()) {
    console.log(key);
}

// 遍历 values
for (let value of m.values()) {
    console.log(value);
}

// 遍历所有实体 [key, value]
for (let pair of m) {
    console.log(pair);
}
```


### 5.2 Set

`Set` 是一个特殊的类型集合，每一个值只能出现一次，有以下主要方法：

```js
new Set(iterable) 	// 根据 iterable 对象创建 set
set.add(value)
set.delete(value)	// 返回 true 或 false
set.has(value)		
set.clear()			// 清空
set.size
```

可以使用 `for...of` 或 `forEach` 来遍历：

```js
let s = new Set([5, 1, 9, 7, 2, 4, 3]);
s.forEach((value) => console.log(value));
```

应用举例：过滤一个数组中重复的元素

```js
function unique(arr) {
    return Array.from(new Set(arr));
}
```

---


## 6. 对象和数组转换

### 6.1 对象到数组

`Map` 容器支持：`map.keys()`, `map.values()` 和 `map.entries()`普通对象也支持类似方法：

**方法：**

- `	object.keys(obj)`：返回一个对象所有的键的数组；
- `Object.values(obj)`：返回一个包含该对象所有值的数组；
- `Object.entries(obj)`：返回一个包含该对象所有 [KEY, VALUE] 键值对的数组；

对象的 key 会转化为字符串，返回值都是可迭代的数组类型。


**举个栗子**：

```js
let user = {
    name: "Tom",
    age: 30
};

console.log(Object.keys(user));     // ['name', 'age']
console.log(Object.values(user));   // ['Tom', 30]
console.log(Object.entries(user));  // [['name', 'Tom'], ['age', 30]]

for (let entry of Object.entries(user)) {
    console.log(entry);
}
```


### 6.2 数组到对象

**转换对象**

对象缺少类似数组的方法，如 `map` 和 `filter`，可以用 `Object.entries(obj)`将对象转为数组，再用数组方法操作数组，再通过 `Object.fromEntries(array)`将数组转为对象：

举个栗子：下面的代码将水果价格加倍

```js
let fruits = {
    apple: 20,
    peach: 18
};

let doubleFruitPrice = Object.fromEntries(
    Object.entries(fruits).map(([key, value]) => ([key, value * 2]))
);
console.log(doubleFruitPrice);
```


**应用例子**：计算工资之和

```js
let sumSalaries = function(salaries) {
    return Object.values(salaries).reduce((a, b) => a + b, 0);
}

alert(sumSalaries(salaries)); // 650
```


### 6.3 数组解构

```js
let arr = [1, 2];
let [num1, num2] = arr;
console.log(num1);  // 1
console.log(num2);  // 2

let [num3, ] = arr;	// 丢弃第二个元素
```

上面的代码将数组中的元素取出并存放到变量 `num1` 和 `num2` 中。


等号右侧可以是任何可迭代的对象：

```js
let [a, b, c] = "are";
console.log(b);		// 'r'

let [word1, word2] = "it's me!".split(' ');
console.log(word1);
console.log(word2);
```


### 6.4 对象解构

```js
let user = {
    name: 'Tom',
    age: 20
}

let {name, age} = user;
```


**应用例子**

求最高工资：

```js
let salaries = {
    "John": 100,
    "Pete": 300,
    "Mary": 250
};

console.log(Object.values(salaries).reduce((max, curr) => Math.max(max, curr), 0));
```

---

## 7. 日期与时间

### 7.1 创建

使用 `new Date()` 创建对象：

```js
let now = new Date();	// 当前时间
console.log(now);
console.log(typeof now);

// 从 1970-1-1 开始过了 24*3600*1000ms 后
let date = new Date(24 * 3600 * 1000);
console.log(date);
```


### 7.2 访问

```js
let now = new Date();

console.log(now.getFullYear());  // 4 位数
console.log(now.getMonth() + 1); // 0-11
console.log(now.getDate());
console.log(now.getHours());
console.log(now.getMinutes());
console.log(now.getSeconds());
console.log(now.getMilliseconds());

// 1970-1-1 00:00 至今经历的毫秒数
console.log(now.getTime());
```

计算时间：

```js
let start = new Date();

for (let i = 0; i < 10000; i++) {
    let j = i ** 3;
}

let end = new Date();
console.log(`cost ${end - start} ms`);
```


参考内容：[现代 JavaScript 教程](https://zh.javascript.info/)
