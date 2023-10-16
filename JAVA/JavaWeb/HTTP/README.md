## HTTP 协议

### 什么是 HTTP 协议

什么是协议？

 协议是指双方，或多方，相互约定好，大家都需要遵守的规则。

HTTP 协议是指，客户端和服务器之间通信时，发送数据需要遵守的规则。HTTP 协议中的数据又叫报文。

### 请求的 HTTP 协议格式

1. GET 请求

2. 请求行

3. 1. 请求的方式
   2. 请求的资源路径（+？+请求参数）
   3. 请求协议的版本号

\3. 请求头
key：value 组成 不同的键值对，表示不同的含义



![img](https://pic3.zhimg.com/80/v2-f6322b466f01bf1732560dd6d1574da6_720w.webp)



1. POST 请求

2. 请求行

3. 1. 请求的方式
   2. 请求的资源路径（+？+请求参数）
   3. 请求协议的版本号

\3. 请求头
key: value
\4. 空行
\5. 请求体 就是发送给服务器的数据



![img](https://pic4.zhimg.com/80/v2-af07ff3ae8b7a3c26c15547e652f8abf_720w.webp)



### 常用请求头

Accept：客户端可以接收的数据类型

Accept-Language：客户端可以接收的语言类型

User-Agent：客户端浏览器的信息

Host：请求时的服务器 ip 和端口号

### 常见 GET 请求和 POST 请求

GET 请求：

1. form 标签 method = get
2. a 标签
3. link 标签引入 css
4. Script 标签引入 js 文件
5. img 标签引入图片
6. iframe 引入 html 页面
7. 在浏览器地址栏输入地址后回车

POST 请求：

 form 标签 method = post

### 响应的 HTTP 协议格式

1. 响应行
2. 响应的协议和版本号
3. 响应状态码
4. 响应状态描述符
5. 响应头

key：value

1. 空行
2. 响应体 （就是回传给客户端的数据）



![img](https://pic4.zhimg.com/80/v2-af2bc54633eb5b0f172d05fd986bf54b_720w.webp)



### 常用的响应码说明

200：请求成功

302：表示请求重定向

404：表示请求服务器已经收到，但是要的数据不存在

500：表示服务器已经收到请求，但是服务器内部错误

### MIME 类型说明

MIME 是 HTTP 协议中数据类型 。

MIME 类型格式：“大类型/小类型”，并与某一种文件的拓展名相对应。



![img](https://pic2.zhimg.com/80/v2-ddced29df8c07e9b4a57005664dc7941_720w.webp)



### 谷歌浏览器查看 HTTP 协议

1. 打开浏览器的调试工具（F12）
2. 点击 Network
3. 当在浏览器输入网址后就会显示对应协议



![img](https://pic1.zhimg.com/80/v2-a7d39e07c76603713e78e38ad43c6d80_720w.webp)



## HttpServletRequest 类介绍

### HttpServletRequest 类的作用：

 每次只要有进入 Tomcat 服务器，Tomcat 服务器就会把请求过来的 HTTP 协议信息解析封装到 Request 对象中。然后传递到 service 方法中给我们使用，我们可以通过该对象获取到所有的请求信息。

### HttpServletRequest 类常用方法

- getRequestURI()：获取请求的资源地址
- getRequestURL(): 获取请求的绝对路径
- getRemoteHost(): 获取客户端的 ip 地址
- getHeader(): 获取请求头
- getParameter(): 获取请求参数
- getParameterValues(): 获取请求的参数(多个值时使用)
- getMethod(): 获取请求方式
- getAttribute(key): 获取域数据
- setAttribute(key,value):设置域数据
- getRequestDispatcher():获取请求转发对象

### 解决 post 请求中文乱码

- 设置请求字体的字符集为 UTF：

 req.setCharacterEncodeing("UTF-8")；

- 要在获取请求参数前调用才有效

### 请求的转发

请求转发：服务器收到请求后，从一个资源跳转到另一个资源的操作。

跳转的实现：

1. 获取请求转发的对象：getRequestDispatcher("路径")；
2. 跳转：请求转发对象.forward(req，resp)；



![img](https://pic1.zhimg.com/80/v2-b5bd4377deefab6af3f619d95e8fe154_720w.webp)





![img](https://pic2.zhimg.com/80/v2-cb39700529cc44285710ce9043148eb5_720w.webp)



### base 标签

作用：base 标签可设置当前页面中所有相对路径工作时，参照哪个路径来进行跳转。

### Web 中的相对路径和绝对路径

相对路径时：

 . 表示当前目录

 .. 表示上一级目录

 资源名 表示当前目录/资源名

绝对路径：

 [http://ip](https://link.zhihu.com/?target=http%3A//ip):port/工程路径/资源路径

### web 中 / 斜杠的不同意义

在 web中 / 斜杠 是一种绝对路径。

/ 斜杠 如果被浏览器解析，得到的是：[http://ip](https://link.zhihu.com/?target=http%3A//ip):port/

/ 斜杠 如果被服务器解析，得到的是：[http://ip](https://link.zhihu.com/?target=http%3A//ip):port/工程路径

## HttpServletResponse 类

### HttpServletResponse 类的作用

HttpServletResponse 表示所有响应的信息，我们可以通过 HttpServletResponse 对象设置返回给客户端的信息。

### 两个输出流的说明

字节流：通过 getOutputStream()；获取，常用于下载，传递二进制数据

字符流：通过 getWriter(); 获取，常用于回传字符串

两个流同时只能使用一个

### 往客户端回传数据

1. 先得到两个输出流之一的对象
2. 使用输出流对象的 write 方法

### 解决响应的中文乱码

方案1：

1. 调用 HttpServletResponse 对象的 setCharacterEncoding() 方法设置 UTF-8 字符集
2. 通过响应头，设置浏览器也使用 UTF-8 字符集，调用HttpServletResponse 对象的 setHeader() 方法设置 UTF-8 字符集



![img](https://pic4.zhimg.com/80/v2-9a996a6ebadfbbcf17000fd22030523b_720w.webp)



方案2:

 使用 HttpServletResponse 对象的 setContentType() 方法，它会同时设置服务器和客户端都使用 UTF-8 字符集，还设置了响应头。



![img](https://pic2.zhimg.com/80/v2-e355216b9eebebbbe9d6bdd92433689d_720w.webp)



### 请求重定向

**请求重定向**：客户端给服务器发请求，然后服务器给客户端新的地址去访问（因为之前的地址可能被废弃）。

**特点**：

1. 浏览器地址栏会发生变化
2. 两次请求
3. 能访问工程外的资源

实现方案1：



![img](https://pic3.zhimg.com/80/v2-c42d05a843edff879d7fa29d99428126_720w.webp)





![img](https://pic4.zhimg.com/80/v2-754e988c25686d03aea98a652ebd7a17_720w.webp)



实现方案2：



![img](https://pic2.zhimg.com/80/v2-10982200ee9e4671978b36dbf58b4e35_720w.webp)