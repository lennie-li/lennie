@(node.js)[javascript, node.js, 服务器]

# Node.js-02-基础、模块化

### 介绍
- 将chrome的 v8 引擎（一种js的解析引擎）单独抽离出来，建立了一个新的运行在服务器端的平台。
- 对ES6的支持度较高
- 模块化编程，对于学习过seajs框架的人来说，编程模式几乎一模一样。

### 基础

1. 运行方式：
	- REPL（read-eval-print-loop）
		- 在命令行窗口中使用 `node` 指令进入。类似与浏览器的控制台。
		- 运行js文件

2. 模块式开发，形式与seajs框架完全相同。
	- `require(...)`依赖模块
	- `exports.add = function(){...}`
	- `module.exports = funtion Person(){...}`
	- `exports === module.exports`，但实际导出的对象是`module.exports`，`exports`只是与其指向相同而已，故`exports`只能添加成员不能改变指向，否则无效。
	- 通过`global`也可以公开成员，为当前模块的`global`添加属性方法后，其他模块`require`该模块后，`global`中的成员也会添加到新的模块中去。

3. node.js在服务端执行，没有浏览器中的window等对象，但也有一些类似的全局对象。
	- global

4. 执行文件时，不加后缀名一样可以运行，运行的优先级：
	- 无后缀->.js->.json->.node->文件夹

5. package.json属性注意事项
    - name
        - 长度必须小于等于214个字符。
        - 不能以"."(点)或者"_"(下划线)开头。
        - 中不能包含大写字母。
        - 不能含有非URL安全的字符。
    - version
        - version和name字段是package.json文件中最重要的字段，都是必须的字段，如果你的包没有指定这两个字段，将无法通过npm被安装。
        - 包内容的更改和包版本的更改是同步的。
        - verion的格式必须正确（符合[semver](https://docs.npmjs.com/misc/semver)规则)
        - 语义化版本
            - 一个标准的版本号必须是X.Y.Z的形式：X是主版本，Y是副版本，Z是补丁版本。.
                - X: 代表发生了不兼容的API改变
                - Y: 代表向后兼容的功能性变化
                - Z: 代表向后兼容bug fixes
            - ~与^符号的区别
                - ~x.y.z: 匹配大于 x.y.z 的 z 的最新版
                - ^x.y.z: 匹配大于 x.y.z 的 y.z 的最新版
            - ~举例
                - ~1.2.3 等价于 >=1.2.3 <1.(2+1).0 等价于="">=1.2.3 <1.3.0
                - ~1.2 等价于 >=1.2.0 <1.(2+1).0 等价于="">=1.2.0 <1.3.0 (Same - as 1.2.x)
                - ~1 等价于 >=1.0.0 <(1+1).0.0 等价于 >=1.0.0 <2.0.0 (Same as 1.- x)
                ---
                - ~0.2.3 等价于 >=0.2.3 <0.(2+1).0 等价于="">=0.2.3 <0.3.0
                - ~0.2 等价于 >=0.2.0 <0.(2+1).0 等价于="">=0.2.0 <0.3.0 (Same - as 0.2.x)
                - ~0 等价于 >=0.0.0 <(0+1).0.0 等价于 >=0.0.0 <1.0.0 (Same as 0.- x)
            - ^举例
                - ^1.2.3 等价于 >=1.2.3 <2.0.0
                - ^0.2.3 等价于 >=0.2.3 <0.3.0
                - ^0.0.3 等价于 >=0.0.3 <0.0.4，即等价于0.0.3
                ---
                - ^1.2.x 等价于 >=1.2.0 <2.0.0
                - ^0.0.x 等价于 >=0.0.0 <0.1.0
                - ^0.0 等价于 >=0.0.0 <0.1.0
    - main 
        - 包的入口文件

6. npm  node.js package management / Node.js包管理器
	- npm常用命令
	    - `npm install` 包名@版本号
	    - `npm update` 包名
	    - `npm uninstall` 包名
	    - ......
	- dependencies与devDependencies
	    - `dependencies` 程序正常运行需要的包
	    - `devDependencies` 程序开发测试需要的包
	- `npm install` 与 `npm install --production`
	    - `npm install` 安装所有的依赖
	    - `npm install --production` 只安装dependencies的依赖
	- npm 下载地址设置
		- 由于服务器在国外，网络不稳定，可使用淘宝镜像具体如下
			1. `npm config set registry http://registry.npm.taobao.org`该指令其实就是修改 `用户\用户名\npmrc` 这个文件内的地址。手动修改也可以。
			2. 安装cnpm包，使用cnpm指令代替npm指令
			3. 安装arm包，代替我们手动更改 1 中文件地址，帮我们管理下载地址。

7. 模块的加载
    - 核心模块的加载（优先级最高）（指定字符串）
    - 文件模块加载（路径）
    - 文件夹的加载（路径）
    - 从node_modules目录下加载（指定字符串）
    - 从缓存加载（多次引入模块也只会调用一次被引入模块中的代码，重复引入会直接从缓冲中加载，不会再次请求调用模块）
	- 模块加载的时候`require`的参数要么是路径，要么是特定的字符串
