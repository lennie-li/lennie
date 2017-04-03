@(JavaScript)[基础, 前端]

# JS运行机制

1. 引用类型赋值

		
		var a = {n : 2};    
		var b = a ; 
		a.x = a = {n : 1}; 
		console.log(a.x); 
		console.log(b.x);
		a.n = 3;
		console.log(a.x); 
		console.log(b.x);
