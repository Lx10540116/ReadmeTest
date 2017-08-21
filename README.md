# ReadmeTest
这是一个学习ReadMe文件写法的项目

# HelloMySQL
网易云课堂java web微专业数据库开发

## 云课堂昵称
偶是小菜鸟

============================

## 总体说明
> 本项目为网易云课堂java web微专业数据库开发的课件部分.

本项目一共包含五个部分，分别为JDBC、数据库连接池、SQL注入与防范、事务以及MyBatis.

目录
=================

* [1. JDBC](#1-jdbc)
	* [1.1 JDBC重要意义](#11-jdbc重要意义)
	* [1.2 JDBC优势](#12-jdbc优势)
	* [1.3 JDBC体系架构](#13-jdbc体系架构)
	* [1.4 JDBC安装](#14-jdbc安装)
	* [1.5 JDBC API](#15-jdbc-api)
		* [1.5.1 Driver & DriverManager](#151-driver--drivermanager)
		* [1.5.2 Connection](#152-connection)
		* [1.5.3 Statement](#153-statement)
		* [1.5.4 ResultSet](#154-resultset)
		* [1.5.5 SQLException](#155-sqlexception)
	* [1.6 JDBC URL](#16-jdbc-url)
		* [1.6.1 介绍](#161-介绍)
		* [1.6.2 常用JDBC URL](#162-常用jdbc-url)
	* [1.7 构建步骤](#17-构建步骤)
	* [1.8 JDBC高级功能](#18-jdbc高级功能)
		* [1.8.1 游标](#181-游标)
		* [1.8.2 流方式](#182-流方式)
		* [1.8.3 批处理](#183-批处理)
		* [1.8.4 字符集设置](#184-字符集设置)
* [2. 数据库连接池](#2-数据库连接池)
	* [2.1 连接池存在的重要原因](#21-连接池存在的重要原因)
	* [2.2 连接池](#22-连接池)
	* []()
* [3. SQL注入与防范](#3-sql注入与防范)
* [4. 事务](#4-事务)
* [5. MyBatis](#5-mybatis)

## 1. JDBC

### 1.1 JDBC重要意义
&emsp;&emsp;应用程序通过调用统一接口可以访问任意数据库。JDBC屏蔽了客户端与服务器端交互协议的实现细节，只要能熟练的使用JDBC提供的标准接口，无需关心底层数据库的实现方式。  
&emsp;&emsp;对于Java应用程序，JDBC就是一个普通的架包，在应用程序中引用架包提供的类和方法，通过操作Java对象的方式，就可以获取数据库中的数据。  
&emsp;&emsp;对数据库厂商来说，JDBC就是一套接口规范，每个数据库厂商都必须实现JDBC定义的接口，确保用户通过JDBC正确的访问数据库。

### 1.2 JDBC优势
1. 简单。掌握一套接口就可以实现对任意数据库的访问。
2. 便捷。提高了程序开发的效率，压缩了开发的时间，让Java Web的开发变得更加快捷。
3. 移植性。有了统一的接口和规范，使得Java Web程序面向不同的数据库时，具备跨平台的可移植性。
4. 框架。JDBC仅仅提供了基本的接口和功能，满足最基本的功能需求。基于JDBC功能之上，可以定制更加强大的框架。

### 1.3 JDBC体系架构
<p align="center">
<img src="/img/JDBC/JDBC体系架构.png" alt="JDBC体系架构图">
</p>

1. JDBC API层。负责与Java Web进行通信。
2. JDBC Driver API数据库驱动层。负责与数据库建立连接。一般来说，下层的Driver都是由数据库厂商来提供的，负责和实现与自己提供的数据库的通信。
	
### 1.4 JDBC安装
安装方式有两种：
1. 数据库官网下载。从数据库官网下载JDBC驱动，下载下来的是一个JAR包，然后加入到Java Web项目中。
2. Maven管理。通过Maven配置JDBC驱动。

### 1.5 JDBC API
<p align="center">
<img src="/img/JDBC/JDBC-API.png" alt="JDBC API">
</p>

#### 1.5.1 Driver & DriverManager
* Driver是一个接口，定义了各个驱动程序都必须实现的功能，是驱动程序的抽象。通过操作Driver接口即可以实现对各个驱动程序的操作。
* DriverManager是Java的管理类，用户通过Class.forName的方式就可以向DriverManager注册一个驱动程序，然后通过DriverManager的getConnection方法就可以调用该驱动程序，建立到后端数据库的物理链接。

#### 1.5.2 Connection
代表Java应用程序对后端数据库的一条物理链接，基于这条链接可以执行一些SQL语句。
* 常用方法
```Java
Statment stmt = conn.createStatement();
```

#### 1.5.3 Statement
Statement对象是一个SQL的容器，容器中可以执行诸如insert、delete、update、select也就是增删改查等操作。

容器可以承载放入的一些SQL语句：
1. 通过Statement的executeQuery方法可以执行一个数据库查询，得到数据库查询结果的一个集合，集合是以一个ResultSet对象来表示；
2. 通过Statement对象执行更新、删除语句，这时候调用execute和executeUpdate方法，它返回的是一个int值的对象，它代表的是执行的语句影响了多少条数据库记录。
* 常用方法
```Java
ResultSet rs = stmt.executeQuery("select userName form user");
```

#### 1.5.4 ResultSet
ResultSet代表了一个SQL的查询结果。关系型数据库的本质是一个二元表，所以ResultSet对象实际也是一个由行和列所组成的二元表。

ResultSet对象的内部存在一个指针，用来指向当前的一个行记录，默认该指针指向第一行记录。

移动指针的方法：
* .next()		将指针指向下一行
* .previous()	将指针指向上一行
* .absolute()	将指针指向某一行
* .beforeFirst()将指针指向第一行的最开始部分。通过调用.beforeFirst().next()获取第一行记录
* .afterLast()	将指针指向最后一条记录的下一条记录

获取列结果：
* .getString(ColumnName/Index)
* .getInt(ColumnName/Index)
* .getObject(ColumnName/Index)

每个方法都有两种方式：获取列名和获取列序号（从0开始排序）。建议采用列名的方式获取结果，因为更加直观。

#### 1.5.5 SQLException
通过SQLException来表示异常。在应用程序的处理过程中，要通过捕获SQLException来进行相应的异常处理。

### 1.6 JDBC URL
#### 1.6.1 介绍
JDBC URL是后端数据库的唯一标识符，应用程序通过该标识符即可唯一的确定后端的某个数据库实例。它是由三个部分构成的：
1. 协议：这是固定和统一的，为jdbc；
2. 子协议：getConnection方法就是通过URL中子协议部分确定调用对应的驱动程序，来建立到后端数据库的物理链接。以MySQL数据库为例，就是mysql；
3. 子名称：由三个部分组成：主机、端口、数据库.

以下是一个JDBC URL（后文例子中称之为<strong>DB_URL</strong>）的示例：
<pre>
jdbc:mysql://<a href="#db_url" title="IP"><strong>10.164.172.20</strong></a>:<a href="#db_url" title="端口"><strong>3306</strong></a>/<a href="#db_url" title="数据库"><strong>cloud_study</strong></a>
</pre>
* 协议：jdbc
* 子协议：mysql
* 子名称：10.164.172.20:3306/cloud_study
	* 主机IP：10.164.172.20
	* 端口：3306
	* 数据库：cloud_study
	
#### 1.6.2 常用JDBC URL
1. MySQL
```Java
jdbc:mysql://<ip>:<port>/database
```
2. ORACLE
```Java
jdbc:oracle:thin@<ip>:<port>/database
```
3. SQL Server
```Java
jdbc:microsoft:sqlserver://<ip>:<port>;DatabaseName=database
```

### 1.7 构建步骤
构建一个完整的Java Web程序至少应该包含以下五个步骤：
1. 装载驱动程序
2. 建立数据库连接
3. 执行SQL语句
4. 获取执行结果
5. 清理环境

[构建实例：JDBC基础-HelloJDBC](/src/main/java/com/micro/profession/jdbc/practice/HelloJDBC.java)

### 1.8 JDBC高级功能
#### 1.8.1 游标
游标提供一种客户端能够部分读取服务器端结果集的功能支持，允许分批读取SQL查询的结果。
* 如何使用游标
1. DB_URL中新增一个参数
<pre>
jdbc:mysql://&lt;ip&gt;:&lt;port&gt;/&lt;database&gt;<a href="#db_url"><strong>?useCursorFetch=true</strong></a>
</pre>

2. 使用`PreparedStatement`接口  
&emsp;&emsp;`PreparedStatement`继承自`Statement`接口，可以使用`PreparedStatement`接口来替代`Statement`接口，`PreparedStatement`接口相比`Statement`接口要求程序员在生成`PreparedStatement`的时候就要传入SQL语句，这个SQL语句是一个`参数格式化`的SQL，也就是说，SQL的`WHERE`过滤条件的参数都是通过`?`的形式来表示的，后续是通过`PreparedStatement`的`setString`和`setInt`方法来设置这些参数，然后进行执行。  
&emsp;&emsp;`PreparedStatement`有个`setFetchSize`接口，这个接口可以实现游标的功能。通过`setFetchSize`，就可以设置客户端JDBC每次从服务器端取回的记录的数量。

应用场景：查询时返回记录过多。

[构建实例：JDBC进阶-游标-HelloJDBC_cursor](/src/main/java/com/micro/profession/jdbc/practice/HelloJDBC_cursor.java)

#### 1.8.2 流方式
流方式就是将大字段的内容以二进制的方式按照区间进行划分，划分为多个区间，每次读取一个区间中的内容，在处理结束后再读取下一个区间。

应用场景：读取大字段数据。

[构建实例：JDBC进阶-流方式-LiuFang](/src/main/java/com/micro/profession/jdbc/practice/LiuFang.java)

#### 1.8.3 批处理
批处理就是通过发送一次SQL可以插入多条数据，即将多条SQL一次性进行发送。

批处理使用涉及到`Statement`的三个函数：
* addBatch()		把SQL打包成一个执行单元(Batch)
* executeBatch()	执行SQL
* clearBatch()		清空Batch中的SQL语句，准备下次执行

注：`PreparedStatement`同样可以使用这三个函数

应用场景：大量数据插入操作。

[构建实例：JDBC进阶-批处理-BatchTest](/src/main/java/com/micro/profession/jdbc/practice/BatchTest.java)

#### 1.8.4 字符集设置
1. 获取数据库编码设置
```mysql
show variables like '%character%';
```
2. 编码级别
* 实例级别：character_set_server
* 数据库级别：character_set_database
* 表级别：DEFAULT CHARSET=utf8
* 列级别：CHARACTER SET utf8
`
编码级别优先顺序：Server < Database < Table < Column
`
3. JDBC设置

`DB_URL` = `DB_URL` + `characterEncoding=utf8`

注：为了保证中文不出错，无论是数据库的还是JDBC的，建议设置为`utf8`

## 2. 数据库连接池
### 2.1 连接池存在的重要原因
1. **连接复用**
	1.  建立连接  
	通过`DriverManager`的`getConnection`方法可以建立一条Java应用程序到后端数据库的物理链接。虽然对我们来说，仅仅是一个方法的调用，但是在JDBC的数据库驱动中完成了大量客户端与服务器端的交互，这里以MySQL为例：  
	<p align="center">
	<img src="/img/JDBC/getConnection交互.png" alt="getConnection交互">
	</p>
	&emsp;&emsp;该方法首先会在客户端（也就是应用程序所在的服务器）发起一个到服务器端的TCP请求，然后服务器端随机生成一个密码种子返回给客户端，客户端利用这个密码种子和自己保存的数据库密码按照约定的加密算法可以计算得到一个加密的密码，然后再将这个加密的密码发送给MySQL服务器端进行验证，MySQL服务器端通过验证以后，返回给客户端确认该连接建立成功。一个`getConnection`方法需要四次客户端与服务器端的网络传输，由于跨机器的网络传输有较大的时间开销，所以`getConnection`也需要花费较多的时间，建立连接时间开销大。

	2. 多线程数据库访问  
	&emsp;&emsp;在实际业务场景中，当用户要访问Java程序时，会启动一个线程去处理用户的请求。在处理过程中，如果涉及到要访问后端数据库，需要创建一个`Connection`对象，建立到后端数据库的物理连接，在SQL执行完成以后，随着`Close`方法调用，数据库连接会被销毁。业务处理完成后，线程也会被释放。  
	在用户再一次发起请求时，又会被分配一个新的线程去处理。同样，涉及到数据库访问时，又会去创建一个新的数据库连接。连接执行完成之后，又被销毁。这样一来，每次用户请求，都需要创建一个数据库连接，每次都需要花费大量的时间在建立连接上，用户的响应时间会变的很慢。

	3. 连接复用  
	&emsp;&emsp;每个线程在使用数据库连接后并不是立即销毁，而是把它交给下一个需要访问数据库的线程，多个线程共用后端相同的物理连接，实现连接的复用，这就是为什么需要连接池的最重要的一个原因。
	<p align="center">
	<img src="/img/JDBC/连接复用.png" alt="连接复用">
	</p>
	&emsp;&emsp;以连接池的形式来管理数据库连接，每个需要访问数据库的线程每次从连接池中租借数据库连接，使用完毕后，归还给连接池，这样就可以实现连接的重复使用，避免了每次访问数据库都要去创建数据库连接，从创建改变为租借。
	
2. **限制连接**

&emsp;&emsp;数据库服务器端在处理数据库请求时，会在服务器端分配一定的资源，比如说内存，用来保存数据库查询的结果。在请求处理结束后，这些资源也会被释放。服务器端的资源是有限的，不能无限制的分配，当同时有多个数据库请求访问数据库的时候，服务器端能处理的连接数是有限的。当超过最大可分配的资源时，就会出现服务器宕机的故障。为了限制并发访问的连接数，数据库服务器端一般会设置最大的并发连接数。如果超过最大连接数，就会抛`too many connections`的异常。
<p align="center">
<img src="/img/JDBC/限制连接.png" alt="限制连接">
</p>
&emsp;&emsp;虽然服务器端做了必要的保护限制，但是对于应用程序，一方面服务器端直接抛SQL异常，对Java程序的处理不够友好，Java程序必须要捕获这些SQL异常进行异常处理；另一方面，不应该仅仅依靠服务器端的最大连接限制，应该在数据库访问客户端的时候，就应该实现这种限流，数据库连接必须有序、可控的被线程使用，一旦出现异常，也多了一种保护手段。
&emsp;&emsp;所以需要在客户端的Java程序中，就实现业务线程排队获取数据库连接，限制同时获得数据库连接的线程数，起到一个限流对后端数据库保护的作用。同时，连接数过多也会对后端数据库的性能造成严重的影响，因为连接数的增多，后端数据库就会存在更多的锁的冲突与检测，加大数据库服务器端的资源的消耗，所以应该保证应用程序有序、可控的获得数据库连接。

### 2.2 连接池
* 什么是连接池？  
&emsp;&emsp;连接池本质上是一组Java架包，介于Java应用程序和JDBC数据库物理连接之间，负责帮助应用程序来管理JDBC连接。通过连接池暴露的接口，业务程序可以获取数据库连接，使用完毕以后可以将数据库连接归还到连接池，供下一个线程使用。
<p align="center">
<img src="/img/JDBC/连接池.png" alt="连接池">
</p>
&emsp;&emsp;连接池对JDBC连接实行有效的管理。在连接池中，JDBC连接不足时，会自动创建连接；在空闲的连接较多时，也会自动的销毁连接。在由多个线程同时获得数据库连接时，连接池还提供了排队等待的功能，能够保证应用程序有序的获得数据库连接。

## 3. SQL注入与防范


## 4. 事务


## 5. MyBatis

