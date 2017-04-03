@(CSS3)[CSS3, CSS, 前端]

# CSS3

### 属性选择器

```css
/* 获取到 拥有 该属性的元素 */
	li[type]{
		border: 1px solid gray;
	}
/* 获取 属性等于某个值的 元素 属性值 可以使用 引号进行包裹 */
	li[type="vegetable"]{
		background-color: green;
	}
/* 使用空格分隔的 多个属性 其中有某个属性即可获取 */
	li[type~="hot"]{
		font-size: 40px;
	}
/* 获取以某个属性开头的语法  */
	li[color^='green']{
		background-color: orange;
	}
/* 获取以某个值 结尾的属性 */
	li[type$='t']{
		color: hotpink;
		font-weight: 900;
	}

/* 获取 属性中 拥有某个值的 元素 */
	li[type*=ea]{
		font-size: 100px;
	}
/*  如果属性的值 只有very 也能够获取  用来获取 多个属性 并且 使用-连接 */
	li[price|='very']{
		background-color: darkred;
	}
```


### 伪类选择器

```css
/* :empty 选中空元素  :target 选中锚点指向的元素 */
.class : first-child {

}
.class : first-of-type {

}
element:nth-child(1){

}
element:nth-child(2n+1){

}

//使用此类伪类选择器时，作用原理是先找到该元素的父级，然后在父级内部找到名为element的元素，所以element必须有父级，否则无法生效（body不算父级元素）
```

### 结构选择器


```css
div + div {
/* 选中目标元素（div）后面的下一个元素（div） */
}

div~div{
/* 选中目标元素（div）后面的同级元素（div） */
}
```

### 伪元素选择器


```css
/* 伪元素 选择器 是用的是 :: 两个 冒号分隔;
早期的写法是 一个冒号 浏览器 也能够识别,但是不建议这样写
*/
/*::before ::fter*/
/* p::first-letter{ */
p::first-letter{
	font-size: 20px;
	color: hotpink;
	float: left;
}

/* 首行特殊样式 */
p::first-line{
	color: skyblue;
}
/* 只能够设置 color background-color text-shadow
	文本选中时的样式
 */
p::selection{
	/* font-size: 50px; */
	color: orange;
	background-color: hotpink;
}
/* 设置 站位提示符的 字体效果 但是兼容性 奇差无比*/
input::-webkit-input-placeholder{
	color: orange;
}
```


### 弹性布局 

- 父盒子设置属性
 	- 开启弹性布局：`display:flex;`
	- 默认 主轴x轴（justify-content），副轴y轴（align-items）
		- 设置主轴方向：
		
```css
flex-direction：row；//column 默认为row  改变主轴方向后，justify-content还是设置主轴的排布
flex-direction：row-reverse；//column-reverse 改变主轴的方向 变为反向
```


	-  开启换行：
	
```css
flex-wrap：wrap;
//开启换行后，子元素变为多行后，align-item就不可用了，需要使用align-content代替，使用方法与justify-content完全相同
//**注意**：若开启换行后，子元素内容不够多，无法换行，那么align-content无法正常使用
```
		
	- 设置排布方式
	
```css
justify-content：space-around；//space-between,center,flex-start,flex-end
align-items：center；//flex-start,flex-end
```

- 子元素可单独设置属性
	
```css
align-self：flex-start；//center,flex-end
```

### transform&transition

```css
color:transparent;//全透明的颜色
transform:translateX(20px) translateY(20px); //移动
transform:translate(20px , 20px);
transform:scale(0.5 , 0.5 );//缩放效果，用法同上
transform:rotate(45deg);//旋转角度 平面旋转
transform:rotateX(45deg);//沿X轴进行旋转  3D旋转
transform:skew(20deg,20deg)//扭曲
transform-style:preserve-3d; //想看到进行了3D变换的元素的3D效果，需要给3D变换的元素的父元素添加该属性。
transition:width 1s 1s linear; 需要过度的类型 变换时间 延迟时间 过度的速度变化类型（linear ease ease-in ease-out）
transition:all 1s;//变换类型的过度 变换时间
transform-origin:left top	;//选择变换基准点
perspective:500px;//透视 元素距离显示器窗口的距离 距离越近 透视效果（近大远小）越明显//只能看到2D效果
backface-visibility:hidden;
```

### animate

```css
@keyframes animationName{
	from{
		transform:translateX(10px) rotate(45deg);
	}
	to{}
}
@keyframes animationName{
	0%{}
	50%{}
	100%{}
}

.animation{
	animation : animationName 2s 2s infinite linear/steps(60);
}
```


`background-size:cover/contain;`


### 渐变 

