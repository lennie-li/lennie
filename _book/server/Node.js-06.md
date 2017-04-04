@(node.js)[node.js, mysql]
# Node.js-06-mySQL

### 简介

- 关系型数据库：类似于excel表格，帐号密码等，有严格的行、列关系
	- mysql =》 mariadb
	- oracle
	- db2
	- sqlserver
	- ...
- 非关系型数据库NoSQL（）：关系性较低，可能是以文件或字符串或键值对等的形式储存。没有严格的行、列关系
	- redis
	- mongodb
	- Hbase
	- ...



- 数据库 => 表 => 数据

- 建表，必须设置主键，类似于身份证，唯一标识。
- navicate 数据库图形化工具


### 基本语法

- 增：
	- `insert into tablename(key1,key2,key3,key4) values(value1,value2,value3,value4)`
	- `insert into books(name,author,description,category) values ('a','b','c','d')`

- 改：
	- `update tablename set key = value,key = value WHERE key = value and key = value` 
	- `update books set name = 'aaa', category='bbb', author='ccc', description='ddd' where id=1`
	- `WHERE` 后跟键值对，指定条件，不指定条件会将所有数据修改，危险。

- 删：
	- `delete tablename WHERE key = value`
	- `delete from books where id=2 and name = '白说'`
- 查：
	- 基本
		- `select key,key form tablename WHERE key = value`
		- `select name as '白夜行' from books where id = 3`
	- 分页查询 
		- `limit` 参数1：开始查询的位置（0开始），参数2：每次查询多少条数据
		- `select * from tablename limit num1,num2`
	- 多表查询
		- `select * from tablename1,tablename2 where tablename1.key = tablename2.key`？




### nodejs操作数据库

1. mysql模块`npm install mysql --save`
2. 配置连接`let connection = mysql.createConnection({...})`
3. 开始连接`connection.connect()`
4. 执行数据库操作`connection.query(sql,data,callback)`
5. 结束`connection.end()`


```
const mysql = require('mysql');

//创建连接
let connection = mysql.createConnection({
    hots : 'localhost',
    user : 'root',
    password : '',
    database : 'book_list'
});

//开始连接
connection.connect();

//设置sql语句，?为占位符，data中的每一项会依次赋值至每个?处。
let sql = 'select * from books where id = ? and name = ?';
//设置参数
let data = [10, 'abcd'];

//执行数据库操作
connection.query(sql, data, (err, results, fileds) => {
    if (err) throw err;
    //返回值
    console.log(results);
});
//关闭数据库，结束
connection.end();
```





















- 如果存在数据库School，则删除。否则创建数据库

		drop database if exists `School`;

- 创建数据库

		create database `School`;
		use `School`;- #如果存在数据表，则删除，否则创建
		drop table if exists `tb_class`;


- 创建一个学生班级表：班级id（主键，自增），班级名称。

		create table `tb_class`
		(
		`id` int(11) not null AUTO_INCREMENT primary key ,
		`Name` varchar(32) not null
		
		);
		Drop table if  exists tb_student;

- 创建一个学生信息表：学生id（自增，主键），姓名，年龄，性别，入学时间，所属班级id（外键）。

		create table `tb_student`
				(
		 `id` int(11) not null auto_increment primary key,
		 `Name` varchar(32) not null,
		 `Age` int default 0
		 `gender` boolean default 0
		 `date` datetime
		);

- 创建一个学生成绩表：成绩id（自增，主键），科目，成绩，学生id（外键），创建时间。

		drop table if exists `tb_score`;
		create table `tb_score`
		(`id` int(11) not null AUTO_INCREMENT PRIMARY key,
		`course` varchar(32) not null,
		`Score` float(3,1) not null,
		`stuId` int(11) not null 
		);
