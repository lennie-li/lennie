@(node.js)[javascript, node.js, 服务器]

# Node.js-03-ES6语法&path、fs模块


### 语法
- 新的变量声明方式
	- `let` 、`const` 无预解析，不可重名，块级作用域。
	- 除此以外：`const`必须在声明时进行赋值，且不能改变（常量）

- 解构赋值

	```
	let { name:str=1, age } = { name: 'zhangsan', age: 18 };
	console.log(str, age);
	let [a, b] = [1, [1, 2]];
	console.log(a, b)
	```

- ``模版语法 字符串扩展

	```
	let data ={
		name:'lisi',
		age:18,
		gender:'nan',
		id:'d13a0b'
	}
	var str = `
		<div>${data.name}</div>
		<div>${data.age}</div>
		<div>${data.gender}</div>
		<input type="text" value="${data.id}"/>
	`;
	console.log(str);
	```



- `...data`
	- 函数声明时使用，将传入的参数变成一个数组，类似于sum.arguments，可以直接使用
		
		```
		function sum (...data) {
			let result = 0;
			data.forEach(function (v, i) {
				result += v;
			});
			return result;
		}
		console.log(sum(1,2,3,4,5,6))
		```

	- 函数调用时使用，可以将传入的数组转换为单个的实参来使用。

		```
		var arr = [1,2,3];
		function abc (a,b,c) {
			return a + b + c;
		}
		console.log(abc(...arr));
		```

	- 数组的扩展
		
		```
		let arr1 = [1,2,3];
		let arr2 = [4,5,6];
		let resArr = [...arr1,...arr2];
		console.log(resArr);
		```

- 箭头函数

	1. `this` 不再指向调用他的对象，而是指向声明时的对象
	2. 没有`arguments`属性
	3. 不能`new`，也就是不能当作构造函数，此时需要用到**类**，见下。

	4. 示例：
		- 单个参数，单个语句。此时 `()` `{}`均可省略 ，后部只能是表达式，不能写语句（不能加分号），默认return后部的表达式
		```
		let foo = a => a; //let foo = function(a){return a;};
		console.log(foo(1));
		var getSum = (...data) => {
			console.log(this);
			let res = 0;
			data.forEach((v, i) => res += v);
			return res;
		};
		console.log(getSum(1,2,3,4,5,6));
		```

- 类（本质上就是构造函数）
	- 语法：使用`class`关键字，`static`静态方法只能使用类名来调用。
	```
	class Person {
		//静态方法
		//静态方法只能使用类名调用，不能通过实例来调用
		static showInfo(){
			console.log('Person')
		}
		//构造函数
		constructor (name,gender){
			this.name = name;
			this.gender = gender;
		}	
		//方法
		showName(){
			...
		}
		showGender(){
			...
		}
	}
	```

	- 继承
		- 使用`extends`来继承父类的`constructor`及方法。
		- 使用`super`调用父类构造函数。
	```
	class Student extends Person {
		constructor(name, gende, score){
			//super：调用父类的构造函数，必须的，否则后面不能和使用this
			super(name, gender);
			this.score = score; 
		}
	}
	```


### 核心模块：path & fs
- 核心模块中，通常每种方法均包含**同步**和**异步**两种方法，通常使用异步方法，避免阻塞。
- 核心模块 `path`(路径相关) `fs`(file system 文件操作相关)
	- 依赖nodejs核心模块，依赖时，直接使用字符串即可。

		```
		const path = require('path');
		const fs = require('fs'); //file system
		```

### API

##### 基本API
- 异步方式查看文件/文件夹的信息
	- `let ret = fs.statSync(path)` 这是同步的方式，没有回调函数，通过返回值查看信息。
	- `fs.stat(path,callback(报错信息,文件信息){});`
	- 读取文件比较耗时，通常使用异步的方式

		```
		fs.stat(path.join(__dirname, '123.js'), (err, stats) => {
		    console.log(err); //第一个参数为报错信息，如路径错误。
				if(!err){
					console.log('文件存在！')
				} else ｛
					console.log('文件不存在')
				｝
		    console.log(stats);//第二个参数是文件的信息
		    console.log('访问时间' + stats.atime);
		    console.log('内容修改时间' + stats.mtime);
		    console.log('文件状态修改时间' + stats.ctime);
		    console.log('创建时间' + stats.birthtime);
		    console.log(stats.isFile());
		    console.log(stats.isDirectory());
		    console.log(2);
		})
		console.log(3); //因为是异步的形式，所有现dayin 3，再打印2.
		```

