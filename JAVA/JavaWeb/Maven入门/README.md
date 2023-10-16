## 1-Maven 概念

### 1.1-Maven 介绍

- Maven 是一个项目管理工具，它包含了一个项目对象模型（pom），一组标准的集合、一个项目生命周期（Project Lifecycle），一个依赖管理系统（Dependency Management System），和用来运行定义在生命周期阶段（phase）中插件（plugin）目标（goal）的逻辑
- 解决了jar包的版本冲突，管理jar
- 可以方便快速的利用集成开发工具编译
- 可以写一些单元测试案例，进行检查
- 可以快捷的将代码和配置文件、资源整合到一起打包部署
- 在以前进行web项目的时候，当需要使用jar时，是将需要的jar包导入到项目的lib文件夹中，如果每个web项目都需要相同的jar包，会导致同一个jar导入多次，这样会使得项目的占用外存很大。如果使用maven来构建项目，当使用jar包时，在 pom.xml 中进行配置坐标，使用的是本地仓库中的jar包，这样不管有多少个项目，都只会共用同一个jar资源，会让项目的体积减小。（这样在maven项目的工程文件中，就没有导入实体的jar）

### 1.2-Maven的经典作用

> **maven的依赖管理**

- 传统的web项目中，必须将工程所依赖的jar包复制到工程中，导致了工程的体积变得很大。
- maven工程中不需要直接将jar包导入到工程中，而是通过 pom.xml 文件中添加引入所需要的jar包的坐标，这样就不需要将jar包直接的引入到项目中。在maven项目中，需要使用jar包的时候，会去查找 pom.xml 文件，再通过 pom.xml 文件中的坐标，到本地仓库中，根据坐标从而找到这些jar包，再把jar包拿去运行。
- 在使用 pom.xml 文件去查找我们需要的jar，是通过建立索引

> **项目的一键构建**

- 项目一般需要经历 编译、测试、运行、打包、安装、部署 等过程
- 构建是将上述的整个过程交给 maven 进行管理，这个过程称为构建
- 一键构建指的是在整个构建过程中，使用 maven 一个命令完成上述过程

## 2-Maven 使用

### 2.1-maven 安装

> **maven软件下载**

