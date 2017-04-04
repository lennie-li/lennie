@(node.js)[javascript, node.js]

# Node.js-05-express(路由、中间件)

```
const express = require('express');
const app = express();

//监听指定路径的请求
//get post put delete 
app.get(url, (req, res) => {
	//req.query 获取请求参数
	console.log(req.query);
	res.send('这是请求地址' + url );
});
app.post(url, (req, res) => {
	res.send('这是请求地址' + url );
});

//任意方式的请求都监听
//app.all(url,(req,res) => {});
```

```
//use 方法监听所有的请求
app.use((req, res) => {
	//send end 都可以返回数据
	//res.send(req.url);
	res.end(req.url);
});
app.listen(3000, ip, () => {
	console.log(running);	
});
```

```
//设置静态资源服务器，所有www下的静态资源都可以直接访问
app.use(express.static('www'));
app.listen(3000, () => {
	console.log('running...')
});
```

- 除此以外，监听的url还可以使用类似正则的方式

	- /abcd   /acd `app.get('/ab?cd', (req, res) => {})`

- /abcd   /abbcd /abbbcd `app.get('/ab+cd', (req, res) => {})`

- /acd   /abcd /abbcd `app.get('/ab*cd', (req, res) => {})`

- /abcd   /ad `app.get('/a(bc)?d', (req, res) => {})`


```
app.route('/book')
	.get((req, res) => {
		res.send('get')
	});
	.post((req, res) => {
		res.send('get')
	});
```






- 路由模块模式，将路由抽离出来

//router.js

```
const express = require('express');
const router = express.Router();

router.get('/test', (req, res) => {
	res.send('get');
});
router.post('/test', (req, res) => {
	res.send('post');
});
router.put('/test', (req, res) => {
	res.send('put');
});
router.delete('/test', (req, res) => {
	res.send('delete');
});

module.exports = router;	
```

//index.js

```
const express = require('express');
const router = require('./router.js');
const app = express();

app.use(express.static('www/'));
//这里就是在路径前又增加了一个虚拟路径，访问时，需要添加一个/admin 即可访问
//app.use('admin',router);
app.use(router);
app.listen(3000, () => {
	console.log('running...');
});
```


### 中间件

- 各类方法中的回调函数如下，其中的`(req, res, next) => {}`就是中间件，可以理解为：从请求开始到请求结束，我们处理的一些事情。

```
app.use((req, res, next) => {
	...
	next();
});
```
- 客户端的每次请求，服务器都会把相应的js文件从头到尾执行一边，要调用多个中间件的话，就需要通过`next()`来调用，每个中间件的逻辑执行完成后，使用`next()`来调用下一个中间件的代码。


```
//中间件
const express = require('express');
const app = express();
//有一个/user的请求，代码执行流程就是从上至下依次执行，调用了三次中间件，如果不使用next()的话，那么下一个中间件就不会执行了，因此服务器也无法给浏览器返回数据。而浏览器就会一直等待服务器响应。
app.use((req, res, next) => {
	console.log(new Data());
	next();
});
app.use((req, res, next) => {
	console.log('1');
	next();
});
app.get('/user', (req, res, next) => {
	res.send('result');
});
app.listen(3000, () => {
    console.log('running....');
});
```

- 另一种写法，更加清晰，每次get请求经过了三个中间件，按数组中的顺序一次执行。
```
let cb0 = (req, res, next) => {
	console.log(new Date());
	next();
};
let cb1 = (req, res, next) => {
	console.log('1');
	next();
};
let cb2 = (req, res, next) => {
	console.log('end');
	res.send('result');
};
app.get('/user', [cb0, cb1, cb2]);
app.listen(3000, () => {
    console.log('running....');
});
```

### body-parser 插件
- 获取post请求的参数
- 处理默认表单的提交数据 `app.use(bodyParser.urlencoded({extended: false}))`
- 处理json形式数据 `app.use(bodyParser.json())`

```
//引入express
const express = require('express');
//引入body-parser
const bodyParser = require('body-parser');

//用于获取默认表单提交格式的数据
app.use(bodyParser.urlencoded({extended : false}));
//用于获取json形式的数据
//ajax以post方式请求，请求参数为json格式时，需要在ajax请求中设置参数 contentType : 'application/json;charset=utf8;' 具体查一下，可能存在误差??
app.use(bodyParser.json());

//上面两个方法会自动将获取到的数据保存到req.body中去。
app.post('/user', (req, res) => {
	//通过req.body来获取post参数
	console.log(req.body);
});
app.listen(3000, () => {
	console.log('running...')
})
```


### 模版引擎

1. 后端模版引擎 EJS  jade(更名为pug)  smarty（php）
2. artTemplate
3. 使用方法与artTemplate基本完全相同 双大括号 => <%= %> <%- %> <% %>

- express 原生支持的模版引擎 配置：

```
const express = require('express');
const app = express();
const template = requirejs('ejs');

//指定模版存放路径
app.set('views', path.join(__dirname, './views'));
//指定模版文件后缀
app.set('view engine', 'ejs');
    
```

- 渲染

```
(req, res) => {
	//将data作为数据，将指定模版存放文件夹内的index.ejs作为模版，渲染并返回。
	res.render('index', data);
}
```



### express-generator 应用生成器


`npm install express-generator -g`

在当前工作目录下创建一个命名为 myapp 的应用

`express myapp`

	Options:
	
	    -h, --help          output usage information
	    -V, --version       output the version number
	    -e, --ejs           add ejs engine support (defaults to jade)
	        --hbs           add handlebars engine support
	    -H, --hogan         add hogan.js engine support
	    -c, --css <engine>  add stylesheet <engine> support (less|stylus|compass|sass) (defaults to plain css)
	        --git           add .gitignore
	    -f, --force         force on non-empty directory



然后安装所有依赖包：

 `cd myapp `
 `npm install`

Windows 平台使用如下命令：

`> set DEBUG=myapp & npm start`

然后在浏览器中打开 http://localhost:3000/ 网址就可以看到这个应用了