- `exists()` 判断文件存在性，官方不推荐使用
- `fs.access(path[,mode],callback)`
	- 参数：`mode` 改变文件可读可写等，躲在linux系统中使用，windows存在问题，不推荐使用
	- 不推荐使用，直接使用open方法即可，了解

		```
		fs.access(path.join(__dirname,'1.js'), (err) => {
			console.log(err);
		});
		```

- 打开并读取
	- `open(path,flag[,mode],callback)`
	- `flag` 标识符，打开文件的方式，r===>read w===>write 具体可选参数见文档
	- 同步方法返回值为fd

		```
		fs.open(path.join(__dirname,'data.txt'),'r',(err, fd) => {
			//fd file discriptor 文件描述符，通过fd可以操作文件，默认从3开始，打开多次接着累加
			//fd 就类似于timer = setInterval()中的timer，指向文件。
			console.log(err, fd);
			//        fd  空间    起始位  长度    null
			//fs.read(fd, buffer, offset, length, positon, callback)
			//把从fd读取出来的数据中 从第offset个开始，长length的数据储存到buffer中
			let buf = Buffer.alloc(5);
			fs.read(fd, buf, 0, 3, null, (err,bytes,buffer) => {
				//err报错信息，
				console.log(err,bytes,buffer);
				console.log(buf);
				//buf 与 buffer 指向同一对象
				console.log(buf == buffer);
			});
		});
		```


- 打开并写入 
	- write1

		```
		fs.open(path.join(__dirname,'data.txt'),'w',(err, fd) => {
			//fd file discriptor 文件描述符，通过fd可以操作文件，默认从3开始，打开多次接着累加
			//fd 就类似于timer = setInterval()中的timer，指向文件。
			console.log(err, fd);
			//        fd  空间    起始位  长度    
			//fs.write(fd, buffer, offset, length[, positon], callback)
			//把buf中的数据 从第offset个开始，长length的数据储存到fd中去
			let buf = Buffer.from('lennie');
			//written指的字节数量，不是字符数量！
			//一个中文 3个字节 utf-8
			fs.write(fd, buf, 1, 3, (err,written,buffer) => {
				//err报错信息，
				console.log(err,written,buffer);
				console.log(buf);
				//buf 与 buffer 指向同一对象
				console.log(buf == buffer);
			});
		});
	```

	- write2：**方法重载**  方法名相同，但参数个数不同，参数类型不同

		```
		fs.open(path.join(__dirname,'data.txt'),'w',(err, fd) => {
			//直接将字符串写入fd指向的文件中去，written表示字符串所占的字节数
			fs.write(fd, '程序员', (err,written,buffer) => {
				console.log(err,written,buffer);
			});
		});
		```



##### 简便方法，常用的操作，可以看作是对以上方法的合并封装。

- 读取文件：`fs.readFile(file[,options],callback)`
	- 同步方式没有`callback`，返回buffer 形式 data，添加编码格式返回字符串data
	- 参数：`file` 文件路径；`options` 选项：编码格式，默认为空；`callback(err,data){}`，`err` 报错信息 ，`data`返回读取到的数据，默认为`buffer`类型
		
		```
		fs.readFile(path.join(__dirname, 'data.txt'), (err, data) => {
			console.log(data);
			console.log(data.toString());
		});
		fs.readFile(path.join(__dirname, 'data.txt'), 'utf8',(err, data) => {
			console.log(data);
			// console.log(data.toString());
		});
		```


- 写入文件：`fs.writeFile(file, data[,options],callback)`
	- 形式与readFile基本完全相同，多一个data参数，需要写入什么
	- `fs.writeFile(path.join(__dirname, 'data.txt'), 'abcdeft', err => console.log(err));`

- demo： 把 data 中的文件的读取出来并放入 data2 中

	```
	fs.readFile(path.join(__dirname,'data.txt'),'utf8',(err,data) => {
		fs.writeFile(path.join(__dirname,'data2.txt'),data,(err) => {
	
		});
	});
	```


- 追加内容：`fs.appendFile(file, data[,options],callback)`
	- `fs.appendFile(path.join(__dirname, 'data2.txt'),'lennie', err => console.log(err));`

