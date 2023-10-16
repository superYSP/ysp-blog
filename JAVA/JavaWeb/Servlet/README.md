## **1、Serlvet概述**

　　Servlet是**运行在Web服务器**或**应用服务器上的java程序**，它是一个中间层，负责连接来自web浏览器或其他HTTP客户程序和HTTP服务器上应用程序。是sun公司提供的一门用于开发动态web资源的技术。

　　Servlet执行下面的任务：

1）读取客户发送的显示数据。如：html表单数据

2）读取由浏览器发送的隐式请求数据。如：http请求头

3）生成结果。

4）向客户端发送显示数据（即文档）。servlet和jsp最重要的任务就是将结果包装在文本（html）、二进制（图片）等格式的文件中。

5）发送隐式的HTTP响应数据。如：http响应头

　　如果想要用浏览器请求一个动态web资源，我们需要完成一下2个步骤即可：a）编写一个Java类，实现servlet接口。b）把开发好的Java类部署到web服务器中。

　　Servlet就是Sun公司在其API中提供了一个servlet接口（其实就是java程序），只不过该java程序要遵循servlet开发规范，他能帮我们处理浏览器发送的HTTP请求，并返回一个响应给浏览器。

## **2、servlet开发入门**

　　编写第一个servlet程序。

### **2.1、第一个servlet程序**

### **2.1.1、步骤一**

　　新建一个web工程，新建一个名为：FirstServletDemo的程序，并实现Servlet接口，会要求我们实现其抽象方法。init【初始化】，destroy【销毁】,service【服务】,ServletConfig【Servlet配置】,getServletInfo【Serlvet信息】

```java
import java.io.IOException;
import java.io.OutputStream;
import javax.servlet.Servlet;
import javax.servlet.ServletConfig;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;

public class FirstServletDemo implements Servlet{

    @Override
    public void destroy() {
        System.out.println("我是destroy，我执行了");
    }
    
    @Override
    public ServletConfig getServletConfig() {
        return null;
    }

    @Override
    public String getServletInfo() {
        return null;
    }

    @Override
    public void init(ServletConfig arg0) throws ServletException {
        System.out.println("我是init，我执行了");
    }

    @Override
    public void service(ServletRequest req, ServletResponse rsp) throws ServletException, IOException {
        System.out.println("我是service，我执行了");
        OutputStream out = rsp.getOutputStream();
        out.write("Hello Servlet!".getBytes());
    }

}
```

### **2.2、步骤二**

　　配置tomcat的web.xml，并在其中添加如下信息。

```xml
<!--servlet标签中：
    servlet-name配置servlet的名字，servlet-class配置servlet对应的java类
-->
<servlet>
    <servlet-name>FirstServletDemo</servlet-name>
    <servlet-class>com.cn.test.FirstServletDemo</servlet-class>
</servlet>
<!--servlet-mapping标签中：
    servlet-name配置servlet的名字，url-pattern配置访问的url
    "/"：开头默认代表：http://localhost:8080/
-->
<servlet-mapping>
    <servlet-name>FirstServletDemo</servlet-name>
    <url-pattern>/FirstServletDemo</url-pattern>
</servlet-mapping>
```

