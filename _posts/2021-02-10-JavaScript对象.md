---
layout: post
title: 10-JavaScript 对象
date: 2021-02-10
description:
tags: JavaScript 前端
---

## 1. 对象

### 1.1 创建对象

可以使用下面两种方法创建对象：

```js
let user = {};		// 字面量
let user = new Object();	// 构造函数
```

**注意**：

- 当对象作为函数参数时，函数可以直接修改对象内部的属性！
- 对象的属性键只能是<font color=red>字符串类型</font>或者 Symbol 类型。



### 1.2 文本和属性

**创建和访问属性**

创建对象的时候，可以将一些属性和键值对放入到 `{}`中：

```js
let user = {
    name: "Jerry",
    age: 20,
    "is girl": false
};

console.log(user.name);		// Jerry
console.log(user[name]);	// Jerry
```

也可以将对象嵌套：

```js
let circle = {
    radius: 2,
    center: {
        x: 3,
        y: 5
    }
};

console.log(circle.center.x);
```






**删除属性**

使用 `delete` 删除属性：

```js
delete user.age;
console.log(user.age);		// undefined
```





**多个单词构成的属性名**

可以给对象添加多个单词组成的属性名，访问时只能使用 `[]` 访问：

```js
let user = {
    name: "Jerry",
    "is girl": false
};

console.log(user["is girl"]);	// false
```





### 1.3 方括号

方括号同样提供了一种可以通过任意表达式来获取属性名的方法，`key`值可以通过用户输入，也可以是程序计算的到的，但 `[]` 内的属性名需要加上引号。

举个栗子：

```js
let user = {
    name: "Jerry",
    "is girl": false
};

let getValue = (Object, key) => Object[key];
alert(getValue(user, "name"));	// Jerry
```





### 1.4 in 操作符

语法：

```js
"key" in object
```

若要判断属性是否存在，可以这样子：

```js
let user = {
    name: "Jerry",
    age: 20
};

alert("key" in user);			// 存在则返回 true
```

下面的方法也可以也可以判断属性是否存在（只限于当属性的值不为 `udefined` 时）：

```js
alert(user.key === undefined);	// 不存在则返回 true
```




### 1.5 for ... in

语法：

```js
for (let key in object){
    // body
}
```

举个栗子，打印所有属性的值：

```js
let cat = {
    name: "Tom",
    color: "brown"
};

for (let key in cat) {
    console.log(key + ": " + cat[key]);
}
```

结果为：

```js
name: Tom
color: brown
```

应用栗子：将所有属性值为数字类型的属性值乘2

```js
let menu = {
    width: 200,
    height: 300,
    title: "My menu"
};

multiplyNumeric(menu);

console.log(menu);

function multiplyNumeric(menu) {
    for (let key in menu) {
        if (typeof menu[key] == "number") {
            menu[key] *= 2;
        }
    }
}
```




### 4.6 使用对象实现单链表

```js
let list = {
    value: 1,
    next: {
        value: 2,
        next: {
            value: 3,
            next: {
                value: 4,
                next: null
            }
        }
    }
}

let curr = list;
while (curr != null) {
    console.log(curr.value);
    curr = curr.next;
}
```

其中每一个节点包含了 `value` 和 `next` 属性，`next` 指向从下一个节点开始的部分链表。



---

## 2 对象引用与克隆

### 2.1 对象引用

对象名存储的不是整个对象，而是对象的内存首地址，如下如所示：

<img src="/images/posts/blogImages/image-20210208154623522.png" width=800>

复制对象时，复制的是对象的引用：

```js
let user = {name: "Tom"};
let admin = user;
```

因此，使用对象名作为函数参数时，形参相当于复制了对象的地址，形参和实参共享内存，因此函数内对对象的修改在函数外也有效。

当通过 `admin` 修改 `name` 属性时，`user` 的属性也会随之变化：

```js
admin.name = "Jerry";
console.log(user.name); // "Jerry"
```




### 2.2 克隆对象

可以创建一个新对象，在逐个属性复制实现对象的克隆。

使用 `Object.addign` 也可以实现克隆，语法：

```js
Object.assign(dest, [src1, src2, src3...])
```

**举个栗子**

克隆 `user` 对象到 `admin`：

```js
let user = { name: "Tom" };
let admin = Object.assign({}, user);
console.log(user);		//{name: "Tom"}
```

将 `user1` 和 `user2` 的所有属性复制到 `admin`：

```js
let admin = {};
let user1 = { name: "Tom" };
let user2 = { age: 20 };
Object.assign(admin, user1, user2);
console.log(admin); 	// {name: "Tom", age: 20}
```

注意：当遇到一个含嵌套对象的对象时，需要使用遍历检查每一个属性值是否是一个对象，否则对象的属性值可能只复制了引用，而未克隆！

