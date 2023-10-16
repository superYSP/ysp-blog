## **1、web服务器概述**

###### 提前告诉大家，Tomcat在idea中已自动整合，运行spring时自动开启，基本不需要操作

### **1.1、什么是服务器**

　　服务器：就是安装了服务器软件的计算机。

　　服务器软件：接收用户请求、处理请求以及响应请求的软件。而web服务器，可以部署web项目，让用户通过浏览器来访问这些项目。

### **1.2、常见的web服务器**

　　webLogic：oracle公司的，大型的javaEE服务器，支持有所的javaEE规范，收费的。

　　webSphere：IBM公司的，大型的javaEE服务器，支持有所的javaEE规范，收费的。

　　JBOSS：JBOSS公司的，大型的javaEE服务器，支持有所的javaEE规范，收费的。

　　Tomcat：Apache基金组织，中小型的javaEE服务器，支持部分javaEE规范，全面支持serlvet/jsp规范，开源的、免费的。

### **1.3、什么是javaWeb**

　　Internet上供外界访问的Web资源分为：

　　静态web资源（如html 页面）：指web页面中供人们浏览的数据始终是不变。

　　动态web资源：指web页面中供人们浏览的数据是由程序产生的，不同时间点访问web页面看到的内容各不相同。

　　静态web资源开发技术：Html、css

　　常用动态web资源开发技术：JSP+Servlet、ASP、PHP等，在Java中，动态web资源开发技术统称为Javaweb。

　　在小型的应用系统或者有特殊需要的系统中，可以使用一个免费的Web服务器：Tomcat，该服务器支持全部JSP以及Servlet规范。Tomcat三大功能：

　1）web服务器（底层是Socket的一个程序）；

　2）JSP容器

　3）servlet容器

### **2、Tomcat目录结构**

