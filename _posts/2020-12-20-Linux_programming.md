---
layout: post
title: 03-Linux C语言编程
date: 2020-12-10
description: 这周学习Linux C语言编程以及C的指针和Linux操作系统内存管理，通过写这篇笔记来记录点滴知识，成长自己。
tags: Linux C/C++
---
<br>
# 1. Linux C程序

## 1.1 第一个Linux C语言程序

在 workspace/ 中创建一个文件夹，使用命令 `vi hello.c` 创建 .c 文件：

```c
#include <stdio.h>

int main(){
    printf("Hello world!\n");
    return 0;
}
```

**编译运行**：

使用 `gcc` 或 `cc` 来编译 **hello.c** ，生成 **.out** 文件: `gcc xxx.c -o xxx.out`

```shell
$ gcc hello.c -o main.out		# 编译 hello.c 生成的可执行文件为 .out
$ ls -l
-rw-r--r-- 1 yin yin   197 12月 19 20:03 hello.c		# 不可执行
-rwxr-xr-x 1 yin yin 16448 12月 19 20:04 main.out	# 可执行

$ ./main.out		# 运行 ./ 表示当前目录下
```

## 1.2 gcc

**文件类型**

> .out是可执行文件，相当于win上的exe，一般是经过链接产生的可执行文件
>
> .o是编译中间机器码，相当于win上的.obj，一般是通过编译但未链接的文件
>
> .a是静态库，多个.o练链接得到，用于静态链接

### gcc 命令

参数介绍

- `gcc -c hello.c` : 把程序 hello.c 做成 hello.o 文件 <font color=red>xxx.c $\rightarrow$ xxx.o</font>
- `gcc -S hello.c` : 生成 .s 的汇编代码  <font color=red>xxx.c $\rightarrow$ xxx.s</font>
- `gcc hello.c -o hello.out` : 生成可执行文件 .out，-o 后面接 .o 文件名

```shell
$ gcc -c hello.c		# hello.o

$ gcc -S hello.c		# hello.s

$ gcc -o hh.out hh.c	# hello.out
$ gcc hh.c -o hh.out	# hello.out
```

---

# 2. 多文件操作

## 2.1 多文件分而治之

创建一个文件夹 project1 ，使用 vim 编辑器添加 max.c 和 compare.c :

```c
// max.c
int max(int a, int b){
    return a > b ? a : b;
}

// compare.c
#include <stdio.h>
#include "max.c"

int main(){
    int a = 5;
    int b = 2;
    int maxNum = max(a, b);
    printf("The greater number is : %d\n", maxNum);
    return 0;
}
```

使用下面的命令编译运行：

由于`compare.c ` 中包含了 `max.c` ，因此编译 `compare.c ` 的同时会链接到  `max.c` ，命令如下：

```shell
$ gcc compare.c -o main.out	# compire and generate main.out
$ ls
hello.c  max.c  main.out
$ ./main.out				# run
```

## 2.2 头文件与函数定义分离

### **先编译部分函数**

```shell
$ gcc -c max.c		# 将 max.c 编译为 max.o
$ ls -l

-rw-r--r-- 1 yin yin  196 12月 20 17:40 hello.c
-rw-r--r-- 1 yin yin   48 12月 20 17:38 max.c
-rw-r--r-- 1 yin yin 1232 12月 20 17:47 max.o
```

接着删除包含头文件(因为后面编译会包含 .o 文件；若不删除，头文件会重复)

```c
#include <stdio.h>
// #include "max.c"

int main(){
    int a = 5;
    int b = 2;
    int maxNum = max(a, b);
    printf("The greater number is : %d\n", maxNum);
    return 0;
}
```

**重新编译主函数**

```shell
$ gcc max.o hello.c -o main.out
```

此时编译速度<font color=red>很快</font>，对于包含了大量函数调用时，先将不常改动的函数编译为`静态库`，可以提高变异速度。

---

**创建头文件 .h**

由于编译得到的 max.o 是二进制文件，需要生成一个可读文件来说明函数的功能，方便他人调用 max.o 文件：

- 创建头文件 `max.h`，添加函数的声明

- 在 `hello.c` 中使用 `#include "max.h"` 包含头文件

```c
// vi max.h
int max(int a, int b);

// hello.c
#include <stdio.h>
#include "max.h"		// 添加头文件
```

重新编译

```shell
$ ls
hello.c  max.c  max.h  max.o
gcc max.o hello.c -o main.out	// compile
```

---

# 3. makeFile

**Install make**

```shell
$ sudo apt-get install make
$ make -v
```

首先编辑好 `hello.c max.c min.c max.h min.h`

```c
#include <stdio.h>
#include "max.h"	// int max(int a, int b);
#include "min.h"	// int min(int a, int b);

int main()
{
        int a;
        int b;
        printf("Input two integers: ");
        scanf("%d %d", &a, &b);
        int c = max(a, b);
        int d = min(a, b);
        printf("The max number is: %d\nThe min number is: %d\n", c, d);
        return 0;
}
```

