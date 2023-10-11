## JavaSE_第1章 Java概述

### 1、Java语言跨平台原理

Java程序运行在Java虚拟机JVM上，不同的操作系统有不同的JVM，而Java程序可以在任意JVM上运行，所以Java程序可以不考虑系统，只要找到JVM即可运行，从而通过JVM实现跨平台。

### 2、JVM、JRE、JDK的关系

- **JVM**（Java Virtual Machine ）：Java虚拟机，是运行所有Java程序的假想计算机，是Java程序的运行环境之一，也是Java 最具吸引力的特性之一。我们编写的Java代码，都运行在**JVM** 之上。
- **JRE** (Java Runtime Environment) ：是Java程序的运行时环境，包含`JVM` 和运行时所需要的`核心类库`。
- **JDK** (Java Development's Kit)：是Java程序开发工具包，包含`JRE` 和开发人员使用的工具。

### 3、程序开发步骤说明

JDK安装完毕，可以开发我们第一个Java程序了。

Java程序开发三步骤：**编写**、**编译**、**运行**。

## JavaSE_第2章 Java基础语法

### 1 注释（*annotation*）

- **注释**：就是对代码的解释和说明。其目的是让人们能够更加轻松地了解代码。为代码添加注释，是十分必须要的，它不影响程序的编译和运行。
- Java中有`单行注释`、`多行注释`和`文档注释`
- 单行注释以 `//`开头，以`换行`结束，格式如下：
  `java // 注释内容`
- 多行注释以 `/*`开头，以`*/`结束，格式如下：
  `java /* 注释内容 */`
- 文档注释以`/**`开头，以`*/`结束，Java特有的注释，结合
  `java /** 注释内容 */`

### 2 关键字（*keyword*）

**关键字**：是指在程序中，Java已经定义好的单词，具有特殊含义。

- HelloWorld案例中，出现的关键字有 `public` 、`class` 、 `static` 、 `void` 等，这些单词已经被Java定义好
- 关键字的特点：全部都是`小写字母`。
- 关键字比较多，不需要死记硬背，学到哪里记到哪里即可。

![img](https://pic4.zhimg.com/80/v2-6aa777b721a70804df6f40e3f93fb2d3_720w.webp)