---



## 3. 对象方法 与 this

### 3.1 对象方法

语法：

```js
var object = {
    attribute1 : value1,    // attribute
    attribute2 : value2,

    method : function(){     // method
        return this.arrtribute1;
    }
};

object.method();
```

对象内定义方法是通过将一个`函数表达式`与 `key` 绑定实现的。

举个栗子：

```js
let user = {
    name: "Tom",
    getUserName: function() {
        // this 指向当前对象
        return this.name;
    }
};

alert(user.getUserName);
```




### 3.2 this

`this` 只有在被函数调用时，指向函数，未调用时为 `undefined`

如通过 `return this` 返回对象：

```js
let cat = {
    name: "tom",
    getThis: function() {
        return this;	// 返回对象
    }
}

console.log(cat.getThis());	// {name: "tom", getThis: ƒ}
```

注意：箭头函数没有 `this`。在箭头函数内部访问到的 `this` 都是从外部获取的。


**应用栗子**

创建一个小计算器，实现读取数据、加和乘的功能

```js
let caculator = {
    a: 0,
    b: 0,
    read: function() {
        this.a = +prompt("a = ", '');
        this.b = +prompt("b = ", '');
    },
    sum: function() {
        return this.a + this.b;
    },
    mul: function() {
        return this.a * this.b;
    }
}

caculator.read();
console.log(caculator.sum());
console.log(caculator.mul());
```

---

## 4. 构造器和 new 操作符

### 4.1 构造器

通过构造函数可以创建多个对象，构造函数有以下约定：

- 函数名大写开头
- 只能由 `new` 操作符来执行



举个栗子：

```js
function Cat(name, color = "brown") {
    this.name = name;
    this.color = color;
}

let tom = new Cat("tom");
console.log(tom.name);
```




### 4.2 new 操作符

`new` 操作符的过程如下：

- 创建新的对象，并分配 `this`
- 函数体执行，通过 `this` 修改属性
- 返回 `this`




### 4.3 构造器中的方法

通过将函数表达式赋值给 `this.method` 来添加方法：

```js
function Cat(name, color = "brown") {
    this.name = name;
    this.color = color;
    this.showInfo = function() {
        alert(this.name + " is a " + this.color + " cat.");
    }
}

let tom = new Cat("tom");
tom.showInfo();
```




**应用实例**

创建一个生成加法器的构造器：

```js
function Caculator() {
    this.a = 0;
    this.b = 0;
    this.read = function() {
        this.a = +prompt("a = ", '');
        this.b = +prompt("b = ", '');
    }
    this.sum = function() {
        return this.a + this.b;
    }
    this.mul = function() {
        return this.a * this.b;
    }
}

let caculator = new Caculator();
caculator.read();
alert(caculator.mul());
```

---

## 5. 可选链

### 5.1 可选链

若用户无 address，则输出 `undefined`，否则输出地址的街道，可以通过下面的代码实现：

```js
let user = {};

alert(user.address ? user.address.street : undefined);
```

但这样套重写两次 `user.address`且代码可读性差。

**可选链**

如果可选链 `?.` 前面的部分是 `undefined` 或者 `null`，它会停止运算并返回该部分。

上面的例子可以用可选链实现：

```js
let user = {};

alert(user?.address?.street);	// undefined
```

如果地址存在：

```js
let user = {
    address: {
        street: "sdasf"
    }
};

alert(user?.address?.street);	// "sdasf"
```




### 5.2 可选链变体

变体：

- 可以使用 `?.` 来访问一个可能不存在的属性；
- 可以使用 `?.()` 来调用一个可能不存在的函数；
- 可以使用 `?.[]` 来访问一个可能不存在的属性。

```js
let user = {};

alert(user?.["key"]);	// undefined
alert(user.show?.());   // undefined

alert(user.show());  	// error
alert(user["key"]);		// undefined
```

注意：可以使用 `?.` 来访问或删除属性，但不能放在左边写入！






## 6. getter 和 setter

### 6.1 语法

访问器属性由 “getter” 和 “setter” 方法表示。在对象字面量中，它们用 `get` 和 `set` 表示

```js
let obj = {
    get propName() {
        // 当读取 obj.propName 时，getter 起作用
    },

    set propName(value) {
        // 当执行 obj.propName = value 操作时，setter 起作用
    }
};
```

举个栗子：

```js
let user = {
    name: 'John',
    surname: 'Smith',

    get fullName() {
        return `${this.name} ${this.surname}`;
    },

    set fullName(value) {
        [this.name, this.surname] = value.split(' ');
    }
}

// set fullName
user.fullName = "Alice Cooper";
console.log(user.surname); // cooper

// get fullName
console.log(user.fullName)
```






参考内容：[现代 JavaScript 教程](https://zh.javascript.info/)
