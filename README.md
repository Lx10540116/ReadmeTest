# ReadmeTest
这是一个学习ReadMe文件写法的项目

## 总体说明
> 本项目为网易云课堂java web微专业数据库开发的课件部分.

本项目一共包含五个部分，分别为JDBC、数据库连接池、SQL注入与防范、事务以及MyBatis.

## 目录

* [1. JDBC](#1-JDBC)
	* [1.1 JDBC重要意义](#11-JDBC重要意义)
	* [1.2 JDBC优势](#12-JDBC优势)
	* [1.3 JDBC体系架构](#13-JDBC体系架构)
	* [1.4 JDBC安装](#14-JDBC安装)
	* []()
* [2. 数据库连接池](#2-数据库连接池)
* [3. SQL注入与防范](#3-SQL注入与防范)
* [4. 事务](#4-事务)
* [5. MyBatis](#5-MyBatis)

## 1. JDBC

## 1.1 JDBC重要意义
&nbsp;&nbsp;&nbsp;&nbsp;应用程序通过调用统一接口可以访问任意数据库。JDBC屏蔽了客户端与服务器端交互协议的实现细节，只要能熟练的使用JDBC提供的标准接口，无需关心底层数据库的实现方式。<br/>
&nbsp;&nbsp;&nbsp;&nbsp;对于Java应用程序，JDBC就是一个普通的架包，在应用程序中引用架包提供的类和方法，通过操作Java对象的方式，就可以获取数据库中的数据。<br/>
&nbsp;&nbsp;&nbsp;&nbsp;对数据库厂商来说，JDBC就是一套接口规范，每个数据库厂商都必须实现JDBC定义的接口，确保用户通过JDBC正确的访问数据库。<br/>

## 1.2 JDBC优势
1. 简单。掌握一套接口就可以实现对任意数据库的访问。
2. 便捷。提高了程序开发的效率，压缩了开发的时间，让Java Web的开发变得更加快捷。
3. 移植性。有了统一的接口和规范，使得Java Web程序面向不同的数据库时，具备跨平台的可移植性。
4. 框架。JDBC仅仅提供了基本的接口和功能，满足最基本的功能需求。基于JDBC功能之上，可以定制更加强大的框架。

## 1.3 JDBC体系架构
![JDBC体系架构图](https://raw.githubusercontent.com/Lx10540116/HelloMySQL/master/img/JDBC/JDBC%E4%BD%93%E7%B3%BB%E6%9E%B6%E6%9E%84.png)

1. JDBC API层。负责与Java Web进行通信。
2. JDBC Driver API数据库驱动层。负责与数据库建立连接。一般来说，下层的Driver都是由数据库厂商来提供的，负责和实现与自己提供的数据库的通信。
	
## 1.4 JDBC安装
安装方式有两种：
1. 数据库官网下载。从数据库官网下载JDBC驱动，下载下来的是一个JAR包，然后加入到Java Web项目中。
2. Maven管理。通过Maven配置JDBC驱动。

## 2. 数据库连接池


## 3. SQL注入与防范


## 4. 事务


## 5. MyBatis

