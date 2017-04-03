@(framework)[angular]


# Angular-01简介和入门

## 开放性讨论

- 为什么这几年前端行业好像突然间多了那么多东西
    + 我们现在做的网站不再是简简单单的呈现静态页面，而是一个web应用程序。
    + 一大批后端程序员转型为前端，大大提高了前端的水准。
- 静态&动态
	1. 静态页面
		- 静态页面就是一个完整的html
		- 动态页面就是一个html，页面内容由js生成
	2. 静态网站和动态网站
		- 静态网站：服务器不做任何事情，只是简单的将请求页面发送给浏览器
		- 动态网站：服务器接收用户的请求以及参数，根据参数在服务器端执行相应代码(后代代码php、java、c#等)，拼接字符串合成html文本并返回给浏览器。


## Angular简介
 - jQuery ：库 library
  + 封装了一些常用的方法，我们主动的调用这些方法
    -- 提高了代码的利用，以及代码后期的维护

 - Angular: 前端框架 framework
  + 框架提供了一些结构或者模式，
  + 我们是根据框架提供的结构或者模式去书写代码
  + 由框架帮助我们去执行相应的操作。

### 什么是 Angular

- 一款非常优秀的前端高级 JS 框架
- 最早由 Misko Hevery 等人创建
- 2009 年被 Google 公式收购，用于其多款产品
- 目前有一个全职的开发团队继续开发和维护这个库
- 有了这一类框架就可以轻松构建 SPA 应用程序
- 其核心就是通过指令扩展了 HTML，通过表达式绑定数据到 HTML。
- *Angular不推崇DOM操作，也就是说在NG中几乎找不到任何的DOM操作*
- 表达式( expression )
	- 将 运算符 与 数据(变量) 连接起来，可以计算得到结果的式子（序列）就是表达式。
	- 例如：
		- 123
		- 1+2+3
		- 1+2+3; 不是表达式，是语句（计算出来有结果，也就是说可以赋值给变量，也可以括起来使用）
	- ng 表达式：是一个类似于js代码的式子。使用在ng-xxx等指令中，或 `｛｛    ｝｝` 这类语法中。
		- 如：
			- `ng-click = ‘val’`
			- `｛｛ xxxx ｝｝`
	- ng表达式
		- 上下文不同
			- 原生js中:`var num = 123;` 等同与 `window.num = 123;`，原生js中上下文指的就是`window`对象
			- 在angular中上下文指的就是`$scope`
		- 容错
		- ng 中可以使用过滤器（filter）
		- ng中没有流程控制
		- 没有正则字面量
		- 没有函数声明
		- 不能使用new创建对象
		- 没有void运算符 没有位运算符 逗号预算符
	**注意，ng表达式中的所有名字都是$scope下的一个属性，需要使用window下的属性方法需要在controller内进行一次赋值转移**

#### 什么是 SPA
- single page application
- 单页应用程序

### 为什么使用 Angular
- 模块化组件化开发


### 安装 Angular

- 通过工具安装
  + npm 方式 `npm install angular`

- CDN - 扩展内容


### 指令
- angular中以ng-开头的属性叫作指令
- ng-app 告诉angular来管理html代码，管理ng-app所在元素及其子元素。
- ng-click 用来注册点击事件

```
  var add = document.getElementById('add');
   add.onclick=function(){
    val = (val-0)+1; // num.value = (num.value - 0) +1
   }
```

- ng-model：var num = document.getElementById('num').value

- ng-init :进行初始化操作:  `ng-init="user.name='小明'"`

### Angular 表达式
- 就是把ng-model对应的值显示到页面中。
- 语法：两个大括号的形式：`｛｛ ｝｝`
- `｛｛user.name｝｝`
- `｛｛'hello'+ user.name｝｝`
- `｛｛1+2｝｝`
- `｛｛[1,2,3,4]｝｝`


----------


## Angular 基础概念

### Angular 的核心特性

- 指令
	- `ng-app` 页面只有一个
	- `ng-init` 初始化数据
	- `ng-model` 设置value属性的值
	- `ng-controller` 设置控制器
	- `ng-watch` 监视

- MVC

- 模块化  `angular.module()`

- 双向数据绑定

### 模块(module)
- angular.module('myApp',[])
  > 第一个参数是模块的名字
  > 第二个参数是一个数组，数组的元素是该模块所依赖其他模块的名字
*注意,即使不依赖任何模块，也需要给第二个参数传递一个空数组*
*否则angular.module('myApp')就是去获取名为myApp的模块对象*

### 控制器(controller)
- angular.module('myApp',[]).controller('demoController',function($scope){})
> 第一个参数，是控制器的名字
> 第二个参数，是一个回调函数，在回调函数里写我们想要的js代码。

- `$rootScope=>$scope=>$scope`  根据controller的层级，每个层级的controller的$scope也是痛仰的嵌套关系，子集是继承自父级


### 双向数据绑定
- 数据模型的值发生改变，就会导致页面值的改变.
  页面值的改变，就会导致数据模型值的改变，这各种相互影响的关系就是双向数据绑定。
- ng-model
- 脏循环 反复循环判断文本的值与对象内的值是否完全一致

### 单向数据绑定
- 使用表达式显示数据模型的值。
- p标签这类标签中只能显示数据模型的值，无法改变数据模型的值，称之为单向数据绑定。

### MVC 思想


#### 什么是 MVC 思想

> 最传统的开发方式(BS)

- BS (Browser - Server)
- 纯静态页面
- 浏览器发出请求，服务器返回写好的html+css+js
- url:   `协议 :// 主机或IP:端口/路径?参数`
- 业务逻辑非常单一

>动态网站(在服务器端运行代码，拼接html字符串返回)

核心：就是根据请求的数据，拿到需要的东西拼接字符串返回数据。
无论是MVC还是MVVM都是将后端的技术引入到前端得到
在前端就是平时看到的，将数据生成成相应的页面效果

>WebAPI

给一个请求地址，发送ajax或跨域请求，就可以得到json数据
通过请求拿到数据，但是是在客户端实现渲染，而非服务器端进行渲染

>MVC

- M:Model 模型  :数据存储，一些业务逻辑
- V:View  视图 ：就是用来展示数据
- C:Controller 控制器: 调度业务逻辑
- 核心思想就是解耦，避免三者之间的紧耦合关系，需求的更改只需要更改部分代码。


	**demo**

	```
	//view
	<div class="box" ng-app="myApp" ng-controller="signInCtrl">
		账户<input type="text" ng-model="name"><br>
		密码<input type="text" ng-model="pwd"><br>
		确认密码<input type="text" ng-model="confirm"><br>
		同意协议<input type="checkbox" ng-model="isAgree"><br>
		<p>｛｛msg｝｝</p>
		注册<input type="button" ng-click="register()" value="注册">
	</div>
	<script>

		//model
		var myApp = angular.module('myApp' ,[]);
		myApp.controller('signInCtrl',function($scope){
			$scope.name = '';
			$scope.pwd = '';
			$scope.confirm = '';
			$scope.isAgree = false;
			$scope.msg = '';
			$scope.register = function(){
				if($scope.name.length>6){
					$scope.msg = '用户名过长';
					return ;
				}
				if($scope.pwd !== $scope.confirm){
					$scope.msg = '两次密码输入不同';
					return ;
				}
				if(!$scope.isAgree){
					$scope.msg = '请同意协议';
					return ;
				}
				var user = new User($scope.name , $scope.pwd);
				var res = user.save();
				if(res){
					$scope.msg = '注册成功';
				} else {
					$scope.msg = '账户已存在';
				}
			};


			// controller
			function User (user , pwd){
				this.userName = user ;
				this.passWord =  pwd;
			};
			User.prototype.save = function( ){
				var data = localStorage.getItem('user') || '[]';
				var users = JSON.parse(data);
				for (var i = 0; i < users.length; i++) {
					var temp = users[i];
					if(temp.userName == this.userName){
						return false ;
					}
				}
				users.push({userName:this.userName , passWord:this.passWord});
				localStorage.setItem('user' , JSON.stringify(users));
				return true ;
			};
		});
	</script>
	```

#### MVVM
- M：
- V:
- VM: ViewModel-  $scope
	- 有了双向数据绑定后，Controller的功能弱化了(控制器是练习Model与View的桥梁)
	- 核心：View 与ViewModel之间的数据交互，model与viewmodel也有一定的关系

> Model传输的数据与V显示的数据不一定相同。

### $watch
- 用于监视数据模型的变化（并且**只能监视数据模型的变化**）(`$scope.unstable` ,只能监视 unstable 这类`$scope`下的属性，而无法监视`$scope` `$location` 等这类注入的模型)
- `$scope.$watch`('数据模型名的字符串形式',function(变化后的值,变化前的值){},true)  第三个参数默认为false，意为是否深度监听，深度监听会监听每一个参数的每一个属性是否改变
- `$scope.$watch`里的回调函数会默认执行一次。

	```
	var app = angular.module('myApp',[]);
	app.controller('demoController',function($scope){
	    $scope.name='a';
	    //var a;
	    // 监视数据模型的变化
	    // 第一个参数,数据模型名的字符串形式
	    // 第二个参数，是一个回调函数：有两个参数，分别是变化后的值，和变化前的值
	    $scope.$watch('name',function(nowValue,oldValue){
	        // console.log(nowValue);
	        // console.log(oldValue);
	    })
	})
	```


## CDN - 扩展内容
>content delivery network
>内容分发网络

- 速度快，服务器较多，各地都有，本地缓存后不需要在此请求。
- 减轻了服务器自身在带宽压力。

### 相关链接

- AngularJS 1.x 官方网站
  + https://angularjs.org/
- AngularJS 2.x 官方网站
  + https://angular.io/
- Google Material Design for Angular
  + https://material.angularjs.org
- Angular UI（Angular最大的第三方社区）
  + http://angular-ui.github.io/
- AngularJS中文社区
  + http://www.angularjs.cn/
- AngularJS中文社区提供的文档（不用翻墙）
  + http://docs.angularjs.cn/api
  + http://www.apjs.net/