## JavaSE_第5章 面向对象基础（上）

### 1 面向对象编程思想概述

### 1、编程语言概述

Java是一种计算机程序设计语言。所有的计算机程序一直都是围绕着两件事在进行的，程序设计就是用某种语言编写代码来完成这两件事，所以程序设计语言又称为编程语言（编写程序的语言）。

1. 如何表示和存储数据
2. 基本数据类型的常量和变量：表示和存储一个个独立的数据
3. 对象：表示和存储与某个具体事物相关的多个数据（例如：某个学生的姓名、年龄、联系方式等）
4. 数据结构：表示和存储一组对象，数据结构有数组、链表、栈、队列、散列表、二叉树、堆......
5. 基于这些数据都有什么操作行为，其实就是实现什么功能
6. 数据的输入和输出
7. 基于一个或两个数据的操作：赋值运算、算术运算、比较运算、逻辑运算等
8. 基于一组数据的操作：统计分析、查找最大值、查找元素、排序、遍历等

### 2、程序设计方法

C语言是一种面向过程的程序设计语言，因为C语言是在面向过程思想的指引下去设计、开发计算机程序的。

Java语言是一种面向对象的程序设计语言，因为Java语言是在面向对象思想的指引下去设计、开发计算机程序的。

其中面向对象和面向过程都是一种编程思想，基于不同的思想会产生不同的程序设计方法。

1. 面向过程的程序设计思想（Process-Oriented Programming），简称POP
2. 关注的焦点是过程：过程就是操作数据的步骤，如果某个过程的实现代码在很多地方重复出现，那么就可以把这个过程抽象为一个函数，这样就可以大大简化冗余代码，也便于维护。
3. 代码结构：以函数为组织单位。独立于函数之外的数据称为全局数据，在函数内部的称为局部数据。
4. 面向对象的程序设计思想（ Object Oriented Programming），简称OOP
5. 关注的焦点是类：面向对象思想就是在计算机程序设计过程中，参照现实中事物，将事物的属性特征、行为特征抽象出来，用类来表示。某个事物的一个具体个体称为实例或对象。
6. 代码结构：以类为组织单位。每种事物都具备自己的**属性**（即表示和存储数据，在类中用成员变量表示）和**行为/功能**（即操作数据，在类中用成员方法表示）。

### 2 类和对象

### 1、什么是类

**类**是一类具有相同特性的事物的抽象描述，是一组相关**属性**和**行为**的集合。

- **属性**：就是该事物的状态信息。
- **行为**：就是在你这个程序中，该状态信息要做什么操作，或者基于事物的状态能做什么。

### 2、什么是对象

**对象**是一类事物的一个具体个体（对象并不是找个女朋友）。即对象是类的一个**实例**，必然具备该类事物的属性和行为。

### 3、类与对象的关系

- 类是对一类事物的描述，是**抽象的**。
- 对象是一类事物的实例，是**具体的**。
- **类是对象的模板，对象是类的实体**。

### 3 如何定义类

### 1、类的定义格式

关键字：class（小写）

```java
【修饰符】 class 类名{

}
```

类的定义格式举例：

```java
public class Student{

}
```

### 2、对象的创建

关键字：new

```java
new 类名()//也称为匿名对象

//给创建的对象命名
//或者说，把创建的对象用一个引用数据类型的变量保存起来，这样就可以反复使用这个对象了
类名 对象名 = new 类名();
```

### 4 包的作用

（1）可以避免类重名：有了包之后，类的全名称就变为：包.类名

（2）可以控制某些类型或成员的可见范围

如果某个类型或者成员的权限修饰缺省的话，那么就仅限于本包使用。

（3）分类组织管理众多的类

### 5 如何声明包

关键字：package

```java
package 包名;
```

> 注意：
> (1)必须在源文件的代码首行
> (2)一个源文件只能有一个声明包的package语句

包的命名规范和习惯： （1）所有单词都小写，每一个单词之间使用.分割 （2）习惯用公司的域名倒置开头

例如：com.atguigu.xxx;

> 建议大家取包名时不要使用“java.xx"包

### 6 如何跨包使用类

==注意：==只有public的类才能被跨包使用

（1）使用类型的全名称

