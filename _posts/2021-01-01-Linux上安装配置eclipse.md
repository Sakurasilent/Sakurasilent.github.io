---
layout: post
title: 06-Linux上安装配置eclipse
date: 2021-01-01
description: 接下来的两周开始学习两门编程语言了，首先安装好 Java 环境
tags: Linux Java
---

## 1. 配置 Java 环境变量

要在Linux上使用eclipse，首先需要配置好 Java 环境变量，具体步骤如下：

### 1.1 安装好 JDK

在官网上下载 [Click me to download](https://www.oracle.com/cn/java/technologies/javase-downloads.html)，推荐下载 [Java SE 8](https://www.oracle.com/cn/java/technologies/javase/javase-jdk8-downloads.html)，我的电脑是64位的Linux系统，可以直接下载压缩包 [jdk-8u271-linux-x64.tar.gz](https://www.oracle.com/cn/java/technologies/javase/javase-jdk8-downloads.html#license-lightbox)（注意下载的时候需要登录 ORACLE 帐号）

默认下载存储的位置为 `/home/yin/Downloads`

将压缩包解压到 ``/usr/local/jdk1.8.0_172`：

```shell
$ sudo cp /home/yin/Downloads/jdk-8u172-linux-x64.tar /usr/local
$ cd usr/local
$ sudo tar -zxvf jdk-8u172-linux-x64.tar
```

### 1.2 更改环境变量

使用下面的命令：

```shell
$ sudo vim /etc/profile
```

将下面的内容追加到末尾（相关参数注意根据需要改动）：

```shell
export JAVA_HOME=/usr/local/jdk1.8.0_172
export PATH=.:$JAVA_HOME/bin:$PATH
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
```

刷新配置并检测结果：

```shell
$ source /etc/profile
$ java -version		## 查看是否配置成功
java version "1.8.0_172"
Java(TM) SE Runtime Environment (build 1.8.0_172-b11)
Java HotSpot(TM) 64-Bit Server VM (build 25.172-b11, mixed mode)
```

---

## 2. 安装 eclipse

### 2.1 下载 ECLIPSE

在官网下载 [ECLIPSE for Java developer](https://www.eclipse.org/downloads/download.php?file=/technology/epp/downloads/release/2020-12/R/eclipse-java-2020-12-R-linux-gtk-x86_64.tar.gz)，再将得到的 **gz** 压缩包解压到 `/usr/local/eclipse`，压缩包解压会得到 eclipse 文件夹，将它复制到 `/usr/local/` 即可，完成后就可以点击 eclipse 文件夹中的 eclipse 运行软件了，如下图：

<img src="/images/posts/blogImages/image-20201231210908855.png" width=720>



### 2.2 创建快捷方式

为了方便打开 eclipse，下面创建一个快捷方式：

```shell
$ cd /usr/share/applications
$ sudo vim eclipse.desktop		## 创建快捷方式，下面是文件内容
[Desktop Entry]
Encoding=UTF-8
Name=eclipse
Comment=Eclipse IDE
Exec=/usr/local/eclipse/eclipse        
Icon=/usr/local/eclipse/icon.xpm
Terminal=false
starttupNotify=true
Type=Application
Categories=Application;Development;
```

完成后，可以在桌面左下角点击运行按钮找到快捷方式！



## 3. 运行 Java 程序

除了可以使用 eclipse 来编译运行程序，也可以在终端执行，首先创建一个 java 文件：

```shell
$ vim HelloWorld.java

public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello World");
    }
}
```

使用下面的命令可以执行 java：

```shell
$ javac HelloWorld.java	## 生成字节码格式文件
$ java HelloWorld		## 运行
```
testtesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttesttest
