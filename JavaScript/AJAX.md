@(JavaScript)[前端, ajax, js]

# AJAX

### ajax代码段

> 原生ajax请求代码

- get

```html
<form>
	<input type="text" id="userName">
	<input type="password" id="passWord">
	<input type="button" id="btn" value = "登录">
</form>
```

```javascript
window.onload = function () {
	var btn =  document.getElementById("btn");
	btn.onclick = function () {
		var userName = document.getElementById("userName").value;
		var passWord = document.getElementById("passWord").value;
		// 1. 创建XMLHttpRequest对象
		var xhr = null;
		if(window.XMLHttpRequest){
			xhr = new XMLHttpRequest();
		} else {
			xhr = new ActiveXObject("Microsoft.XMLHTTP");
		}
		// 2.准备请求
		var param = "username="+username+"&password="+password
		xhr.open("get","ajax.php?"+param,true);
		// 3.请求
		xhr.send(null);
		// 4.回调函数
		xhr.onreadystatechange = function () {
			if(xhr.readyState == 4 && xhr.status == 200){
				var data = xhr.responseText;
			};
		};
	};
};
```

- post

```html
<form>
	<input type="text" id="userName">
	<input type="password" id="passWord">
	<input type="button" id="btn" value = "登录">
</form>
```

```javascript
window.onload = function () {
	var btn =  document.getElementById("btn");
	btn.onclick = function () {
		var userName = document.getElementById("userName").value;
		var passWord = document.getElementById("passWord").value;
		// 1. 创建XMLHttpRequest对象
		var xhr = null;
		if(window.XMLHttpRequest){
			xhr = new XMLHttpRequest();
		} else {
			xhr = new ActiveXObject("Microsoft.XMLHTTP");
		}
		// 2.准备请求
		var param = "username="+username+"&password="+password
		xhr.open("get","ajax.php","true");
		// 3.请求
		xhr.setRequestHeader("Content-Type","application/x-www-form-urlencoded");
		xhr.send(param);
		// 4.回调函数
		xhr.onreadystatechange = function () {
			if(xhr.readyState == 4 && xhr.status == 200){
				var data = xhr.responseText;
				var obj = data.JSON.parse(data); // json格式转字符串  JSON.stringity()
			};
		};
	};
};
```

- 状态码含义

```javascript
xhr.onreadystatechange = function(){
    switch(xhr.readyState){
        case 0 : 
            console.log(0,'未初始化....');
            break;
        case 1 : 
            console.log(1,'请求参数已准备，尚未发送请求...');
            break;
        case 2 : 
            console.log(2,'已经发送请求,尚未接收响应');
            break;
        case 3 : 
            console.log(3,'正在接受部分响应.....');
            data.innerHTML = xhr.responseText;
            break;
        case 4 : 
            console.log(4,'响应全部接受完毕');
            if ((xhr.status >= 200 && xhr.status < 300) || xhr.status == 304) {
                document.write(xhr.responseText);
            }else{
                document.write('error:' + xhr.status);
            }
            break;
    }
}
xhr.open('get','/products/getProduct?id=1');
xhr.send(null);
```

- jQuery

```javascript
$.ajax({
	url:'www.xx.com',
	data:{a:1,b:2},
	type:'get',  //'post'.....  返回数据为jsonp时，只能使用get方式请求   
	dataType:"json",  //"text","jsonp","xml".... 
	success：function(data){
		console.log(data);
	}
});
```

- jsonp	
	- 以上ajax请求不支持跨域请求
	- 需要异步跨域请求数据时，需要使用jsonp方式。动态添加一个script标签，并设定script.src为请求的地址。再将该script标签插入到head标签中。
	- 该方式返回的数据为函数的调用，传递参数时需要设定。
	- `<script src="www.xxx.php?xxxxx"></script> `  写死src跨域请求（静态），请求方式为同步的


	- 仿jQuery封装jsonp
```javascript
function ajax(obj){
    // jsonp仅仅支持get请求
    var defaults = {
        url : '#',
        dataType : 'jsonp',
        jsonp : 'callback',
        data : {},
        success:function(data){console.log(data);}
    };

    for(var key in obj){
        defaults[key] = obj[key];
    }
    // 这里是默认的回调函数名称
    // expando: "jQuery" + ( version + Math.random() ).replace( /\D/g, "" ),
    var cbName = 'jQuery' + ('1.11.1' + Math.random()).replace(/\D/g,"") + '_' + (new Date().getTime());
    if(defaults.jsonpCallback){
        cbName = defaults.jsonpCallback;
    }

    // 这里就是回调函数，调用方式：服务器响应内容来调用
    // 向window对象中添加了一个方法，方法名称是变量cbName的值
    window[cbName] = function(data){
        defaults.success(data);//这里success的data是实参
    };

    var param = '';
    for(var attr in defaults.data){
        param += attr + '=' + defaults.data[attr] + '&';
    }
    if(param){
        param = param.substring(0,param.length-1);
        param = '&' + param;
    }
    var script = document.createElement('script');
    script.src = defaults.url + '?' + defaults.jsonp + '=' + cbName + param;
    var head = document.getElementsByTagName('head')[0];
    head.appendChild(script);

    // abc({"username":"zhangsan","password":"123"})
}
```











