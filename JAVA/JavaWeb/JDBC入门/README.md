## **一、 JDBC 简介**

## **1 什么是 JDBC**

•JDBC（JavaDataBaseConnectivity）java 数据库连接 • 是 JavaEE 平台下的技术规范 • 定义了在 Java 语言中连接数据，执行 SQL 语句的标准 • 可以为多种关系数据库提供统一访问

## **2 什么是数据库驱动程序**

• 数据库厂商对 JDBC 规范的具体实现 • 不同数据产品的数据库驱动名字有差异 • 在程序中需要依赖数据库驱动来完成对数据库的操作

## **3 程序操作数据库流程**

![img](https://pic4.zhimg.com/80/v2-296a97c0bd6ad843a184b6d52cf6a017_720w.webp)

## **二、 JDBC3.0 标准中常用接口与类**

## **1 Driver 接口**

Driver 接口的作用是来定义数据库驱动对象应该具备的一些能力。比如与数据库建立连 接的方法的定义所有支持 java 语言连接的数据库都实现了该接口，实现该接口的类我们称 之为数据库驱动类。在程序中要连接数据库，必须先通过 JDK 的反射机制加载数据库驱动 类，将其实例化。不同的数据库驱动类的类名有区别。 加载 MySql 驱动：Class.forName("com.mysql.jdbc.Driver"); 加载 Oracle 驱动：Class.forName("oracle.jdbc.driver.OracleDriver");

## **2 DriverManager 类**

DriverManager 通过实例化的数据库驱动对象，能够建立应用程序与数据库之间建立连 接。并返回 Connection 接口类型的数据库连接对象。

2.1常用方法

•getConnection(StringjdbcUrl,Stringuser,Stringpassword) 该方法通过访问数据库的 url、用户以及密码，返回对应的数据库的 Connection 对象。

2.2JDBCURL

与数据库连接时，用来连接到指定数据库标识符。在 URL 中包括了该数据库的类型、 地址、端口、库名称等信息。不同品牌数据库的连接 URL 不同。

## **3 Connection 接口**

Connection 与数据库的连接（会话）对象。我们可以通过该对象执行 sql 语句并返回结

果。

连接 MySql 数据库： Connection conn = DriverManager.getConnection("jdbc:mysql://host:port/database", "user", "password"); 连接 Oracle 数据库： Connection conn = DriverManager.getConnection("jdbc:oracle:thin:@host:port:database", "user","password"); 连接 SqlServer 数据库： Connection conn = DriverManager.getConnection("jdbc:microsoft:sqlserver://host:port; DatabaseName=database","user","password");

3.1常用方法

•createStatement()：创建向数据库发送 sql 的 Statement 接口类型的对象。 •preparedStatement(sql) ：创建向数据库发送预编译 sql 的 PrepareSatement 接口类型的

对象。 •prepareCall(sql)：创建执行存储过程的 CallableStatement 接口类型的对象。 •setAutoCommit(booleanautoCommit)：设置事务是否自动提交。 •commit() ：在链接上提交事务。 •rollback() ：在此链接上回滚事务。

## **4 Statement 接口**

用于执行静态 SQL 语句并返回它所生成结果的对象。 由 createStatement 创建，用于发送简单的 SQL 语句（不支持动态绑定）。

4.1常用方法

•execute(String sql):执行参数中的 SQL，返回是否有结果集。 •executeQuery(Stringsql)：运行 select 语句，返回 ResultSet 结果集。 •executeUpdate(Stringsql)：运行 insert/update/delete 操作，返回更新的行数。 •addBatch(Stringsql) ：把多条 sql 语句放到一个批处理中。 •executeBatch()：向数据库发送一批 sql 语句执行。

## **5 PreparedStatement 接口**

继承自 Statement 接口，由 preparedStatement 创建，用于发送含有一个或多个参数的 SQL 语句。PreparedStatement 对象比 Statement 对象的效率更高，并且可以防止 SQL 注入，所以 我们一般都使用 PreparedStatement。

5.1常用方法

•addBatch()把当前 sql 语句加入到一个批处理中。 •execute() 执行当前 SQL，返回个 boolean 值 •executeUpdate()运行 insert/update/delete 操作，返回更新的行数。 •executeQuery() 执行当前的查询，返回一个结果集对象 •setDate(intparameterIndex,Date x)[向当前SQL语句中的指定位置绑定一个java.sql.Date](https://link.zhihu.com/?target=http%3A//%E5%90%91%E5%BD%93%E5%89%8DSQL%E8%AF%AD%E5%8F%A5%E4%B8%AD%E7%9A%84%E6%8C%87%E5%AE%9A%E4%BD%8D%E7%BD%AE%E7%BB%91%E5%AE%9A%E4%B8%80%E4%B8%AAjava.sql.Date)

值。

• setDouble(int parameterIndex, double x)向当前 SQL 语句中的指定位置绑定一个 double

值

•setFloat(intparameterIndex,floatx)向当前 SQL 语句中的指定位置绑定一个 float 值 •setInt(intparameterIndex,intx)向当前 SQL 语句中的指定位置绑定一个 int 值 •setString(intparameterIndex,Stringx)向当前 SQL 语句中的指定位置绑定一个 String 值

## **6 ResultSet 接口**

ResultSet 提供检索不同类型字段的方法。

6.1常用方法

•getString(intindex)、getString(StringcolumnName) 获得在数据库里是 varchar、char 等类型的数据对象。 •getFloat(intindex)、getFloat(StringcolumnName) 获得在数据库里是 Float 类型的数据对象。 •getDate(intindex)、getDate(StringcolumnName) 获得在数据库里是 Date 类型的数据。 •getBoolean(intindex)、getBoolean(StringcolumnName) 获得在数据库里是 Boolean 类型的数据。 •getObject(intindex)、getObject(StringcolumnName) 获取在数据库里任意类型的数据。

6.2ResultSet 对结果集进行滚动的方法

•next()：移动到下一行。 •Previous()：移动到前一行。 •absolute(introw)：移动到指定行。 •beforeFirst()：移动 resultSet 的最前面。 •afterLast() ：移动到 resultSet 的最后面。

## **7 CallableStatement 接口**

继承自 PreparedStatement 接口，由方法 prepareCall 创建，用于调用数据库的存储过程。

## **三、 JDBC 的使用**

加载数据库驱动程序 → 建立数据库连接 Connection → 创建执行 SQL 的语句 Statement→ 处理执行结果 ResultSet→ 释放资源

**1 下载数据库驱动**

1.1MySQL 驱动

[Download Connector/J](https://link.zhihu.com/?target=https%3A//dev.mysql.com/downloads/connector/j/)

1.2Oracle 驱动

数据库安装目录\oracle\product\11.2.0\dbhome_1\jdbc\lib

![img](https://pic1.zhimg.com/80/v2-ef24185dcca363660bfbc0e6b76b3038_720w.webp)

**2 创建项目添加驱动**

![img](https://pic3.zhimg.com/80/v2-155e624e2ed0492db11fbcf32d5faa5a_720w.webp)

![img](https://pic1.zhimg.com/80/v2-9e23b158a6caccea6de7249a9b1360ec_720w.webp)

## **3 通过 Statement 向表中插入数据**

3.1注册驱动

```text
Class.forName("com.mysql.jdbc.Driver");
```

3.2获取连接

```text
// 创建连接 conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/
+ mysql?useSSL=false&useUnicode=true&" +
"characterEncoding=utf-8", "root", "root");
// jdbc:mysql://连接地址:连接端口号/连接那个数据库?是否验证&开启编码&编码方式
// root登陆数据库的账户 //root登陆数据库的密码
```

3.3执行 SQL

```text
sta = conn.createStatement();//用于去提交事务的对象
String sql = "insert into usertable values(default,'"+age+"','"+userName+"','"+password+"')";
//所要执行的sql语句
	boolean flage = sta.execute(sql);//执行sql语句
```

3.4释放资源

```text
finally {
			try {
				if(sta!=null) {   //先关闭Statement
					sta.close();
				}
				if(conn!=null) {//后关闭连接
					conn.close();
				}
			} catch (SQLException e) {
				e.printStackTrace();
			}
```

更新表中的数据

```text
	public void updateUer(String userName,String passward,int age) {
		Connection conn = null;
		Statement sta = null;
		try {
			Class.forName("com.mysql.jdbc.Driver");
			conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/mysql?"
					+ "useSSL=false&useUnicode=true&" +
				"characterEncoding=utf-8", "root", "root");
			sta = conn.createStatement();
			String sql = "update usertable set name='"+userName+"',passward="
					+ "'"+passward+"',age='"+age+"'";
			sta.executeUpdate(sql);
		} catch (Exception e) {
			e.printStackTrace();
		}finally {
			try {
				if(sta!=null) {
					sta.close();
				}
				if(conn!=null) {
					conn.close();
				}
			} catch (SQLException e) {
				e.printStackTrace();
			}
		}
```

## **4 ResultSet 讲解**

注意 ResultSet 中封装的并不是我们查询到的所有的结果集，而是返回了查询到的结果 集的数据库游标。通过 ResultSet 中的 next()方法操作游标的位置获取结果集。

![img](https://pic4.zhimg.com/80/v2-0dbf1e8d2605b1f27e5a06c13ee902ff_720w.webp)

## **5 通过 ResultSet 实现逻辑分页**

```text
	sta = conn.createStatement();
	String sql = "select * from usertable";
	res = sta.executeQuery(sql);
	int begin = (curretPage - 1) * pageSize + 1;// 指向当前页的起始数据
	int end = curretPage * pageSize; // 指向当前页的最后一条数据
	int currentCount = begin; // 指向当前页的第几条数据,最开始是指向第一条的
	while (res.next()) {
	    if (currentCount >= begin && currentCount <= end) {
	System.out.println(
	     res.getInt("id") + "\t" + res.getString("name") + "\t" + res.getString("passward"));
				if (currentCount == end) {
						break;
					}
					currentCount++;
				}
			}
```

## **6 SQL 注入问题**

6.1什么是 SQL 注入

所谓 SQL 注入，就是通过把含有 SQL 语句片段的参数插入到需要执行的 SQL 语句中， 最终达到欺骗数据库服务器执行恶意操作的 SQL 命令。

6.2SQL 注入案例

```text
// SQL注入
	public void sqlInject(String name) {
		Connection conn = null;
		Statement sta = null;
		ResultSet res = null;
		try {
			
	 Class.forName("com.mysql.jdbc.Driver");
	conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/mysql?useSSL=false&
             +"useUnicode=true&characterEncoding=utf-8", "root", "root");
			sta = conn.createStatement();
			String sql = "select * from usertable where name='" + name + "'";
			res = sta.executeQuery(sql);
			while (res.next()) {
			System.out.println(res.getString("name") + "\t" + res.getString("id"));
			}
		} catch (SQLException e) {
			e.printStackTrace();
		}
	}

	public static void main(String[] args) {
		JDBCTest test = new JDBCTest();
	test.sqlInject("李四' or 1=1 -- ");// --空格在数据库中表示注释,会查出数据库中的所有数据
	}
```

## **7. PreparedStatement 对象的使用(重点)**

**7.1 PreparedStatement 特点：**

**•PreparedStatement 接口继承 Statement 接口**

**•PreparedStatement 效率高于 Statement**

**•PreparedStatement 支持动态绑定参数**

**•PreparedStatement 具备 SQL 语句预编译能力**

**• 使用 PreparedStatement 可防止出现 SQL 注入问题**

**7.2通过 PreparedStatement 对象向表中插入数据**

**代码**

```text
public void insert(int age, String name, String password) {
		Connection conn = null;
		PreparedStatement ps = null;

           Class.forName("com.mysql.jdbc.Driver");
	conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/mysql?useSSL=false&
             +"useUnicode=true&characterEncoding=utf-8", "root", "root");
		
          String sql = "insert into usertable values(default,?,?,?)";

              try {

			ps = conn.prepareStatement(sql);
			ps.setInt(1, age);//将第一个问号的位置设置值
			ps.setString(2, name);//将第二个问号的位置设置值
			ps.setString(3, password);//将第三个问号的位置设置值
			ps.execute();
		} catch (SQLException e) {
			e.printStackTrace();
		} 
	}
```

## **8 PreparedStatement 的预编译能力**

8.1什么是预编译

8.1.1 SQL 语句的执行步骤

• 语法和语义解析

• 优化 sql 语句，制定执行计划

• 执行并返回结果

但是很多情况，我们的一条 sql 语句可能会反复执行，或者每次执行的时候只有个别的 值不同（比如 select 的 where 子句值不同，update 的 set 子句值不同,insert 的 values 值不同）。 如果每次都需要经过上面的词法语义解析、语句优化、制定执行计划等，则效率就明显不行 了。 所谓预编译语句就是将这类语句中的值用占位符替代，可以视为将 sql 语句模板化或者 说参数化 预编译语句的优势在于：一次编译、多次运行，省去了解析优化等过程；此外预编译语 句能防止 sql 注入

8.1.2 解析过程

8.1.2.1 硬解析 在不开启缓存执行计划的情况下，每次 SQL 的处理都要经过：语法和语义的解析，优 化器处理 SQL，生成执行计划。整个过程我们称之为硬解析。

8.1.2.2 软解析 如果开启了缓存执行计划，数据库在处理 sql 时会先查询缓存中是否含有与当前 SQL 语句相同的执行计划，如果有则直接执行该计划。

8.2预编译方式

开始数据库的日志 showVARIABLESlike '%general_log%' setGLOBALgeneral_log=on setGLOBALlog_output='table'

8.2.1 依赖数据库驱动完成预编译

如果我们没有开启数据库服务端编译，那么默认的是使用数据库驱动完成 SQL 的预编 译处理。

8.2.2 依赖数据库服务器完成预编译

我们可以通过修改连接数据库的 URL 信息，添加 useServerPrepStmts=true 信息开启服 务端预编译。

## **9 PreparedStatement 批处理操作**

**代码**

```text
	String sql = "insert into usertable values(default,?,?,?)";
			ps = conn.prepareStatement(sql);
			for (User user : list) {
				ps.setInt(1, user.getAge());
				ps.setString(2, user.getName());
				ps.setString(3, user.getPassword());
				ps.addBatch();
			}
                            ps.executeBatch();//提交
```

## **10 JDBC 中的事务处理**

在 JDBC 操作中数据库事务默认为自动提交。如果事务需要修改为手动提交，那么我们 需要使用 Connection 对象中的 setAutoCommit 方法来关闭事务自动提交。然后通过 Connection 对象中的 commit 方法与 rollback 方法进行事务的提交与回滚。

代码

```text
String sql = "delete from usertable where name like ?";
			conn.setAutoCommit(false);//将事务设置为手动提交，默认是自动提交
			ps = conn.prepareStatement(sql);
			ps.setString(1,"%"+name+"%");
			ps.execute();
			conn.commit();//提交事务
```

## **四、 JDBC 进阶**

## **1 动态查询**

动态删除：根据用户给定的条件来决定执行什么样的查询。

```java
	//动态删除
	public void dynamicDelete(User user) {
		Connection conn = null;
		PreparedStatement ps = null;
	conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/mysql?useSSL=false&
             +"useUnicode=true&characterEncoding=utf-8", "root", "root");
		String sql = connectSQL(user);
		try {
			conn.prepareStatement(sql);
		} catch (SQLException e) {
			e.printStackTrace();
		}
	}
	public String connectSQL(User user) {
		StringBuilder sb = new StringBuilder("delete from usertable where 1=1 ");
		if(user.getAge()>0) {
			sb.append(" and age = ").append(user.getAge());
		}
		if (user.getName()!=null) {
			sb.append(" add name = '").append(user.getName()).append("'");
		}
		if (user.getPassword()!=null) {
		sb.append(" and password = '").append(user.getPassword()).append("'");	
		}
		return sb.toString();
	}
```