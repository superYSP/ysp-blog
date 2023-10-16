### 1 Mybatis

**1. 定义及相关概念**

### **1.1 定义**

MyBatis是一款流行的轻量级ORM（对象关系映射）框架，它能够将Java对象映射到关系型数据库中的数据表上，让Java程序员可以使用Java对象来操作数据库，而不用直接编写复杂的SQL语句。

### **1.2.特点**

MyBatis具有以下特点：

1. 灵活的SQL映射：MyBatis使用XML或注解配置方式来映射Java对象和SQL语句之间的关系（SQL 和 Java 代码分离），开发人员可以根据需要自定义SQL语句的逻辑，从而实现更灵活的数据库操作。
2. 易于使用：MyBatis的配置简单，上手容易，开发人员可以很快地编写出高效、可靠的数据库访问程序。
3. 可以与Spring等框架集成：MyBatis可以与Spring等常用框架集成，提供更全面的解决方案，帮助开发人员快速构建稳健的企业级应用程序。



### **1.3 工作原理**

MyBatis的工作原理如下：

1. 首先，开发人员通过XML或注解配置文件来描述Java对象和SQL语句之间的映射关系。
2. 当Java程序需要访问数据库时，MyBatis会根据配置文件自动生成SQL语句，并将Java对象转换为数据库中的数据类型。
3. MyBatis使用JDBC API来执行SQL语句，并将查询结果映射回Java对象中。
4. 最后，Java程序可以通过MyBatis提供的API来访问数据库，并对数据进行增删改查等操作。



### **2. MyBatis的使用**

### **2.1 添加相关依赖**

在**pom.xml**中添加**mybatis**依赖

```xml
<dependency>
  <groupId>org.mybatis</groupId>
  <artifactId>mybatis</artifactId>
  <version>3.5.2</version>
</dependency
```



### **2.2 实体类+Mapper**

在mybatis中，Mapper等同于Dao（都是接口）



### **2.3 MyBatis相关配置文件**



### **2.3.1 主配置文件**

在resurces资源文件夹下添加主配置文件

主配置文件通常被命名为**mybatis_config.xml**，并配置数据源和映射文件的位置

```xml
<configuration>
<!--    设置数据连接信息的来源-->
    <properties resource="db.properties"/>

<!--    为包中的类设置别名，作用：mapper映射文件实体不用输入完整名称（包名.类名）输入类名即可-->
    <typeAliases>
<!--        指定哪些包设置别名-->
        <package name="com.woniu.mybatis.entity"/>
    </typeAliases>

<!--    environment环境-->
    <environments default="development">
        <environment id="development">
<!--            事务处理器（jdbc常规的事务处理方式，通过connection（commit，rollback））-->
            <transactionManager type="JDBC"></transactionManager>

<!--            数据源类型 POOLED池，使用连接池-->
            <dataSource type="POOLED">
                <property name="driver" value="${jdbc.driver}"/>
                <property name="url" value="${jdbc.url}"/>
                <property name="username" value="${jdbc.user}"/>
                <property name="password" value="${jdbc.password}"/>
            </dataSource>
        </environment>
    </environments>

<!--    配置mapper映射文件的位置-->
    <mappers>
        <!--单个mapper映射-->
        <!--<mapper resource="com/woniu/mybatis/mapper/buildingMapper.xml"></mapper>-->
        <!--<mapper resource="com/woniu/mybatis/mapper/houseMapper.xml"></mapper>-->
        
        <!--整个mapper包下的接口都映射 二选一，当有多个Mapper接口时，使用第二个更方便-->
        <package name="com.woniu.mybatis.mapper"/>
    </mappers>
    
</configuration>
```



### **2.3.2 映射文件Mapper.xml**

### **1）相关要求**

- 定义的mapper映射文件，文件名必须和接口名称一致
- 在这个文件中，我们定义mapper中方法对应的sql语句
- 如果在主配置文件中没有设置实体类的别名，则写类型需要填写完整的名称（包名.类型）
- 要求每一个接口要有一个Mapper.xml文件，而且这些Mapper文件存放的位置必须和接口所在的包的层级保持一致。