　　启动tomcat服务，并在浏览器中输入：[http://localhost:8080/FirstServletDemo](https://link.zhihu.com/?target=http%3A//localhost%3A8080/FirstServletDemo)

展示页面如下：

![img](https://pic2.zhimg.com/80/v2-e8c15ec5ed778289af140ad796fabf91_720w.webp)

![img](https://pic2.zhimg.com/80/v2-6b4faefb642f0624e56645f83af8042d_720w.webp)

### **2.2、servlet的生命周期**

　　很多程序都有自身执行的一个事件流，servlet也一样。Servlet 运行在 Servlet 容器中，并由容器管理从创建到销毁的整个过程。

### **2.2.1、servlet的生命周期概述**

　　Servlet生命周期大致可以分为：

　1） 加载和实例化

　　如果Servlet容器还没实例化一个Servlet对象，此时容器装载和实例化一个 Servlet。创建出该 Servlet 类的一个实例。如果已经存在一个Servlet对象，此时不再创建新实例。

　2）初始化 init()

　　当Servlet被实例化后，Tomcat会调用init()方法初始化这个对象。

　3）处理请求service()

　　当 Servlet 容器接收到一个 Servlet 请求时，便运行与之对应的 Servlet 实例的 service() 方法来处理用户请求及对客户的响应。

　4）销毁destroy()

　　当 Servlet 容器决定将一个 Servlet 从服务器中移除时 ( 如：Servlet 文件被更新，一个Servlet如果长时间不被使用的话，也会被Servlet 容器自动销毁 )，便调用该 Servlet 实例的 destroy() 方法。

### **2.2.2、servlet的生命周期特别说明**

　　init()方法执行初始化有两种类型：常规初始化和参数化初始化。

　1）常规初始化

　　在没有配置web.xml中配置<load-on-startup></load-on-startup>的时候， init()只初始化一次，并且只有访问对应的Servlet的时候才去执行init()方法。

　2）参数化初始化

　　当然，Servlet多起来了，如果想要有个加载顺序，程序员可以通过代码去实现什么时候初始化以及初始化顺序，但如果突然想要调整，这需要开发人员修改代码，为了方便，于是提供了一种可以通过参数化配置实现初始化顺序，即参数化初始化。

```java
import java.io.IOException;
import java.io.PrintWriter;
import javax.servlet.Servlet;
import javax.servlet.ServletConfig;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;

public class FirstServletDemo implements Servlet{

    int num = 0;
    @Override
    public void destroy() {
        System.out.println("我是destroy，我执行了");
    }

    @Override
    public ServletConfig getServletConfig() {
        return null;
    }

    @Override
    public String getServletInfo() {
        return null;
    }

    @Override
    public void init(ServletConfig arg0) throws ServletException {
        System.out.println("我是init，我执行了");
        num = (int) (Math.random()*100);
    }

    @Override
    public void service(ServletRequest req, ServletResponse rsp) throws ServletException, IOException {
        System.out.println("我是service，我执行了");
        PrintWriter out = rsp.getWriter();
        out.println(num);
    }

}
```

修改web.xml

```xml
<servlet>
    <servlet-name>FirstServletDemo</servlet-name>
    <servlet-class>com.cn.test.FirstServletDemo</servlet-class>
    <!--小于0：表示不执行初始化方法
        大于或等于0： 当服务器启动的时候就会按照配置的顺序依次创建Servlet类的实例，并且优先级0最大。
    -->
    <load-on-startup>2</load-on-startup>
</servlet>
```

案例效果：

![img](https://pic4.zhimg.com/80/v2-9bdb4db929cab7b6ed36673d5fc4e3db_720w.webp)

　　每次启动的时候，就初始化了。再次访问时，页面直接显示71了。

配置参数化初始化的作用：

1）为web应用写一个InitServlet，这个servlet配置为启动时装载，为整个web应用创建必要的数据库表和数据。

2）完成一些定时的任务【定时写日志，定时备份数据】

## **3、serlvet三种开发方式**

　　实现Servlet接口有三种方式：

　1）实现**Servlet**接口；2）通过继承**GenericServlet**；3）通过继承**HttpServlet**。

　　前面我们已经知道怎么使用**Servlet**接口了，对Servlet接口SUN公司定义了两个默认实现类：**GenericServlet**、**HttpServlet**。

### **3.1、继承GenericServlet**

　　GenericServlet是一个抽象类，它实现了Servlet接口。只需要我们重写service方法。

```java
 public abstract class GenericServlet implements Servlet, ServletConfig, Serializable{
    public abstract void service(ServletRequest req, ServletResponse res)
    throws ServletException, IOException;
     ......
 }
```

使用案例：

```java
import java.io.IOException;
import java.io.PrintWriter;
import javax.servlet.GenericServlet;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;

public class GenericServletDemo extends GenericServlet{

    @Override
    public void service(ServletRequest req, ServletResponse rsp) throws ServletException, IOException {
        // TODO Auto-generated method stub
        PrintWriter out = rsp.getWriter();
        out.println("GenericServletDemo!");
    }
}
```

### **3.2、继承HttpServlet**

　　HttpServlet在实现Servlet接口时，覆写了service方法，该方法体内的代码会自动判断用户的请求方式，如为GET请求，则调用HttpServlet的doGet方法，如为Post请求，则调用doPost方法。因此，开发人员在编写Servlet时，通常只需要覆写doGet或doPost方法，而不要去覆写service方法。

```java
public abstract class javax.servlet.http.HttpServlet extends javax.servlet.GenericServlet{
    protected void service(HttpServletRequest req, HttpServletResponse resp)
        throws ServletException, IOException
    {
        String method = req.getMethod();
        if (method.equals(METHOD_GET)) {
            long lastModified = getLastModified(req);
            if (lastModified == -1) {
                doGet(req, resp);
            } else {
                long ifModifiedSince = req.getDateHeader(HEADER_IFMODSINCE);
                if (ifModifiedSince < lastModified) {
                    maybeSetLastModified(resp, lastModified);
                    doGet(req, resp);
                } else {
                    resp.setStatus(HttpServletResponse.SC_NOT_MODIFIED);
                }
            }
        } else if (method.equals(METHOD_HEAD)) {
            long lastModified = getLastModified(req);
            maybeSetLastModified(resp, lastModified);
            doHead(req, resp);
        } else if (method.equals(METHOD_POST)) {
            doPost(req, resp); 
        } else if (method.equals(METHOD_PUT)) {
            doPut(req, resp);   
        } else if (method.equals(METHOD_DELETE)) {
            doDelete(req, resp);   
        } else if (method.equals(METHOD_OPTIONS)) {
            doOptions(req,resp); 
        } else if (method.equals(METHOD_TRACE)) {
            doTrace(req,resp);   
        } else {
            String errMsg = lStrings.getString("http.method_not_implemented");
            Object[] errArgs = new Object[1];
            errArgs[0] = method;
            errMsg = MessageFormat.format(errMsg, errArgs);
            resp.sendError(HttpServletResponse.SC_NOT_IMPLEMENTED, errMsg);
        }
    }
    ......
}
```

使用案例：

```java
import java.io.IOException;
import java.io.PrintWriter;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class HttpServletDemo extends HttpServlet{

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        PrintWriter out = resp.getWriter();
        out.println("HttpServletDemo!");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        super.doPost(req, resp);
    }

}
```

　　Servlet的继承体系：自定义的Servlet一般继承》抽象类HttpServlet》继承抽象类GenericServlet》继承接口Servlet。

### **扩展：Servlet调用过程分析**

![img](https://pic1.zhimg.com/80/v2-a6dd0e4082c644a6c5b6029bb363ed84_720w.webp)

## **4、Servlet开发细节**

### **4.1、Servlet可以被多次映射**

web.xml配置如下：

```xml
<servlet>
    <servlet-name>HttpServletDemo</servlet-name>
    <servlet-class>com.cn.test.HttpServletDemo</servlet-class>
</servlet>
<!-- 多个url映射同一个servlet -->
<servlet-mapping>
    <servlet-name>HttpServletDemo</servlet-name>
    <url-pattern>/HttpServletDemo</url-pattern>
</servlet-mapping>
<servlet-mapping>
    <servlet-name>HttpServletDemo</servlet-name>
    <url-pattern>/HttpServletDemo/Demo2</url-pattern>
</servlet-mapping>
```

案例效果：

![动图封面](https://pic4.zhimg.com/v2-a148e7f0b97e7ad8aa35de6a3942dbbf_b.jpg)



### **4.2、使用通配符**

　　在Servlet映射到的URL中也可以使用*通配符，但是只能有两种固定的格式：一种格式是“*.扩展名”，另一种格式是以正斜杠（/）开头并以“/*”结尾。

### **4.2.1、/\*结尾**

![img](https://pic4.zhimg.com/80/v2-0f90ed0eddd7777009c9c75d8ba70843_720w.webp)

### **4.2.2、 \*.扩展名**

![动图封面](https://pic2.zhimg.com/v2-62c4aef74ab1aededdcc77eac903a679_b.jpg)



**案例分析：**

假如我有：

Servlet1 映射到 /abc/*

Servlet2 映射到 /*

Servlet3 映射到 /abc

Servlet4 映射到 *.do

　问题1：当请求URL为“/abc/a.html”，“/abc/*”和“/*”都匹配，对应的哪个servlet会响应请求？

　答案1：Servlet引擎将调用Servlet1

　问题2：当请求URL为“/abc/a.do”时，“/abc/*”和“*.do”都匹配，对应的哪个servlet会响应请求？

　答案2：Servlet引擎将调用Servlet1

**注：在匹配的时候，要参考的标准：1）谁的匹配度高，谁就被选择；2）\*.do** **的优先级最低。**

### **5、Servlet的线程安全问题**

　　因为Servlet是单例，当多个客户端并发访问同一个Servlet时，web服务器会为每一个客户端的访问请求创建一个线程，并在这个线程上调用Servlet的service方法，因此service方法内如果访问了同一个资源的话，就有可能引发线程安全问题。

案例：创建一个ServletDemo1的servlet

```java
import java.io.IOException;
import java.io.PrintWriter;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class ServletDemo1 extends HttpServlet{
    int i = 0;
    public void doGet(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
          response.setContentType("text/html;charset=gbk");
          PrintWriter out = response.getWriter();
          out.println("<form action='/ServletDemo1' method='post' >");
          out.println("<input type='submit' value='点我' /><br />");
          out.println("</form><br />");
          out.println("我被点了："+i+"次");
          out.flush();
          out.close();
    }

    public void doPost(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
          i++;
          try {
              //为了突出并发量，这里设置一个延时，量大反映慢了的时候
              Thread.sleep(5000);
          } catch (InterruptedException e) {
              e.printStackTrace();
          }
          doGet(request, response);
    }
}
```

web.xml配置：

```xml
<servlet>
    <servlet-name>ServletDemo1</servlet-name>
    <servlet-class>com.cn.test.ServletDemo1</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>ServletDemo1</servlet-name>
    <url-pattern>/ServletDemo1</url-pattern>
</servlet-mapping>
```

案例效果：

![动图封面](https://pic2.zhimg.com/v2-4208b7199ecdbb49ca2593e22f60f565_b.jpg)



　　我们发现在不同的客户端操作，共享变量i的值也被共享了。右边先点一次，左边再点两次，还未等右边反映过来，变量i其实已经发生了变化，所以左边也出现了3次，实际只点了一次。

### **5.1、解决线程安全问题**

　1）如果一个变量需要多个用户共享，则应当在访问该变量的时候，加同步机制：synchronized (对象){..}

　2）如果一个变量不需要共享，则直接在 doGet() 或者 doPost()定义.这样不会存在线程安全问题。（实例变量线程共享，局部变量线程私有）

方案一：使用synchronized关键字

```java
import java.io.IOException;
import java.io.PrintWriter;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class ServletDemo1 extends HttpServlet{
    int i = 0;
    public void doGet(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
          response.setContentType("text/html;charset=gbk");
          PrintWriter out = response.getWriter();
          out.println("<form action='/ServletDemo1' method='post' >");
          out.println("<input type='submit' value='点我' /><br />");
          out.println("</form><br />");
          out.println("我被点了："+i+"次");
          out.flush();
          out.close();
    }

    public void doPost(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        synchronized (this){
          i++;
          try {
              //为了突出并发量，这里设置一个延时，量大反映慢了的时候
              Thread.sleep(5000);
          } catch (InterruptedException e) {
              e.printStackTrace();
          }
          doGet(request, response);
    }
    }
}
```

使用synchronized 关键字能保证一次只有一个线程可以访问被保护的区段，仅解决了左右次数显示问题。但是并未实质上解决变量共享问题。

方案二：使用SingleThreadModel接口，Servlet引擎将以单线程模式来调用其service方法

```java
public class ServletDemo1 extends HttpServlet implements SingleThreadModel{
    int i = 0;
    public void doGet(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
          response.setContentType("text/html;charset=gbk");
          PrintWriter out = response.getWriter();
          out.println("<form action='/ServletDemo1' method='post' >");
          out.println("<input type='submit' value='点我' /><br />");
          out.println("</form><br />");
          out.println("我被点了："+i+"次");
          out.flush();
          out.close();
    }

    public void doPost(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
          i++;
          try {
              //为了突出并发量，这里设置一个延时，量大反映慢了的时候
              Thread.sleep(5000);
          } catch (InterruptedException e) {
              e.printStackTrace();
          }
          doGet(request, response);
    }
}
```

案例效果：

![动图封面](https://pic2.zhimg.com/v2-d614ff7f899a4b67fc9b65e1dfbcbd75_b.jpg)



　　SingleThreadModel以另一种方式（单线程模式调用）解决多线程并发问题，而真正意义上解决多线程安全问题是指一个Servlet实例对象被多个线程同时调用的问题。事实上，在Servlet API 2.4中，已经将SingleThreadModel标记为Deprecated（过时的）。

　　真正避免线程安全问题的：可以将变量写在方法里，局部变量为线程私有，则不存在线程安全问题。

### **6、ServletConfig和ServletContext详解**

### **6.1、ServletConfig详解**

　　Servlet给我们提供了一个ServletConfig对象，该对象主要用于读取Servlet的配置信息。允许我们将配置信息写在**web.xml**中。

　　web容器（Tomcat）在创建servlet实例对象时，会自动将这些初始化参数封装到ServletConfig对象中，并在调用servlet的init方法时，将ServletConfig对象传递给servlet。

![img](https://pic1.zhimg.com/80/v2-788451a57392742a268bbbf16617f7c0_720w.webp)

　ServletConfig类的三大作用：

　1）可以获取Servlet程序的别名servlet-name的值；2）获取初始化参数init-param;3）获取ServletContext对象。

使用案例：

```java
import java.io.IOException;
import javax.servlet.ServletConfig;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class ServletConfigDemo extends HttpServlet{

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // TODO Auto-generated method stub
        doPost(req,resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        resp.setContentType("text/html");
        //ServletConfig对象是整个Servlet
        String charset = this.getServletConfig().getInitParameter("charset");
        String sname = this.getServletConfig().getServletName();
        resp.getWriter().println(charset);//输出： GBK
        resp.getWriter().println(sname);//输出： ServletConfigDemo
    }

    @Override
    public void init(ServletConfig config) throws ServletException {
        super.init(config);
        //可用于启动时初始化时连接数据库
        System.out.println(config.getInitParameter("username")); //控制台输出：root
        System.out.println(config.getInitParameter("url")); //控制台输出：jdbc:mysql://localhost:3306/test
    }

}
```

web.xml配置

```xml
<servlet>
    <servlet-name>ServletConfigDemo</servlet-name>
    <servlet-class>com.cn.test.ServletConfigDemo</servlet-class>
    <!-- 初始化参数 -->
    <init-param>
        <param-name>charset</param-name>
        <param-value>GBK</param-value>
    </init-param>
    <init-param>
        <param-name>username</param-name>
        <param-value>root</param-value>
    </init-param>
    <init-param>
        <param-name>url</param-name>
        <param-value>jdbc:mysql://localhost:3306/test</param-value>
    </init-param>
</servlet>
<servlet-mapping>
    <servlet-name>ServletConfigDemo</servlet-name>
    <url-pattern>/ServletConfigDemo</url-pattern>
</servlet-mapping>
```

### **6.2、ServletContext 详解**

　　ServletConfig中返回的ServletContext对象的引用。这个对象是干嘛的呢？ServletConfig是对应的单个servlet，而ServletContext对象，代表的是当前web应用，它会为每个WEB应用程序都创建一个对应的ServletContext对象。

　　ServletContext对象被包含在ServletConfig对象中，可以通过ServletConfig.getServletContext方法获得对ServletContext对象的引用。Servlet对象之间可以通过ServletContext对象来实现通讯。ServletContext对象通常也被称之为context域对象（可以像Map一样存取数据：setAttribute、getAttribute、removeAttribute）。

　　ServletContext 是当web应用启动的时候，自动创建，当web应用关闭/tomcat关闭/对web应用reload，会造成servletContext销毁。

　　ServletContext的主要作用：

1）获取web.xml中配置的上下文参数context-param；(getInitParameter(name))

2）获取当前工程路径，格式：/工程路径；

3）获取工程部署在服务器上的绝对路径；

4）像Map一样存取数据。

5）读取资源文件。

### **6.2.1、读取配置文件及获取路径**

使用案例一：

```java
import java.io.IOException;
import java.io.PrintWriter;
import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class ServletContextDemo extends HttpServlet{

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // TODO Auto-generated method stub
        doPost(req,resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        resp.setContentType("text/html;charset=gbk");
        //ServletConfig对象是整个Servlet
        ServletContext sc = this.getServletConfig().getServletContext();
        String username = (String) sc.getInitParameter("username");
        PrintWriter out = resp.getWriter();
        out.println("获取的上下文配置getInitParameter(username)："+username);//输出：context
        //如果在：server.xml中的Context中没有配置path,则此处获取到的getContextPath()为""
        out.println("<br/>当前工程路径getContextPath()："+sc.getContextPath());//
        out.println("<br/>当前工程部署路径getRealPath()："+sc.getRealPath("/"));//
        //设置上下文配置
        sc.setAttribute("password", "test");
    }
}
```

web.xml配置：

```xml
<context-param>
    <description>我是整个工程的配置</description>
    <param-name>username</param-name>
    <param-value>context</param-value>
</context-param>

<servlet>
    <servlet-name>ServletContextDemo</servlet-name>
    <servlet-class>com.cn.test.ServletContextDemo</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>ServletContextDemo</servlet-name>
    <url-pattern>/ServletContextDemo</url-pattern>
</servlet-mapping>
```

server.xml配置：

```xml
<Context docBase="JspWeb" path="/JspWeb" reloadable="true" source="org.eclipse.jst.jee.server:JspWeb"/></Host>
```

案例效果：

![img](https://pic1.zhimg.com/80/v2-6d7d9ba1acfdb7b4c59f8b3c66782f74_720w.webp)

### **6.2.2、存取数据**

使用案例二：修改下ServletConfigDemo

```java
import java.io.IOException;
import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class ServletConfigDemo extends HttpServlet{

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // TODO Auto-generated method stub
        doPost(req,resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        ServletContext sc = this.getServletConfig().getServletContext();
        String password = (String) sc.getAttribute("password");
        resp.getWriter().println(password);//
    }

}
```

案例效果：能取到ServletContextDemo中设置的值

![img](https://pic2.zhimg.com/80/v2-2cfa5613bda7a02e5cc84d3b7fa88411_720w.webp)

### **6.2.3、读取资源文件**

使用案例三：读取资源文件

```java
import java.io.IOException;
import java.io.InputStream;
import java.io.PrintWriter;
import java.util.Properties;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class ServletContextDemo2 extends HttpServlet{
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // TODO Auto-generated method stub
        doPost(req,resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //首先读取到文件
        resp.setContentType("text/html;charset=gbk");
        PrintWriter out = resp.getWriter();
        //文件放到WEB-INF目录下
        InputStream in=this.getServletContext().getResourceAsStream("/WEB-INF/info.properties");
        //文件放到src目录下
//        ClassLoader classLoader = ServletContextDemo2.class.getClassLoader();
//        InputStream in =  classLoader.getResourceAsStream("info.properties");
        Properties pro=new Properties();
        pro.load(in);
        out.println("username="+pro.getProperty("username")+"<br />");
        out.println("password="+pro.getProperty("password")+"<br />");
        out.flush();
        out.close();
    }
}
```

info.properties

```properties
username=root
password=123456
```

## **7、HttpServletRequest、HttpServletResponse详解**

　　前面我们看到HttpServlet中的service（doGet或doPost）方法中接收两个参数：service(HttpServletRequest req, HttpServletResponse resp)。下面分别介绍下这两个对象。

### **7.1、HttpServletRequest**

　　HttpServletRequest对象代表客户端的请求。当客户端通过HTTP协议访问Tomcat服务器时，服务器救护把请求过来的HTTP请求头中的所有信息都封装到Request这个对象中，然后传递给service方法（doGet或doPost）中给我们使用。开发人员通过这个对象的方法，可以获得客户这些信息。

常用API：

![img](https://pic3.zhimg.com/80/v2-4bfb469b674dd3697c5a0142fcbc59aa_720w.webp)

使用案例：

```java
import java.io.IOException;
import java.io.PrintWriter;
import java.util.Arrays;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class ReqServletDemo extends HttpServlet{
    
    public void doGet(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
          response.setContentType("text/html;charset=gbk");
          PrintWriter out = response.getWriter();
          out.println("<form action='/ReqServletDemo' method='post' >");
          out.print("<input type='txt' name='username' /><br /><br />");
          out.print("<input type='checkbox' name='hobby' value='java'/>java<br />");
          out.print("<input type='checkbox' name='hobby' value='python'/>python<br />");
          out.print("<input type='checkbox' name='hobby' value='html'/>html<br />");
          out.println("<input type='submit' value='提交' /><br />");
          out.println("</form><br />");
          out.flush();
          out.close();
    }

    public void doPost(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        //设置请求响应字符编码，默认是ISO-8859-1,不设置的就会出现乱码
        request.setCharacterEncoding("GBK");
        //response.setCharacterEncoding("GBK");
        //推荐使用下面这种
        response.setHeader("Content-Type","text/html;charset=GBK");
        
        //回显请求数据
        PrintWriter out = response.getWriter();
        out.println("url:"+request.getRequestURL().toString());//
        out.println("User-Agent:"+request.getHeader("User-Agent"));//
        out.println("获取请求参数username:"+request.getParameter("username"));//
        out.println("获取请求参数hobby:"+Arrays.asList(request.getParameterValues("hobby")));//
    }

}

```

案例效果：

![动图封面](https://pic3.zhimg.com/v2-e0f6fe1b9211e0d09cd4ca1db850c30e_b.jpg)



### **7.1.1、RequestDispatcher请求转发**

　　HttpServletRequest对象返回RequestDispatcher对象，叫做请求转发：服务器收到请求后，从一次资源跳转到另一个资源的操作。

　　首先我们在WEB-INF目录下方一张图片，然后在浏览器中输入：[http://localhost:8080/WEB-INF/logo.jpg](https://link.zhihu.com/?target=http%3A//localhost%3A8080/WEB-INF/logo.jpg)，去访问，会出现如下页面。默认浏览器是不能直接访问WEB-INF目录的。

![img](https://pic1.zhimg.com/80/v2-653dda5d8e8197b1f9294d36981624b4_720w.webp)

使用案例：

```java
import java.io.IOException;
import java.io.PrintWriter;
import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class ReqServletDemo extends HttpServlet{
    
    public void doGet(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
          response.setContentType("text/html;charset=gbk");
          PrintWriter out = response.getWriter();
          out.println("<form action='/ReqServletDemo' method='post' >");
          out.print("<input type='txt' name='username' /><br /><br />");
          out.print("<input type='checkbox' name='hobby' value='java'/>java<br />");
          out.print("<input type='checkbox' name='hobby' value='python'/>python<br />");
          out.print("<input type='checkbox' name='hobby' value='html'/>html<br />");
          out.println("<input type='submit' value='提交' /><br />");
          out.println("</form><br />");
          out.flush();
          out.close();
    }

    public void doPost(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        //请求转发
        RequestDispatcher rd = request.getRequestDispatcher("/WEB-INF/logo.jpg");
        //前进到转发的目录：正常浏览器是访问不到WEB-INF目录的资源的，但是请求转发可以
        rd.forward(request, response);
    }

}
```

案例效果：

![动图封面](https://pic4.zhimg.com/v2-a18f842af825ce8fcdffe0eee4f58f5b_b.jpg)



请求转发的特点：

1）浏览器地址栏没有变化，他们是一次请求；

2）共享Request域中的数据；

3）可以转发到WEB-INF目录下的资源，但是不可以访问工程以外的资源。

使用案例二：refresh的应用

　我们经常看到一个网站贴出即将跳转到哪里，5秒后没跳转请点击链接。

```java
import java.io.IOException;
import java.io.PrintWriter;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class RspServletDemo extends HttpServlet{
    
    public void doGet(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
          response.setContentType("text/html;charset=gbk");
          PrintWriter out = response.getWriter();
          out.println("<form action='/first' method='post' >");
          out.println("<input type='submit' value='跳转' /><br />");
          out.println("</form><br />");
          out.flush();
          out.close();
    }

    public void doPost(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        response.setHeader("refresh", "3;url='https://www.baidu.com/'");
        //页面跳转
        request.getRequestDispatcher("first.jsp").forward(request, response);
    }
}
```

web.xml

```xml
<servlet-mapping>
    <servlet-name>RspServletDemo</servlet-name>
    <url-pattern>/RspServletDemo</url-pattern>
</servlet-mapping>
<servlet-mapping>
    <servlet-name>RspServletDemo</servlet-name>
    <url-pattern>/first</url-pattern>
</servlet-mapping>
```

first.jsp

```jsp
<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
  <head>
    <title>RequestDispatcher</title>
  </head>
  <body>
      3秒后跳转百度
      <a href="https://www.baidu.com/">未跳转请点击</a>
  </body>
</html>
```

案例效果：

![动图封面](https://pic2.zhimg.com/v2-6966192a9d1d2a3c3306ef9abd7addcd_b.jpg)



虽然不可以访问外部资源，但是我们可以使用设置响应头response.setHeader("refresh", "3;url='[https://www.baidu.com/](https://link.zhihu.com/?target=https%3A//www.baidu.com/)'");的方式，让浏览器跳转到指定的url。

### **7.2、HttpServletResponse**

　　HttpServletResponse对象服务器的响应。这个对象中封装了向客户端发送数据、发送响应头，发送响应状态码的方法。

常用API：

![img](https://pic1.zhimg.com/80/v2-a8c933da92a0e540d5a5b244419e06c0_720w.webp)

### **7.2.1、getOutputStream和getWriter的区别**

　　字符流：getWriter() 用于向客户机回送**字符数据**

　　字节流：getOutputStream() 回送**字节数据**(二进制数据，常用于下载)，当然它也可以回送字符，但是没有PrintWriter对象 ,效率高。

　　OutputStream os=response.getOutputStream();

　　os.write("hello,world".getBytes());

　　注：两个流同时只能使用一个，Web服务器会自动检查并关闭流。

使用案例：

```java
import java.io.IOException;
import java.io.OutputStream;
import java.io.PrintWriter;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class RspServletDemo2 extends HttpServlet{

    public void doGet(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
          response.setContentType("text/html;charset=gbk");
          PrintWriter out = response.getWriter();
          out.println("<form action='/first' method='post' >");
          out.println("<input type='submit' value='跳转' /><br />");
          out.println("</form><br />");
          OutputStream out2 = response.getOutputStream();
          out2.write("hello,world".getBytes());
    }
}
```

案例效果：

![img](https://pic1.zhimg.com/80/v2-2f097f7162f254111aae28c68760a84c_720w.webp)

### **7.2.2、sendRedirect重定向的使用**

　　请求重定向是指：一个web资源收到客户端请求后，通知客户端去访问另外一个web资源，这称之为请求重定向。

实现方式一：设置Location（不推荐）

```java
import java.io.IOException;
import java.io.PrintWriter;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class sendRedirectDemo1 extends HttpServlet{

    public void doGet(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
          response.setContentType("text/html;charset=gbk");
          PrintWriter out = response.getWriter();
          out.println("<form action='/sendRedirectDemo1' method='post' >");
          out.println("<input type='submit' value='设置Location' /><br />");
          out.println("</form><br />");
          out.flush();
          out.close();
    }

    public void doPost(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        response.setStatus(302);
        response.setHeader("Location", "http://localhost:8080/");
    }
}
```

案例效果：

![动图封面](https://pic3.zhimg.com/v2-072d826d9e68a16277284b020b48d61e_b.jpg)



实现方式二：请求重定向，效果一样

```java
import java.io.IOException;
import java.io.PrintWriter;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class sendRedirectDemo2 extends HttpServlet{

    public void doGet(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
          response.setContentType("text/html;charset=gbk");
          PrintWriter out = response.getWriter();
          out.println("<form action='/sendRedirectDemo2' method='post' >");
          out.println("<input type='submit' value='设置Location' /><br />");
          out.println("</form><br />");
          out.flush();
          out.close();
    }

    public void doPost(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        response.sendRedirect("http://localhost:8080/");
    }
}
```

　请求重定向的特点：

1）浏览器地址会发送变化；

2）两次请求，不共享Request域中的数据；

3）不能访问WEB-INF下的资源，可以访问工程外的资源。

总结：请求重定向(sendRedirect)和请求转发(forward)的区别

- RequestDispatcher.forward方法只能将请求转发给同一个WEB应用中的组件；而HttpServletResponse.sendRedirect 方法还可以重定向到同一个站点上的其他应用程序中的资源，甚至是使用绝对URL重定向到其他站点的资源。
- 调用HttpServletResponse.sendRedirect方法重定向的访问过程结束后，浏览器地址栏中显示的URL会发生改变，由初始的URL地址变成重定向的目标URL；调用RequestDispatcher.forward 方法的请求转发过程结束后，浏览器地址栏保持初始的URL地址不变。
- RequestDispatcher.forward方法的调用者与被调用者之间共享相同的request对象和response对象，它们属于同一个访问请求和响应过程；而HttpServletResponse.sendRedirect方法调用者与被调用者使用各自的request对象和response对象，它们属于两个独立的访问请求和响应过程。