```css
.div1{
	width: 200px;
	height: 200px;
	/*   方向
		从哪个颜色 渐变到 哪个颜色
		to 方向 具体的方向值
		颜色1,
		颜色2
		颜色之后 可以跟上 具体的 位置 百分比的方式
	  */
	/* background-image: linear-gradient(to left,red ,skyblue 20%,yellow); */
	/* background-image: linear-gradient(45deg,red ,skyblue 20%,yellow); */
	background: linear-gradient(left , rgb(6, 73, 234) 29% , rgb(231, 150, 14) 84%);
	background: -o-linear-gradient(left , rgb(6, 73, 234) 29% , rgb(231, 150, 14) 84%);
	background: -ms-linear-gradient(left , rgb(6, 73, 234) 29% , rgb(231, 150, 14) 84%);
	background: -moz-linear-gradient(left , rgb(6, 73, 234) 29% , rgb(231, 150, 14) 84%);
	background: -webkit-linear-gradient(left , rgb(6, 73, 234) 29% , rgb(231, 150, 14) 84%);
}
.div2{
	width: 200px;
	height: 200px;
	
	/* 径向渐变 设置的是 圆形的渐变 */
	/* 
		直径
		位置
		颜色1,
		颜色2,
	 */
	/* background: radial-gradient(100px at center,skyblue,yellowgreen,orange); */

	background: radial-gradient(left bottom , circle cover , rgb(197, 89, 235) 38% , rgb(16, 202, 180) 77%);
	background: -o-radial-gradient(left bottom , circle cover , rgb(197, 89, 235) 38% , rgb(16, 202, 180) 77%);
	background: -ms-radial-gradient(left bottom , circle cover , rgb(197, 89, 235) 38% , rgb(16, 202, 180) 77%);
	background: -moz-radial-gradient(left bottom , circle cover , rgb(197, 89, 235) 38% , rgb(16, 202, 180) 77%);
	background: -webkit-radial-gradient(left bottom , circle cover , rgb(197, 89, 235) 38% , rgb(16, 202, 180) 77%);
}

```


### background-origin&clip 

```css
div{
	width: 300px;
	height: 300px;
	border: 5px dashed gray;
	margin: 0 auto;
	padding: 10px;
	background: url('images/gyt.jpg') no-repeat top left /200px 200px;
	/* 设置的是 背景图片 开始的位置
		border-box: 从边框开始;
		下面这两个属性 如果没有设置 padding 那么 看起来视觉效果一样
		content-box: 从内容开始;
		padding-box padding开始的位置
	*/
	background-origin: border-box;


	/* 背景图 切割的位置
		padding-box padding之外的区域切割
		content-box 内容之外的区域切割css
	 */
	background-clip: content-box;
}
```




```css
 /*实例：增大使用精灵图制作的a标签的背景图片的点击范围，且只显示当前图片，切除多余部分*/
   a {
       /* 基础设置 */
       width: 36px;
       height: 36px;
       padding: 40px;
       /* border: 1px solid gray; */
       display: block;css

       /* 背景图片 */
       background-image: url('images/sprites.png');
       background-repeat: no-repeat;
       background-position: -361px -140px;

       /* 设置背景图片的开始位置 */
       background-origin: content-box;
       /*背景图片开始显示的位置*/

       /* 除了内容之外的区域 不要了 */
       background-clip: content-box;
       /*背景图片开始裁剪的位置，即content外的部分都裁剪掉 不要*/
   }

   /* 希望在 不改变 背景图片大小的 请款下 让 a的点击范围增大 并且 放大镜 是在a标签的中间的 */
```

### webfont&iconfont 

```css
/* 准备字体的样式
	如果 想要更改文字的放置位置 需要修改对应的路径信息
	iconfont能够获取字体文件
	有字库 等等
 */
@font-face {
	   font-family: 'webfont';
	    src: url('webfont/webfont.eot'); /* IE9*/
	    src: url('webfont/webfont.eot?#iefix') format('embedded-opentype'), /* IE6-IE8 */
	    url('webfont/webfont.woff') format('woff'), /* chrome、firefox */
	    url('webfont/webfont.ttf') format('truetype'), /* chrome、firefox、opera、Safari, Android, iOS 4.2+*/
	    url('webfont/webfont.svg#webfont') format('svg'); /* iOS 4.1- */
}
.web-font{
	    font-family:"webfont" !important;
	    font-size:16px;font-style:normal;
	    -webkit-font-smoothing: antialiased;
	    -webkit-text-stroke-width: 0.2px;
	    -moz-osx-font-smoothing: grayscale;
}
/* 文字的大小 是支持修改的 
	我们只能够使用制作出来的那些文字 其他的字用不了
	文字的顺序是可以修改的
*/
#webfont{
	font-size: 100px;
}
```

```html
<p class="web-font" id='webfont'>每天都要早睡早起,这样身体才会棒棒哒,我是中国人,早起,早睡</p>
```






```css
@font-face {
	  font-family: 'iconfont';
	  src: url('iconfont/iconfont.eot');
	  src: url('iconfont/iconfont.eot?#iefix') format('embedded-opentype'),
	  url('iconfont/iconfont.woff') format('woff'),
	  url('iconfont/iconfont.ttf') format('truetype'),
	  url('iconfont/iconfont.svg#iconfont') format('svg');
}
.iconfont{
	  font-family:"iconfont" !important;
	  font-size:16px;font-style:normal;
	  -webkit-font-smoothing: antialiased;
	  -webkit-text-stroke-width: 0.2px;
	  -moz-osx-font-smoothing: grayscale;
}
#iconfont{
	font-size: 200px;
}

.icon-erji:before { content: "\e650"; }

.icon-player:before { content: "\e651"; }

.icon-jita:before { content: "\e652"; }
```

```html
<i class="iconfont" id='iconfont'>&#xe650;</i>
<p class='iconfont icon-erji'></p>
```