![img](https://pic4.zhimg.com/80/v2-556d69f8656cdd5cea88580798466697_720w.webp)







### **2）Mapper.xml文件内相应配置（mybatis实现）**

```xml
<!--在mapper配置节中，namespace属性必填，填写对应mapper接口的完整名称-->
<mapper namespace="对应mapper接口的完整名称">
    
<!--    如果是查询方法，使用select配置节进行配置-->
<!--    id属性值就是接口方法名称，除了这个属性之外，还可设置返回类型resultType，参数类型parameterType等-->
<!--    如果参数类型不是简单类型，而是包装类型，则需要定义参数类型parameterType-->
<!--    resultType定义的是返回结果类型，即使是集合，也只需填写元素类型，mybatis会自动将结果转换为集合-->
<!--    sql语句在select配置节中间写-->
    <select id="接口方法名" resultType="返回结果类型" parameterType="复杂类型"> 
        sql语句
    </select>
</mapper>
```



### **3）返回结果的属性中包含复杂（包装）属性时的配置**

- 添加resultMap配置节为返回结果赋值

```xml
<resultMap id="houseResultMap" type="House">
    <!--主键列使用id配置节，非主键列使用result配置节，column="字段名" property="属性名"-->
    <id column="id" property="id"/>
    <result column="numbers" property="numbers"/>
    ...
    
    <!--对于复杂类型的属性，既不使用id，也不使用result，需要使用关联配置节association-->
    <association property="building" javaType="Building"> <!-- Building复杂属性类型 -->
        <id column="bid" property="id"/>
        <result column="bnumbers" property="numbers"/>
        ...
    </association>
</resultMap>
<select id="selectAllHouses" resultMap="houseResultMap">
   sql语句（用到关联查询）
</select>
```



### **4）映射文件常用配置节**

1. `<mapper>`：mapper 配置文件的根元素，定义 namespace 属性和子元素 select、insert、update 和 delete 等 SQL 语句映射规则。
2. `<select>`：定义 SELECT 查询语句映射规则，可以指定参数类型和返回值类型等属性。
3. `<insert>`：定义 INSERT 插入语句映射规则，可以指定参数类型和返回值类型等属性。
4. `<update>`：定义 UPDATE 更新语句映射规则，可以指定参数类型和返回值类型等属性。
5. `<delete>`：定义 DELETE 删除语句映射规则，可以指定参数类型和返回值类型等属性。
6. `<resultMap>`：定义 SQL 语句的返回结果集映射规则，可以指定每个返回值的属性和类型等信息。
7. `<result>`：定义一个 SQL 语句的返回值映射规则，可以指定 Java 对象的属性和数据库字段的映射关系。
8. `<parameterMap>`：定义 SQL 语句的输入参数映射规则，可以指定参数的 Java 类型和 JDBC 类型等信息。
9. `<parameter>`：定义一个 SQL 语句的输入参数映射规则，可以指定 Java 对象的属性和 SQL 语句中的参数位置等信息。
10. `<include>`：引用外部的 SQL 语句映射规则，用于避免重复编写 SQL 语句。
11. `<sql>`：定义 SQL 片段，在其他 SQL 语句中引用。
12. `<selectKey>`：定义在插入数据时获取自增主键的方式。
13. `<foreach>`: 用于循环遍历一个集合或数组，并在SQL语句中使用这些参数。

- `collection`: 要遍历的集合或数组，可以是List、Set、数组或Map类型的属性名或表达式。
- `item`: 遍历出来的每个元素的变量名，在循环过程中，可以通过这个变量名访问当前元素。
- `index`: 遍历出来的每个元素的索引值变量名，如果遍历的是List或数组，可以通过这个变量名访问当前元素的索引值。
- `open`: 在循环开始时添加的字符串。
- `close`: 在循环结束时添加的字符串。
- `separator`: 在每个元素之间添加的分隔符。



### **5）例子（添加building）**

Mapper接口添加方法

```java
int insertBuilding(Building building); 
// building里有id，numbers，units，remarks四个属性，id对应数据库中的字段为自增列
```

映射文件（传入的参数用`#{numbers}`取值）

```xml
<insert id="addBuilding" parameterType="Building"
        useGeneratedKeys="true" keyColumn="id" keyProperty="id">
    insert into building(numbers, units, remarks)
    values (#{numbers}, #{units}, #{remarks})
</insert>
```



### **2.4 Mapper中的方法的调用**

```java
 // 1. 通过mybatis主配置文件mybatis_config.xml，获取文件流
InputStream is = App.class.getClassLoader().getResourceAsStream("mybatis_config.xml");

// 2.通过获取到的文件流is创建一个会话工厂
SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(is);

// 3. 通过会话工厂factory获取会话
SqlSession sqlSession = factory.openSession();

// 4. 通过sqlSession得到目标Mapper的实现类
// 需要得到BuildingMapper实现类的实例
BuildingMapper buildingMapper = sqlSession.getMapper(BuildingMapper.class);

// 5. 通过目标Mapper的实现类对象调用对象方法实现相应功能
List<Building> buildings = buildingMapper.selectAllBuildings();


```

### **2.5 查看执行的sql语句**

要能够观察到mybatis真正的sql语句，需要导入log4j

```xml
<dependency>  
    <groupId>org.slf4j</groupId>  
    <artifactId>slf4j-log4j12</artifactId>  
    <version>1.7.25</version>
</dependency>
```

加入log4j.properties

```properties
#console表示在控制台输出
log4j.rootLogger=debug,console

log4j.appender.console=org.apache.log4j.ConsoleAppender
log4j.appender.console.layout=org.apache.log4j.PatternLayout
log4j.appender.console.layout.ConversionPattern=%p %c %d{yyyy-MM-dd HH:mm:ss} %m%n
```

完成上面两步，即可观察到日志输出，无需自己创建Logger对象



### **2.6 多参数查询**

多参数时，默认不允许直接使用接口方法的参数名，而要求使用arg0|arg1, param1|param2

解决方法

**方法一：**

在接口方法参数前面加上**@Param**注解

**List<T>**集合也需要添加注解**@Param**才能被识别

```java
List<House> queryHouses(@Param("buildingId") int buildingId, @Param("area") double area);
```

**方法二：**

将参数以Map的形式提供

```java
List<House> queryHouses2(Map<String, Object> map);
//用map集合存相应参数
Map<String, Object> param = new HashMap<>();
    param.put("buildingId", 11);
    param.put("area", 90);
//调用方法，传入map集合
mapper.queryHouses(param);
```



### **2.7 mybatis动态查询**

MyBatis动态查询是指根据不同的查询条件，使用不同的SQL语句进行查询。这样可以避免写大量的if-else或者switch-case语句，使代码更加简洁易读，提高开发效率。在MyBatis中，动态查询可以通过动态SQL来实现。

动态SQL可以根据不同的条件生成不同的SQL语句，MyBatis提供了以下几种动态SQL标签：

### **2.7.1 if标签：**

根据某个条件生成SQL片段。

示例代码：

```xml
<select id="findUserByCondition" resultType="User">
  select * from user
  <where>
    <if test="username != null">
      and username like '%${username}%'
    </if>
    <if test="age != null">
      and age = #{age}
    </if>
  </where>
</select>
```

### **2.7.2 choose,when,otherwise标签：**

根据多个条件选择不同的SQL片段。

示例代码：

```xml
<select id="findUserByCondition" resultType="User">
  select * from user
  <where>
    <choose>
      <when test="username != null">
        and username like '%${username}%'
      </when>
      <when test="email != null">
        and email like '%${email}%'
      </when>
      <otherwise>
        and age = #{age}
      </otherwise>
    </choose>
  </where>
</select>
```

### **2.7.3 trim,where,set标签：**

可以去掉生成SQL语句中多余的空格和逗号。

示例代码：

```xml
<update id="updateUser" parameterType="User">
  update user
  <set>
    <if test="username != null">username = #{username},</if>
    <if test="password != null">password = #{password},</if>
    <if test="email != null">email = #{email},</if>
    <if test="age != null">age = #{age},</if>
  </set>
  where id = #{id}
</update>
```

**2.7.4 foreach标签：**

根据集合中的元素生成SQL片段。

示例代码：

```xml
<select id="findUsersByIds" resultType="User">
  select * from user
  where id in
  <foreach collection="ids" item="id" separator="," open="(" close=")">
    #{id}
  </foreach>
</select>
```

通过以上标签的组合，可以实现复杂的动态查询语句。

## 一、MyBatis----Lombok 简介

Lombok 是一款 Java 开发插件，使得 Java 开发者可以通过其定义的一些注解来消除业务工程中冗长和繁琐的代码，尤其对于简单的 Java 模型对象（POJO）。

在开发环境中使用 Lombok 插件后，Java 开发人员可以节省出重复构建，诸如 hashCode 和 equals 这样的方法以及各种业务对象模型的 accessor 和 toString 等方法的大量时间。

对于这些方法，Lombok 能够在编译源代码期间自动帮我们生成这些方法，但并不会像反射那样降低程序的性能。

## 二、Lombok 安装

### 2.1 构建工具

**Gradle**

在 build.gradle 文件中添加 lombok 依赖：

```text
dependencies {
    compileOnly 'org.projectlombok:lombok:1.18.10'
    annotationProcessor 'org.projectlombok:lombok:1.18.10'
}
```

**Maven**

在 Maven 项目的 pom.xml 文件中添加 lombok 依赖：

```text
<dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <version>1.18.10</version>
        <scope>provided</scope>
</dependency>
```

**Ant**

假设在 lib 目录中已经存在 lombok.jar，然后设置 javac 任务：

```text
<javac srcdir="src" destdir="build" source="1.8">
    <classpath location="lib/lombok.jar" />
</javac>
```

### 2.2 IDE

由于 Lombok 仅在编译阶段生成代码，所以使用 Lombok 注解的源代码，在 IDE 中会被高亮显示错误，针对这个问题可以通过安装 IDE 对应的插件来解决。

这里不详细展开，具体的安装方式可以参考:

> [https://www.baeldung.com/lombok-ide](https://link.zhihu.com/?target=https%3A//www.baeldung.com/lombok-ide)

## 三、Lombok 详解

> 注意：以下示例所使用的 Lombok 版本是 1.18.10

### 3.1 @Getter and @Setter 注解

你可以使用 @Getter 或 @Setter 注释任何类或字段，Lombok 会自动生成默认的 getter/setter 方法。

**@Getter 注解**

```text
@Target({ElementType.FIELD, ElementType.TYPE})
@Retention(RetentionPolicy.SOURCE)
public @interface Getter {
  // 若getter方法非public的话，可以设置可访问级别
    lombok.AccessLevel value() default lombok.AccessLevel.PUBLIC;
    AnyAnnotation[] onMethod() default {};
  // 是否启用延迟初始化
    boolean lazy() default false;
}
```

**@Setter 注解**

```text
@Target({ElementType.FIELD, ElementType.TYPE})
@Retention(RetentionPolicy.SOURCE)
public @interface Setter {
  // 若setter方法非public的话，可以设置可访问级别
    lombok.AccessLevel value() default lombok.AccessLevel.PUBLIC;
    AnyAnnotation[] onMethod() default {};
    AnyAnnotation[] onParam() default {};
}
```

使用示例

```text
package com.semlinker.lombok;

@Getter
@Setter
public class GetterAndSetterDemo {
    String firstName;
    String lastName;
    LocalDate dateOfBirth;
}
```

以上代码经过 Lombok 编译后，会生成如下代码：

```text
package com.semlinker.lombok;

public class GetterAndSetterDemo {
    String firstName;
    String lastName;
    LocalDate dateOfBirth;

    public GetterAndSetterDemo() {
    }

    // 省略其它setter和getter方法
    public String getFirstName() {
        return this.firstName;
    }

    public void setFirstName(String firstName) {
        this.firstName = firstName;
    }
}
```

**Lazy Getter**

@Getter 注解支持一个 lazy 属性，该属性默认为 false。当设置为 true 时，会启用延迟初始化，即当首次调用 getter 方法时才进行初始化。

示例

```text
package com.semlinker.lombok;

public class LazyGetterDemo {
    public static void main(String[] args) {
        LazyGetterDemo m = new LazyGetterDemo();
        System.out.println("Main instance is created");
        m.getLazy();
    }

    @Getter
    private final String notLazy = createValue("not lazy");

    @Getter(lazy = true)
    private final String lazy = createValue("lazy");

    private String createValue(String name) {
        System.out.println("createValue(" + name + ")");
        return null;
    }
}
```

以上代码经过 Lombok 编译后，会生成如下代码：

```text
package com.semlinker.lombok;

public class LazyGetterDemo {
    private final String notLazy = this.createValue("not lazy");
    private final AtomicReference<Object> lazy = new AtomicReference();

    // 已省略部分代码
    public String getNotLazy() {
        return this.notLazy;
    }

    public String getLazy() {
        Object value = this.lazy.get();
        if (value == null) {
            synchronized(this.lazy) {
                value = this.lazy.get();
                if (value == null) {
                    String actualValue = this.createValue("lazy");
                    value = actualValue == null ? this.lazy : actualValue;
                    this.lazy.set(value);
                }
            }
        }

        return (String)((String)(value == this.lazy ? null : value));
    }
}
```

通过以上代码可知，调用 getLazy 方法时，若发现 value 为 null，则会在同步代码块中执行初始化操作。

### 3.2 Constructor Annotations

**@NoArgsConstructor 注解**

使用 @NoArgsConstructor 注解可以为指定类，生成默认的构造函数，@NoArgsConstructor 注解的定义如下：

```text
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.SOURCE)
public @interface NoArgsConstructor {
  // 若设置该属性，将会生成一个私有的构造函数且生成一个staticName指定的静态方法
    String staticName() default "";    
    AnyAnnotation[] onConstructor() default {};
  // 设置生成构造函数的访问级别，默认是public
    AccessLevel access() default lombok.AccessLevel.PUBLIC;
  // 若设置为true，则初始化所有final的字段为0/null/false
    boolean force() default false;
}
```

示例

```text
package com.semlinker.lombok;

@NoArgsConstructor(staticName = "getInstance")
public class NoArgsConstructorDemo {
    private long id;
    private String name;
    private int age;
}
```

以上代码经过 Lombok 编译后，会生成如下代码：

```text
package com.semlinker.lombok;

public class NoArgsConstructorDemo {
    private long id;
    private String name;
    private int age;

    private NoArgsConstructorDemo() {
    }

    public static NoArgsConstructorDemo getInstance() {
        return new NoArgsConstructorDemo();
    }
}
```

**@AllArgsConstructor 注解**

使用 @AllArgsConstructor 注解可以为指定类，生成包含所有成员的构造函数，@AllArgsConstructor 注解的定义如下：

```text
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.SOURCE)
public @interface AllArgsConstructor {
  // 若设置该属性，将会生成一个私有的构造函数且生成一个staticName指定的静态方法
    String staticName() default "";
    AnyAnnotation[] onConstructor() default {};
  // 设置生成构造函数的访问级别，默认是public
    AccessLevel access() default lombok.AccessLevel.PUBLIC;
}
```

示例

```text
package com.semlinker.lombok;

@AllArgsConstructor
public class AllArgsConstructorDemo {
    private long id;
    private String name;
    private int age;
}
```

以上代码经过 Lombok 编译后，会生成如下代码：

```text
package com.semlinker.lombok;

public class AllArgsConstructorDemo {
    private long id;
    private String name;
    private int age;

    public AllArgsConstructorDemo(long id, String name, int age) {
        this.id = id;
        this.name = name;
        this.age = age;
    }
}
```

**@RequiredArgsConstructorDemo 注解**

使用 @RequiredArgsConstructor 注解可以为指定类必须初始化的成员变量，如 final 成员变量，生成对应的构造函数，@RequiredArgsConstructor 注解的定义如下：

```text
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.SOURCE)
public @interface RequiredArgsConstructor {
  // 若设置该属性，将会生成一个私有的构造函数且生成一个staticName指定的静态方法
    String staticName() default "";
    AnyAnnotation[] onConstructor() default {};
  // 设置生成构造函数的访问级别，默认是public
    AccessLevel access() default lombok.AccessLevel.PUBLIC;
}
```

示例

```text
package com.semlinker.lombok;

@RequiredArgsConstructor
public class RequiredArgsConstructorDemo {
    private final long id;
    private String name;
    private int age;
}
```

以上代码经过 Lombok 编译后，会生成如下代码：

```text
package com.semlinker.lombok;

public class RequiredArgsConstructorDemo {
    private final long id;
    private String name;
    private int age;

    public RequiredArgsConstructorDemo(long id) {
        this.id = id;
    }
}
```

**3.3 @EqualsAndHashCode 注解**

使用 @EqualsAndHashCode 注解可以为指定类生成 equals 和 hashCode 方法， @EqualsAndHashCode 注解的定义如下：

```text
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.SOURCE)
public @interface EqualsAndHashCode {
  // 指定在生成的equals和hashCode方法中需要排除的字段列表
    String[] exclude() default {};

  // 显式列出用于identity的字段，一般情况下non-static,non-transient字段会被用于identity
    String[] of() default {};

  // 标识在执行字段计算前，是否调用父类的equals和hashCode方法
    boolean callSuper() default false;

    boolean doNotUseGetters() default false;

    AnyAnnotation[] onParam() default {};

    @Deprecated
    @Retention(RetentionPolicy.SOURCE)
    @Target({})
    @interface AnyAnnotation {}

    @Target(ElementType.FIELD)
    @Retention(RetentionPolicy.SOURCE)
    public @interface Exclude {}

    @Target({ElementType.FIELD, ElementType.METHOD})
    @Retention(RetentionPolicy.SOURCE)
    public @interface Include {
        String replaces() default "";
    }
}
```

示例

```text
package com.semlinker.lombok;

@EqualsAndHashCode
public class EqualsAndHashCodeDemo {
    String firstName;
    String lastName;
    LocalDate dateOfBirth;
}
```

以上代码经过 Lombok 编译后，会生成如下代码：

```text
package com.semlinker.lombok;

public class EqualsAndHashCodeDemo {
    String firstName;
    String lastName;
    LocalDate dateOfBirth;

    public EqualsAndHashCodeDemo() {
    }

    public boolean equals(Object o) {
        if (o == this) {
            return true;
        } else if (!(o instanceof EqualsAndHashCodeDemo)) {
            return false;
        } else {
            EqualsAndHashCodeDemo other = (EqualsAndHashCodeDemo)o;
            if (!other.canEqual(this)) {
                return false;
            } else {
              // 已省略大量代码
        }
    }

    public int hashCode() {
        int PRIME = true;
        int result = 1;
        Object $firstName = this.firstName;
        int result = result * 59 + ($firstName == null ? 43 : $firstName.hashCode());
        Object $lastName = this.lastName;
        result = result * 59 + ($lastName == null ? 43 : $lastName.hashCode());
        Object $dateOfBirth = this.dateOfBirth;
        result = result * 59 + ($dateOfBirth == null ? 43 : $dateOfBirth.hashCode());
        return result;
    }
}
```

### 3.4 @ToString 注解

使用 @ToString 注解可以为指定类生成 toString 方法， @ToString 注解的定义如下：

```text
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.SOURCE)
public @interface ToString {
  // 打印输出时是否包含字段的名称
    boolean includeFieldNames() default true;

  // 列出打印输出时，需要排除的字段列表
    String[] exclude() default {};

  // 显式的列出需要打印输出的字段列表
    String[] of() default {};

  // 打印输出的结果中是否包含父类的toString方法的返回结果
    boolean callSuper() default false;

    boolean doNotUseGetters() default false;

    boolean onlyExplicitlyIncluded() default false;

    @Target(ElementType.FIELD)
    @Retention(RetentionPolicy.SOURCE)
    public @interface Exclude {}

    @Target({ElementType.FIELD, ElementType.METHOD})
    @Retention(RetentionPolicy.SOURCE)
    public @interface Include {
        int rank() default 0;
        String name() default "";
    }
}
```

示例

```text
package com.semlinker.lombok;

@ToString(exclude = {"dateOfBirth"})
public class ToStringDemo {
    String firstName;
    String lastName;
    LocalDate dateOfBirth;
}
```

以上代码经过 Lombok 编译后，会生成如下代码：

```text
package com.semlinker.lombok;

public class ToStringDemo {
    String firstName;
    String lastName;
    LocalDate dateOfBirth;

    public ToStringDemo() {
    }

    public String toString() {
        return "ToStringDemo(firstName=" + this.firstName + ", lastName=" + 
          this.lastName + ")";
    }
}
```

### 3.5 @Data 注解

@Data 注解与同时使用以下的注解的效果是一样的：

- @ToString
- @Getter
- @Setter
- @RequiredArgsConstructor
- @EqualsAndHashCode

@Data 注解的定义如下：

```text
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.SOURCE)
public @interface Data {
    String staticConstructor() default "";
}
```

示例

```text
package com.semlinker.lombok;

@Data
public class DataDemo {
    private Long id;
    private String summary;
    private String description;
}
```

以上代码经过 Lombok 编译后，会生成如下代码：

```text
package com.semlinker.lombok;

public class DataDemo {
    private Long id;
    private String summary;
    private String description;

    public DataDemo() {
    }

    // 省略summary和description成员属性的setter和getter方法
    public Long getId() {
        return this.id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public boolean equals(Object o) {
        if (o == this) {
            return true;
        } else if (!(o instanceof DataDemo)) {
            return false;
        } else {
            DataDemo other = (DataDemo)o;
            if (!other.canEqual(this)) {
                return false;
            } else {
               // 已省略大量代码
            }
        }
    }

    protected boolean canEqual(Object other) {
        return other instanceof DataDemo;
    }

    public int hashCode() {
        int PRIME = true;
        int result = 1;
        Object $id = this.getId();
        int result = result * 59 + ($id == null ? 43 : $id.hashCode());
        Object $summary = this.getSummary();
        result = result * 59 + ($summary == null ? 43 : $summary.hashCode());
        Object $description = this.getDescription();
        result = result * 59 + ($description == null ? 43 : $description.hashCode());
        return result;
    }

    public String toString() {
        return "DataDemo(id=" + this.getId() + ", summary=" + this.getSummary() + ", description=" + this.getDescription() + ")";
    }
}
```

### 3.6 @Log 注解

若你将 @Log 的变体放在类上（适用于你所使用的日志记录系统的任何一种）；之后，你将拥有一个静态的 final log 字段，然后你就可以使用该字段来输出日志。

**@Log**

```text
private static final java.util.logging.Logger log = 
java.util.logging.Logger.getLogger(LogExample.class.getName());
```

**@Log4j**

```text
private static final org.apache.log4j.Logger log = 
org.apache.log4j.Logger.getLogger(LogExample.class);
```

**@Log4j2**

```text
private static final org.apache.logging.log4j.Logger log = 
org.apache.logging.log4j.LogManager.getLogger(LogExample.class);
```

**@Slf4j**

```text
private static final org.slf4j.Logger log = 
org.slf4j.LoggerFactory.getLogger(LogExample.class);
```

**@XSlf4j**

```text
private static final org.slf4j.ext.XLogger log = 
org.slf4j.ext.XLoggerFactory.getXLogger(LogExample.class);
```

**@CommonsLog**

```text
private static final org.apache.commons.logging.Log log = 
org.apache.commons.logging.LogFactory.getLog(LogExample.class);
```

### 3.7 @Synchronized 注解

@Synchronized 是同步方法修饰符的更安全的变体。与 synchronized 一样，该注解只能应用在静态和实例方法上。它的操作类似于 synchronized 关键字，但是它锁定在不同的对象上。synchronized 关键字应用在实例方法时，锁定的是 this 对象，而应用在静态方法上锁定的是类对象。

对于 @Synchronized 注解声明的方法来说，它锁定的是 `$LOCK` 或 `$lock`。@Synchronized 注解的定义如下：

```text
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.SOURCE)
public @interface Synchronized {
  // 指定锁定的字段名称
    String value() default "";
}
```

示例

```text
package com.semlinker.lombok;

public class SynchronizedDemo {
    private final Object readLock = new Object();

    @Synchronized
    public static void hello() {
        System.out.println("world");
    }

    @Synchronized
    public int answerToLife() {
        return 42;
    }

    @Synchronized("readLock")
    public void foo() {
        System.out.println("bar");
    }
}
```

以上代码经过 Lombok 编译后，会生成如下代码：

```text
package com.semlinker.lombok;

public class SynchronizedDemo {
    private static final Object $LOCK = new Object[0];
    private final Object $lock = new Object[0];
    private final Object readLock = new Object();

    public SynchronizedDemo() {
    }

    public static void hello() {
        synchronized($LOCK) {
            System.out.println("world");
        }
    }

    public int answerToLife() {
        synchronized(this.$lock) {
            return 42;
        }
    }

    public void foo() {
        synchronized(this.readLock) {
            System.out.println("bar");
        }
    }
}
```

### 3.8 @Builder 注解

使用 @Builder 注解可以为指定类实现建造者模式，该注解可以放在类、构造函数或方法上。@Builder 注解的定义如下：

```text
@Target({TYPE, METHOD, CONSTRUCTOR})
@Retention(SOURCE)
public @interface Builder {
    @Target(FIELD)
    @Retention(SOURCE)
    public @interface Default {}

  // 创建新的builder实例的方法名称
    String builderMethodName() default "builder";
    // 创建Builder注解类对应实例的方法名称
    String buildMethodName() default "build";
    // builder类的名称
    String builderClassName() default "";

    boolean toBuilder() default false;

    AccessLevel access() default lombok.AccessLevel.PUBLIC;

    @Target({FIELD, PARAMETER})
    @Retention(SOURCE)
    public @interface ObtainVia {
        String field() default "";
        String method() default "";
        boolean isStatic() default false;
    }
}
```

示例

```text
package com.semlinker.lombok;

@Builder
public class BuilderDemo {
    private final String firstname;
    private final String lastname;
    private final String email;
}
```

以上代码经过 Lombok 编译后，会生成如下代码：

```text
package com.semlinker.lombok;

public class BuilderDemo {
    private final String firstname;
    private final String lastname;
    private final String email;

    BuilderDemo(String firstname, String lastname, String email) {
        this.firstname = firstname;
        this.lastname = lastname;
        this.email = email;
    }

    public static BuilderDemo.BuilderDemoBuilder builder() {
        return new BuilderDemo.BuilderDemoBuilder();
    }

    public static class BuilderDemoBuilder {
        private String firstname;
        private String lastname;
        private String email;

        BuilderDemoBuilder() {
        }

        public BuilderDemo.BuilderDemoBuilder firstname(String firstname) {
            this.firstname = firstname;
            return this;
        }

        public BuilderDemo.BuilderDemoBuilder lastname(String lastname) {
            this.lastname = lastname;
            return this;
        }

        public BuilderDemo.BuilderDemoBuilder email(String email) {
            this.email = email;
            return this;
        }

        public BuilderDemo build() {
            return new BuilderDemo(this.firstname, this.lastname, this.email);
        }

        public String toString() {
            return "BuilderDemo.BuilderDemoBuilder(firstname=" + this.firstname + ", lastname=" + this.lastname + ", email=" + this.email + ")";
        }
    }
}
```

### 3.9 @SneakyThrows 注解

@SneakyThrows 注解用于自动抛出已检查的异常，而无需在方法中使用 throw 语句显式抛出。@SneakyThrows 注解的定义如下：

```text
@Target({ElementType.METHOD, ElementType.CONSTRUCTOR})
@Retention(RetentionPolicy.SOURCE)
public @interface SneakyThrows {
    // 设置你希望向上抛的异常类
    Class<? extends Throwable>[] value() default java.lang.Throwable.class;
}
```

示例

```text
package com.semlinker.lombok;

public class SneakyThrowsDemo {
    @SneakyThrows
    @Override
    protected Object clone() {
        return super.clone();
    }
}
```

以上代码经过 Lombok 编译后，会生成如下代码：

```text
package com.semlinker.lombok;

public class SneakyThrowsDemo {
    public SneakyThrowsDemo() {
    }

    protected Object clone() {
        try {
            return super.clone();
        } catch (Throwable var2) {
            throw var2;
        }
    }
}
```

### 3.10 @NonNull 注解

你可以在方法或构造函数的参数上使用 @NonNull 注解，它将会为你自动生成非空校验语句。@NonNull 注解的定义如下：

```text
@Target({ElementType.FIELD, ElementType.METHOD, ElementType.PARAMETER, ElementType.LOCAL_VARIABLE, ElementType.TYPE_USE})
@Retention(RetentionPolicy.CLASS)
@Documented
public @interface NonNull {
}
```

示例

```text
package com.semlinker.lombok;

public class NonNullDemo {
    @Getter
    @Setter
    @NonNull
    private String name;
}
```

以上代码经过 Lombok 编译后，会生成如下代码：

```text
package com.semlinker.lombok;

public class NonNullDemo {
    @NonNull
    private String name;

    public NonNullDemo() {
    }

    @NonNull
    public String getName() {
        return this.name;
    }

    public void setName(@NonNull String name) {
        if (name == null) {
            throw new NullPointerException("name is marked non-null but is null");
        } else {
            this.name = name;
        }
    }
}
```

### 3.11 @Clean 注解

@Clean 注解用于自动管理资源，用在局部变量之前，在当前变量范围内即将执行完毕退出之前会自动清理资源，自动生成 try-finally 这样的代码来关闭流。

```text
@Target(ElementType.LOCAL_VARIABLE)
@Retention(RetentionPolicy.SOURCE)
public @interface Cleanup {
  // 设置用于执行资源清理/回收的方法名称，对应方法不能包含任何参数，默认名称为close。
    String value() default "close";
}
```

示例

```text
package com.semlinker.lombok;

public class CleanupDemo {
    public static void main(String[] args) throws IOException {
        @Cleanup InputStream in = new FileInputStream(args[0]);
        @Cleanup OutputStream out = new FileOutputStream(args[1]);
        byte[] b = new byte[10000];
        while (true) {
            int r = in.read(b);
            if (r == -1) break;
            out.write(b, 0, r);
        }
    }
}
```

以上代码经过 Lombok 编译后，会生成如下代码：

```text
package com.semlinker.lombok;

public class CleanupDemo {
    public CleanupDemo() {
    }

    public static void main(String[] args) throws IOException {
        FileInputStream in = new FileInputStream(args[0]);

        try {
            FileOutputStream out = new FileOutputStream(args[1]);

            try {
                byte[] b = new byte[10000];

                while(true) {
                    int r = in.read(b);
                    if (r == -1) {
                        return;
                    }

                    out.write(b, 0, r);
                }
            } finally {
                if (Collections.singletonList(out).get(0) != null) {
                    out.close();
                }

            }
        } finally {
            if (Collections.singletonList(in).get(0) != null) {
                in.close();
            }
        }
    }
}
```

### 3.11 @With 注解

在类的字段上应用 @With 注解之后，将会自动生成一个 withFieldName(newValue) 的方法，该方法会基于 newValue 调用相应构造函数，创建一个当前类对应的实例。@With 注解的定义如下：

```text
@Target({ElementType.FIELD, ElementType.TYPE})
@Retention(RetentionPolicy.SOURCE)
public @interface With {
    AccessLevel value() default AccessLevel.PUBLIC;

    With.AnyAnnotation[] onMethod() default {};

    With.AnyAnnotation[] onParam() default {};

    @Deprecated
    @Retention(RetentionPolicy.SOURCE)
    @Target({})
    public @interface AnyAnnotation {
    }
}
```

示例

```text
public class WithDemo {
    @With(AccessLevel.PROTECTED)
    @NonNull
    private final String name;
    @With
    private final int age;

    public WithDemo(String name, int age) {
        if (name == null) throw new NullPointerException();
        this.name = name;
        this.age = age;
    }
}
```

以上代码经过 Lombok 编译后，会生成如下代码：

```text
public class WithDemo {
    @NonNull
    private final String name;
    private final int age;

    public WithDemo(String name, int age) {
        if (name == null) {
            throw new NullPointerException();
        } else {
            this.name = name;
            this.age = age;
        }
    }

    protected WithDemo withName(@NonNull String name) {
        if (name == null) {
            throw new NullPointerException("name is marked non-null but is null");
        } else {
            return this.name == name ? this : new WithDemo(name, this.age);
        }
    }

    public WithDemo withAge(int age) {
        return this.age == age ? this : new WithDemo(this.name, age);
    }
}
```

### 3.12 其它特性

**val**

val 用在局部变量前面，相当于将变量声明为 final，此外 Lombok 在编译时还会自动进行类型推断。val 的使用示例：

```text
public class ValExample {
  public String example() {
    val example = new ArrayList<String>();
    example.add("Hello, World!");
    val foo = example.get(0);
    return foo.toLowerCase();
  }

  public void example2() {
    val map = new HashMap<Integer, String>();
    map.put(0, "zero");
    map.put(5, "five");
    for (val entry : map.entrySet()) {
      System.out.printf("%d: %s\n", entry.getKey(), entry.getValue());
    }
  }
}
```

以上代码等价于：

```text
public class ValExample {
  public String example() {
    final ArrayList<String> example = new ArrayList<String>();
    example.add("Hello, World!");
    final String foo = example.get(0);
    return foo.toLowerCase();
  }

  public void example2() {
    final HashMap<Integer, String> map = new HashMap<Integer, String>();
    map.put(0, "zero");
    map.put(5, "five");
    for (final Map.Entry<Integer, String> entry : map.entrySet()) {
      System.out.printf("%d: %s\n", entry.getKey(), entry.getValue());
    }
  }
}
```

至此功能强大的 Lombok 工具就介绍完了。

