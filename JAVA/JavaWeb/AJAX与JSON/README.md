## 1.AJAX

### 　　1. 概念：Asynchronous JavaScript And XML 异步的JavaScript 和 XML

### 　　　　1. 异步和同步：客户端和服务器端相互的基础上

### 　　　　　　* 同步：客户端必须等待服务器端的响应。在等待的期间客户端不能做其他操作

### 　　　　　　* 异步：客户端不需要等待服务器端的响应。在服务器处理请求的过程中，客户端可以去进行其他的操作

![img](https://img2020.cnblogs.com/blog/1894727/202008/1894727-20200804160341306-995148028.png)

 

 

### 　　　　　　* Ajax 即“**A***synchronous* **J***avascript **A**nd* **X***ML*”（异步 JavaScript 和 XML），是指一种创建交互式、快速动态[网页](https://baike.baidu.com/item/网页/99347)应用的网页开发技术，无需重新加载整个网页的情况下，能够更新部分网页的技术。

### 　　　　　　* 通过在后台与服务器进行少量数据交换，Ajax 可以使网页实现异步更新。这意味着可以在不重新加载整个网页的情况下，对网页的某部分进行更新。

### 　　2. 实现方式：

### 　　　　1. 原生的JS实现方式（了解）

![img](https://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif) 原生方式实现AJAX

 

### 　　　　2. JQuery实现方式

### 　　　　　　1. $.ajax（）

### 　　　　　　　　* 语法：$.ajax（url,[ settings ]） 一般为$.ajax（{键值对 }）

![img](https://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif) JQuery实现方式1

### 　　　　　　2. $.get（）：发送get请求

### 　　　　　　　　* 语法：$.get（url，【data】，【callback】，【type】）

### 　　　　　　　　* 参数：

### 　　　　　　　　　　* url：请求路径

### 　　　　　　　　　　* data：请求参数

### 　　　　　　　　　　* callback：回调函数

### 　　　　　　　　　　* type：响应结果类型

### 　　　　　　3. $.post（）：发送post请求 同上

![img](https://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif) get和post实现方式

 

## 2. JSON

### 　　1. 概念： JavaScript Object Notation 既：JavaScript对象表示法

### 　　　　var p = { "name" : "张三" , "age" : 23 , "gender" : "男" }；

### 　　　　* json现在多用于存储和交换文本信息的语法

### 　　　　* 进行数据的传输

### 　　　　* json 比 xml 更小、更快、更易解析。

### 　　2. 语法：

### 　　　　1. 基本规则　　

### 　　　　　　1. 数据在名称 / 值对中：json数据是由键值对构成的

### 　　　　　　　　* 键用引号（单双都行）引起来，也可以不使用引号

### 　　　　　　　　* 值的取值类型：

### 　　　　　　　　　　1. 数字（整形或浮点数）

### 　　　　　　　　　　2. 字符串（在双引号中）

### 　　　　　　　　　　3. 逻辑值（true或false）

### 　　　　　　　　　　4. 数组（在方括号中【】）{ "person" :[{ } , { }] }

### 　　　　　　　　　　5. 对象（在花括号中）{ "address" :{ "province" : "陕西"}}

### 　　　　　　　　　　6. null

### 　　　　　　2. 数据由逗号分隔：多个键值对由逗号分隔

### 　　　　　　3. 花括号保存对象：使用 { } 定义json格式

### 　　　　　　4. 方括号保存数组：【】

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
    <script>
        var person = {"name":"张三","age":23,"gender":true};
        alert(person);

        // 嵌套格式
        var person2 = {"persons":[
                {"name":"李四","age":24,"gender":false},
                {"name":"李四","age":24,"gender":false},
                {"name":"李四","age":24,"gender":false}
                ]}

    </script>
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

###  

### 　　　　2. 获取数据

### 　　　　　　1. json对象.键名

### 　　　　　　2. json对象["键名"]

### 　　　　　　3. 数组对象【索引】

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script>


        // 嵌套格式
        var person2 = {"persons":[
                {"name":"李四","age":24,"gender":false},
                {"name":"李四","age":24,"gender":false},
                {"name":"李四","age":24,"gender":false}
                ]};
        var name3 = person2["persons"][2]["name"];
        var name4 = person2.persons[2].name; //同上
        alert(name3);

        // 获取person对象中所有的键和值
        var person1 = {"name":"张三","age":23,"gender":true};
        // for in 循环
        for(var key in person1){
            alert(key + ":" + person1.key ); // 这样的方式获取不行。因为相当于person."name" 不符合
            alert(key + ":" + person1[key]);
        };

        // 获取person3的所有制
        var person3 = [
                {"name":"李四","age":24,"gender":false},
                {"name":"李四","age":24,"gender":false},
                {"name":"李四","age":24,"gender":false}
            ]

        for(var i = 0; i < person3.length; i++){
            var single = person3[i]; // 获取的是每一个person对象
            for(var key in single){
                alert(key + ":" + single[key]);
            }
        }
    </script>
</head>
<body>

</body>
</html>
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

 

### 　　3. JSON数据和Java对象的相互转换

### 　　　　* JSON解析器：

### 　　　　　　* 常见的解析器：Jsonlib、Gson、fastjson、jackson

### 　　　　1. JSON转为Java对象

### 　　　　　　1. 使用步骤：

### 　　　　　　　　1. 导入jackson的相关jar包

### 　　　　　　　　2. 创建jackson核心对象ObjectMapper

### 　　　　　　　　3. 调用ObjectMapper的相关方法进行转换

### 　　　　　　　　　　1. readvalue（json字符串数据，Class）

![img](https://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif) JSON转为java对象

 

### 　　　　2. Java对象转为JSON

### 　　　　　　1. 使用步骤：

### 　　　　　　　　1. 导入jackson的相关jar包

### 　　　　　　　　2. 创建jackson核心对象ObjectMapper

### 　　　　　　　　3. 调用ObjectMapper的相关方法进行转换 

![img](https://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif) 将java对象转为json写入

 

### 　　　　　　2. 注解：

### 　　　　　　　　1. @JsonIgnore：排除属性 忽略 

### 　　　　　　　　2. @JsonFormat：属性值的格式化

![img](https://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif) 对Person类进行注解

 　　　　　

### 　　　　　　3. 复杂java对象转换

### 　　　　　　　　1. List：数组

### 　　　　　　　　2. Map：与对象格式一致

![img](https://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif) List和Map转换