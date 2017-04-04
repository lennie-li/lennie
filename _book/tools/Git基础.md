@(Git)[git]

## Git基础
1. 计算机系统 
	- win
	- 类 unix: ubuntu , redhat ,  centos , mac  OS
2. 路径表示
	- 标志 :
		-  `/` 正斜线
		-  `\` 反斜线
	-  win :
		-  `\`表示路径
	-  linux :
		-  没有盘符概念，只有一个系统根目录 :  `/`
		-  `/`表示路径 ，网页也是这样
		-  `src = /xxx.html` 根目录下的xxx文件
		-  `./` 当前目录
		-  `../` 上一级目录
3. 关于换行符
	- 在 win 中，一个换行相当于 换行加回车 ‘ **\s\n** ’
	- 在 linux 中，换行就是一个换行 : ' **\n** ' ， 回车是回车：' **\r** ' 
4. 路径 当前家目录（home）
	- 用 `~` 表示家目录
	- linux 命令中 ， 使用 `cd`  (change directory) 来切换目录 
		- 切换到家目录 : `cd ~`
		- 打开桌面可以使用 : `cd/desktop`
	- win : `dir /a` 显示所有文件
	- linux : `ls -a` 显示所有文件 linux 中所有以`.`开头的都是隐藏文件
		- `copy /b 1.mp3+1.mp3 2.mp3`  以2进制合并文件
		- `mkdir test` 创建目录
		- `rmdir test` 删除
		- `clear` 清除命令
		- `cd ..` 上一级目录
5.   
	- 配置用户名  --global 全局模式 代表这台机器上所有的Git仓库都会使用这个配置

		`$ git config [--global] user.name "Your Name"`

	- 配置email

		`$ git config [--global] user.email "email@example.com"	`

	- 配置信息
		
		`$ git config --list`
		
6. commit
	- 如果 commit 提交的时候 忘记添加 -m 会自动进入 vi 编辑器状态 。分为两种状态 一种是命令状态 一种是编辑状态 。
	- 命令模式    : 输入命令
	- 编辑模式    i 输入文本 
		- 退出 ESC => `:q`
		- 保存退出 ==> `i` ==> 输入信息 ==> ESC ==> `:wq` write&quit 写入并退出
		- `!` 强制 `i`insert `w` write `q` quit

7. `touch`创建文件
	`cd`切换目录
	`mkdir`创建文件夹
	`rmdir`移除文件夹
	`rm`删除文件
	`vi xx.md`编辑文件 ，文件不存在时，新建xx.md文件并编辑
	`vi` 进入vi编辑器  ，退出时`:wq xx.js`保存为xx.js文件
	**注**：若文件夹非空，则需要使用`rm -rf xxx` 
	
	| r         |      f   |
	| :--------: | :--------:|
	|remove删除    |  直接删除 不提示 |

8. 把代码提交到仓库 
	- commit与add指令合并：`git commit -a -m "message"`
	- **注意**：该方法只能提交修改过的文件 ，对于新添加的文件该命令无效，需要单独使用add

9. 查看工作区状态：`git status`
10. 添加需要忽略的文件，不提交到仓库。
	- 与 .git 同级目录新建文件 .gitignore ，内部添加需要忽略的文件的路径，*表示通配符
	>/.gitignore 
	/test.txt
	/css/*.css
	
11. `./`表示当前目录，强调语义，严谨
	`/`表示根目录
12. 比较文件差异
	- `git diff`
		+ 用来比较暂存区文件内容与工作区文件内容的区别
		+ 若暂存区没有文件，则会将工作区文件与最近一次commit的文件（仓库）进行比较
	- `git diff --cached`
		+ 比较将暂存区的文件与最近一次提交的代码进行对比
	- `git diff xxx.xxx`
		+ 比较具体文件
	- `git diff [版本号1] [版本号2] [文件名]`
		+ 比较具体版本的差异 
		+ 示例：`git diff 3d5124da56 34df456fd readme.md`
13. 查看日志
	- `git log [--pretty=oneline] [-graph]` ，`git log [--oneline]`
		- 查看日志[只显示版本号和commit信息]，查看日志[只显示缩略版本号和comiit信息]，查看图形化分支[-graph]
	- `git reflog`
		- 查看之前的版本切换操作记录
14. 版本回退
	- `git reset --hard head^^` --hard 表示重置并覆盖工作区内容
	- `git reset --hard head~2`
	- `git reset --hard {id}`
15. 分支
	- `git branch`查看分支
	- `git branch branchName`创建分支
	- `git checkout branchName`切换分支
	- `git merge branchName` 将branchName分支合并 到当前上
	- `git branch -d branchName`删除branchName分支  [ -d ==> delete ]
16. 合并冲突
	- 若git合并分支时存在冲突，我们需要手动解决冲突，然后再次提交。*git会在文件中会标识出具体冲突的文本*
17. 忽略文件
	- .gitignore 文件内配置不需要git的文件
	```
	node_modules/
	.gitignore
	
	./src
	!./src/*.js
	```

## Github

### git & github
> git 版本管理工具
> github 一个提供git服务器功能的开源社区网站

### 上传代码到github
- `git push [-u] [服务器地址] [服务器分支]`
	- `-u` 第一次 push 将本地当前分支与远程服务分支关联，以后在该分支提交到服务器相应的分支，不再需要添加服务器分支的名字，即`git push [服务器地址]`即可上传文件
- `git remote add origin github地址`设置变量*origin* 储存远程服务器地址 **仅本仓库可用**
- `git remote`查看存储的变量
- `git push origin master`  *origin*替代服务器地址上传文件
- 开源中国(码云) git.oschina.net
- gitlab  git服务端  个人服务器

### ssh方式上传
- `ssh-keygen -t rsa` 在用户家目录的 .ssh 文件夹内 生成公钥私钥
- git服务器设置公钥，并设置上传代码地址为ssh方式地址。

### 服务器与本地代码冲突fix
- 在每次要`push`文件到服务器前，先从服务器`pull`代码到本地，`pull`会自动将服务器的文件与本地文件进行合并，合并成功后再执行`push`文件到服务器。
- 若`pull`产生冲突，则需要在本地修改文件后，再次`pull`文件到本地，确保没有冲突后，再进行`push`操作。

### 利用github搭建blog
1. 创建一个项目
2. 创建`gh-pages`分支
3. 将项目上传至该分支
4. 利用 `[用户名].github.io/[项目名]/[页面]` 访问

## 小结
> 基本的linux命令

- 语法： 命令名 选项与参数
- linux的哲学就是一切从简，因此命令一般都是简写单词：pwd( print working directory )，ls( list )，cd( change directory )
- 学会对文件与文件夹的增删改差
	- 增加：
		mkdir 文件夹名
		touch 文件名
	- 删除：
		rmdir 空的额外年间家
		rm 文件
		rm -rf 含有内容的文件夹
	- 修改
	- 查询
		cat
	- 补充：vi编辑（如何退出，如何切换模式）
- git 本地操作
	- 背景：分布式( git )和集中式( svn ,subversion )
	- 创建仓库 `git init`
	- 追踪文件 `git add`
	- 提交数据 `git commit -m "message"`
	- 查看状态 `git status`
	- 查看日志 `git log`
	- 对比 `git diff`
	- 查看分支 `git branch`
	- 创建分支 `git branch branchName`
	- 删除分支 `git branch -d branchName`
	- 切换分支 `git checkout branchName`
	- 合并分支 `git merge branchName`
	- 版本回退
		1. `git reset --hard head~num` 
		2. `git reset --hard SHA1值`
		3. `git reset --hard head^^`
	- 查看所有版本 `git reflog`

> git 远程操作

- 服务器：github.com , git.oschina.net , gitlab 
- 基本操作步骤
	1. 注册
	2. 新建仓库
	3. 得到地址 url:( http://... )
- 本地：
	1. `git push url master`
	2. `git remote add origin url`
	3. `git push -u origin master`
	4. `git clone` 下来的项目可以直接使用 `git push` 上传
- 注意 linux 输入密码不会显示任何东西，直到遇到回车表示确认输入
	
### tortoiseGit

- git图形化工具

### NPM

> node package manager
> 官网 www.npmjs.com 查询相关插件

- LTS 长期稳定维护版本
- `npm init [-f]`初始化  `-f`自动填写信息 mac OS 手动创建node_modules
- `npm install packageName[@1.7.9] [--save]` 安装包，[@版本号]，[并储存包相关信息]。
- `npm install --production`自动下载package.json中dependencies属性对应的包及依赖项
- 安装相同的包时，旧的包会被新的包替换。
- `npm remove packageName [--save]`删除包[同时删除package.json内的相关信息]


#### browser-sync

- 更改代码后自动刷新浏览器，多浏览器多设备同步。
- `npm install browser-sync -g`
- `browser-syne start --server './fileDirectory' --files './css/*.css,*.*'`
	- [--server] 网站根目录
	- 项目目录下执行该命令行，启动服务，监听某文件的变化，自动刷新页面
	- 引号内跟需要监视的文件路径，多个文件使用逗号分隔
- 开启服务后，在浏览器内使用 control+v 打开browser-sync的控制面板

#### Http-Server
- 将当前目录当作服务器运行
	- `hs -o` 默认以8080端口自动打开浏览器
	- `hs -o -p 8081` 以8081端口自动打开浏览器


#### gulp

> 前端自动化构建工具
> gulp  grunt webpack 等

- 核心方法
	1. task ，gulp中是以一个个任务的形式来实现功能		


			task(' 任务名 ' , function( ){ 
				...
			}); 

	2. src
		`src(' ./ *.js' )`
	3. `dest(' ./minjs/ ') ;` 指定输出目录
	4. `watch(' ./ *.js ' , ['任务名1','任务名2'] );`
	5. `run( '任务名' );` 执行指定的任务
- gulp安装
	- `npm install -g gulp-cli`
	- `npm install gulp --save-dev` [--save-dev]表示工具

- gulp使用
	1. 新建 gulpfile.js
	2. gulpfile.js 文件内写入需要执行的代码
		`gulp.src(...).pipe().pipe(gulp.dest('./xxx'))`通过src指定需要处理的文件，再通过pipe(  ...  )来指定一系列任务来处理，最后使用`pipe(gulp.dest('./xxx'))`指定输出目录
	3. gulp 插件
		- gulp-gulify 压缩js文件
		- gulp-concat 合并js、css文件
		- gulp-cssnano 压缩css文件
		- gulp-htmlmin 压缩html

	
				// 得到gulp对象
				var gulp = require('gulp');
				var press = require('gulp-uglify');
				var concat = require('gulp-concat');
				var cssnano = require('gulp-cssnano');
				var htmlmin = require('gulp-htmlmin');
				
				//新建任务
				
				gulp.task('script' , function(){
					gulp.src(['./LEN.js','./L.js'])   		//匹配到LEN.js , L.js 如果使用多个规则，则需要使用数组来书写第一个参数，数组中的每一个元素都是一个规则
					.pipe(concat('all.js'))     //把文件合并成all.js
					.pipe(press())				//执行什么任务(压缩)
					.pipe(gulp.dest('./lib'));	//输出到指定目录
				});
	
				gulp.task('style',function(){
					gulp.src(['./css/a.css','./css/b.css'])
					.pipe(concat(all.css))
					.pipe(cssnano())
					.pipe(gulp.dest('./dist'))
				});
				gulp.task('html',function(){
					gulp.src('./index.html')
					.pipe(htmlmin({collapseWhitespace:true})) //合并空白符
					.pipe(gulp.dest('./'))
				});
				gulp.task('watch',function(){
					gulp.watch(['./LEN.js','./L.js'] , ['script']);
				});

4. 执行gulpfile.js文件
	- `gulp script`
	- `gulp style`
	- `gulp html`  分别执行script style html 任务
5. 监视文件变化，并调用指定任务处理代码
	- `gulp.watch(['./LEN.js','./L.js'] , ['script']);`多个参数用分别数组表示，监视LEN.js 和L.js ，若有文件发生变化，则调用script任务并执行。

		
#### browser-sync与gulp配合使用



	var gulp = require('gulp');
	var html = require('gulp-htmlmin');
	var browserSync = require('browser-sync');
	gulp.task('html', function () {
	    gulp.src('./src/index.html')
	        .pipe(html({collapseWhitespace: true}))
	        .pipe(gulp.dest('./dist'));
	});
	
	gulp.task('watch', function () {
	    browserSync.init({
	        server: './dist', //服务器地址
	        files: ['./dist/index.html'] //browser-sync监视文件
	    });
	    gulp.watch('./src/index.html', ['html']); //gulp监视的文件
	});







