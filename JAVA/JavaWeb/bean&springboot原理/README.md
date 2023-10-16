![img](https://img-blog.csdnimg.cn/f13d2cce9a654a2492dcf22844df44b9.png)

**目录**

一、配置优先级

1、配置（3种：.properties、yml、yaml）

①、配置文件优先级

②、如何指定Java系统属性和命令行参数

③、5种配置文件的优先级

二、Bean管理

1、获取bean（3种方法）

2、bean作用域（5种）

①、@Scope注解：配置作用域

3、第三方bean

①、在启动类中声明第三方bean：（以Dom4j：解析XML文件对象为例）

②、声明配置类来管理第三方bean对象

③、如何在配置类中的第三方bean中引入方法对象

④、思考：@Component及衍生注解 与 @Bean注解使用场景？

三、SpringBoot原理

1、起步依赖

2、自动配置

①、什么是自动配置？

②、自动配置常见方案1 —— @ComponentScan 组件扫描（不推荐）

③、自动配置常见方案2 —— @Import导入（普通类/配置类/ImportSelector接口实现类）

④、自动配置原理 -- 源码跟踪

⑤、自动配置原理 -- 条件装配注解（@Conditional）

⑥、自动配置原理 —— 小结

⑦、案例（自定义starter）

四、Web后端开发总结

1、三层架构

------



# 一、配置优先级

## 1、配置（3种：.properties、yml、yaml）

### ①、配置文件优先级

![img](https://img-blog.csdnimg.cn/06bb37208e8f43adaf5606f9697a8010.png)

优先级最高的是（**.properties > .yml > .yaml**）



### ②、如何指定Java系统属性和命令行参数

![img](https://img-blog.csdnimg.cn/fe30ad90091f4611a130dcbe2c5dc08d.png)

program arguments 优先级比 VM options 要高



命令行执行：

![img](https://img-blog.csdnimg.cn/b948e75e7c474747a63743419bdabee5.png)

示例：

| Ⅰ、正常运行一个 jar 包：                                     |
| ------------------------------------------------------------ |
| 运行结果：![img](https://img-blog.csdnimg.cn/c7d93a044b494070921a08c7cb47b563.png) |
| Ⅱ、指定Java系统属性和命令行参数：                            |
| ![img](https://img-blog.csdnimg.cn/cf90b9e8386842238dc2a15ebf7356a6.png) |



### ③、5种配置文件的优先级

![img](https://img-blog.csdnimg.cn/da71d1059f504f0fb573a558329df8e0.png)



# 二、Bean管理

![img](https://img-blog.csdnimg.cn/0c6316c8164b402da1120d1bc3f2d466.png)

## 1、获取bean（3种方法）

默认情况下，SpringBoot项目在启动的时候会自动的创建**IOC****容器**(也称为**Spring****容器**)，并且在启动的过程当中会自动的将bean对象都创建好，存放在IOC容器当中。应用程序在运行时需要依赖什么bean对象，就直接进行依赖注入就可以了。

![img](https://img-blog.csdnimg.cn/97e46f986d6c4dd9a99f108480aec0d8.png)



![img](https://img-blog.csdnimg.cn/98ddec9ea4b14b62b56ebdb6115d467d.png)

代码示例：（3种获得bean对象的方法）

| ![img](https://img-blog.csdnimg.cn/12832a7f36964232a3dc70f1a1719c85.png) |
| ------------------------------------------------------------ |
| 运行结果：（但现在我们获取的bean对象是同一个，是单例的，我们可以通过某些方法将它设置为非单例的）![img](https://img-blog.csdnimg.cn/b4020c90af9843f7b564979fd763d653.png) |



## 2、bean作用域（5种）

单例和多例取决于作用域

![img](https://img-blog.csdnimg.cn/d988e71d8b88486195ef850339bd4fae.png)

注意事项：

![img](https://img-blog.csdnimg.cn/8914f5a67ce447feab2b927ff0c9e5f4.png)

### ①、@Scope注解：配置作用域

![img](https://img-blog.csdnimg.cn/d6357e2158d84fcdaa88480be3b5dcb6.png)



测试代码：

| ![img](https://img-blog.csdnimg.cn/8f453b1192574d72aef987c0ef7227b2.png) |
| ------------------------------------------------------------ |
| Ⅰ、默认情况下：（单例）![img](https://img-blog.csdnimg.cn/d1c1c06305724699b2cdce2e65c52992.png) |
| Ⅱ、懒加载：                                                  |
| 给DeptController类加上**@Lazy**注解，让其进行**懒加载**，即：不让Spring在程序启动时就实例化bean对象，而是在我们第一次请求它们时再创建：![img](https://img-blog.csdnimg.cn/1764232cc9d9463bba8b60568c0a4100.png)测试结果：程序启动时，bean对象并没有被创建![img](https://img-blog.csdnimg.cn/135ff53d4e29499388176597331274b7.png)当依次使用这个bean对象时，这个对象才进行实例化：![img](https://img-blog.csdnimg.cn/06a404ada05d47849e7cbf830584ff99.png) |
| Ⅲ、加上作用域@Scope("prototype")：表示每次使用bean时，都会创建新的实例 |
| DeptController：![img](https://img-blog.csdnimg.cn/ff94fd99a33d4bd4a2a7d7ace5c02f04.png)测试结果：![img](https://img-blog.csdnimg.cn/d8a86edefd7341938b0ccc3cb3e78f67.png) |



## 3、第三方bean

![img](https://img-blog.csdnimg.cn/9dde1265780548f993880fb364e019ef.png)

以上bean对象以外的bean

![img](https://img-blog.csdnimg.cn/1b41ccfa7b6844aa8102c4ea65d3633f.png)

注意事项：

![img](https://img-blog.csdnimg.cn/940c1bc334774d1191d082ef7b8bca54.png)

### ①、在启动类中声明第三方bean：（以Dom4j：解析XML文件对象为例）

![img](https://img-blog.csdnimg.cn/917772911c08475ab6d4214a6012d504.png)

示例：

| 在启动类中声明第三方bean：（**不建议使用这种方法**）         |
| ------------------------------------------------------------ |
| ![img](https://img-blog.csdnimg.cn/e99691ab0d6f4fef9b5aa0cb81631fa5.png) |



### ②、声明配置类来管理第三方bean对象

| 配置类示例：![img](https://img-blog.csdnimg.cn/33d7339d949e4f09b1c24dacb6cedc5f.png) |
| ------------------------------------------------------------ |
| 其次，我们可以通过@Bean注解的name/value属性来指定bean的名称，如果未指定，默认是方法名:测试bean对象的名称是否为方法名：![img](https://img-blog.csdnimg.cn/206d7f828cee4fd1956f565b9654b011.png)测试结果：![img](https://img-blog.csdnimg.cn/78786d4bf5a64f2b990bd68167080d6d.png) |



### ③、如何在配置类中的第三方bean中引入方法对象

直接作为形参引入：

![img](https://img-blog.csdnimg.cn/8423570b9735480d9ddae98baa8a33ff.png)



### ④、思考：@Component及衍生注解 与 @Bean注解使用场景？

![img](https://img-blog.csdnimg.cn/d4d893de81d34dcc9e8691dd8055b1d2.png)



# 三、SpringBoot原理

![img](https://img-blog.csdnimg.cn/83810fbf1fa24324b43ad961f273843e.png)



## 1、起步依赖

假如我们没有使用SpringBoot，用的是Spring框架进行web程序的开发，此时我们就需要引入web程序开发所需要的一些依赖。

![img](https://img-blog.csdnimg.cn/8318a9ee1e82409e891e0a7ccfd28165.png)

**spring-webmvc**依赖：这是Spring框架进行web程序开发所需要的依赖**servlet-api**依赖：Servlet基础依赖**jackson-databind**依赖：JSON处理工具包如果要使用AOP，还需要引入aop依赖、aspect依赖项目中所引入的这些依赖，还需要保证版本匹配，否则就可能会出现版本冲突问题。

如果我们使用了SpringBoot，就不需要像上面这么繁琐的引入依赖了。我们只需要引入一个依赖就可以了，那就是web开发的起步依赖：**springboot-starter-web**。

![img](https://img-blog.csdnimg.cn/167c0128d5004795bdd4edc80164cf99.png)

起步依赖的原理就是**依赖传递**

![img](https://img-blog.csdnimg.cn/44b3061d51774a58b6ef02a0618fb0c9.png)

## 2、自动配置

![img](https://img-blog.csdnimg.cn/54c6a76a91274697afac56a429ef48e8.png)

### ①、什么是自动配置？

![img](https://img-blog.csdnimg.cn/966c542039a04f9399ffcbedeb9db9f1.png)



示例：注入Gson对象

| 注入Gson对象：![img](https://img-blog.csdnimg.cn/31f026ab55e745d08633162cc026a34a.png) |
| ------------------------------------------------------------ |
| Ⅰ、编写测试类（AutoConfigurationTests），并在其中自动注入Gson对象： |
| ![img](https://img-blog.csdnimg.cn/80f3363f6f3047f4803fe4dddf41fd98.png) |
| Ⅱ、运行结果：                                                |
| ![img](https://img-blog.csdnimg.cn/c3e711a3c3804f82ab088dfb7e83a984.png) |

###  

| **问题**：在当前项目中我们并没有声明谷歌提供的Gson这么一个bean对象，然后我们却可以通过@Autowired从Spring容器中注入bean对象，那么这个bean对象怎么来的？ |
| ------------------------------------------------------------ |
| **答案**：SpringBoot项目在启动时**通过自动配置完成了bean对象的创建**。 |



![img](https://img-blog.csdnimg.cn/b15ab25dec7a4eefb0c8916748bcd580.png)

我们知道了什么是自动配置之后，接下来我们就要来剖析自动配置的原理。解析自动配置的原理就是分析在 SpringBoot项目当中，我们引入对应的依赖之后，是如何将依赖jar包当中所提供的bean以及配置类直接加载到当前项目的SpringIOC容器当中的。

![img](https://img-blog.csdnimg.cn/3ec5636823984c24943a53971a2f3a7c.png)



### ②、自动配置常见方案1 —— @ComponentScan 组件扫描（不推荐）

![img](https://img-blog.csdnimg.cn/b7df98f89f084512b1ee6ef55ba2e950.png)

示例：

| 单元测试：获取itheima-utils中的bean以及配置类：              |
| ------------------------------------------------------------ |
| ![img](https://img-blog.csdnimg.cn/42f3e502f40744f486dd13e577ce597c.png)运行结果：![img](https://img-blog.csdnimg.cn/f17f67e4e9a943d1b90fb0b10d32fdbe.png) |



### ③、自动配置常见方案2 —— @Import导入（普通类/配置类/ImportSelector接口实现类）

![img](https://img-blog.csdnimg.cn/462d95e91f0847529d89dd51c4f9c98c.png)

示例：

| Ⅰ、在启动类中导入 **普通类**：                               |
| ------------------------------------------------------------ |
| ![img](https://img-blog.csdnimg.cn/0d5cf832fbac45b288c691699647af5a.png) |
| Ⅱ、在启动类中导入 **配置****类**：（多个Bean）               |
| ![img](https://img-blog.csdnimg.cn/25bb1cf88c014d90a9a69ec82aab536f.png) |
| Ⅲ、在启动类中导入 **ImportSelector** 接口实现类：            |
| ImportSelector 接口实现类：![img](https://img-blog.csdnimg.cn/d3eff35bf38c4efe952bf9771a2b0d98.png)启动类导入：![img](https://img-blog.csdnimg.cn/27747cdbb0d2429581c726555549b1a7.png) |
| Ⅳ、**@EnableXxxx**注解，封装了@Import注解**（推荐）**        |
| 让第三方依赖自己来指定要导入哪些bean和配置类![img](https://img-blog.csdnimg.cn/50fee8c8bc6c47e380eafd0b63b9901e.png)在启动类上加上对应注解：![img](https://img-blog.csdnimg.cn/1144135fedeb4f869a4240532d0e2d27.png) |



### ④、自动配置原理 -- 源码跟踪

| @interface **SpringBootApplication**：                       |
| ------------------------------------------------------------ |
| ![img](https://img-blog.csdnimg.cn/43448a9b4b3e4320807653f0aa144e3c.png)**@SpringBootConfiguration**注解上使用了**@Configuration**，**表明SpringBoot启动类就是一个配置类**@Indexed注解，是用来加速应用启动的（不用关心）。![img](https://img-blog.csdnimg.cn/35f4a9b6d22d493288ceb55255a4503a.png) |
| @interface **EnableAutoConfiguratiion**：                    |
| ![img](https://img-blog.csdnimg.cn/8df516d7f7534ceab5aafb90597632ec.png)![img](https://img-blog.csdnimg.cn/64c55c0167bf46bcaf23b3200644414b.png) |



### ⑤、自动配置原理 -- 条件装配注解（@Conditional）

![img](https://img-blog.csdnimg.cn/6c1c740cdd5543bab9e6045246d11aef.png)



示例：

| **@ConditionalOnClass**：判断环境中是否有对应的字节码文件，才注册bean到IOC容器 |
| ------------------------------------------------------------ |
| ![img](https://img-blog.csdnimg.cn/ec0450fdd86446d48b9dcb2350c83988.png) |
| **@ConditionalOnMissingBean**：判断环境中没有对应的bean（类型 或 名称），才注册bean到IOC容器（该注解的应用场景，通常是用来设置一个默认的bean对象） |
| ![img](https://img-blog.csdnimg.cn/590364f47a374e9da87f12db412601ad.png) |
| **@ConditionalOnProperty**：判断配置文件中有对应属性和值，才注册bean到IOC容器中 |
| ![img](https://img-blog.csdnimg.cn/99f108ea2f404e48af0b86c7ecf6725f.png) |



@Conditional小结

![img](https://img-blog.csdnimg.cn/c5706bdeace34373b886f886ed8544ac.png)





### ⑥、自动配置原理 —— 小结

![img](https://img-blog.csdnimg.cn/e68dfa16f21140788f46580255f1234e.png)



| 自动配置的核心就在**@SpringBootApplication**注解上，SpringBootApplication这个注解底层包含了3个注解，分别是：- @SpringBootConfiguration- @ComponentScan- @EnableAutoConfiguration |
| ------------------------------------------------------------ |
| **@EnableAutoConfiguration**这个注解才是自动配置的核心。它封装了一个**@Import**注解，Import注解里面指定了一个ImportSelector接口的实现类。在这个实现类中，重写了ImportSelector接口中的**selectImports()**方法。而selectImports()方法中会去读取两份配置文件，并将配置文件中定义的配置类做为selectImports()方法的返回值返回，返回值代表的就是需要将哪些类交给Spring的IOC容器进行管理。那么所有自动配置类的中声明的bean都会加载到Spring的IOC容器中吗? 其实并不会，因为这些配置类中在声明bean时，通常都会添加@Conditional开头的注解，这个注解就是进行条件装配。而Spring会根据Conditional注解有选择性的进行bean的创建。@Enable 开头的注解底层，它就封装了一个注解 import 注解，它里面指定了一个类，是 ImportSelector 接口的实现类。在实现类当中，我们需要去实现 ImportSelector 接口当中的一个方法 selectImports 这个方法。这个方法的返回值代表的就是我需要将哪些类交给 spring 的 IOC容器进行管理。此时它会去读取两份配置文件，一份儿是 spring.factories，另外一份儿是 autoConfiguration.imports。而在 autoConfiguration.imports 这份儿文件当中，它就会去配置大量的自动配置的类。而前面我们也提到过这些所有的自动配置类当中，所有的 bean都会加载到 spring 的 IOC 容器当中吗？其实并不会，因为这些配置类当中，在声明 bean 的时候，通常会加上这么一类@Conditional 开头的注解。这个注解就是进行条件装配。所以SpringBoot非常的智能，它会根据 @Conditional 注解来进行条件装配。只有条件成立，它才会声明这个bean，才会将这个 bean 交给 IOC 容器管理。 |



### ⑦、案例（自定义starter）

业务场景：

![img](https://img-blog.csdnimg.cn/8f6d778a04fc4f45a714c473b6d44c17.png)



业务场景：我们前面案例当中所使用的阿里云OSS对象存储服务，现在阿里云的官方是没有给我们提供对应的起步依赖的，这个时候使用起来就会比较繁琐，我们需要引入对应的依赖。我们还需要在配置文件当中进行配置，还需要基于官方SDK示例来改造对应的工具类，我们在项目当中才可以进行使用。大家想在我们当前项目当中使用了阿里云OSS，我们需要进行这么多步的操作。在别的项目组当中要想使用阿里云OSS，是不是也需要进行这么多步的操作，所以这个时候我们就可以**自定义一些公共组件**，在这些公共组件当中，我就可以提前把需要配置的bean都提前配置好。将来在项目当中，我要想使用这个技术，我直接将组件对应的坐标直接引入进来，就已经自动配置好了，就可以直接使用了。我们也可以把公共组件提供给别的项目组进行使用，这样就可以大大的简化我们的开发。

![img](https://img-blog.csdnimg.cn/c58cb29fd4c84697bcb7f2a6de38cb28.png)



实现步骤：

| Ⅰ、创建**aliyun-oss-spring-boot-starter**模块：              |
| ------------------------------------------------------------ |
| 文件结构：![img](https://img-blog.csdnimg.cn/73d4ce552e684e4d8427622db9ef8149.png) 由于aliyun-oss-spring-boot-starter模块只做依赖管理，所以其它文件就都可以删掉pom.xml：![img](https://img-blog.csdnimg.cn/0ca59698b0ff476096c599a38523c67b.png) |
| Ⅱ、创建**aliyun-oss-spring-boot-autoconfigure**模块，在starter中引入该模块 |
| 文件结构：![img](https://img-blog.csdnimg.cn/a5bd2effd4754b36a20e18fdac47451b.png)pom.xml：![img](https://img-blog.csdnimg.cn/8f8a936c0a2c415d96283065b0e7068c.png)在starter中引入autoconfigure模块：![img](https://img-blog.csdnimg.cn/4a0f9799f4984b8d8b2070e1c1afb610.png) |
| Ⅲ、在**aliyun-oss-spring-boot-autoconfigure**模块中定义自动配置功能，并定义自动配置文件 META-INF/spring/xxxx.imports |
| 在autoconfigure模块中的pom.xml文件引入阿里云OSS的依赖：![img](https://img-blog.csdnimg.cn/d02629e2b7514950a21adc795a92bead.png)将相关工具类也引入autoconfigure模块（如果有报错，需要自行修改）![img](https://img-blog.csdnimg.cn/92cefd8a3965431d8d2d80e4075a5996.png)定义自动配置类（AliOSSAutoConfiguration）：![img](https://img-blog.csdnimg.cn/4ad6784d45084c28a6a12058152b5605.png)定义自动配置文件 META-INF/spring/xxxx.imports：![img](https://img-blog.csdnimg.cn/608b08558c134af48f49a3675d67abd4.png) |
| Ⅳ、功能测试：                                                |
| 配置文件（application.yml）![img](https://img-blog.csdnimg.cn/215a6fb4167a45029a092ea9059034ee.png)测试方法：![img](https://img-blog.csdnimg.cn/18882da804f14e4ba70ee90b6371836b.png)Postman测试：![img](https://img-blog.csdnimg.cn/ce0db2b1ff9748bf9a20023c58232f61.png) |



# 四、Web后端开发总结

## 1、三层架构

| web后端开发现在基本上都是基于标准的**三层架构**进行开发的，在三层架构当中，Controller控制器层负责接收请求响应数据，Service业务层负责具体的业务逻辑处理，而Dao数据访问层也叫持久层，就是用来处理数据访问操作的，来完成数据库当中数据的增删改查操作。![img](https://img-blog.csdnimg.cn/621227817bb248baa7d9405c660541fd.png)在三层架构当中，前端发起请求首先会到达Controller(不进行逻辑处理)，然后Controller会直接调用Service 进行逻辑处理， Service再调用Dao完成数据访问操作。 |
| ------------------------------------------------------------ |
| 如果我们在执行具体的业务处理之前，需要去做一些通用的业务处理，比如：我们要进行统一的登录校验，我们要进行统一的字符编码等这些操作时，我们就可以借助于Javaweb当中三大组件之一的过滤器**Filter**或者是Spring当中提供的拦截器**Interceptor**来实现。![img](https://img-blog.csdnimg.cn/0b0b6565c2af4a3aba79d6b42af0be5e.png)而为了实现三层架构层与层之间的解耦，我们学习了Spring框架当中的第一大核心：**IOC**控制反转与**DI**依赖注入所谓**控制反转**，指的是将对象创建的控制权由应用程序自身交给外部容器，这个容器就是我们常说的IOC容器或Spring容器。而DI**依赖注入**指的是容器为程序提供运行时所需要的资源。 |
| 除了IOC与DI我们还讲到了**AOP**面向切面编程，还有Spring中的事务管理、全局异常处理器，以及传递会话技术Cookie、Session以及新的会话跟踪解决方案JWT令牌，阿里云OSS对象存储服务，以及通过Mybatis持久层架构操作数据库等技术。![img](https://img-blog.csdnimg.cn/08e31257203c475391717655d96797f5.png) |
| 我们在学习这些web后端开发技术的时候，我们都是基于主流的SpringBoot进行整合使用的。而SpringBoot又是用来简化开发，提高开发效率的。像过滤器、拦截器、IOC、DI、AOP、事务管理等这些技术到底是哪个框架提供的核心功能？![img](https://img-blog.csdnimg.cn/b64625181c974321b258f33c79d4eca7.png)Filter过滤器、Cookie、 Session这些都是传统的JavaWeb提供的技术。JWT令牌、阿里云OSS对象存储服务，是现在企业项目中常见的一些解决方案。IOC控制反转、DI依赖注入、AOP面向切面编程、事务管理、全局异常处理、拦截器等，这些技术都是 Spring Framework框架当中提供的核心功能。Mybatis就是一个持久层的框架，是用来操作数据库的。 |
| 在Spring框架的生态中，对web程序开发提供了很好的支持，如：全局异常处理器、拦截器这些都是Spring框架中web开发模块所提供的功能，而Spring框架的web开发模块，我们也称为：**SpringMVC**![img](https://img-blog.csdnimg.cn/92233ec1d6c74068a9dbce3070865be2.png)SpringMVC不是一个单独的框架，它是Spring框架的一部分，是Spring框架中的web开发模块，是用来简化原始的Servlet程序开发的。外界俗称的SSM，就是由：**SpringMVC、Spring Framework、Mybatis**三块组成。基于传统的SSM框架进行整合开发项目会比较繁琐，而且效率也比较低，所以在现在的企业项目开发当中，基本上都是直接基于SpringBoot整合SSM进行项目开发的。 |