- 删除 `fs.unlink(path, callback)`
	- 只能删除文件或者链接(快捷方式，默认后缀为lnk，必须添加后缀，否则认为是文件夹，无法删除，报错)，不能删除目录
	- `fs.unlink(path.join(__dirname,'to-remove.txt'),err => console.log(err));`


- 创建目录：判断路径是否存在，不存在就创建
	- `fs.mkdir(path.join(__dirname,'newDir'), err => console.log(err));`

- 读取目录
	- `fs.readdir(path[,option],callback`
	- `option` 编码格式 默认 utf8 
	- `fs.readdirSync`同步方式没有`callback`，返回值为files

		```
		fs.readdir(__dirname,(err,files) => {
			//file 为数组形式，数组内保存文件夹下的文件及文件夹
			console.log(files);
		});
		```


- 删除目录
	- `rmdir(path,callback)`
	- `fs.rmdir(path.join(__dirname,'newDir/'), err => console.log(err));`

- 文件监视
	- `fs.watchFile(fileName[, options], listener)`
	- `fileName` 文件名 
	- `options`(persistent = true , interval = 5007);
		- 默认`persistent=true`,保持监控 ；`interval`每隔一定时间检查一次，默认5007ms
	- `listener`有两个参数： `curr` 当前状态， `pre` 变化之前的状态

		```
		fs.watchFile(path.join(__dirname, 'data.txt'), {interval : 100}, (curr, pre) => {
			console.log(curr);
			console.log(pre);
		});
		```

- 文件监视解绑 
	- `fs.unwatchFile(filename[, listener])`
	- 指定文件的所有监听函数都解绑  ，指定`listener`监听函数名，可解绑指定的摸一个监听函数。这种方法需要在`watchFile`的方法中，绑定监听函数时使用变量代替函数。具体如下：

		```
		let cb = (curr, pre) => {
			console.log(curr);
			console.log(pre);
		};
		fs.watchFile(path.join(__dirname, 'data.txt'), {interval : 100}, cb);
		setTimeout(function () {
			fs.unwatchFile(path.join(__dirname, 'data.txt'),cb);
		},5000);
		```


- 监听目录
	- fs.watch(filename[, option][, listener])
	- 不同系统的文件修改创建等操作的底层实现不太相同，修改文件的表现效果不太相同。可以使用第三方的包`chokidar`来解决不同系统的兼容性问题，使其表现一致。
	- 使用`chokidar`包来代替原生的`watch`方法;


- 大型文件的读取：文件的流式操作
	- 文件的读取复制工作需要占用内存，大型文件直接通过`readFile`和`writeFile`来执行会占用大量内存，需要使用流式操作来完成。
	- `creatReadStream(path[, option])  creatWriteStream(path[, option])`
	- 路径中：`\`为转义符  需要进行转义 故使用`\\`

		```
		//将路径用变量保存
		let sourcePath = 'D:\\study\\lesson\\17-nodejs-day3（ES6）\\exec\\1.js';
		let targetPath = 'D:\\study\\lesson\\17-nodejs-day3（ES6）\\exec\\1-copy.js';
		//获取读写流 stream
		let readStream = fs.createReadStream(sourcePath);
		let writeStream = fs.createWriteStream(targetPath);
		//检查执行了多少次 data 事件后完成了复制的过程
		let num = 1;
		//基于事件的回调函数
		//data事件，readStream每拿到一次数据就触发
		//chunk [tʃʌŋk] 厚块，大块
		readStream.on('data', (chunk) => {
			num++;
			writeStream.write(chunk);
		});
		//数据读取完毕后，触发end事件
		readStream.on('end', () => {
			//文件较小，2次就完成了复制的工作，大型文件可能需要成千上万次。
			console.log('over' + num);//2
		});
		```

	- 数据流读写优化：上述方法读写速度不一定匹配
		
		```
		let sourcePath = 'D:\\传智播客\\就业班\\课程\\17-nodejs-第03天（ES6）\\exec\\1.js';
		let targetPath = 'D:\\传智播客\\就业班\\课程\\17-nodejs-第03天（ES6）\\exec\\1-copy.js';
		let readStream = fs.createReadStream(sourcePath);
		let writeStream = fs.createWriteStream(targetPath);
		//将read和write连接起来，完成边读边写的功能
		readStream.pipe(writeStream);
		```

	- 上述进一步简写为：`fs.createReadStream(sourcePath).pipe(fs.createWriteStream(targetPath));`


- 命令行操作模块 `readline`
	- 命令行相关的操作
	- const readline = require('readline');