![img](https://pic3.zhimg.com/80/v2-65c095f1f3ed6e9845e5dcf5819886c6_720w.webp)

![img](https://pic4.zhimg.com/80/v2-a08240c721d5642d2daa0950fb352d57_720w.webp)

### **3、web应用组织结构和web.xml文件的作用**

![img](https://pic3.zhimg.com/80/v2-4459e357dfeb0b58bc622f3c1ad0fdca_720w.webp)

　　web.xml文件为web应用的配置文件，它必须放在web应用目录／WEB-INF目录下

## **4、配置Tomcat**

### **4.1、配置JDK**

　　配置JDK运行环境**JAVA_HOME**变量配置。Tomcat会通过JAVA_HOME找到所需要的JDK。

　　注：不同版本的Tomcat对JDK的依赖不同，可以在DOS窗口中使用命令运行。如果版本不匹配可能会出现各种问题甚至无法启动。我使用tomcat：apache-tomcat-9.0.41、JDK：1.8.0_181。

　　例如：我的环境变量如果配置JDK1.6

![img](https://pic1.zhimg.com/80/v2-1f696daf92f18870e690f6a8cec00a34_720w.webp)

　　就会出现： Unsupported major.minor version 52.0，不受支持的主要版本52.0，也就是说JDK低版本不兼容高版本。

```xml
Tomcat 10.0.x  supported Java Versions  8 and later   JDK编译器内部版本号：J2SE 8 = 52.0,
Tomcat 9.0.x   supported Java Versions  8 and later   JDK编译器内部版本号：J2SE 8 = 52.0,
Tomcat 8.5.x   supported Java Versions  7 and later   JDK编译器内部版本号：J2SE 7 = 51.0,
Tomcat 8.0.x   supported Java Versions  7 and later   JDK编译器内部版本号：J2SE 7 = 51.0,
Tomcat 7.0.x   supported Java Versions  6 and later   JDK编译器内部版本号：J2SE 7 = 50.0,
Tomcat 6.0.x   supported Java Versions  5 and later   JDK编译器内部版本号：J2SE 7 = 49.0,
Tomcat 5.5.x   supported Java Versions  1.4 and later JDK编译器内部版本号：J2SE 7 = 48.0,
Tomcat 4.1.x   supported Java Versions  1.3 and later JDK编译器内部版本号：J2SE 7 = 47.0,
Tomcat 3.3.x   supported Java Versions  1.1 and later JDK编译器内部版本号：J2SE 7 = 45.0,
```

　　如果我不想修改我本地的JDK环境变量，我们可以修改Tomcat的配置，指定JDK版本：

```xml
1）在tomcat的安装目录的bin目录下找到：setclasspath.sh
2）加入如下配置即可：
set JAVA_HOME=D:\program files\Java\jdk1.8.0_181
set JRE_HOME=D:\program files\Java\jdk1.8.0_181\jre
注：启动tomcat可以通过运行bin下的startup.bat，startup.bat会调用catalina.bat文件，而catalina.bat会调用setclasspath.bat文件来获取JAVA_HOME和JRE_HOME这两个环境变量的值
```

![img](https://pic4.zhimg.com/80/v2-6bd9b07166b3566efbcdeea999248607_720w.webp)

### **4.2、在IDE中配置Tomcat服务器**

　　菜单Window下选择Preferences，找到Server，选择Runtime Environments。

![img](https://pic3.zhimg.com/80/v2-396e4ca3cb999fd5e00d84ecd80d2fea_720w.webp)

配置完成后：

![img](https://pic3.zhimg.com/80/v2-8783d62dce1ad9b6552b4e21548af60e_720w.webp)

　　然后在IDE中启动Tomcat即可。我的项目的工作目录是servlet，项目的部署目录我们可以在D:\servlet.metadata.plugins\org.eclipse.wst.server.core\tmp0\wtpwebapps目录下找到，如果访问不了[http://localhost:8080/](https://link.zhihu.com/?target=http%3A//localhost%3A8080/)，可以将Tomcat目录下的ROOT替换掉这个。

![img](https://pic3.zhimg.com/80/v2-1150715bc7d9c58f6f57c0030589ef76_720w.webp)

### **4.3、直接在Tomcat安装目录中使用**

　　我们将D:\servlet.metadata.plugins\org.eclipse.wst.server.core\tmp0\wtpwebapps下的JspWeb项目直接拷贝到Tomcat的webapps目录下，然后启动（startup.bat）：

![img](https://pic3.zhimg.com/80/v2-7f59f1a50eb35cf14a4e1f67b2c049ee_720w.webp)

访问项目：

![img](https://pic3.zhimg.com/80/v2-0f86c7344779acd14bab89f3a3cfa712_720w.webp)

## **5、Tomcat整体架构分析**

　　Tomcat本质上就是一款servlet容器，Catalina(Servlet容器)是Tomcat的核心，其他模块都是为Catalina提供支撑的。比如：通过Coyote模块提供链接通信，Jasper 模块提供JSP引擎，Naming提供JDNI服务，Juli提供日志服务。

　　注：JNDI(Java Naming and Directory Interface)是一个应用程序设计的API，为开发人员提供了查找和访问各种命名和目录服务的通用、统一的接口，类似JDBC都是构建在抽象层上。

![img](https://pic2.zhimg.com/80/v2-300e09536ac1fc524a2c8ff576bec679_720w.webp)

　　Catalina负责管理Server,而Server代表整个Tomcat容器。Server下面有多个Services，每个服务都包含着多个连接器Connector（Coyote实现）和一个容器组件Container。在Tomcat启动的时候，就会创建一个Catalina的实例。

　　我们也可以通过Tomcat的配置server.xml文件来窥测Tomcat的设计。当然有时间可以去研究下源码。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!--Server代表整个整个Tomcat容器。只有一个。一个Server元素中可以有一个或多个Service元素。
    shutdown属性表示关闭Server的指令
    port属性表示Server接收shutdown指令的端口号
    Server的主要任务，就是提供一个接口让客户端能够访问到这个Service
-->
<Server port="8005" shutdown="SHUTDOWN">
  <!--监听器：
      VersionLoggerListener：监听器记录Tomcat、Java和操作系统的信息；
      AprLifecycleListener：检查APR库，如果存在则加载。
      JasperListener：在Web应用启动之前初始化Jasper，Jasper是JSP引擎，把JVM不认识的JSP文件解析成java文件，然后编译成class文件供JVM使用。
      JreMemoryLeakPreventionListener：与类加载器导致的内存泄露有关。
      GlobalResourcesLifecycleListener：初始化< GlobalNamingResources>标签中定义的全局JNDI资源；如果没有该监听器，任何全局资源都不能使用。
      ThreadLocalLeakPreventionListener：当Web应用因thread-local导致的内存泄露而要停止时，该监听器会触发线程池中线程的更新。
  -->
  <Listener className="org.apache.catalina.startup.VersionLoggerListener" />
  <Listener className="org.apache.catalina.core.AprLifecycleListener" SSLEngine="on" />
  <Listener className="org.apache.catalina.core.JreMemoryLeakPreventionListener" />
  <Listener className="org.apache.catalina.mbeans.GlobalResourcesLifecycleListener" />
  <Listener className="org.apache.catalina.core.ThreadLocalLeakPreventionListener" />
  <GlobalNamingResources>
    <Resource name="UserDatabase" auth="Container"
              type="org.apache.catalina.UserDatabase"
              description="User database that can be updated and saved"
              factory="org.apache.catalina.users.MemoryUserDatabaseFactory"
              pathname="conf/tomcat-users.xml" />
  </GlobalNamingResources>
    
  <!--Service的作用，是在Connector和Engine外面包了一层，把它们组装在一起，对外提供服务。一个Service可以包含多个Connector，但是只能包含一个Engine；
      Server中包含一个名称为“Catalina”的Service
      Tomcat可以提供多个Service，不同的Service监听不同的端口。

      配置多个Service服务，可以实现通过不同的端口号来访问同一台机器上部署的不同Web应用。可以在：Service下面在加<Service name="Catalina2"></Service>
  -->
  <Service name="Catalina">
      <!-- Connector的主要功能，是接收连接请求，创建Request和Response对象用于和请求端交换数据；
           然后分配线程让Engine来处理这个请求，并把产生的Request和Response对象传给Engine。
           通过配置Connector，可以控制请求Service的协议及端口号。
           客户端可以通过8080端口号使用http协议访问Tomcat。protocol属性规定了请求的协议，port规定了请求的端口号，redirectPort表示当强制要求https而请求是http时，重定向至端口号为8443的Connector，connectionTimeout表示连接的超时时间。
-->
    <Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />
     <!--
    <Connector protocol="AJP/1.3"
               address="::1"
               port="8009"
               redirectPort="8443" />
    --> 
      <!-- Engine组件在Service组件中有且只有一个；Engine是Service组件中的请求处理组件。Engine组件从一个或多个Connector中接收请求并处理，并将完成的响应返回给Connector，最终传递给客户端。
-->
    <Engine name="Catalina" defaultHost="localhost">
      <Realm className="org.apache.catalina.realm.LockOutRealm">
        <Realm className="org.apache.catalina.realm.UserDatabaseRealm"
               resourceName="UserDatabase"/>
      </Realm>
        <!--Host是Engine的子容器。Engine组件中可以内嵌1个或多个Host组件，每个Host组件代表Engine中的一个虚拟主机。Host组件至少有一个，且其中一个的name必须与Engine组件的defaultHost属性相匹配。
            Host组件代表的虚拟主机，对应了服务器中一个网络名实体（IP或域名）。
            name属性指定虚拟主机的主机名
            unpackWARs指定了是否将代表Web应用的WAR文件解压；如果为true，通过解压后的文件结构运行该Web应用，如果为false，直接使用WAR文件运行Web应用。
-->
      <Host name="localhost"  appBase="webapps"
            unpackWARs="true" autoDeploy="true">
        <Valve className="org.apache.catalina.valves.AccessLogValve" directory="logs"
               prefix="localhost_access_log" suffix=".txt"
               pattern="%h %l %u %t &quot;%r&quot; %s %b" />
      </Host>
    </Engine>
  </Service>
</Server>
```