生成 `Makefile` 文件（注意文件名）：

```makefile
# this is make file
hello.out:max.o min.o hello.c
	gcc max.o min.o hello.c -o hello.out
max.o:max.c
	gcc -c max.c
min.o:min.c
	gcc -c min.c
```

使用 `make` 命令进行编译：

```shell
$ make
gcc -c max.c
gcc -c min.c
gcc max.o min.o hello.c -o hello.out

$ ls
hello.c  hello.out  Makefile  max.c  max.h  max.o  min.c  min.h  min.o
```

使用Makefile的优点：

- 不需要每次都重新输入大量的命令来编译
- makefile 只对修改过的内容重新编译链接（节省了时间）



# 4. main 函数

main函数的完整形式为：

```c
#include <sdio.h>

int main(int argv, char* argc)
{
    printf("hello world~\n");
    return 0;
}
```

## 4.1 return 0

使用 `&&` 连接两条命令编译运行：

```shell
$ gcc main.c -o main.out && ./main.out
```

查看程序是否正常执行：

```shell
$ echo $?
0				# 得到0 说明程序正常执行
```

若将 `return 0;` 改为 `return 100;` 使用 `echo $?` 后得到的结果是100（因为命令的后半部分是运行 main.out，接着运行下面的命令：

```shell
$ ./main.out && ls
hello world!
```

没有执行 ls，说明 `return 0` 返回值0不是随意的！(把程序改回去，ls 可正常执行)

## 4.2 main 参数

修改上面的main函数，打印argv：

````c
printf("argv is %d\n", argv);
````

编译运行：

```shell
$ gcc main2.c -o main2.out
$ ./main2.out			# 1
$ ./main2.out -l		# 2
$ ./main2.out -l -a		# 3
```

再添加一个打印语句：

```c
for (int i=0; i<argv; i++){
    printf("argc[%d] is %s\n", i, argc[i]);
}
```

编译运行：

```shell
$ gcc main3.c -o main3.out
$ ./main3.out -a -l abcd
argc is 4
argc[0] is ./main3.out
argc[1] is -a
argc[2] is -l
argc[3] is abcd
```

可见，`main(int argv, char* argc[])` 中，传入的参数是字符串，<font color=red>argv 是字符串参数的个数，argc 是一个字符串数组的地址，argc[i]是一个个字符串参数</font>。



---

# 5. 输入输出流和错误流

## 5.1 标准输入流、标准输出流和标准错误流

在 <font color=oran> stdio.h </font> 中包含了 `stdin`、`stdout` 和 `stderr`，看个例子：

```c
#include <stdio.h>	// stdio stdout stderr

int main()
{
	// printf("Input the value a:\n");
	fprintf(stdout, "Input the value a:\n");	// printf 的原型

	int a;
	// scanf("%d", &a);
	fscanf(stdin, "%d", &a);	// scanf 的原型

	if (a < 0){
		fprintf(stderr, "The value mast > 0!\n");
		return 1;
	}
	return 0;
}
```

- fprintf 是 printf 的原型，将数据打印到输出设备（默认显示屏）
- fscanf 是 scanf 的原型，将数据存放到对应的地址上 &a（默认从键盘读取）
- stderr 是标准错误流，当发生错误时输出信息，返回值不为0

## 5.2 重定向

```c
#include <stdio.h>

int main()
{
    scanf("%d", a);
    if (0 != a){
    	printf("The number is: %d\n", a);
    }else{
        fprintf(stderr, "a != 0 !\n");
        return 1;
    }
        return 0;
}
```

编译运行上述代码：

```shell
$ gcc ciore.c -o a2.out
$ ./a2.out
2
2

$ ./a2.out 1>> t.txt	# 默认为标准输出流 (1可以不写) 不覆盖原有的内容
2						# 输入 2
$ cat t.txt
The number is: 2

$ ls >> ls.txt			# 将 ls 的输出追加到 ls.txt 结尾
$ ls > ls.txt			# 将 ls 的输出覆盖到 ls.txt

vi input.txt				# 创建 input.txt 写入数字 2
2
$ ./a2.out < input.txt		# 标准输入流
The number is: 2

$ ./a2.out 1> t.txt 2> f.txt <input.txt		# 三种流一起用
0
$ cat fal.txt
a != 0 !
```

**重定向**：标准输入流为0；标准输出流为1；默认为标准输出流

---

# 6. 管道的原理与应用

看个例子：

```shell
$ ls /etc/ | grep ab	# 查看 etc 中所有文件名包含 ab 的文件
# ls有输出，利用了输出流
# grep 可以筛选文件，但是需要输入流
# 竖线 | 表示管道
crontab
crypttab
fstab
mtab
```

管道的作用：<font color=red>将前一个命令的输出流作为下一个命令的输入流来使用</font>，联系了两个应用程序。

```shell
$ ps -e | grep ssh		# 查看是否有 ssh 的进程
```

---

参考内容: [Linux 指针与内存](https://www.imooc.com/learn/394)