- [链接 Maven – Download Apache Maven](https://link.zhihu.com/?target=https%3A//maven.apache.org/download.cgi)
- 找到 Files -> Binary zip archive 点击下载

> **maven软件安装**

- 将 maven 解压到一个没有中文没有空格的路径下。
- 配置环境变量
- bin：存放了maven的命令
- boot：存放一些maven本身的引导程序
- conf：存放了maven的配置文件
- lib：存放凉了maven本身运行所需要的一些jar包

### 2.2-maven 仓库

> **maven仓库分类**

![img](https://pic1.zhimg.com/80/v2-f6bb8fa7fe641452823036736e54c5b8_720w.webp)

在maven项目中，第一次使用某个jar包时，maven软件从远程仓库下载jar包并保存到本地仓库，当第二次需要此jar包时则不再从远程仓库下载，而是直接从本地仓库中获取，有了本地仓库就不用每次从远程仓库下载了

- 本地仓库：用来存储从远程仓库或中央仓库下载的插件和jar包，项目使用一些插件或jar包优先从本地仓库查找
- 远程仓库：如果本地需要插件或者jar包，本地仓库没有，默认从远程仓库下载
- 中央仓库：服务于整个互联网，它是由maven团队维护，里面存储了非常全的jar包

> **本地仓库的配置**

- 新建一个文件夹，用来作为本地仓库，存放在没有空格没有中文的路径下。
- 在 maven 软件 config/setting.xml 文件中配置本地仓库位置

![img](https://pic1.zhimg.com/80/v2-0c1f04567551f596d95f157f4570093c_720w.webp)

> **远程仓库的网址配置**

- 由于国内访问国外的中央仓库的网络不稳定，阿里将中央仓库的jar包和插件下载到自己的服务器中，我们直接访问阿里的远程仓库即可
- 在 maven 软件 config/setting.xml 文件中配置远程仓库位置

![img](https://pic1.zhimg.com/80/v2-6ea773109687b1f37509fc6c3b9a3450_720w.webp)

```xml
<mirror>
      <id>alimaven</id>
      <name>aliyun maven</name>
      <url>http://maven.aliyun.com/nexus/content/groups/public</url>
      <mirrorOf>central</mirrorOf>
</mirror>
```

### 2.3-maven 工程

> **maven工程的目录结构**

![img](https://pic2.zhimg.com/80/v2-0a36f4cf432e19f1e60d387371ed780d_720w.webp)

- 如果普通的Java项目，那么就没有webapp目录

## 3-maven 常用命令

### 3.1-maven 命令

**当后面命令执行时，前面的操作过程也都会自动执行**

> **compile**

- compile是maven工程的编译命令
- 将 src/main/java 下的文件编译成class文件输出到target目录

> **test**

- test是maven工程的测试命令
- 执行 src/test/java 下的单元测试类

> **clean**

- claen是maven工程的清理命令
- 执行clean会删除target目录及内容

> **package**

- package是maven工程的打包命令
- 对于Java工程执行package打成jar包，对于web工程打成war包

> **install**

- install是maven工程的安装命令
- 执行install将maven打成jar包或war包发布到本地仓库

### 3.2-maven 指令的生命周期

- maven 项目构建过程被分为三套相互独立的生命周期
- Clean Lifecyle：在进行真正构建之前进行一些清理工作
- Default Lifecyle：构建的核心部分，编译、测试、打包、部署 等
- Site Lifecyle：生成项目报告、站点、发布站点

![img](https://pic2.zhimg.com/80/v2-977b5e468f41a52d153f4d42e097632d_720w.webp)

### 3.3-maven 概念模型

![img](https://pic1.zhimg.com/80/v2-c3fc8bd7bb398f9c8041f1d778700254_720w.webp)

- **项目对象模型：**一个maven工程都有一个 pom.xml 文件，通过 pom.xml 文件定义项目的坐标、项目依赖、项目信息、插件目标
- **依赖管理系统：**通过maven依赖管理对项目所依赖的jar包进行统一管理

```xml
<!--导入依赖的jar-->
    <dependencies>
        <!--单元测试的jar包-->
        <!-- https://mvnrepository.com/artifact/junit/junit -->
        <<dependency>
            <!--项目名称-->
            <groupId>junit</groupId>
            <!--模块名称-->
            <artifactId>junit</artifactId>
            <!--版本-->
            <version>4.11</version>
            <!--依赖范围-->
            <scope>test</scope>
        </dependency>
    </dependencies>
```

- **一个项目的生命周期：**使用maven完成项目的构建，项目构建包括：清理、编译、测试、部署等，maven将这些过程规范为一个生命周期
- **一组标准集合：**maven将整个项目管理过程定义一组标准，通过maven构建工程的目录结构，有标准的生命周期阶段，依赖管理有标准的坐标定义
- **插件目标：**maven 管理项目生命周期过程都是基于插件完成的

## 4-Idea 开发 maven 项目

### 4.1-idea 的 maven 配置

![img](https://pic2.zhimg.com/80/v2-43f0c57cb59ee41938fa4b4e8865b11d_720w.webp)

### 4.2-idea 中利用骨架创建一个 maven 的 web 工程

> **在 maven 中常用的骨架是 maven-archetype-webapp（web工程）和 maven-archetype-quickstart（Java工程）**

![img](https://pic4.zhimg.com/80/v2-d0f56265dcd8a1e1cc02d33c3243e4af_720w.webp)

![img](https://pic1.zhimg.com/80/v2-dfc6117d96a997aee3f68ae338d6aaec_720w.webp)

![img](https://pic1.zhimg.com/80/v2-38d12b94aed39dd6e08c7df3e8f827c4_720w.webp)

> **由于最终的项目结构是不完整的，需要手动的补齐**

![img](https://pic4.zhimg.com/80/v2-749395c36df00cbd2f8c6608c1787deb_720w.webp)

![img](https://pic4.zhimg.com/80/v2-bf68750a637fab38315071c3517950e3_720w.webp)

![img](https://pic3.zhimg.com/80/v2-3f08bc86e27d1cf5ab0f0aa5fd5fac82_720w.webp)

> **利用骨架创建的Java项目目录结构**

![img](https://pic4.zhimg.com/80/v2-d16a83ec578592ac559a0a071a2ae533_720w.webp)

### 4.3-在 pom.xml 文件添加 jar 坐标

> **每个 maven 工程都需要定义工程的坐标，坐标是 maven 对 jar 包的身份定义**

```xml
<!--当前项目的坐标-->
    <!--项目名称，定义为组织名+项目名，类似包名-->
    <groupId>cn.test</groupId>
    <!--模块名称-->
    <artifactId>maven_java_1</artifactId>
    <!--当前项目版本号，snapshot 为快照版本即非正式版本，release 为正式发布版本-->
    <version>1.0-SNAPSHOT</version>
    <!--
        打包方式：
            1-jar:java项目 默认值
            2-war:web项目
            3-pom:用于 maven 工程的继承，通常父工程设置为 pom
    -->
    <packaging>jar</packaging>
```

> **添加 jar 包的坐标，还可以指定这个 jar 包将来的作用范围**

- 默认引入的 jar 包：compile（默认范围，可以不写，编译，测试，运行都有效）
- servlet-api、jsp-api：provided（编译、测试有效，运行时无效，防止和tomcat下 jar 冲突）
- jdbc：runtime（测试、运行有效）
- junit：test（测试有效）
- 依赖范围由强到弱的顺序是：complied > provided > runtime > test

```xml
<dependencies>
    <!--单元测试的jar包-->
    <!-- https://mvnrepository.com/artifact/junit/junit -->
    <dependency>
        <!--项目名称-->
        <groupId>junit</groupId>
        <!--模块名称-->
        <artifactId>junit</artifactId>
        <!--版本-->
        <version>4.11</version>
        <!--依赖范围-->
        <scope>test</scope>
    </dependency>
    <!--mysql驱动-->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>5.1.26</version>
        <scope>compile</scope>
    </dependency>
    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>javax.servlet-api</artifactId>
        <version>3.1.0</version>
        <scope>provided</scope>
    </dependency>
</dependencies>
```

### 4.4-在 pom.xml 文件添加插件坐标

> **设置jdk编译版本**

```xml
<build>
    <!--maven插件-->
    <plugins>
        <!--jdk编译插件-->
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <configuration>
                <source>1.8</source>
                <target>1.8</target>
                <encoding>utf-8</encoding>
            </configuration>
        </plugin>
    </plugins>
</build>
```

> **添加tomcat7插件**

```xml
<build>
    <!--tomcat插件-->
    <plugin>
        <groupId>org.apache.tomcat.maven</groupId>
        <!-- tomcat7的插件， 不同tomcat版本这个也不一样 -->
        <artifactId>tomcat7-maven-plugin</artifactId>
        <version>2.1</version>
        <configuration>
            <!-- 通过maven tomcat7:run运行项目时，访问项目的端口号 -->
            <port>80</port>
            <!-- 项目访问路径  本例：localhost:9090,  如果配置的aa， 则访问路径为localhost:9090/aa-->
            <path>/travel</path>
        </configuration>
    </plugin>
</build>
```

- **有两种运行 tomcat 的方式**
- 双击 tomcat7插件下 tomcat7:run 命令直接运行项目

![img](https://pic2.zhimg.com/80/v2-1889c5ec05cffaa9f48103b13408f405_720w.webp)

- 也可以直接点击如图按钮，手动输入 tomc7:run 命令运行项目

![img](https://pic1.zhimg.com/80/v2-49869ad3613b575982501d2019b06f04_720w.webp)

![img](https://pic4.zhimg.com/80/v2-5e3060db6a72e30d2b9797ea4b2f207b_720w.webp)

![img](https://pic3.zhimg.com/80/v2-584da22d09a1a564a3af0c76789320d6_720w.webp)

![img](https://pic4.zhimg.com/80/v2-04df59ac1c21128ab0c11f00100dc0d3_720w.webp)

![img](https://pic1.zhimg.com/80/v2-cc31555b7cd33286cba41b4bb833dc40_720w.webp)

### 4.5-idea 中导入 maven 项目

![img](https://pic2.zhimg.com/80/v2-8429e3738df49697447a4e4d0b5643cd_720w.webp)

![img](https://pic4.zhimg.com/80/v2-c2e5fa0914c608b8c4d0e77fa9bad0fb_720w.webp)