例如：java.util.Scanner input = new java.util.Scanner([http://System.in](https://link.zhihu.com/?target=http%3A//System.in));

（2）使用import 语句之后，代码中使用简名称

import语句告诉编译器到哪里去寻找类。

import语句的语法格式：

```java
import 包.类名;
import 包.*;
```

> 注意：
> 使用java.lang包下的类，不需要import语句，就直接可以使用简名称
> import语句必须在package下面，class的上面
> 当使用两个不同包的同名类时，例如：java.util.Date和java.sql.Date。一个使用全名称，一个使用简名称

### 7 如何声明成员变量

```java
【修饰符】 class 类名{
    【修饰符】 数据类型  成员变量名; 
}
```

示例：

```java
public class Person{
    String name;
    char gender;
    int age;
}
```

### 1、实例变量的特点

（1）实例变量的值是属于某个对象的

- 必须通过对象才能访问实例变量
- 每个对象的实例变量的值是独立的

（2）实例变量有默认值

| 分类     | 数据类型                       | 默认值   |
| -------- | ------------------------------ | -------- |
| 基本类型 | 整数（byte，short，int，long） | 0        |
|          | 浮点数（float，double）        | 0.0      |
|          | 字符（char）                   | '\u0000' |
|          | 布尔（boolean）                | false    |
|          | 数据类型                       | 默认值   |
| 引用类型 | 数组，类，接口                 | null     |

### 2、实例变量的访问

```java
对象.实例变量
```

例如：

```java
package com.atguigu.test03.field;

public class TestPerson {
    public static void main(String[] args) {
        Person p1 = new Person();
        p1.name = "张三";
        p1.age = 23;
        p1.gender = '男';
}
```

### 3、实例变量的内存分析

内存是计算机中重要的部件之一，它是与CPU进行沟通的桥梁。其作用是用于暂时存放CPU中的运算数据，以及与硬盘等外部存储器交换的数据。只要计算机在运行中，CPU就会把需要运算的数据调到内存中进行运算，当运算完成后CPU再将结果传送出来。我们编写的程序是存放在硬盘中的，在硬盘中的程序是不会运行的，必须放进内存中才能运行，运行完毕后会清空内存。Java虚拟机要运行程序，必须要对内存进行空间的分配和管理，每一片区域都有特定的处理数据方式和内存管理方式。

### 8 方法的概念

方法也叫函数，是一组代码语句的封装，从而实现代码重用，从而减少冗余代码，通常它是一个独立功能的定义，方法是一个类中最基本的功能单元。

```java
Math.random()的random()方法
Math.sqrt(x)的sqrt(x)方法
System.out.println(x)的println(x)方法

Scanner input = new Scanner(System.in);
input.nextInt()的nextInt()方法
```

### 9 声明方法的位置

声明方法的位置==必须在类中方法外==，即不能在一个方法中直接定义另一个方法。

声明位置示例：

```java
类{
    方法1(){

    }
    方法2(){

    }
}
```

错误示例：

```java
类{
    方法1(){
        方法2(){  //位置错误

        }
    }
}
```

### 10 声明方法的语法格式

```java
【修饰符】 返回值类型 方法名(【形参列表 】)【throws 异常列表】{
        方法体的功能代码
}
```

### 11 方法调用语法格式

```java
对象.非静态方法(【实参列表】)
```

### 1、形参和实参

- 形参（formal parameter）：在定义方法时方法名后面括号中声明的变量称为形式参数（简称形参）即形参出现在方法定义时。
- 实参（actual parameter）：调用方法时方法名后面括号中的使用的值/变量/表达式称为实际参数（简称实参）即实参出现在方法调用时。
- 调用时，实参的个数、类型、顺序顺序要与形参列表一一对应。如果方法没有形参，就不需要也不能传实参。
- 无论是否有参数，声明方法和调用方法是==()都不能丢失==

### 2、返回值问题

方法调用表达式是一个特殊的表达式：

- 如果被调用方法的返回值类型是void，调用时不需要也不能接收和处理（打印或参与计算）返回值结果，即方法调用表达式==只能==直接加;成为一个独立语句。
- 如果被调用方法有返回值，即返回值类型不是void，
- 方法调用表达式的结果可以作为赋值表达式的值，
- 方法调用表达式的结果可以作为计算表达式的一个操作数，
- 方法调用表达式的结果可以作为另一次方法调用的实参，
- 方法调用表达式的结果可以不接收和处理，方法调用表达式直接加;成为一个独立的语句，这种情况，返回值丢失。

### 12 方法调用内存分析

方法不调用不执行，调用一次执行一次，每次调用会在栈中有一个入栈动作，即给当前方法开辟一块独立的内存区域，用于存储当前方法的局部变量的值，当方法执行结束后，会释放该内存，称为出栈，如果方法有返回值，就会把结果返回调用处，如果没有返回值，就直接结束，回到调用处继续执行下一条指令。

栈结构：先进后出，后进先出。

### 13 特殊参数之一：可变参数

在**JDK1.5**之后，当定义一个方法时，形参的类型可以确定，但是形参的个数不确定，那么可以考虑使用可变参数。可变参数的格式：

```text
【修饰符】 返回值类型 方法名(【非可变参数部分的形参列表,】参数类型... 形参名){  }
```

可变参数的特点和要求：

（1）一个方法最多只能有一个可变参数

（2）如果一个方法包含可变参数，那么可变参数必须是形参列表的最后一个

（3）在声明它的方法中，可变参数当成数组使用

（4）其实这个书写“≈”

```text
【修饰符】 返回值类型 方法名(【非可变参数部分的形参列表,】参数类型[] 形参名){  }
```

只是后面这种定义，在调用时必须传递数组，而前者更灵活，既可以传递数组，又可以直接传递数组的元素，这样更灵活了。

### 14 方法的重载

- **方法重载**：指在同一个类中，允许存在一个以上的同名方法，只要它们的参数列表不同即可，与修饰符和返回值类型无关。
- 参数列表：数据类型个数不同，数据类型不同（按理来说数据类型顺序不同也可以，但是很少见，也不推荐，逻辑上容易有歧义）。
- 重载方法调用：JVM通过方法的参数列表，调用匹配的方法。
- 先找个数、类型最匹配的
- 再找个数和类型可以兼容的，如果同时多个方法可以兼容将会报错

### 15 构造器

我们发现我们new完对象时，所有成员变量都是默认值，如果我们需要赋别的值，需要挨个为它们再赋值，太麻烦了。我们能不能在new对象时，直接为当前对象的某个或所有成员变量直接赋值呢。

可以，Java给我们提供了构造器（Constructor)。

### 1、构造器的作用

new对象，并在new对象的时候为实例变量赋值。

### 2、构造器的语法格式

构造器又称为构造方法，那是因为它长的很像方法。但是和方法还是有所区别的。

```java
【修饰符】 class 类名{
    【修饰符】 构造器名(){
        // 实例初始化代码
    }
    【修饰符】 构造器名(参数列表){
        // 实例初始化代码
    }
}
```

注意事项：

1. 构造器名必须与它所在的类名必须相同。
2. 它没有返回值，所以不需要返回值类型，甚至不需要void
3. 如果你不提供构造器，系统会给出无参数构造器，并且该构造器的修饰符默认与类的修饰符相同
4. 如果你提供了构造器，系统将不再提供无参数构造器，除非你自己定义。
5. 构造器是可以重载的，既可以定义参数，也可以不定义参数。
6. 构造器的修饰符只能是权限修饰符，不能被其他任何修饰

### 16 静态关键字（static）

在类中声明的实例变量，其值是每一个对象独立的。但是有些成员变量的值不需要或不能每一个对象单独存储一份，即有些成员变量和当前类的对象无关。

在类中声明的实例方法，在类的外面必须要先创建对象，才能调用。但是有些方法的调用和当前类的对象无关，那么创建对象就有点麻烦了。

此时，就需要将和当前类的对象无关的成员变量、成员方法声明为静态的（static）。

### 1、静态变量

有static修饰的成员变量就是静态变量。

```java
【修饰符】 class 类{
    【其他修饰符】 static 数据类型  静态变量名;
}
```

### 2、静态变量的特点

- 静态变量的默认值规则和实例变量一样。
- 静态变量值是所有对象共享。
- 静态变量的值存储在方法区。
- 静态变量在本类中，可以在任意方法、代码块、构造器中直接使用。
- 如果权限修饰符允许，在其他类中可以通过“类名.静态变量”直接访问，也可以通过“对象.静态变量”的方式访问（但是更推荐使用类名.静态变量的方式）。
- 静态变量的get/set方法也静态的，当局部变量与静态变量重名时，使用“类名.静态变量”进行区分。

### 3、静态类变量和非静态实例变量、局部变量

- 静态类变量（简称静态变量）：存储在方法区，有默认值，所有对象共享，生命周期和类相同，还可以有权限修饰符、final等其他修饰符
- 非静态实例变量（简称实例变量）：存储在堆中，有默认值，每一个对象独立，生命周期每一个对象也独立，还可以有权限修饰符、final等其他修饰符
- 局部变量：存储在栈中，没有默认值，每一次方法调用都是独立的，有作用域，只能有final修饰，没有其他修饰符

### 4、静态方法

有static修饰的成员方法就是静态方法。

```java
【修饰符】 class 类{
    【其他修饰符】 static 返回值类型 方法名(形参列表){
        方法体
    }
}
```

### 5、静态方法的特点

- 静态方法在本类的任意方法、代码块、构造器中都可以直接被调用。
- 只要权限修饰符允许，静态方法在其他类中可以通过“类名.静态方法“的方式调用。也可以通过”对象.静态方法“的方式调用（但是更推荐使用类名.静态方法的方式）。
- 静态方法可以被子类继承，但不能被子类重写。
- 静态方法的调用都只看编译时类型。

### 6、本类中的访问限制区别

静态的类变量和静态的方法可以在本类的任意方法、代码块、构造器中直接访问。

非静态的实例变量和非静态的方法==只能==在本类的非静态的方法、非静态代码块、构造器中直接访问。

即：

- 静态直接访问静态，可以
- 非静态直接访问非静态，可以
- 非静态直接访问静态，可以
- 静态直接访问非静态，不可以

### 7、在其他类的访问方式区别

静态的类变量和静态的方法可以通过“类名.”的方式直接访问；也可以通过“对象."的方式访问。（但是更推荐使用==”类名."==的方式）

非静态的实例变量和非静态的方法==只能==通过“对象."方式访问。

## JavaSE_第6章 面向对象基础--中

### 1 封装概述

### 1、为什么需要封装？

面向对象编程语言是对客观世界的模拟，客观世界里每一个事物的内部信息都是隐藏在对象内部的，外界无法直接操作和修改，只能通过指定的方式进行访问和修改。封装可以被认为是一个保护屏障，防止该类的代码和数据被其他类随意访问。适当的封装可以让代码更容易理解与维护，也加强了代码的安全性。

### 2、如何实现封装呢？

实现封装就是指控制类或成员的可见性范围？这就需要依赖访问控制修饰符，也称为权限修饰符来控制。

权限修饰符：public,protected,缺省,private

| 修饰符    | 本类 | 本包 | 其他包子类 | 其他包非子类 |
| --------- | ---- | ---- | ---------- | ------------ |
| private   | √    | ×    | ×          | ×            |
| 缺省      | √    | √    | ×          | ×            |
| protected | √    | √    | √          | ×            |
| public    | √    | √    | √          | √            |

外部类：public和缺省

成员变量、成员方法、构造器、成员内部类：public,protected,缺省,private

### 2 成员变量/属性私有化问题

**成员变量（field）私有化之后，提供标准的get/set方法，我们把这种成员变量也称为属性（property）。**

> 或者可以说只要能通过get/set操作的就是事物的属性，哪怕它没有对应的成员变量。

### 1、成员变量封装的目的

- 隐藏类的实现细节
- 让使用者只能通过事先预定的方法来访问数据，从而可以在该方法里面加入控制逻辑，限制对成员变量的不合理访问。还可以进行数据检查，从而有利于保证对象信息的完整性。
- 便于修改，提高代码的可维护性。主要说的是隐藏的部分，在内部修改了，如果其对外可以的访问方式不变的话，外部根本感觉不到它的修改。例如：Java8->Java9，String从char[]转为byte[]内部实现，而对外的方法不变，我们使用者根本感觉不到它内部的修改。

### 2、实现步骤

1. 使用 `private` 修饰成员变量

```java
private 数据类型 变量名 ；
```

代码如下：

```java
public class Person {
    private String name;
    private int age;
    private boolean marry;
}
```

1. 提供 `getXxx`方法 / `setXxx` 方法，可以访问成员变量，代码如下：

```java
public class Person {
    private String name;
    private int age;
    private boolean marry;

    public void setName(String n) {
        name = n;
    }

    public String getName() {
        return name;
    }

    public void setAge(int a) {
        age = a;
    }

    public int getAge() {
        return age;
    }

    public void setMarry(boolean m){
        marry = m;
    }

    public boolean isMarry(){
        return marry;
    }
}
```

3、测试

```java
package com.atguigu.encapsulation;

public class TestPerson {
    public static void main(String[] args) {
        Person p = new Person();

        //实例变量私有化，跨类是无法直接使用的
/*        p.name = "张三";
        p.age = 23;
        p.marry = true;*/

        p.setName("张三");
        System.out.println("p.name = " + p.getName());

        p.setAge(23);
        System.out.println("p.age = " + p.getAge());

        p.setMarry(true);
        System.out.println("p.marry = " + p.isMarry());
    }
}
```

### 3 继承的概述

### Java中的继承

其中，多个类可以称为**子类**，也叫**派生类**；多个类抽取出来的这个类称为**父类**、**超类（superclass）**或者**基类**。

继承描述的是事物之间的所属关系，这种关系是：`is-a` 的关系。例如，图中猫属于动物，狗也属于动物。可见，父类更通用或更一般，子类更具体。我们通过继承，可以使多种事物之间形成一种关系体系。

### 继承的好处

- 提高**代码的复用性**。
- 提高**代码的扩展性**。
- 表示类与类之间的is-a关系

### 4 继承的语法格式

通过 `extends` 关键字，可以声明一个子类继承另外一个父类，定义格式如下：

```java
【修饰符】 class 父类 {
    ...
}

【修饰符】 class 子类 extends 父类 {
    ...
}
```

### 5 子类会继承父类所有的实例变量和实例方法

从类的定义来看，类是一类具有相同特性的事物的抽象描述。父类是所有子类共同特征的抽象描述。而实例变量和实例方法就是事物的特征，那么父类中声明的实例变量和实例方法代表子类事物也有这个特征。

- 当子类对象被创建时，在堆中给对象申请内存时，就要看子类和父类都声明了什么实例变量，这些实例变量都要分配内存。
- 当子类对象调用方法时，编译器会先在子类模板中看该类是否有这个方法，如果没找到，会看它的父类甚至父类的父类是否声明了这个方法，遵循从下往上找的顺序，找到了就停止，一直到根父类都没有找到，就会报编译错误。

### 6 Java只支持单继承，不支持多重继承

```java
public class A{}
class B extends A{}

//一个类只能有一个父类，不可以有多个直接父类。
class C extends B{}     //ok
class C extends A，B...  //error
```

### 7 Java支持多层继承(继承体系)

```java
class A{}
class B extends A{}
class C extends B{}
```

> 顶层父类是Object类。所有的类默认继承Object，作为父类。

### 8 一个父类可以同时拥有多个子类

```java
class A{}
class B extends A{}
class D extends A{}
class E extends A{}
```

### 9 方法重写（Override）

我们说父类的所有方法子类都会继承，但是当某个方法被继承到子类之后，子类觉得父类原来的实现不适合于子类，该怎么办呢？我们可以进行方法重写 (Override)

### 1、方法重写

比如新的手机增加来电显示头像的功能，代码如下：

```java
package com.atguigu.inherited.method;

public class Phone {
    public void sendMessage(){
        System.out.println("发短信");
    }
    public void call(){
        System.out.println("打电话");
    }
    public void showNum(){
        System.out.println("来电显示号码");
    }
}
package com.atguigu.inherited.method;

//smartphone：智能手机
public class Smartphone extends Phone{
    //重写父类的来电显示功能的方法
    public void showNum(){
        //来电显示姓名和图片功能
        System.out.println("显示来电姓名");
        System.out.println("显示头像");
    }
}
package com.atguigu.inherited.method;

public class TestOverride {
    public static void main(String[] args) {
        // 创建子类对象
        Smartphone sp = new Smartphone();

        // 调用父类继承而来的方法
        sp.call();

        // 调用子类重写的方法
        sp.showNum();
    }
}
```

### 2、重写方法的要求

1.必须保证父子类之间重写方法的名称相同。

2.必须保证父子类之间重写方法的参数列表也完全相同。

2.子类方法的返回值类型必须【小于等于】父类方法的返回值类型（小于其实就是是它的子类，例如：Student < Person）。

> 注意：如果返回值类型是基本数据类型和void，那么必须是相同

3.子类方法的权限必须【大于等于】父类方法的权限修饰符。

> 注意：public > protected > 缺省 > private
> 父类私有方法不能重写
> 跨包的父类缺省的方法也不能重写

### 10 普通代码块

和构造器一样，也是用于实例变量的初始化等操作。

### 1、普通代码块的语法格式

```java
【修饰符】 class 类{
    {
        普通代码块
    }
    【修饰符】 构造器名(){
        // 实例初始化代码
    }
    【修饰符】 构造器名(参数列表){
        // 实例初始化代码
    }
}
```

### 11 静态代码块

如果想要为静态变量初始化，可以直接在静态变量的声明后面直接赋值，也可以使用静态代码块。

### 1、语法格式

在代码块的前面加static，就是静态代码块。

```java
【修饰符】 class 类{
    static{
        静态代码块
    }
}
```

### 2、静态代码块的特点

每一个类的静态代码块只会执行一次。

静态代码块的执行优先于非静态代码块和构造器。

### 12 Object根父类

### 1、如何理解根父类

类 `java.lang.Object`是类层次结构的根类，即所有类的父类。每个类都使用 `Object` 作为超类。

- Object类型的变量与除Object以外的任意引用数据类型的对象都多态引用
- 所有对象（包括数组）都实现这个类的方法。
- 如果一个类没有特别指定父类，那么默认则继承自Object类。例如：

```java
public class MyClass /*extends Object*/ {
    // ...
}
```

### 2、Object类的其中5个方法

### （1）toString()

### （2）getClass()

### （3）equals()

### （4）hashCode()

### （5）finalize()

### 13 final关键字

### 1、final的意义

final：最终的，不可更改的

### 2、final修饰类

表示这个类不能被继承，没有子类

```java
final class Eunuch{//太监类

}
class Son extends Eunuch{//错误

}
```

### 3、final修饰方法

表示这个方法不能被子类重写

```java
class Father{
    public final void method(){
        System.out.println("father");
    }
}
class Son extends Father{
    public void method(){//错误
        System.out.println("son");
    }
}
```

### 4、final修饰变量

final修饰某个变量（成员变量或局部变量），表示它的值就不能被修改，即常量，常量名建议使用大写字母。

> 如果某个成员变量用final修饰后，没有set方法，并且必须初始化（可以显式赋值、或在初始化块赋值、实例变量还可以在构造器中赋值）

### 13 多态的形式和体现

### 1、多态引用

Java规定父类类型的变量可以接收子类类型的对象，这一点从逻辑上也是说得通的。

```java
父类类型 变量名 = 子类对象；
```

> 父类类型：指子类继承的父类类型，或者实现的父接口类型。
> 所以说继承是多态的前提

### 2、多态引用的表现

表现：编译时类型与运行时类型不一致，编译时看“父类”，运行时看“子类”。

### 3、多态引用的好处和弊端

弊端：编译时，只能调用父类声明的方法，不能调用子类扩展的方法；

好处：运行时，看“子类”，如果子类重写了方法，一定是执行子类重写的方法体；变量引用的子类对象不同，执行的方法就不同，实现动态绑定。代码编写更灵活、功能更强大，可维护性和扩展性更好了。

### 14 应用多态解决问题

### 1、声明变量是父类类型，变量赋值子类对象

- 方法的形参是父类类型，调用方法的实参是子类对象
- 实例变量声明父类类型，实际存储的是子类对象

### 2、数组元素是父类类型，元素对象是子类对象

### 3、方法返回值类型声明为父类类型，实际返回的是子类对象

### 15 向上转型与向下转型

向上转型：自动完成

向下转型：（子类类型）父类变量

```java
package com.atguigu.polymorphism.grammar;

public class ClassCastTest {
    public static void main(String[] args) {
        //没有类型转换
        Dog dog = new Dog();//dog的编译时类型和运行时类型都是Dog

        //向上转型
        Pet pet = new Dog();//pet的编译时类型是Pet，运行时类型是Dog
        pet.setNickname("小白");
        pet.eat();//可以调用父类Pet有声明的方法eat，但执行的是子类重写的eat方法体
//        pet.watchHouse();//不能调用父类没有的方法watchHouse

        Dog d = (Dog) pet;
        System.out.println("d.nickname = " + d.getNickname());
        d.eat();//可以调用eat方法
        d.watchHouse();//可以调用子类扩展的方法watchHouse

        Cat c = (Cat) pet;//编译通过，因为从语法检查来说，pet的编译时类型是Pet，Cat是Pet的子类，所以向下转型语法正确
        //这句代码运行报错ClassCastException，因为pet变量的运行时类型是Dog，Dog和Cat之间是没有继承关系的
    }
}
```

### instanceof关键字

为了避免ClassCastException的发生，Java提供了 `instanceof` 关键字，给引用变量做类型的校验，只要用instanceof判断返回true的，那么强转为该类型就一定是安全的，不会报ClassCastException异常。

```text
变量/匿名对象 instanceof 数据类型
```

那么，哪些instanceof判断会返回true呢？

- 变量/匿名对象的编译时类型 与 instanceof后面数据类型是直系亲属关系才可以比较
- 变量/匿名对象的运行时类型<= instanceof后面数据类型，才为true

### 16 抽象类

### 1 语法格式

- **抽象方法**：被abstract修饰没有方法体的方法。
- **抽象类**：被abstract修饰的类。

抽象类的语法格式

```java
【权限修饰符】 abstract class 类名{

}
【权限修饰符】 abstract class 类名 extends 父类{

}
```

抽象方法的语法格式

```java
【其他修饰符】 abstract 返回值类型 方法名(【形参列表】);
```

> 注意：抽象方法没有方法体

代码举例：

```java
public abstract class Animal {
    public abstract void eat()；
}
public class Cat extends Animal {
    public void run (){
        System.out.println("小猫吃鱼和猫粮")；   
    }
}
public class CatTest {
     public static void main(String[] args) {
        // 创建子类对象
        Cat c = new Cat(); 

        // 调用eat方法
        c.eat();
    }
}
```

此时的方法重写，是子类对父类抽象方法的完成实现，我们将这种方法重写的操作，也叫做**实现方法**。

### 2 注意事项

关于抽象类的使用，以下为语法上要注意的细节，虽然条目较多，但若理解了抽象的本质，无需死记硬背。

1. 抽象类**不能创建对象**，如果创建，编译无法通过而报错。只能创建其非抽象子类的对象。

> 理解：假设创建了抽象类的对象，调用抽象的方法，而抽象方法没有具体的方法体，没有意义。

1. 抽象类中，也有构造方法，是供子类创建对象时，初始化父类成员变量使用的。

> 理解：子类的构造方法中，有默认的super()或手动的super(实参列表)，需要访问父类构造方法。

1. 抽象类中，不一定包含抽象方法，但是有抽象方法的类必定是抽象类。

> 理解：未包含抽象方法的抽象类，目的就是不想让调用者创建该类对象，通常用于某些特殊的类结构设计。

1. 抽象类的子类，必须重写抽象父类中**所有的**抽象方法，否则，编译无法通过而报错。除非该子类也是抽象类。

> 理解：假设不重写所有抽象方法，则类中可能包含抽象方法。那么创建对象后，调用抽象的方法，没有意义。

### 17 接口

### 1 定义格式

接口的定义，它与定义类方式相似，但是使用 `interface` 关键字。它也会被编译成.class文件，但一定要明确它并不是类，而是另外一种引用数据类型。

> 引用数据类型：数组，类，枚举，接口，注解。

### 1、接口的声明格式

```java
【修饰符】 interface 接口名{
    //接口的成员列表：
    // 公共的静态常量
    // 公共的抽象方法
    // 公共的默认方法（JDK1.8以上）
    // 公共的静态方法（JDK1.8以上）
    // 私有方法（JDK1.9以上）
}
```

### 2、接口的成员说明

接口定义的是多个类共同的公共行为规范，这些行为规范是与外部交流的通道，这就意味着接口里通常是定义一组公共方法。

在JDK8之前，接口中只允许出现：

（1）公共的静态的常量：其中public static final可以省略

（2）公共的抽象的方法：其中public abstract可以省略

> 理解：接口是从多个相似类中抽象出来的规范，不需要提供具体实现

在JDK1.8时，接口中允许声明默认方法和静态方法：

（3）公共的默认的方法：其中public 可以省略，建议保留，但是default不能省略

（4）公共的静态的方法：其中public 可以省略，建议保留，但是static不能省略

在JDK1.9时，接口又增加了：

（5）私有方法

除此之外，接口中不能有其他成员，没有构造器，没有初始化块，因为接口中没有成员变量需要动态初始化。

### 2 接口的使用

### 1、使用接口的静态成员

接口不能直接创建对象，但是可以通过接口名直接调用接口的静态方法和静态常量。

```java
package com.atguigu.interfacetype;

public class TestUsb3 {
    public static void main(String[] args) {
        //通过“接口名.”调用接口的静态方法
        Usb3.show();
        //通过“接口名.”直接使用接口的静态常量
        System.out.println(Usb3.MAX_SPEED);
    }
}
```

### 2、类实现接口（implements）

接口**不能创建对象**，但是可以被类实现（`implements` ，类似于被继承）。

类与接口的关系为实现关系，即**类实现接口**，该类可以称为接口的实现类，也可以称为接口的子类。实现的动作类似继承，格式相仿，只是关键字不同，实现使用 `implements`关键字。

```java
【修饰符】 class 实现类  implements 接口{
    // 重写接口中抽象方法【必须】，当然如果实现类是抽象类，那么可以不重写
    // 重写接口中默认方法【可选】
}

【修饰符】 class 实现类 extends 父类 implements 接口{
    // 重写接口中抽象方法【必须】，当然如果实现类是抽象类，那么可以不重写
    // 重写接口中默认方法【可选】
}
```

注意：

1. 如果接口的实现类是非抽象类，那么必须==重写接口中所有抽象方法==。
2. 默认方法可以选择保留，也可以重写。

> 重写时，default单词就不要再写了，它只用于在接口中表示默认方法，到类中就没有默认方法的概念了

1. **接口中的静态方法不能被继承也不能被重写**

示例代码：

```java
package com.atguigu.interfacetype;

public class MobileHDD implements Usb3 {
    //重写/实现接口的抽象方法，【必选】
    public void out() {
        System.out.println("读取数据并发送");
    }
    public void in(){
        System.out.println("接收数据并写入");
    }

    //重写接口的默认方法，【可选】
    //重写默认方法时，default单词去掉
    public void end(){
        System.out.println("清理硬盘中的隐藏回收站中的东西，再结束");
    }
}
```

### 3、使用接口的非静态方法

- 对于接口的静态方法，直接使用“接口名.”进行调用即可
- 也只能使用“接口名."进行调用，不能通过实现类的对象进行调用
- 对于接口的抽象方法、默认方法，只能通过实现类对象才可以调用
- 接口不能直接创建对象，只能创建实现类的对象

```java
package com.atguigu.interfacetype;

public class TestMobileHDD {
    public static void main(String[] args) {
        //创建实现类对象
        MobileHDD b = new MobileHDD();

        //通过实现类对象调用重写的抽象方法，以及接口的默认方法，如果实现类重写了就执行重写的默认方法，如果没有重写，就执行接口中的默认方法
        b.start();
        b.in();
        b.stop();

        //通过接口名调用接口的静态方法
//        MobileHDD.show();
//        b.show();
        Usb3.show();
    }
}
```

### 4、接口的多实现（implements）

之前学过，在继承体系中，一个类只能继承一个父类。而对于接口而言，一个类是可以实现多个接口的，这叫做接口的**多实现**。并且，一个类能继承一个父类，同时实现多个接口。

实现格式：

```java
【修饰符】 class 实现类  implements 接口1，接口2，接口3。。。{
    // 重写接口中所有抽象方法【必须】，当然如果实现类是抽象类，那么可以不重写
    // 重写接口中默认方法【可选】
}

【修饰符】 class 实现类 extends 父类 implements 接口1，接口2，接口3。。。{
    // 重写接口中所有抽象方法【必须】，当然如果实现类是抽象类，那么可以不重写
    // 重写接口中默认方法【可选】
}
```

> 接口中，有多个抽象方法时，实现类必须重写所有抽象方法。**如果抽象方法有重名的，只需要重写一次**。

定义多个接口：

```java
package com.atguigu.interfacetype;

public interface A {
    void showA();
    void show();
}
package com.atguigu.interfacetype;

public interface B extends A {
    void showB();
    void show();
}
```

定义实现类：

```java
package com.atguigu.interfacetype;

public class C implements A,B {
    @Override
    public void showA() {
        System.out.println("showA");
    }

    @Override
    public void showB() {
        System.out.println("showB");
    }

    @Override
    public void show() {
        System.out.println("show");
    }
}
```

测试类

```java
package com.atguigu.interfacetype;

public class TestC {
    public static void main(String[] args) {
        C c = new C();
        c.showA();
        c.showB();
        c.show();
    }
}
```

### 5、接口的多继承 （extends)

一个接口能继承另一个或者多个接口，接口的继承也使用 `extends` 关键字，子接口继承父接口的方法。

定义父接口：

```java
package com.atguigu.interfacetype;

public interface Chargeable {
    void charge();
    void in();
    void out();
}
```

定义子接口：

```java
package com.atguigu.interfacetype;

public interface UsbC extends Chargeable,Usb3 {
    void reverse();
}
```

定义子接口的实现类：

```java
package com.atguigu.interfacetype;

public class TypeCConverter implements UsbC {
    @Override
    public void reverse() {
        System.out.println("正反面都支持");
    }

    @Override
    public void charge() {
        System.out.println("可充电");
    }

    @Override
    public void in() {
        System.out.println("接收数据");
    }

    @Override
    public void out() {
        System.out.println("输出数据");
    }
}
```

> 所有父接口的抽象方法都有重写。
> 方法签名相同的抽象方法只需要实现一次。

### 6、接口与实现类对象构成多态引用

实现类实现接口，类似于子类继承父类，因此，接口类型的变量与实现类的对象之间，也可以构成多态引用。通过接口类型的变量调用方法，最终执行的是你new的实现类对象实现的方法体。

接口的不同实现类：

```java
package com.atguigu.interfacetype;

public class Mouse implements Usb3 {
    @Override
    public void out() {
        System.out.println("发送脉冲信号");
    }

    @Override
    public void in() {
        System.out.println("不接收信号");
    }
}
package com.atguigu.interfacetype;

public class KeyBoard implements Usb3{
    @Override
    public void in() {
        System.out.println("不接收信号");
    }

    @Override
    public void out() {
        System.out.println("发送按键信号");
    }
}
```

## JavaSE_第7章 面向对象基础（下）

### 1 枚举

某些类型的对象是有限的几个，这样的例子举不胜举：

- 星期：Monday(星期一)......Sunday(星期天)
- 性别：Man(男)、Woman(女)
- 月份：January(1月)......December(12月)
- 季节：Spring(春节)......Winter(冬天)
- 支付方式：Cash（现金）、WeChatPay（微信）、Alipay(支付宝)、BankCard(银行卡)、CreditCard(信用卡)
- 员工工作状态：Busy（忙）、Free（闲）、Vocation（休假）
- 订单状态：Nonpayment（未付款）、Paid（已付款）、Fulfilled（已配货）、Delivered（已发货）、Checked（已确认收货）、Return（退货）、Exchange（换货）、Cancel（取消）

枚举类型本质上也是一种类，只不过是这个类的对象是固定的几个，而不能随意让用户创建。

在JDK1.5之前，需要程序员自己通过特殊的方式来定义枚举类型。

在JDK1.5之后，Java支持enum关键字来快速的定义枚举类型。

### 2 JDK1.5之前

在JDK1.5之前如何声明枚举类呢？

- 构造器加private私有化
- 本类内部创建一组常量对象，并添加public static修饰符，对外暴露这些常量对象

示例代码：

```java
public class Season{
    public static final Season SPRING = new Season();
    public static final Season SUMMER = new Season();
    public static final Season AUTUMN = new Season();
    public static final Season WINTER = new Season();

    private Season(){

    }

    public String toString(){
        if(this == SPRING){
            return "春";
        }else if(this == SUMMER){
            return "夏";
        }else if(this == AUTUMN){
            return "秋";
        }else{
            return "冬";
        }
    }
}
public class TestSeason {
    public static void main(String[] args) {
        Season spring = Season.SPRING;
        System.out.println(spring);
    }
}
```

### 3 JDK1.5之后

### 1、enum关键字声明枚举

```java
【修饰符】 enum 枚举类名{
    常量对象列表
}

【修饰符】 enum 枚举类名{
    常量对象列表;

    其他成员列表;
}
```

示例代码：

```java
package com.atguigu.enumeration;

public enum Week {
    MONDAY,TUESDAY,WEDNESDAY,THURSDAY,FRIDAY,SATURDAY,SUNDAY
}
public class TestEnum {
    public static void main(String[] args) {
        Season spring = Season.SPRING;
        System.out.println(spring);
    }
}
```

### 3、枚举类型常用方法

```java
1.String toString(): 默认返回的是常量名（对象名），可以继续手动重写该方法！
2.String name():返回的是常量名（对象名）
3.int ordinal():返回常量的次序号，默认从0开始
4.枚举类型[] values():返回该枚举类的所有的常量对象，返回类型是当前枚举的数组类型，是一个静态方法
5.枚举类型 valueOf(String name)：根据枚举常量对象名称获取枚举对象
```

### 4 包装类

Java提供了两个类型系统，基本类型与引用类型，使用基本类型在于效率，然而当要使用只针对对象设计的API或新特性（例如泛型），那么基本数据类型的数据就需要用包装类来包装。

| 序号 | 基本数据类型 | 包装类（java.lang包） |
| ---- | ------------ | --------------------- |
| 1    | byte         | Byte                  |
| 2    | short        | Short                 |
| 3    | int          | Integer               |
| 4    | long         | Long                  |
| 5    | float        | Float                 |
| 6    | double       | Double                |
| 7    | char         | Character             |
| 8    | boolean      | Boolean               |
| 9    | void         | Void                  |

### 5 装箱与拆箱

装箱：把基本数据类型转为包装类对象。

> 转为包装类的对象，是为了使用专门为对象设计的API和特性

拆箱：把包装类对象拆为基本数据类型。

> 转为基本数据类型，一般是因为需要运算，Java中的大多数运算符是为基本数据类型设计的。比较、算术等

JDK1.5之后，可以自动装箱与拆箱。

> 注意：只能与自己对应的类型之间才能实现自动装箱与拆箱。

```java
Integer i = 4;//自动装箱。相当于Integer i = Integer.valueOf(4);
i = i + 5;//等号右边：将i对象转成基本数值(自动拆箱) i.intValue() + 5;
//加法运算完成后，再次装箱，把基本数值转成对象。
Integer i = 1;
Double d = 1;//错误的，1是int类型
```

### 6 包装类的一些API

### 1、基本数据类型和字符串之间的转换

（1）把基本数据类型转为字符串

```java
int a = 10;
//String str = a;//错误的
//方式一：
String str = a + "";
//方式二：
String str = String.valueOf(a);
```

（2）把字符串转为基本数据类型

```java
int a = Integer.parseInt("整数的字符串");
double d = Double.parseDouble("小数的字符串");
boolean b = Boolean.parseBoolean("true或false");

int a = Integer.valueOf("整数的字符串");
double d = Double.valueOf("小数的字符串");
boolean b = Boolean.valueOf("true或false");
```

### 7 内部类

1、什么是内部类？

将一个类A定义在另一个类B里面，里面的那个类A就称为**内部类**，B则称为**外部类**。

2、为什么要声明内部类呢？

总的来说，遵循高内聚低耦合的面向对象开发总原则。便于代码维护和扩展。

具体来说，当一个事物的内部，还有一个部分需要一个完整的结构进行描述，而这个内部的完整的结构又只为外部事物提供服务，不在其他地方单独使用，那么整个内部的完整结构最好使用内部类。而且内部类因为在外部类的里面，因此可以直接访问外部类的私有成员。

3、内部类都有哪些形式？

根据内部类声明的位置（如同变量的分类），我们可以分为：

（1）成员内部类：

- 静态成员内部类
- 非静态成员内部类

（2）局部内部类

- 有名字的局部内部类
- 匿名的内部类

### 1、成员内部类

如果成员内部类中不使用外部类的非静态成员，那么通常将内部类声明为静态内部类，否则声明为非静态内部类。

语法格式：

```java
【修饰符】 class 外部类{
    【其他修饰符】 【static】 class 内部类{
    }
}
```

### 2、局部内部类

语法格式：

```java
【修饰符】 class 外部类{
    【修饰符】 返回值类型  方法名(【形参列表】){
            【final/abstract】 class 内部类{
        }
    }    
}
```

### 3、匿名内部类

```java
new 父类(【实参列表】){
    重写方法...
}
//()中是否需要【实参列表】，看你想要让这个匿名内部类调用父类的哪个构造器，如果调用父类的无参构造，那么()中就不用写参数，如果调用父类的有参构造，那么()中需要传入实参
new 父接口(){
    重写方法...
}
//()中没有参数，因为此时匿名内部类的父类是Object类，它只有一个无参构造
```

> 匿名内部类是没有名字的类，因此在声明类的同时就创建好了唯一的对象。

### 8 什么是注解

注解是以“**@注释名**”在代码中存在的，还可以添加一些参数值，例如：

```java
@SuppressWarnings(value=”unchecked”)
@Override
@Deprecated
```

注解Annotation是从JDK5.0开始引入。

虽然说注解也是一种注释，因为它们都不会改变程序原有的逻辑，只是对程序增加了某些注释性信息。不过它又不同于单行注释和多行注释，对于单行注释和多行注释是给程序员看的，而注解是可以被编译器或其他程序读取的一种注释，程序还可以根据注解的不同，做出相应的处理。所以注解是插入到代码中以便有工具可以对它们进行处理的标签。

### 1、 三个最基本的注解

### 1、@Override

 用于检测被修饰的方法为有效的重写方法，如果不是，则报编译错误!

 只能标记在方法上。

 它会被编译器程序读取。

### 2、@Deprecated

 用于表示被标记的数据已经过时，不建议使用。

 可以用于修饰 属性、方法、构造、类、包、局部变量、参数。

 它会被编译器程序读取。

### 3、@SuppressWarnings

 抑制编译警告。

 可以用于修饰类、属性、方法、构造、局部变量、参数

 它会被编译器程序读取。