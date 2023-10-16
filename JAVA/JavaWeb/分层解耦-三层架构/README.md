**一、概述**
![在这里插入图片描述](https://img-blog.csdnimg.cn/fe2d460286c34322a8fd9f2cadf96bc8.png)

**二、请求**

**（一）概念：** 全名为HttpServletRequest，其目标是获取请求数据。

**（二）简单请求：** web端发送基本数据类型数据到服务器进行处理。

**1、获取方式**

**（1）原始方法： 通过参数HttpServletRequest获取请求数据**

** 1、服务端代码**

```java
    @RequestMapping("/hello")public Map<String, String[]> hello(HttpServletRequest request){return request.getParameterMap();}
```

** 2、请求端**

![在这里插入图片描述](https://img-blog.csdnimg.cn/43ff0c56c9c2437095a537744a2d93ac.png)

**（2）SpringBoot接收参数：形参名与请求数据名相同，即可获取对应值 或 添加@RequestParam注解映射参数**

** 1、服务端代码**

```java
    /*** 使用@RequestParam(name = "xxxx",required = true) String name 表示请求端的名称与接口的参数映射并且参数必须传递，name和required属性都默认为参数名和true，当我们需要做出改变时才使用。* 注解的意思是“请求端发送的请求数据 xxxx 映射到 服务端的 name 参数，并且这个参数必须在请求体中”*/@RequestMapping("/hello")public int hello(@RequestParam(name="name",required=true) String name,Integer age){System.out.println(name + " " +age);return 200;}
```

**2、请求端**

![在这里插入图片描述](https://img-blog.csdnimg.cn/bceab487eb0844f4a6332524ea2beaaf.png)
**（三）实体参数：** web端发送数据，且符合实体对象属性，则使用实体参数接收

** 1、服务端代码**

```java
@RestController
public class UserController {@RequestMapping("/regist")public int regist(User user){System.out.println(user);return 200;}
}class User{private String name;private String age;@Overridepublic String toString() {return "User{" +"name='" + name + '\'' +", age='" + age + '\'' +'}';}public User() {}public User(String name, String age) {this.name = name;this.age = age;}public String getName() {return name;}public void setName(String name) {this.name = name;}public String getAge() {return age;}public void setAge(String age) {this.age = age;}
}
```

** 2、请求端**
![在这里插入图片描述](https://img-blog.csdnimg.cn/d981ed560bc94256850c5aab4cedc27e.png)

**注意事项：**
**1、简单参数可以使用 @RequestParam(name,required) 注解规定接收的参数名和传递必须性。**
**2、实体参数在请求端中必须与服务端的实体对象的属性名相同。**
**3、多实体参数在请求端中必须使用“ 实体名.属性名=属性值 ”的结构传递。**

**（四）集合参数传递**

** 1、服务端代码**

```java
@RestController
public class UserController {//数组接收参数@RequestMapping("/arrayParam")public int arrayParam(String[] hobby){System.out.println(Arrays.toString(hobby));return 200;}//集合接收参数@RequestMapping("/collectionParam")public int collectionParam(@RequestParam List<String> hobby){System.out.println(Arrays.toString(hobby.toArray()));return 200;}
}/*打印结果*/
[唱, 跳, Rap, 篮球]
[唱, 跳, Rap, 篮球]
```

** 2、请求端**
![在这里插入图片描述](https://img-blog.csdnimg.cn/c1d9d1245a7d4015bdb74c9e2be38507.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/0709bbe9b0144483ba9b767a40605fec.png)

**（五）日期参数传递**

**1、服务端代码**

```java
@RestController
public class DateController {@RequestMapping("/dateParam")public int dateParam(@DateTimeFormat(pattern = "yyyy-MM-dd HH:mm:ss") LocalDateTime date){System.out.println(date.toString());return 200;}
}/*打印输出*/
2023-06-14T15:47:30
```

**2、请求端**
![在这里插入图片描述](https://img-blog.csdnimg.cn/d65922cfc0ec435aa061af29ebaae42e.png)

**（六）JSON参数传递**

**1、服务端代码**

```java
@RestController
public class JsonController {@RequestMapping("/JsonParam")public int JsonParam(@RequestBody User user){System.out.println(user);return 200;}
}class User{private String name;private String age;private Address address;@Overridepublic String toString() {return "User{" +"name='" + name + '\'' +", age='" + age + '\'' +", address=" + address +'}';}public Address getAddress() {return address;}public void setAddress(Address address) {this.address = address;}public String getName() {return name;}public void setName(String name) {this.name = name;}public String getAge() {return age;}public void setAge(String age) {this.age = age;}
}class Address{private String province;private String city;@Overridepublic String toString() {return "Address{" +"province='" + province + '\'' +", city='" + city + '\'' +'}';}public String getProvince() {return province;}public void setProvince(String province) {this.province = province;}public String getCity() {return city;}public void setCity(String city) {this.city = city;}
}/*打印输出*/
User{name='惊喜', age='23', address=Address{province='浙江', city='杭州'}}
```

**2、请求端**

![在这里插入图片描述](https://img-blog.csdnimg.cn/5df2e224b5684bbc9754ded58ed8f06b.png)

**（七）路径参数传递**

**1、服务器端代码**

```java
@RestController
public class PathController {@RequestMapping("/pathParam/{id}")public int pathParam(@PathVariable Integer id){System.out.println(id);return 200;}
}/*打印输出*/
1024
```

**2、请求端**

![在这里插入图片描述](https://img-blog.csdnimg.cn/c04905ed59504dc2bced623180b3dd11.png)

**三、响应**

**（一）概念：** 全名为HttpServletResponse，其目标是设置响应数据。

**（二）设置响应数据**

**1、关键注解：@ResponseBody**

- **类型：** 方法注解、类注解
- **位置：** Controller方法或类上
- **作用：** 将方法返回值直接响应，如果返回值类型是 实体对象或集合，将转为JSON格式响应
- **说明：** @RestController = @Controller + @ResponseBody

**2、封装响应数据**

```java
public class Result {//状态码private int code;//返回描述private String msg;//返回数据private Object data;public Result() {}public Result(int code, String msg, Object data) {this.code = code;this.msg = msg;this.data = data;}@Overridepublic String toString() {return "Result{" +"code=" + code +", msg='" + msg + '\'' +", data=" + data +'}';}public static Result success(int code,String msg,Object data){return new Result(code,msg,data);}public static Result success(){return success(200,"success",null);}public static Result error(){return new Result(400,"客户端出现错误",null);}public int getCode() {return code;}public void setCode(int code) {this.code = code;}public String getMsg() {return msg;}public void setMsg(String msg) {this.msg = msg;}public Object getData() {return data;}public void setData(Object data) {this.data = data;}
}
```

**3、返回响应数据**

```java
@RestController
public class NewController {@RequestMapping("/testParam")public Result testParam(){String data = "这里有返回数据";return Result.success(200,"成功接收",data);}
}
```

**4、请求端接收数据**
![在这里插入图片描述](https://img-blog.csdnimg.cn/9d287b34619c403faf7ae2cc5e7b3f15.png)

**四、分层解耦**

**（一）三层架构**

![在这里插入图片描述](https://img-blog.csdnimg.cn/b3d518ff57174df08dfe0642843839c8.png)

- **第一层：Contoller：** 控制层，负责接收客户端发送的请求，对请求进行处理，并响应数据。
- **第二次：Service：** 业务逻辑层，负责处理具体的业务逻辑
- **第三层：Dao** 数据访问层（Data Access Object）（持久层），负责数据访问操作。

**（二）分层解耦**

**1、两个概念**

- **内聚：** 软件各个功能模块内部的功能联系。（只调用不改变）
- **耦合：** 衡量软件中各个层/模块之间的依赖、关联程度。（模块改变不影响其它模块使用）

**2、软件设计原则：** 高内聚，低耦合

**3、Spring如何实现解耦？**

**（1）相关概念**

- **控制反转 IOC（Inversion Of Control）：** 对象的创建控制权由程序自身转移到外部（容器）。
- **DI依赖注入（Dependency Injection）：** 容器为应用程序提供运行时，所依赖的资源。
- **Bean对象：** IOC容器中创建、管理的对象。

**（2）改写三层架构**

- **步骤一：** Service层及Dao层实现类交给IOC容器管理。（类上添加注解@Component）
- **步骤二：** 为Controller及Service注入运行时，依赖的对象。（引用对象上添加注解@Autowired）
- **步骤三：** 运行测试。

**（三） IOC**

**1、Bean对象的管理注解**

| 注解        | 说明                                 | 位置                     |
| ----------- | ------------------------------------ | ------------------------ |
| @Component  | 声明bean的基础注解                   | 除以下几类注解位置的位置 |
| @Controller | 声明为控制层，@Component衍生注解     | 标注于控制器类           |
| @Service    | 声明为服务层，@Component衍生注解     | 标注于业务实现类         |
| @Repository | 声明为数据访问层，@Component衍生注解 | 标注于数据访问实现类     |

**注意：**

- 声明bean时，可通过value属性指定bean的名字，默认为类名首字母小写。
- 使用以上四个注解都可以声明bean，但在Springboot集成开发中，声明控制器bean只能用@Controller。

**2、Bean对象的组件扫描**

**（1）使用原因：** Bean对象的四大注解并未生效。
**（2）使用方法：** 启动类上添加@ComponentScan注解，并添加扫描的具体包名集合。

```java
@ComponentScan({xxx.dao,xxx.service....})
@SpringBootApplication
public class SpringbootWebRegRespApplicaiton{public static void main(String[] args){SpringApplication.run(SpringbootWebRegRespApplicaiton.class,args);}
}
```

**注意：** 启动类上 @SpringBootApplication 注解已经默认添加了注解扫描，其范围为启动类所在包

**（四） DI**

**1、作用：** 当IOC容器中的引用对象冲突（比如有多个实现同个接口的实现类，那么控制器调用接口时会发生错误），@Autowired就会发生异常，为明确引用对象，则通过明确的注解进行声明。

**2、DI的解决方案**

| 注解       | 说明                                       |
| ---------- | ------------------------------------------ |
| @Primary   | 强制注解注入                               |
| @Qualifier | 注入指定名称的注解，需要配合@Autowired注解 |
| @Resource  | 按照Bean名称进行注入                       |

![JavaWeb学习路线（4）——请求响应与分层解耦](http://pic.xiahunao.cn/wd/JavaWeb%E5%AD%A6%E4%B9%A0%E8%B7%AF%E7%BA%BF%EF%BC%884%EF%BC%89%E2%80%94%E2%80%94%E8%AF%B7%E6%B1%82%E5%93%8D%E5%BA%94%E4%B8%8E%E5%88%86%E5%B1%82%E8%A7%A3%E8%80%A6)