---
title: node.js 的深入浅出
tags: node
categories: node
---
Node.js 就是运行在服务端的 JavaScript。Node.js 是一个基于Chrome JavaScript 运行时建立的一个平台。Node.js是一个事件驱动I/O服务端JavaScript环境，基于Google的V8引擎，V8引擎执行Javascript的速度非常快，性能非常好。底层选择用c++和v8来实现的
<!-- more -->

## 一、	简介

### (一) 优势：
1. RESTful API
这是NodeJS最理想的应用场景，可以处理数万条连接，本身没有太多的逻辑，只需要请求API，组织数据进行返回即可。它本质上只是从某个数据库中查找一些值并将它们组成一个响应。由于响应是少量文本，入站请求也是少量的文本，因此流量不高，一台机器甚至也可以处理最繁忙的公司的API需求。
2. 统一Web应用的UI层
目前MVC的架构，在某种意义上来说，Web开发有两个UI层，一个是在浏览器里面我们最终看到的，另一个在server端，负责生成和拼接页面。
不讨论这种架构是好是坏，但是有另外一种实践，面向服务的架构，更好的做前后端的依赖分离。如果所有的关键业务逻辑都封装成REST调用，就意味着在上层只需要考虑如何用这些REST接口构建具体的应用。那些后端程序员们根本不操心具体数据是如何从一个页面传递到另一个页面的，他们也不用管用户数据更新是通过Ajax异步获取的还是通过刷新页面。
3. 大量Ajax请求的应用
例如个性化应用，每个用户看到的页面都不一样，缓存失效，需要在页面加载的时候发起Ajax请求，NodeJS能响应大量的并发请求。　
4.Javascript在nosql的应用　
Javascript在nosql数据库中大量应用，使得数据存储和管理使用的都是javascript语句，与web应用有了天然的结合；比如mongoDB；
5．Javascripte运行从前台到后台
	一门语言从前台后台，减少了开发客户端和服务端时，所需的语言切换，使得数据交互效率提升

### (二) 特点：

1. 单线程：
Nodejs跟Nginx一样都是单线程为基础的，这里的单线程指主线程为单线程，所有的阻塞的全部放入一个线程池中，然后主线程通过队列的方式跟线程池来协作。我们写js部分不需要关心线程的问题，简单了解就可以了，主要由一堆callback回调构成的，然后主线程在循环过在适当场合调用。 

2. 事件驱动   
首先，解释下“事件驱动”这个概念。所谓事件驱动，是指在持续事务管理过程中，进行决策的一种策略，即跟随当前时间点上出现的事件，调动可用资源，执行相关任务，使不断出现的问题得以解决，防止事务堆积。   Nodejs设计思想中以事件驱动为核心，事件驱动在于异步回调，他提供的大多数api都是基于事件的、异步的风格。而事件驱动的优势在于充分利用系统资源，执行代码无须阻塞等待某种操作完成，有限的资源用于其他任务。事件驱动机制是通过内部单线程高效率地维护事件循环队列来实现的，没有多线程的资源占用和上下文的切换。

3.	异步、非阻塞I/O
Nodejs提供的很多模块中都是异步执行的。比如，文件操作的函数。 一个异步I/O的大致流程： 
1.	发起I/O调用 :
①用户通过js代码调用nodejs的核心模块，将回调函数和参数传入核心模块 
②将回调函数和参数封装成 
2.	执行回调:
 ①操作完成将结果储存到请求对象的result属性上，并发出完成通知。
 ②循环事件，如果有未完成的，就在进入对象请求I/O观察者队列，之后当做事件处理； 

### (三) 缺点：

1.	不适合CPU密集型应用；CPU密集型应用给Node带来的挑战主要是：由于JavaScript单线程的原因，如果有长时间运行的计算（比如大循环），将会导致CPU时间片不能释放，使得后续I/O无法发起；
2.	只支持单核CPU，不能充分利用CPU
3.	可靠性低，一旦代码某个环节崩溃，整个系统都崩溃
4.	开源组件库质量参差不齐，更新快，向下不兼容

## 二、	安装
### (一)	官网下载：

1.	下载地址：http://nodejs.cn/download/

2.	根据自己的系统进行镜像的下载：
 
3.	下载的为最新的node版本，当前下载的为10.8.0

### (二)	Nvm管理node

## nvm可以方便的在同一台设备上进行多个node版本之间切换
``` bash
1.先下载安装nvm，下载地址：https://github.com/coreybutler/nvm-windows/releases
选择nvm-setup压缩文件，解压后安装；
 
2.安装过程中出现，选择nvm的安装目录
 
3.选择node的安装目录

4.配置环境变量

5.查看nvm是否安装成功：
```

## Nvm -v

### 6.安装nodejs

``` bash
使用nvm install <version> [<arch>]命令下载需要的版本。
```
arch参数表示系统位数，默认是64位

如果是32位操作系统，需要执行命令：
``` bash
nvm install 10.8.0 32
```
``` bash
Nvm install 10.8.0
```
7.使用下载的nodejs
``` bash
执行nvm use <version> [<arch>] 命令开始使用特定版本。比如：
```
``` js
nvm use 10.8.0或者nvm use 10.8.0 32
```
``` js
Nvm use  10.8.0
```
8.当有多个nodejs版本时，设置默认的node版本
``` js
nvm alias default v10.8.0
```
9.查看当前所安装的node版本
``` js
Nvm list
```
## (三)	全局安装和局部安装

### 全局安装：

全局安装方式是键入命令：
``` js
npm install gulp -g 或 npm install gulp --global
```
其中参数-g的含义是代表安装到全局环境里面，包安装在Node安装目录下的node_modules文件夹中，一般在 \Users\用户名\AppData\Roaming\ 目录下，可以使用npm root -g查看全局安装目录。

局部安装（本地安装）
本地安装方式是键入命令
``` js
npm install gulp 或 npm install gulp --save-dev等
```

其中参数--save-dev的含义是代表把你的安装包信息写入package.json文件的devDependencies字段中，包安装在指定项目的node_modules文件夹下。
局部安装的意义：
1、	可以实现多个项目中使用不同版本的包；
2、	可以在不使用全局变量NODE_PATH的情况下，进行包的引入；

### (四)	Node运行

终端运行和外部文件运行

## 三、	Nodejs的模块（commonjs规范）

### (一) 模块化

1.	诞生背景：
全局变量的灾难:
函数命令的冲突:
对于公用方法的封装会出现很多命名冲突，尤其在多人开发的情况下
依赖关系的管理：
比如b.js依赖a.js，在文件引入的过程中，就要先引入b.js

最早的时候在解决上述部分问题时的解决方案是：使用匿名的自执行函数

2.	模块需要解决的问题：
1. 如何安全的包装一个模块的代码？（不污染模块外的任何代码）
2. 如何唯一标识一个模块？
3. 如何优雅的把模块的API暴漏出去？（不能增加全局变量）
4. 如何方便的使用所依赖的模块？

## (二)	Commonjs

### 1.	规范：
1)	模块的标识应遵循的规则（书写规范）
定义，标识，引用
2)	定义全局函数require，通过传入模块标识来引入其他模块，执行的结果即为别的模块暴漏出来的API
3)	如果被require函数引入的模块中也包含依赖，那么依次加载这些依赖
4)	如果引入模块失败，那么require函数应该报一个异常
5)	模块通过变量exports来向往暴漏API，exports只能是一个对象，暴漏的API须作为此对象的属性。

### 2.	模块的简单使用：

``` js
//math.js
exports.add = function() {
    var sum = 0, i = 0, args = arguments, l = args.length;
    while (i < l) {
        sum += args[i++];
    }
    return sum;
};

//increment.js
var add = require('math').add;
exports.increment = function(val) {
    return add(val, 1);
};

//program.js
var inc = require('increment').increment;
var a = 1;
inc(a); // 2
```
### 3.	模块的定义
``` js
1)	全局有一个module变量，用来定义模块

2)	通过module.declare方法来定义一个模块（一般不通过此方式进行模块的定义）

3)	module.declare方法只接收一个参数，那就是模块的factory，次factory可以是函数也可以是对象，如果是对象，那么模块输出就是此对象。

4)	模块的factory函数传入三个参数：require,exports,module，用来引入其他依赖和导出本模块API

5)	如果factory函数最后明确写有return数据（js函数中不写return默认返回undefined），那么return的内容即为模块的输出。
``` 
不常用：

``` js
module.declare(function(require, exports, module) {
    exports.foo = "bar";
});
module.declare(function(require)
{
return { foo: "bar" };
});
``` 
常用：

``` js
module.exports={}
```
				
### 4.	Module.exports和exports的区别：

``` js
1)	module.exports 初始值为一个空对象 {}

2)	exports 是指向的 module.exports 的引用

3)	require() 返回的是 module.exports 而不是 exports

4)	关系为var exports = module.exports={};
```
如：
``` js
module.exports可以赋值一个对象
module.exports={}

exports不可以赋值一个对象，只能添加方法或者属性
exports.add=function(){
    
}

``` 
### 5.	模块引用
require函数的基本功能是，读入并执行一个JavaScript文件，然后返回该模块的exports对象。当我们用require()获取module时，Node会根据module.id找到对应的module，并返回module. exports，这样就实现了模块的输出。
require函数使用一个参数,参数值可以带有完整路径的模块的文件名,也可以为模块名。
假如，有三个文件：一个是a.js（存放路径：home/a.js）,一个是b.js（存放路径：home/user/b.js）, 一个是c.js（存放路径：home/user/c.js）。我们在a.js文件中引用三个模块，实例代码如下：
``` js
var httpModule=require('HTTP');//用 “模块名”加载服务模块http

var b=require('./user/b');//用“相对路径”加载文件b.js

var b=require('../ home/user/c');//用“绝对路径”加载文件c.js
``` 
### 6.	 模块标识
模块标识就是传递给require方法的参数，必须符合小驼峰命名的字符串，或者以.、..开头的相对路径，或者绝对路径，默认文件名后缀.js。在Node实现中，正是基于这样一个标识符进行模块查找的，如果没有发现指定模块会报错。
根据参数的不同格式，require命令去不同路径寻找模块文件。加载规则如下：

（1）如果参数字符串以“/”开头，则表示加载的是一个位于绝对路径的模块文件。比如，require('/home/marco/foo.js')将加载/home/marco/foo.js。

（2）如果参数字符串以“./”开头，则表示加载的是一个位于相对路径（跟当前执行脚本的位置相比）的模块文件。比如，require('./circle')将加载当前脚本同一目录的circle.js。

（3）如果参数字符串不以“./“或”/“开头，则表示加载的是一个默认提供的核心模块（位于Node的系统安装目录中），或者一个位于各级node_modules目录的已安装模块（全局安装或局部安装）。

举例来说，脚本/home/user/projects/foo.js执行了require('bar.js')命令，Node会依次搜索以下文件。
``` js
/usr/local/lib/node/bar.js

/home/user/projects/node_modules/bar.js

/home/user/node_modules/bar.js

/home/node_modules/bar.js

/node_modules/bar.js
```
 
    这样设计的目的是，使得不同的模块可以将所依赖的模块本地化。
    （4）如果参数字符串不以“./“或”/“开头，而且是一个路径，比如require('example-module/path/to/file')，则将先找到example-module的位置，然后再以它为参数，找到后续路径。
    （5）如果指定的模块文件没有发现，Node会尝试为文件名添加.js、.json、.node后，再去搜索。.js件会以文本格式的JavaScript脚本文件解析，.json文件会以JSON格式的文本文件解析，.node文件会以编译后的二进制文件解析。
    （6）如果想得到require命令加载的确切文件名，使用require.resolve()方法。此方法会返回一个完整的路径，并且还会对文件的是否存在做检测
    
    

7.	二进制模块
虽然一般我们使用JS编写模块，但NodeJS也支持使用C/C++编写二进制模块。编译好的二进制模块除了文件扩展名是.node外，和JS模块的使用方式相同。虽然二进制模块能使用操作系统提供的所有功能，拥有无限的潜能，但对于前端同学而言编写过于困难，并且难以跨平台使用，因此不在本教程的覆盖范围内。

## (三) Node模块的分类：

1.	内置模块（核心模块）

核心模块指的是那些被编译进Node的二进制模块，它们被预置在Node中，提供Node的基本功能，如fs、http、https等。核心模块使用C/C++实现，外部使用JS封装。要加载核心模块，直接在代码文件中使用require() 方法即可，参数为模块名称，Node将自动从核心模块文件夹中进行加载。 

2.	第三方模块 
Node使用NPM （Node Package Manager） 安装第三方模块，NPM会将模块安装到应用根目录下的node_modules文件夹中，然后就可以像使用核心模块一样使用第三方模块了。在进行模块加载时，Node会先在核心模块文件夹中进行搜索，然后再到node_modules文件夹中进行搜索。 

3.	文件模块 
上述两种方式都是从当前目录获取模块文件，实际上，可以将文件放在任何位置，然后在加载模块文件时加上路径即可。可以使用以./ 开头的相对路径和以/ 或C: 之类的盘符开头的绝对路径。

4.	文件夹模块 
从文件夹中加载模块，Node首先会在该文件夹中搜索package.json文件。如果存在，Node便尝试解析它，并加载main属性指定的模块文件。如果package.json不存在，或者没有定义main属性，Node默认加载该文件夹下的index.js文件。 如从项目根目录下的 modules/hello 文件夹加载模块： 
``` js
var hello = require("./modules/hello"); 
```

package.json格式如下：
``` js
{ "name": "hello", "version": "1.0.0", "main": "./hello.js" }
```

此时，Node会去加载./modules/hello/hello.js 文件。
如果目录里没有 package.json 文件，则 Node.js 就会试图加载目录下的 index.js 或 index.node 文件。 例如，如果上面的例子中没有 package.json 文件，则 require("./modules/hello") 会试图加载：
./modules/hello /index.js
./modules/hello /index.node
## 四、	Npm与package.json详解

### (一)	npm简介：
世界上最大的软件注册表，每星期大约有 30 亿次的下载量，包含超过 600000 个 包（package） （即，代码模块）。来自各大洲的开源软件开发者使用 npm 互相分享和借鉴。包的结构使您能够轻松跟踪依赖项和版本。
npm 是一个包管理器，它让 JavaScript 开发者分享、复用代码更方便（有点 maven 的感觉）。 在程序开发中我们常常需要依赖别人提供的框架，写 JS 也不例外。这些可以重复的框架代码被称作包（package）或者模块（module），一个包可以是一个文件夹里放着几个文件，同时有一个叫做 package.json 的文件。 一个网站里通常有几十甚至上百个 package，分散在各处，通常会将这些包按照各自的功能进行划分，但是如果重复造一些轮子，不如上传到一个公共平台，让更多的人一起使用、参与这个特定功能的模块。 而 npm 的作用就是让我们发布、下载一些 JS 轮子更加方便。

### (二)	npm构成：

npm 由三个独立的部分组成：
网站：是开发者查找包（package）、设置参数以及管理 npm 使用体验的主要途径。
注册表（registry）：是一个巨大的数据库，保存了每个包（package）的信息
命令行工具 (CLI)：通过命令行或终端运行。开发者通过 CLI 与 npm 打交道

### (三)	npm更新：

查看版本：nvm -V
更新版本：nvm install npm@latest -g

### (四)	npm更改全局目录：

查看npm全局目录：
```js
npm root -g 
```
修改全局包位置 ：
``` js
npm config set prefix '目标目录'
```

查看修改结果 
``` js 
npm config get prefix或npm root -g
``` 

## (五)	package.json

### A .	作用:

1.	作为一个描述文件，描述了你的项目依赖哪些包 
2.	允许我们使用 “语义化版本规则”（后面介绍）指明你项目依赖包的版本
3.	让你的构建更好地与其他开发者分享，便于重复使用 
4.	如果项目构架进行复制时，可以直接使用npm install,直接根据package.json的配置进行包的下载
5.	作为项目的描述文件，对整个项目进行描述。

### B.	创建：

``` js
npm init 即可在当前目录创建一个 package.json 文件：
```

### C.	内容：
基本配置：
1.	name：项目的名字 
2.	version：项目的版本 
3.	description：描述信息，有助于搜索
4.	main: 入口文件，一般都是 index.js 
5.	scripts：支持的脚本，默认是一个空的 test 
6.	keywords：关键字，有助于在人们使用 npm search 搜索时发现你的项目 
7.	author：作者信息 
8.	license：默认是 MIT 
9.	bugs：当前项目的一些错误信息，如果有的话
注：
如果 package.json 中没有 description 信息，npm 使用项目中的 README.md 的第一行作为描述信息。这个描述信息有助于别人搜索你的项目，因此建议好好写 description 信息。

### 依赖包配置：

1.	dependencies：在生产环境中需要用到的依赖 
2.	devDependencies：在开发、测试环境中用到的依赖 

## (六)	package-lock.json文件

npm5之后安装文件之后会多出一个package-lock.json的文件，它的作用是：
1.	安装之后锁定包的版本，手动更改package.json文件安装将不会更新包，
想要更新只能使用
``` js
npm install xxx@1.0.0 --save 
```
 这种方式来进行版本更新package-lock.json 文件才可以
2.	加快了npm install 的速度，因为 package-lock.json 文件中已经记录了整个 node_modules 文件夹的树状结构，甚至连模块的下载地址都记录了，再重新安装的时候只需要直接下载文件即可


## (七) npm的包版本规范和package.json的使用规范

### npm版本规范：
如果一个项目打算与别人分享，应该从 1.0.0 版本开始。以后要升级版本应该遵循以下标准： 
补丁版本：解决了 Bug 或者一些较小的更改，增加最后一位数字，比如 1.0.1 
小版本：增加了新特性，同时不会影响之前的版本，增加中间一位数字，比如 1.1.0 
大版本：大改版，无法兼容之前的，增加第一位数字，比如 2.0.0 

Package.json的版本书写：

我们可以在 package.json 文件中写明我们可以接受这个包的更新程度（假设当前依赖的是 1.0.4 版本）： 
如果只打算接受补丁版本的更新（也就是最后一位的改变），就可以这么写： 
``` js
1.0 
1.0.x 
~1.0.4
``` 
如果接受小版本的更新（第二位的改变），就可以这么写： 
```js
1  
1.x 
^1.0.4 
```
如果可以接受大版本的更新（自然接受小版本和补丁版本的改变），就可以这么写： 
* 
x

## (八)	npm下载包

### 安装方式：
如果你只是想在当前项目里用 require() 加载使用，那你可以安装到本地 npm install 默认就是安装到本地的 
如果你想要在命令行里直接使用，比如 grunt CLI，就需要安装到全局了
如果在你的项目里有 package.json 文件，运行 npm install 后它会查找文件中列出的依赖包，然后下载符合语义化版本规则的版本。 npm install 默认会安装 package.json 中 dependencies 和 devDependencies 里的所有模块。 如果想只安装 dependencies 中的内容，可以使用 --production 字段：
``` js
npm install --production
```

### 1.	本地安装：
1)	安装指定版本：
``` js
$ npm install sax@latest ：最新版本
$ npm install sax@0.1.1 ：指定版本
$ npm install sax@" >=0.1.0 <0.2.0” :安装0.1.0到0.2.0版本
```
注：
有时下载会报错：npm install error saveError ENOENT: no such file or directory, 解决办法： - 在目录下执行 npm init 创建 package.json，输入初始化信息 - 然后再执行下载命令 


2)	安装参数 --save 和 --save -dev 

添加依赖时我们可以手动修改 package.json 文件，添加或者修改 dependencies devDependencies 中的内容即可。 
另一种更酷的方式是用命令行，在使用 npm install 时增加 --save 或者 --save -dev 后缀： 
npm install --save 表示将这个包名及对应的版本添加到 package.json的 dependencies 
npm install --save-dev 表示将这个包名及对应的版本添加到 package.json的 devDependencies 

3)	更新本地package
有时候我们想知道依赖的包是否有新版本，可以使用 npm outdated 查看，如果发现有的包有新版本，就可以使用 npm update 更新它，或者直接 npm update 更新所有：
npm update 的工作过程是这样的： 
先到远程仓库查询最新版本 
然后对比本地版本，如果本地版本不存在，或者远程版本较新 
查看 package.json 中对应的语义版本规则 （一定注意规则）
如果当前新版本符合语义规则，就更新，否则不更新 

4)	卸载本地 package

卸载一个本地 package 很简单，npm uninstall 

2.	全局安装

1)	安装
``` js
npm install -g <packages>
```

2)	权限处理（非windows处理）
在全局安装时可能会遇到 EACCES 权限问题，解决办法办法有如下 2种： 
``` js
1.sudo npm install -g jshint，使用 sudo 简单粗暴，但是治标不治本 
```
2.修改 npm 全局默认目录的权限 先获取 npm 全局目录：
``` js
npm config get prefix，一般都是 /usr/local； 
``` 
然后修改这个目录权限为当前用户：
``` js
sudo chown -R $(whoami) $(npm config get prefix)/{lib/node_modules,bin,share}
```
3)	更新全局包
想知道哪些包需要更新，可以使用 npm outdated -g --depth=0，然后使用 npm update -g 更新指定的包；
要更新所有全局包，可以使用 npm update -g，可以发现对比本地的，只是多了个 -g。

4)	卸载全局包
``` js
npm uninstall -g <package>
```

## 3.	其他命令
``` js
Npm run: 运行 package.json 中 scripts 指定的脚本 
npm install from github: 从github下载资源
npm install git://github.com/package/path.git ；
npm info：npm info 可以查看指定包的信息：
``` 
## (九)	Npm发布包：

## 1.	注册：
npm网站地址：https://www.npmjs.com/
npm网站注册地址：https://www.npmjs.com/signup
## 2.	命令行登录
Windows直接cmd到命令行:
输入以下命令，会提示输入用户名、密码、邮箱，这些都是注册时填写过的。
``` js
npm login
``` 
### 3.	创建项目
创建一个testxxxxx文件夹，cd到testxxxxx文件夹中，然后下载基础配置文件：
1
2	//输入以下命令，会提示配置包的相关信息，名称版本等等，都是包的基本配置信息
npm init
 
配置完毕开始写自己的包内代码： 
创建一个index.js文件，文件内的代码如下，直接输出123456789
module.exports = 123456789;
4.	发布：
开始命令行发布包，命令如下：
``` js
npm publish testxxxxx
``` 
 
发布完毕，在npm网站上搜索，就可以搜索到自己刚刚发布的包了。
### 5.	验证下载：

 
正常下载好了，没有问题了，搞定。
### 6.	撤销发布
接下来说明一下怎么撤销自己发布的版本。这只是一个测试的包，最好当然还是撤销下来：
删除要用force强制删除。超过24小时就不能删除了。自己把握好时间。
``` js
npm --force unpublish testxxxxx
```
 
## (十)	Npm修改镜像源：

由于npm的源在国外，所以国内用户使用起来各种不方便。部分国内优秀的npm镜像资源，国内用户可以选择使用
1.	淘宝npm镜像 
搜索地址：http://npm.taobao.org/ 
registry地址：http://registry.npm.taobao.org/ 
2.	cnpmjs镜像 
搜索地址：http://cnpmjs.org/ registry
地址：http://r.cnpmjs.org/ 
3.	如何使用 
有很多方法来配置npm的registry地址，下面根据不同情境列出几种比较常用的方法。以淘宝npm镜像举例： 
1)	临时使用 
npm --registry https://registry.npm.taobao.org 
2)	持久使用（推荐使用） 
npm config set registry https://registry.npm.taobao.org 
配置后可通过下面方式来验证是否成功 npm config get registry 
3)	通过cnpm使用 （也可以使用cnpm） （常用）
npm install -g cnpm --registry=https://registry.npm.taobao.org 
4.	恢复npm源镜像：
如果将npm的镜像地址改变后，在发布包时，应该将镜像改回：
npm config set registry https://registry.npmjs.org/

## 五、	yarn的使用：

### 1.	简介：
 Yarn是由Facebook、Google、Exponent 和 Tilde 联合推出了一个新的 JS 包管理工具 Yarn 是为了弥补 npm 的一些缺陷而出现的。
### 2.	Npm的缺陷：
1)	npm install的时候巨慢。特别是新的项目拉下来要等半天，删除node_modules，重新install的时候依旧如此。
2)	同一个项目，安装的时候无法保持一致性。由于package.json文件中版本号的特点，下面三个版本号在安装的时候代表不同的含义。
"5.0.3",
"~5.0.3",
"^5.0.3"
“5.0.3”表示安装指定的5.0.3版本，“～5.0.3”表示安装5.0.X中最新的版本，“^5.0.3”表示安装5.X.X中最新的版本。这就麻烦了，常常会出现同一个项目，有的同事是OK的，有的同事会由于安装的版本不一致出现bug。
3)	安装的时候，包会在同一时间下载和安装，中途某个时候，一个包抛出了一个错误，但是npm会继续下载和安装包。因为npm会把所有的日志输出到终端，有关错误包的错误信息就会在一大堆npm打印的警告中丢失掉，并且你甚至永远不会注意到实际发生的错误。
3.	Yarn的优点
1)	速度快 。速度快主要来自以下两个方面：
a)	并行安装：无论 npm 还是 Yarn 在执行包的安装时，都会执行一系列任务。npm 是按照队列执行每个 package，也就是说必须要等到当前 package 安装完成之后，才能继续后面的安装。而 Yarn 是同步执行所有任务，提高了性能。
b)	离线模式：如果之前已经安装过一个软件包，用Yarn再次安装时之间从缓存中获取，就不用像npm那样再从网络下载了。
2)	安装版本统一：为了防止拉取到不同的版本，Yarn 有一个锁定文件 (lock file) 记录了被确切安装上的模块的版本号。每次只要新增了一个模块，Yarn 就会创建（或更新）yarn.lock 这个文件。这么做就保证了，每一次拉取同一个项目依赖时，使用的都是一样的模块版本。npm 其实也有办法实现处处使用相同版本的 packages，但需要开发者执行 npm shrinkwrap 命令。这个命令将会生成一个锁定文件，在执行 npm install 的时候，该锁定文件会先被读取，和 Yarn 读取 yarn.lock 文件一个道理。npm 和 Yarn 两者的不同之处在于，Yarn 默认会生成这样的锁定文件，而 npm 要通过 shrinkwrap 命令生成 npm-shrinkwrap.json 文件，只有当这个文件存在的时候，packages 版本信息才会被记录和更新。
3)	更简洁的输出：npm 的输出信息比较冗长。在执行 npm install <package> 的时候，命令行里会不断地打印出所有被安装上的依赖。相比之下，Yarn 简洁太多：默认情况下，结合了 emoji直观且直接地打印出必要的信息，也提供了一些命令供开发者查询额外的安装信息。
4)	多注册来源处理：所有的依赖包，不管他被不同的库间接关联引用多少次，安装这个包时，只会从一个注册来源去装，要么是 npm 要么是 bower, 防止出现混乱不一致。
5)	更好的语义化： yarn改变了一些npm命令的名称，比如 yarn add/remove，感觉上比 npm 原本的 install/uninstall 要更清晰。
### 4.	Yarn和npm命令对比
``` js
npm	yarn
npm install	yarn
npm install react --save	yarn add react
npm uninstall react --save	yarn remove react
npm install react --save-dev	yarn add react --dev
npm update --save	yarn upgrade
``` 
### 5.	npm的未来：npm5.0（目前使用大多为5.0版本）
默认新增了类似yarn.lock的 package-lock.json；
文件依赖优化：在之前的版本，如果将本地目录作为依赖来安装，将会把文件目录作为副本拷贝到 node_modules 中。而在 npm5 中，将改为使用创建 symlinks 的方式来实现（使用本地 tarball 包除外），而不再执行文件拷贝。这将会提升安装速度。目前yarn还不支持。
总结
在npm5.0之前，yarn的优势特别明显。但是在npm之后，通过以上一系列对比，我们可以看到 npm5 在速度和使用上确实有了很大提升，值得尝试，不过还没有超过yarn。
综上我个人的建议是如果你已经在个人项目上使用 yarn，并且没有遇到更多问题，目前完全可以继续使用。但如果有兼容 npm 的场景，或者身处在使用 npm，cnpm，tnpm 的团队，以及还没有切到 yarn 的项目，那现在就可以试一试 npm5 了。

## 六、 断点调试

### (一) Node-inspector的浏览器调试

1.	安装node-inspector运行环境  
        安装命令：npm install -g node-inspector  
        注意：
            a、参数-g 将node-inspector安装到系统环境变量中，可以在任何路径下执行，尽量保留。
            b、如果是Linux或Unix系统，需要使用root权限安装
 
2. 启动node-inspector
        node-inspector启动后会生成一个服务，主要负责调试工具与nodejs程序之间的沟通工作，一个桥梁。
        a、window：直接在任意路径下执行 node-inspector，进行守护
        b、Linux || Unix：node-inspector &   
        将node-inspcetor作为后台服务，这样就不怕误操作，把窗口关掉了。出现进程PID，表示node-inspcetor已经成为后台进程，可以ctrl+c结束当前任务，node-inspcetor进程依然保持。如果想停止可以 kill -9 pid 杀掉node-inspcetor进程。        
 
3. 打开chrome，输入地址 http://127.0.0.1:8080/debug?port=5858
    NodeJS程序还没起来呢，目前先到这，看看NodeJS程序的变化。
 
4.	 打开NodeJS的调试模式
``` js
    node --debug app.js  debugger的监听端口是5858，这个端口可以修改
```          
5.	  再次打开chrome，刷新页面，chrome通过node-inspector服务连接到nodejs服务上了，并显示nodejs应用的入口文件内容。
         
总结：
1、node-inspector依赖nodejs的运行环境。
2、调试过程中node-inspector的服务不要重启，只需要在重启nodejs应用后刷新一下chrome的页面即可。
3、严格的来说node-inspector不是一个完整的调试工具，它需要一个可视化的调试界面来展示所有的调试信息，node-inspector是调试界面与nodejs之间的桥梁，是调试界面能与nodejs沟通。


### (二)	Vscode的debug

1.	如下图进行点击按钮，弹出配置页面
 
2.	添加node处理程序
 
3.	配置程序名称和程序路径
 
4.	配置完毕启动程序
 
5.	打断点
 

### 七、	文件操作
在node的内置模块中，有一个叫fs的模块，此模块是node用来进行文件处理的；对于所有的文件系统操作都有同步和异步两种形式，异步形式的最后一个参数都是完成时回调函数。 传给回调函数的参数取决于具体方法，但回调函数的第一个参数都会保留给异常。 如果操作成功完成，则第一个参数会是 null 或 undefined。
注：
1、	当使用同步操作时，任何异常都会被立即抛出，可以使用 try/catch 来处理异常，或让异常向上冒泡。
2、	异步的方法不能保证执行顺序，异步方法的执行，应该注意执行的先后顺序
3、	在繁忙的进程中，建议使用函数的异步版本。 同步的方法会阻塞整个进程，直到完成
(一)	文件路径
1、	字符串路径：
绝对路径：'/open/some/file.txt'，
相对路径：'file.txt'（相对于 process.cwd()）
如：
``` js
const fs = require('fs');

fs.open('./file.txt', 'r', (err, fd) => {
    if (err) throw err;
    console.log(fd)
    fs.close(fd, (err) => {
        if (err) throw err;
    });
});

```
2、	Buffer路径：（经过buffer进行编码过的路径）
``` js
fs.open(Buffer.from('/open/some/file.txt'), 'r', (err, fd) => {
  if (err) throw err;
  fs.close(fd, (err) => {
    if (err) throw err;
  });
});
```
### (二)	文件描述符相关操作
在上述执行fs.open时，在回调函数中具备一个fd参数，此参数为文件描述符，内核为所有进程维护着一张当前打开着的文件与资源的表格。 每个打开的文件都会分配一个名为文件描述符的数值标识。 在系统层，所有文件系统操作使用这些文件描述符来识别与追踪每个特定的文件，fs.open() 方法用于分配一个新的文件描述符。 一旦分配了，文件描述符可用于读取数据、写入数据、或查看文件信息
``` js
fs.open('/open/some/file.txt', 'r', (err, fd) => {
    if (err) throw err;
    fs.fstat(fd, (err, stat) => {
        if (err) throw err;
        // 各种操作
        // 必须关闭文件描述符！
        fs.close(fd, (err) => {
            if (err) throw err;
        });
    });
});
```
1、	fs.fchmod(fd, mode, callback)修改文件权限
2、	fs.fstat(fd[, options], callback) 返回文件的详细信息
3、	fs.ftruncate(fd[, len], callback)文件截取，如果文件描述符指向的文件大于 len 个字节，则只有前面 len 个字节会保留在文件中，如果前面的文件小于 len 个字节，则扩展文件，且扩展的部分用空字节（'\0'）填充
console.log(fs.readFileSync('temp.txt', 'utf8'));
// 输出: Node.js

// 获取要截断的文件的文件描述符
``` js
const fd = fs.openSync('temp.txt', 'r+');
```

// 截断文件至前4个字节
``` js
fs.ftruncate(fd, 4, (err) => {
    assert.ifError(err);
    console.log(fs.readFileSync('temp.txt', 'utf8'));
});
```

// 输出: Node

4.	fs.futimes(fd, atime, mtime, callback) 改变 path 所指向的对象的文件系统时间戳。使用方式跟 fs.utimes()一样
atime 参数和 mtime 参数遵循以下规则：
值可以是 Unix 时间戳数值、Date 对象、或数值字符串如 '123456789.0'。
如果值不能被转换为数值，或值是 NaN 、 Infinity 或 -Infinity，则会抛出错误

5.	fs.read(fd, buffer, offset, length, position, callback) 从 fd 指定的文件中读取数据。
    参数：
    buffer 是数据将被写入到的 buffer。
    offset 是 buffer 中开始写入的偏移量。
    length 是一个整数，指定要读取的字节数。
    position 指定从文件中开始读取的位置。 如果 position 为 null，则数据从当前文件读取位置开始读取，且文件读取位置会被更新。 如果 position 为一个整数，则文件读取位置保持不变。
回调有三个参数 (err, bytesRead, buffer)。

6.	fs.write(fd, buffer[, offset[, length[, position]]], callback)向文件中写入buffer数据，
参数：
offset 决定 buffer 中被写入的部分，length 是一个整数，指定要写入的字节数。
position 指向从文件开始写入数据的位置的偏移量。 如果 typeof position !== 'number'，则数据从当前位置写入。
回调有三个参数 (err, bytesWritten, buffer)，其中 bytesWritten 指定从 buffer 写入了多少字节。
7.	fs.write(fd, string[, position[, encoding]], callback) 写入 string 到 fd 指定的文件，如果 string 不是一个字符串，则该值将被强制转换为一个字符串
参数：
position 指向从文件开始写入数据的位置的偏移量。 如果 typeof position !== 'number'，则数据从当前位置写入。
encoding 是期望的字符串编码。
回调有三个参数 (err, written, string)，其中 written 指定传入的字符串被写入多少字节
8.	fs.close(fd, callback)关闭文件描述符

### (三)	Flags参数：
1.	'r' - 以只读方式打开文件，若文件不存在则报错。
2.	'r+' - 以读写方式打开文件，若文件不存在则报错。
3.	'rs+' 在同步模式下，以读写方式打开文件
4.	'w' - 以写方式打开文件，若文件不存在则创建
5.	'wx' - 以写方式打开文件，若文件不存在则抛出异常.
6.	'w+' - 以读写方式打开文件，若文件不存在则创建，相反则清空文件.
7.	'wx+' - 以读写方式打开文件，若文件不存在则抛出异常.
8.	'a' - 以追加方式打开文件，若文件不存则创建文件
9.	'ax' - 以追加方式打开文件，若文件不存则抛出异常.
10.	'a+' - 以追加和读方式打开文件，若文件不存则创建文件
11.	'ax+' - 以追加和读方式打开文件，若文件不存则失败

(四)	文件夹相关操作：
1.	fs.mkdir(path[, mode], callback) 异步地创建目录。 完成回调只有一个可能的异常参数。 mode 默认为 0o777
2.	fs.mkdtemp(prefix[, options], callback)创建临时目录（用途类似于mkdir），可选的 options 参数可以是一个字符串并指定一个字符编码，或是一个对象且由一个 encoding 属性指定使用的字符编码, 创建的目录路径会作为字符串传给回调的第二个参数

// 新建的临时目录的父目录
``` js
const tmpDir = process.cwd()
```

// 该方法是 *正确的*：
``` js
const { sep } = require('path');
fs.mkdtemp(`${tmpDir}/foo-`, (err, folder) => {
    if (err) throw err;
    console.log(folder);
    // 会输出类似于 `/tmp/abc123`。
    // 一个新的临时目录会被创建在 /tmp 目录里。
});

fs.mkdtemp(`${tmpDir}${sep}`, (err, folder) => {
    if (err) throw err;
    console.log(folder);
    // 会输出类似于 `/tmp/abc123`。
    // 一个新的临时目录会被创建在 /tmp 目录里。
});
```

3.	fs.readdir(path[, options], callback) 读取一个目录的内容。

     回调有两个参数 (err, files)，其中 files 是目录中不包括 '.' 和 '..' 的文件名的数组。
    可选的 options 参数用于传入回调的文件名，它可以是一个字符串并指定一个字符编码，或是一个对象且由一个 encoding 属性指定使用的字符编码。 如果 encoding 设为 'buffer'，则返回的文件名会被作为 Buffer 对象传入。
 注意: 'path' 的路径是以当前文件为基准进行查找的,而不是运行的时候的相对路径
``` js
    fs.readdir("./ep4S2E", (err, data) => {
        console.log(data)
    })
```

4.	fs.rmdir(path, callback) 完成回调只有一个可能的异常参数

    注：只能删除空文件夹，如果文件夹非空，则会报出如下错误：
``` js
    fs.rmdir("./foo-A3V5xg", (err) => {
        console.log(err)
    })
```
(五)	文件和文件夹共用操作
1.	fs.rename(oldPath, newPath, callback) 完成回调只有一个可能的异常参数。文件夹或文件重命名
``` js
fs.rename("./ep4S2E", "./test", (err) => {
    console.log(err)
})
```
2.	fs.stat(path[, options], callback)用来检测某个文件或者文件夹是否存在； 回调函数中包含两个参数 (err, stats) ，其中 stats 为 fs.Stats 对象。
``` js
fs.stat("./test/one2", (err, stats) => {
    if (err) {
        console.log(err)
        return
    }
    console.log(stats)
})
```
3.	fs.access(path[, mode], callback) 测试 path 指定的文件或目录的用户权限。（如果不需要操作文件只是想获取文件是否可用，推荐使用，功能和fs.stat类似）
 mode 参数是一个可选的整数，指定要执行的可访问性检查。 文件访问常量定义了 mode 可选的值。 可以创建由两个或更多个值的位或组成的掩码（例如 fs.constants.W_OK | fs.constants.R_OK）。最后一个参数 callback 是一个回调函数，会传入一个可能的错误参数。 如果可访问性检查失败，则错误参数会是一个 Error 对象
// 检查文件是否存在于当前目录。
``` js
    fs.access(file, fs.constants.F_OK, (err) => {
        console.log(`${file} ${err ? '不存在' : '存在'}`);
    });

    // 检查文件是否可读。
    fs.access(file, fs.constants.R_OK, (err) => {
        console.log(`${file} ${err ? '不可读' : '可读'}`);
    });

    // 检查文件是否可写。
    fs.access(file, fs.constants.W_OK, (err) => {
        console.log(`${file} ${err ? '不可写' : '可写'}`);
    });

    // 检查文件是否存在于当前目录，且是否可写。
    fs.access(file, fs.constants.F_OK | fs.constants.W_OK, (err) => {
        if (err) {
            console.error(
                `${file} ${err.code === 'ENOENT' ? '不存在' : '只可读'}`);
        } else {
            console.log(`${file} 存在，且可写`);
        }
    });
```
4.	fs.chmod(path, mode, callback) 异步地改变文件或文件夹的权限。 完成回调只有一个可能的异常参数。
对于mode的权限具备以下描述：
 
举个例子，八进制值 0o765 表示：
文件所有者可以进行读.写和执行。
文件所属组可以读和写。
其他人可以对文件进行读和执行。
``` js
    fs.chmod("./test/one1", "0o000", (err) => {
        console.log(err)
    })
```
5.	fs.realpath(path[, options], callback)返回一个真实的绝对路径，用途跟process.cwd()类似，注意参数为“utf-8”
``` js
    fs.realpath("./file.txt", "utf-8", (err, data) => {
    console.log(data)
    // C:\data\allCode\bwOther\classDemo\node\test\02\file.txt
    })
```
6.	fs.watch(filename[, options][, listener])文件的监听；
监视 filename 的变化，filename 可以是一个文件或一个目录。 返回的对象是一个 fs.FSWatcher。第二个参数是可选的。 如果提供的 options 是一个字符串，则它指定了 encoding。 否则 options 应该以一个对象传入。监听器回调有两个参数 (eventType, filename)。 eventType 可以是 'rename' 或 'change'，filename 是触发事件的文件的名称。
注意，在大多数平台，当一个文件出现或消失在一个目录里时，'rename' 会被触发。
	当recursive参数为true时，可以进行文件夹递归监听
``` js
    fs.watch("./test", { recursive: true }, (eventType, filename) => {
        console.log(filename)
    })
```
(六)	文件相关操作：
1.	fs.appendFile(path, data[, options], callback) 异步地追加数据到一个文件，如果文件不存在则创建文件。 data 可以是一个字符串或 Buffer。如果 options 是一个字符串，则它指定了字符编码。
``` js
    fs.appendFile('./test/message.txt', 'data to append', 'utf8', (err) => {
        if (err) throw err;
        console.log('The "data to append" was appended to file!');
    });
```

2.	fs.copyFile(src, dest[, flags], callback) 异步的将 src 拷贝到 dest，，如果 dest 已经存在会被覆盖。回调函数没有给出除了异常以外的参数。Node.js 不能保证拷贝操作的原子性。如果目标文件打开后出现错误，Node.js 将尝试删除它。
// 默认情况下，destination.txt 将创建或覆盖
``` js
fs.copyFile('source.txt', 'destination.txt', (err) => {
    if (err) throw err;
    console.log('source.txt was copied to destination.txt');
});

const fs = require('fs');
const { COPYFILE_EXCL } = fs.constants; //此值为整数1
``` 
// 使用 COPYFILE_EXCL ，如果 destination.txt 文件存在，操作将失败。
``` js
fs.copyFile('source.txt', 'destination.txt', COPYFILE_EXCL, (err) => {
    if (err) throw err;
    console.log('source.txt was copied to destination.txt');
});
```
3、	fs.link(existingPath, newPath, callback) 使用此方法创建一个硬链接,功能跟copyFile有点类似，将一个源文件中的内容复制一份放到新的文件中去
参数:
existingPath：源文件地址，文件必须存在
newPath：硬链接的存储位置
callback：完成后的回调
``` js
fs.link('./file.txt',
    './test2/file', (err) => {
        if (err) {
            console.log(err);
        } else {
            console.log('创建硬链接完成');
        }
    });
``` 
4、	fs.lstat(path, callback)和fs.fstat(path, callback)获取文件信息，获取到的是fs.Stats 对象
``` js
fs.lstat('./file.txt', (err, stats) => {
    if (err) {
        console.log(err);
    } else {
        console.log(stats);
    }
});
``` 
5、	fs.readFile(path[, options], callback)option为<Object> | <string>，定义的为字符编码
未指定字符编码，返回为buffer数据流
指定字符编码，返回为字符串
``` js
fs.readFile('./file.txt', "utf-8", (err, data) => {
    if (err) {
        console.log(err);
    } else {
        console.log(data)
    }
});
``` 
6、	fs.truncate(path[, len], callback)截取文件，如果文件描述符指向的文件大于 len 个字节，则只有前面 len 个字节会保留在文件中，如果前面的文件小于 len 个字节，则扩展文件，且扩展的部分用空字节（'\0'）填充

7、	fs.unlink(path, callback)删除文件

8、	fs.watchFile(filename[, options], listener) 监视 filename 的变化。 回调 listener 会在每次访问文件时被调用，

注：fs.watch() 比 fs.watchFile 和 fs.unwatchFile 更高效。 可能的话，应该使用 fs.watch 而不是 
``` js
fs.watchFile 和 fs.unwatchFile。
fs.watchFile('./test/message.txt', (curr, prev) => {
    console.log(`the current mtime is: ${curr.mtime}`);
    console.log(`the previous mtime was: ${prev.mtime}`);
});
``` 
9、	fs.unwatchFile(filename[, listener]) 停止监视 filename 文件的变化
10、	fs.writeFile(file, data[, options], callback) 异步地写入数据到文件，如果文件已经存在，则覆盖文件，data 可以是字符串或 buffer。
如果 options 是一个字符串，则它指定了字符编码
``` js
fs.writeFile('文件名.txt', 'Node.js 中文网', 'utf8', (err) => {
    console.log(err)
});
```
## (七)	文件描述对象：（fs.Stats）

1.      stats.isDirectory()：表示一个文件系统目录，则返回 true
2.      stats.isFile()：表示一个普通文件，则返回 true
3.      stats.isSocket()表示一个 socket，则返回 true
4.      stats.size：文件的字节大小。
5.      stats.atimeMs：文件最后一次被访问的时间戳
6.      stats.mtimeMs：最后一次被修改的时间戳
7.      stats.birthtimeMs：文件的创建时间戳
8.      stats.atime：文件最后一次被访问的时间
9.      stats.mtime：文件最后一次被修改的时间
10.     stats.birthtime：文件的创建时间

## (八)	fs常量
由 fs.constants 输出

1、	文件访问常量(fs.access())
常量	描述
``` js
F_OK	该标志表明文件对于调用进程是可见的。
R_OK	该标志表明文件可被调用进程读取。
W_OK	该标志表明文件可被调用进程写入。
X_OK	该标志表明文件可被调用进程执行。
```
2、	文件打开常量(fs.open())
常量	描述
``` js
O_RDONLY	该标志表明打开一个文件用于只读访问。
O_WRONLY	该标志表明打开一个文件用于只写访问。
O_RDWR	该标志表明打开一个文件用于读写访问。
O_CREAT	该标志表明如果文件不存在则创建一个文件。
O_EXCL	该标志表明如果设置了 O_CREAT 标志且文件已经存在，则打开一个文件应该失败。
O_NOCTTY	该标志表明如果路径是一个终端设备，则打开该路径不应该造成该终端变成进程的控制终端（如果进程还没有终端）。
O_TRUNC	该标志表明如果文件存在且为一个常规文件、且文件被成功打开为写入访问，则它的长度应该被截断至零。
O_APPEND	该标志表明数据会被追加到文件的末尾。
O_DIRECTORY	该标志表明如果路径不是一个目录，则打开应该失败。
O_NOATIME	该标志表明文件系统的读取访问权不再引起相关文件 `atime` 信息的更新。该标志只在 Linux 操作系统有效。
O_NOFOLLOW	该标志表明如果路径是一个符号链接，则打开应该失败。
O_SYNC	该标志表明文件打开用于同步 I/O。
O_DSYNC	该标志标明文件为同步I/O打开，写入操作会等待数据完整性
O_SYMLINK	该标志表明打开符号链接自身，而不是它指向的资源。
O_DIRECT	当设置它时，会尝试最小化文件 I/O 的缓存效果。
O_NONBLOCK	该标志表明当可能时以非阻塞模式打开文件。
```
3、	文件类型常量（fs.Stats）
常量	描述
``` js
S_IRWXU	该文件模式表明可被所有者读取、写入、执行。
S_IRUSR	该文件模式表明可被所有者读取。
S_IWUSR	该文件模式表明可被所有者写入。
S_IXUSR	该文件模式表明可被所有者执行。
S_IRWXG	该文件模式表明可被群组读取、写入、执行。
S_IRGRP	该文件模式表明可被群组读取。
S_IWGRP	该文件模式表明可被群组写入。
S_IXGRP	该文件模式表明可被群组执行。
S_IRWXO	该文件模式表明可被其他人读取、写入、执行。
S_IROTH	该文件模式表明可被其他人读取。
S_IWOTH	该文件模式表明可被其他人写入。
S_IXOTH	该文件模式表明可被其他人执行。
```
### (九)	实现文件夹拷贝：
``` js
const fs = require("fs")
function copyDir(origin, target) {
    // 检查文件是否存在于当前目录。
    fs.access(target, fs.constants.F_OK, (err) => {
        if (!err) {

            rmDir(target)
        }
        fs.mkdir(target, (err) => {
            if (err) {
                console.log(err)
            }
        })
    });

    fs.readdir(origin, (err, data) => {
        if (err) {
            console.log(err)
            return
        }
        data.forEach((i) => {
            let filePath = origin + "/" + i
            let targetFilePath = target + "/" + i
            if (fs.statSync(filePath).isDirectory()) {
                copyDir(filePath, targetFilePath)
            } else {
                // 使用copyFile操作
                //    fs.copyFile(filePath,targetFilePath,(err)=>{
                //        if(err){
                //            return err
                //        }
                //    })
                //使用读取文件，写入文件操作
                fs.readFile(filePath, "utf-8", (err, data) => {
                    if (err) {
                        return err
                    }
                    fs.writeFile(targetFilePath, data, "utf8", (err) => {
                        if (err) {
                            console.log(err)
                            return
                        }
                    })
                })
            }
        })

    })
}

function rmDir(dir) {
    //递归删除文件夹
    data = fs.readdirSync(dir)

    data.forEach((i) => {
        let filePath = dir + "/" + i
        if (fs.statSync(filePath).isDirectory()) {
            rmDir(filePath)
        } else {
            fs.unlinkSync(filePath)
        }
    })
    fs.rmdirSync(dir)
}

copyDir("./src", "./dist")
``` 

八、	Buffer (缓冲)
JavaScript 语言没有读取或操作二进制数据流的机制。 在 Node.js 中，Buffer 类是随 Node 内核一起发布的核心库。Buffer 库为 Node.js 带来了一种存储原始数据的方法，可以让 Node.js 处理二进制数据，每当需要在 Node.js 中处理I/O操作中移动的数据时，就有可能使用 Buffer 库。原始数据存储在 Buffer 类的实例中。一个 Buffer 类似于一个整数数组，但它对应于 V8 堆内存之外的一块原始内存。Buffer 类在 Node.js 中是一个全局变量，因此无需使用 require('buffer').Buffer。
(一)	Buffer 与字符编码
Buffer 实例一般用于表示编码字符的序列，比如 UTF-8 、 UCS2 、 Base64 、或十六进制编码的数据。 通过使用显式的字符编码，就可以在 Buffer 实例与普通的 JavaScript 字符串之间进行相互转换
Demo：
const buf = Buffer.from('runoob', 'ascii');

// 输出 72756e6f6f62
console.log(buf.toString('hex'));

// 输出 cnVub29i
console.log(buf.toString('base64'));
// 输出 cnVub29i
console.log(buf.toString('ascii'));
支持的编码：
1)	ascii - 仅支持 7 位 ASCII 数据。如果设置去掉高位的话，这种编码是非常快的。
2)	utf8 - 多字节编码的 Unicode 字符。许多网页和其他文档格式都使用 UTF-8 。
3)	utf16le - 2 或 4 个字节，小字节序编码的 Unicode 字符。支持代理对（U+10000 至 U+10FFFF）。
4)	ucs2 - utf16le 的别名。
5)	base64 - Base64 编码。
6)	latin1 - 一种把 Buffer 编码成一字节编码的字符串的方式。
7)	binary - latin1 的别名。
8)	hex - 将每个字节编码为两个十六进制字符。
(二)	创建 Buffer 类
方式：
1)	Buffer.alloc(size[, fill[, encoding]])： 返回一个指定大小的 Buffer 实例，如果没有设置 fill，则默认填满 0
2)	Buffer.allocUnsafe(size)： 返回一个指定大小的 Buffer 实例，但是它不会被初始化，所以它可能包含敏感的数据
3)	Buffer.from(array)： 返回一个被 array 的值初始化的新的 Buffer 实例（传入的 array 的元素只能是数字，不然就会自动被 0 覆盖）
4)	Buffer.from(arrayBuffer[, byteOffset[, length]])： 返回一个新建的与给定的 ArrayBuffer 共享同一内存的 Buffer。
5)	Buffer.from(buffer)： 复制传入的 Buffer 实例的数据，并返回一个新的 Buffer 实例
6)	Buffer.from(string[, encoding])： 返回一个被 string 的值初始化的新的 Buffer 实例
Demo：
// 创建一个长度为 10、且用 0 填充的 Buffer。
const buf1 = Buffer.alloc(10);

// 创建一个长度为 10、且未初始化的 Buffer。
// 这个方法比调用 Buffer.alloc() 更快，
// 但返回的 Buffer 实例可能包含旧数据，
// 因此需要使用 fill() 或 write() 重写。
const buf3 = Buffer.allocUnsafe(10);

// 创建一个包含 [0x1, 0x2, 0x3] 的 Buffer。
const buf4 = Buffer.from([1, 2, 3]);

// 创建一个包含 UTF-8 字节 [0x74, 0xc3, 0xa9, 0x73, 0x74] 的 Buffer。
const buf5 = Buffer.from('tést');

// 创建一个包含 Latin-1 字节 [0x74, 0xe9, 0x73, 0x74] 的 Buffer。
const buf6 = Buffer.from('tést', 'latin1');
(三)	写入缓冲区
buf.write(string[, offset[, length]][, encoding])
		参数：
1)	string - 写入缓冲区的字符串。
2)	offset - 缓冲区开始写入的索引值，默认为 0 。
3)	length - 写入的字节数，默认为 buffer.length
4)	encoding - 使用的编码。默认为 'utf8' 
demo：
buf = Buffer.alloc(256);
len = buf.write("www.runoob.com");

console.log("写入字节数 : "+  len);
(四)	读取缓冲区：
buf.toString([encoding[, start[, end]]])
参数：
1)	encoding - 使用的编码。默认为 'utf8' 。
2)	start - 指定开始读取的索引位置，默认为 0。
3)	end - 结束位置，默认为缓冲区的末尾
(五)	将 Buffer 转换为 JSON 对象
buf.toJSON()
(六)	缓冲区合并
Buffer.concat(list[, totalLength])
Demo:
var buffer1 = Buffer.from(('tianqi'));
var buffer2 = Buffer.from(('www.runoob.com'));
var buffer3 = Buffer.concat([buffer1, buffer2]);
console.log("buffer3 内容: " + buffer3.toString());
(七)	拷贝缓冲区
buf.copy(targetBuffer[, targetStart[, sourceStart[, sourceEnd]]])
参数：
1)	targetBuffer - 要拷贝的 Buffer 对象。
2)	targetStart - 数字, 可选, 默认: 0
3)	sourceStart - 数字, 可选, 默认: 0
4)	sourceEnd - 数字, 可选, 默认: buffer.length
(八)	缓冲区裁剪
buf.slice([start[, end]])
(九)	缓冲区长度
buf.length;

九、	Node.js Stream(流)

Stream 是一个抽象接口，Node 中有很多对象实现了这个接口。例如，对http 服务器发起请求的request 对象就是一个 Stream，还有stdout（标准输出）。
Node.js，Stream 有四种流类型：
•	Readable - 可读操作。
•	Writable - 可写操作。
•	Duplex - 可读可写操作.
•	Transform - 操作被写入数据，然后读出结果。
所有的 Stream 对象都是 EventEmitter 的实例。常用的事件有：
•	data - 当有数据可读时触发。
•	end - 没有更多的数据可读时触发。
•	error - 在接收和写入过程中发生错误时触发。
•	finish - 所有数据已被写入到底层系统时触发。
(一)	读取流：
var fs = require("fs");
var data = '';

// 创建可读流
var readerStream = fs.createReadStream('input.txt');

// 设置编码为 utf8。
readerStream.setEncoding('UTF8');

// 处理流事件 --> data, end, and error
readerStream.on('data', function(chunk) {
   data += chunk;
});

readerStream.on('end',function(){
   console.log(data);
});

readerStream.on('error', function(err){
   console.log(err.stack);
});

console.log("程序执行完毕");

(二)	写入流
var fs = require("fs");
var data = '天气很多好 ';

// 创建一个可以写入的流，写入到文件 output.txt 中
var writerStream = fs.createWriteStream('output.txt');

// 使用 utf8 编码写入数据
writerStream.write(data,'UTF8');

// 标记文件末尾
writerStream.end();

// 处理流事件 --> data, end, and error
writerStream.on('finish', function() {
    console.log("写入完成。");
});

writerStream.on('error', function(err){
   console.log(err.stack);
});

console.log("程序执行完毕");
(三)	管道流
var fs = require("fs");

// 创建一个可读流
var readerStream = fs.createReadStream('input.txt');

// 创建一个可写流
var writerStream = fs.createWriteStream('output.txt');

// 管道读写操作
// 读取 input.txt 文件内容，并将内容写入到 output.txt 文件中
readerStream.pipe(writerStream);
console.log("程序执行完毕");

十、	Path路径操作：
(一)	path.basename(path[, ext])返回一个 path 的最后一部分
demo：
	path.basename('/foo/bar/baz/asdf/quux.html');
// 返回: 'quux.html'
path.basename('/foo/bar/baz/asdf/quux.html', '.html');
// 返回: 'quux'
(二)	path.dirname(path) 返回一个 path 的目录名
demo：
path.dirname('/foo/bar/baz/asdf/quux');
// 返回: '/foo/bar/baz/asdf'
(三)	path.extname(path) 返回 path 的扩展名，即从 path 的最后一部分中的最后一个 .（句号）字符到字符串结束。 如果 path 的最后一部分没有 . 或 path 的文件名（见 path.basename()）的第一个字符是 .，则返回一个空字符串。
Demo：
path.extname('index.html');
// 返回: '.html'
path.extname('index.coffee.md');
// 返回: '.md'
path.extname('.index');
// 返回: ''
### (四)	path.format(pathObject) path.format() 方法会从一个对象返回一个路径字符串。 与 path.parse() 相反。
// 如果提供了 `dir`、`root` 和 `base`，则返回 `${dir}${path.sep}${base}`。
// `root` 会被忽略
path.format({
    root: '/ignored',
    dir: '/home/user/dir',
    base: 'file.txt'
});
// 返回: '/home/user/dir/file.txt'
// 如果没有指定 `dir`，则 `root` 会被使用。
// 如果只提供了 `root` 或 `dir` 等于 `root`，则平台的分隔符不会被包含。
// `ext` 会被忽略。
path.format({
    root: '/',
    base: 'file.txt',
    ext: 'ignored'
});
// 返回: '/file.txt'

// 如果没有指定 `base`，则 `name` + `ext` 会被使用。
path.format({
    root: '/',
    name: 'file',
    ext: '.txt'
});
// 返回: '/file.txt'
(五)	path.join([...paths]) 方法使用平台特定的分隔符把全部给定的 path 片段连接到一起，并规范化生成的路径。
path.join('/foo', 'bar', 'baz/asdf', 'quux', '..');
// 返回: '/foo/bar/baz/asdf'
(六)	path.parse(path) path.parse() 方法返回一个对象，对象的属性表示 path 的元素。
``` js
path.parse('C:\\path\\dir\\file.txt');
// 返回:
// { root: 'C:\\',
//   dir: 'C:\\path\\dir',
//   base: 'file.txt',
//   ext: '.txt',
//   name: 'file' }
``` 
(七)	path.relative(from, to) path.resolve() 方法会把一个路径或路径片段的序列解析为一个绝对路径。给定的路径的序列是从右往左被处理的，后面每个 path 被依次解析，直到构造完成一个绝对路径如果没有传入 path 片段，则 path.resolve() 会返回当前工作目录的绝对路径。
path.resolve('/foo/bar', './baz');
// 返回: '/foo/bar/baz'

path.resolve('/foo/bar', '/tmp/file/');
// 返回: '/tmp/file'

十一、	文件编码
(一)	Bom的移除
所谓BOM，全称是Byte Order Mark，它是一个Unicode字符，通常出现在文本的开头，用来标识字节序（Big/Little Endian），除此以外还可以标识编码（UTF-8/16/32）BOM的存在还可能引发一些文件读取以及文件处理问题
在我们通常使用的windows系统中，我发现了一个有趣的现象。我新建一个空的文本文档，点击文件-另存为-编码选择UTF-8，然后保存。此时这个文件明明是空的，却占了3字节大小。原因在于：此时保存的编码方式自动会变为UTF-8 
BOM即byte order mark，具体含义可百度百科或维基百科，UTF-8文件中放置BOM主要是微软的习惯，但是放在别的系统上会出现问题。
不含BOM的UTF-8才是标准形式，UTF-8不需要BOM
带BOM的UTF-8文件的开头会有U+FEFF，所以我新建的空文件会有3字节的大小。

删除Bom:
``` js
var fs = require('fs');
var path = require("path")
    // var paths = path.join(__dirname, "/dir/file.txt");
var paths = path.join(__dirname, "/dir");
console.log(paths)

function readDirectory(dirPath) {
    console.log(fs.statSync(dirPath))
    fs.stat(dirPath, (err, stats) => {
        if (err) {
            console.log('Not Found Path : ', dirPath)
            return
        } else if (stats.isFile()) {
            var buff = fs.readFileSync(dirPath);

            if (buff.length >= 3 && buff[0].toString(16).toLowerCase() == "ef" && buff[1].toString(16).toLowerCase() == "bb" && buff[2].toString(16).toLowerCase() == "bf") {
                //EF BB BF 239 187 191
                console.log('\发现BOM文件：', dirPath, "\n");

                // buff = buff.slice(3);
                fs.writeFile(dirPath, buff.toString(), "utf8");
            } else {
                console.log("未发现bom文件")
            }
        } else if (stats.isDirectory()) {
            var files = fs.readdirSync(dirPath);

            files.forEach(function(file) {
                var filePath = dirPath + "/" + file;
                let stat = fs.statSync(filePath);
                if (stat.isDirectory()) {
                    readDirectory(filePath)
                } else {
                    var buff = fs.readFileSync(filePath);

                    if (buff.length >= 3 && buff[0].toString(16).toLowerCase() == "ef" && buff[1].toString(16).toLowerCase() == "bb" && buff[2].toString(16).toLowerCase() == "bf") {
                        //EF BB BF 239 187 191
                        console.log('\发现BOM文件：', filePath, "\n");

                        // buff = buff.slice(3);
                        fs.writeFile(filePath, buff.toString(), "utf8");
                    } else {
                        console.log("未发现bom文件")
                    }
                }
            })
        }
    })
}
readDirectory(paths);
``` 
### (二)	使用iconv-lite进行文件的编码转换
安装：
``` js
Cnpm install –save-dev iconv-lite
``` 
1、	将buffer数据转换成字符串，并且设置编码
var iconv = require('iconv-lite');
var buf = fs.readFileSync(path.join(__dirname, "./dir/file.txt"))
    // var str = iconv.decode(buf, "GBK")
var str = iconv.decode(buf, "utf-8")

2、	将字符串设置成buffer数据
buf = iconv.encode("Sample input string", 'win1251');

3、	基于http服务进行数据的处理

``` js
http.createServer(function(req, res) {
    var converterStream = iconv.decodeStream('utf-8');
    req.pipe(converterStream);

    converterStream.on('data', function(str) {
        console.log(str); // Do something with decoded strings, chunk-by-chunk.
    });
});
```

## 十二、	http模块
### (一)	http协议：
HTTP--Hyper Text Transfer Protocol，超文本传输协议，是一种建立在TCP上的无状态连接，整个基本的工作流程是客户端发送一个HTTP请求，说明客户端想要访问的资源和请求的动作，服务端收到请求之后，服务端开始处理请求，并根据请求做出相应的动作访问服务器资源，最后通过发送HTTP响应把结果返回给客户端。其中一个请求的开始到一个响应的结束称为事务，当一个事物结束后还会在服务端添加一条日志条目。
详见：通讯协议文档
### (二)	http模块客户端请求api
1、	http.request(options[, callback])请求接口，响应数据；返回一个request对象
参数：
1)	protocol <string> 使用的协议。默认为 http:。
2)	host <string> 请求发送至的服务器的域名或 IP 地址。默认为 localhost。  * hostname <string> host 的别名。为了支持 url.parse()，hostname 优先于 host。
3)	family <number> 当解析 host 和 hostname 时使用的 IP 地址族。 有效值是 4 或 6。当未指定时，则同时使用 IP v4 和 v6。
4)	port <number> 远程服务器的端口。默认为 80。
5)	localAddress <string> 为网络连接绑定的本地接口。
6)	socketPath <string> Unix 域 Socket（使用 host:port 或 socketPath）。
7)	method <string> 指定 HTTP 请求方法的字符串。默认为 'GET'。
8)	path <string> 请求的路径。默认为 '/'。 应包括查询字符串（如有的话）。如 '/index.html?page=12'。 当请求的路径中包含非法字符时，会抛出异常。 目前只有空字符会被拒绝，但未来可能会变化。
9)	headers <Object> 包含请求头的对象。
10)	auth <string> 基本身份验证，如 'user:password' 用来计算 Authorization 请求头。
11)	agent <http.Agent> | <boolean> 控制 Agent 的行为。 可能的值有：
a)	undefined (默认): 对该主机和端口使用 http.globalAgent。
b)	Agent 对象：显式地使用传入的 Agent。
c)	false: 创建一个新的使用默认值的 Agent。
12)	createConnection <Function> 当不使用 agent 选项时，为请求创建一个 socket 或流。 这可以用于避免仅仅创建一个自定义的 Agent 类来覆盖默认的 createConnection 函数。详见 agent.createConnection()。 Any Duplex stream is a valid return value.
13)	timeout <number>: 指定 socket 超时的毫秒数。 它设置了 socket 等待连接的超时时间。

Demo：
//创建请求配置项
``` js
const options = {
    hostname: 'blog.cygdream.com',
    port: 80,
    path: '/api/front/article/getArticleAll',
    method: "get"
};

var http = require('http');
//创建请求
var request = http.request(options, (res) => {
        res.on('data', (chunk) => {
            // console.log(`响应主体: ${chunk}`);
        });
        res.on('end', () => {
            console.log('响应中已无数据。');
        });

    })
    //请求的失败监听
request.on('error', (e) => {
    console.error(`请求遇到问题: ${e.message}`);
});
//设置请求头
request.setHeader('Content-Type', 'application/json');
// 删除请求头
// request.removeHeader(name)
// 写入数据到请求主体，
// request.write("sfsdf");
//结束发送请求
// request.end()

const contentType = request.getHeader('Content-Type')
console.log(contentType)
```

2、	http.get(options[, callback])get方式进行请求数据；使用方式跟request同样，返回一个request对像；

3、	http的客户端request对象详解
    1)	request.getHeader(name)获取请求头
    2)	request.setHeader(name, value) 设置请求头
    3)	request.removeHeader(name) 删除请求头
    4)	request.write(chunk[, encoding][, callback])写入数据到请求主体
    5)	request.end([data[, encoding]][, callback]) 结束请求

(三)	http创建服务端：
1、	http.createServer([options][, requestListener])创建服务
demo:
``` ss
var http = require('http');
var server=http.createServer(function(req, res) {
    res.writeHead(200, { 'Content-Type': 'text/plain; charset=utf-8' });
    res.end("响应页面");
})

server.listen(3000);
```

2、	http的服务端server的api详解：

1)	server.close([callback])停止服务端接收新的连接

2)	server.listen()  返回一个布尔值，标识服务器是否在监听连接，参数为监听端口

3)	server.maxHeadersCount：限制请求头的最大数量，默认为 2000。 如果设为 0，则没有限制。

3、	http服务端的request参数详解：

    request.httpVersion:http协议的版本；

    request.headers：http的请求头

    request.method：http的请求方式

    request.url：http的请求地址

4、	http服务端的response参数详解：

    response.end([data][, encoding][, callback]) 该方法会通知服务器，所有响应头和响应主体都已被发送，即服务器将其视为已完成 response.write(data, encoding) 之后再调用 response.end(callback) 
    response.finished返回一个布尔值，表示响应是否已完成。 默认为 false。 执行 response.end() 之后，该值会变为 true

    response.getHeader(name) 尚未发送到客户端的响应头，例：response.getHeader('content-type');

    response.getHeaders()获取所有的响应头

    response.hasHeader(name) 响应头当前有设置 name 头部，返回 true

    response.removeHeader(name) 移除一个响应头

    response.setHeader(name, value)响应头设置值，如果该响应头已存在，则值会被覆盖response.setHeader() 设置的响应头会与 response.writeHead() 设置的响应头合并，且 response.writeHead() 的优先。

    response.writeHead(statusCode[, statusMessage][, headers]) 发送一个响应头给请求。 状态码是一个三位数的 HTTP 状态码，如 404。 最后一个参数 headers 是响应头。 第二个参数 statusMessage 是可选的状态描述

demo：
``` js
const server = http.createServer((req, res) => {
    res.setHeader('Content-Type', 'text/html');
    res.setHeader('X-Foo', 'bar');
    res.writeHead(200, { 'Content-Type': 'text/plain' });
    res.end('ok');
  });
``` 
response.write(chunk[, encoding][, callback]) 方法会发送一块响应主体。 它可被多次调用，以便提供连续的响应主体片段chunk 可以是一个字符串或一个 buffer。 如果 chunk 是一个字符串，则第二个参数指定如何将它编码成一个字节流。 encoding 默认为 'utf8'。当数据块被刷新时，callback 会被调用；
十三、	url模块：
用来操作地址的模块；
(一)	url实例方法：（下列属性都是new URL()实例的属性）
1.	new URL(input[, base])
demo：
``` js
const { URL } = require('url');
const myURL = new URL('/foo', 'https://example.org/');
// https://example.org/foo
```
2.	url.hash：获取及设置URL的分段(hash)部分
3.	url.host：获取及设置URL的主机(host)部分。
4.	url.hostname：获取及设置URL的主机名(hostname)部分。 url.host和url.hostname之间的区别是url.hostname不 包含端口。
5.	url.href：获取及设置序列化的URL。返回值与url.toString（）和url.toJSON()的相同
6.	url.pathname：获取及设置URL的路径(path)部分。
7.	url.port：获取及设置URL的端口(port)部分。
8.	url.protocol获取及设置URL的协议(protocol)部分。
9.	url.search：获取及设置URL的序列化查询(query)部分部分。

### (二)	url.parse()返回url对象（urlObject）：
urlObject相应的属性有：
1.	urlObject.hash
2.	urlObject.host host 属性是 URL 的完整的小写的主机部分，包括 port（如果有）
3.	urlObject.hostname hostname 属性是 host 组成部分排除 port 之后的小写的主机名部分
4.	urlObject.href href 属性是解析后的完整的 URL 字符串，protocol 和 host 都会被转换为小写的
5.	urlObject.path path 属性是一个 pathname 与 search 组成部分的串接。例如：'/p/a/t/h?query=string' 
6.	urlObject.pathname pathname 属性包含 URL 的整个路径部分例如：'/p/a/t/h'
7.	urlObject.port
8.	urlObject.protocol
9.	urlObject.query query 属性是不含开头 ASCII 问号（?）的查询字符串，或一个被 querystring 模块的 parse() 方法返回的对象。 query 属性是一个字符串还是一个对象是由传入 url.parse() 的 parseQueryString 参数决定的
10.	urlObject.search search 属性包含 URL 的整个查询字符串部分，包括开头的 ASCII 问号字符（?）。
### (三)	url.resolve(from, to)
url.resolve() 方法会以一种 Web 浏览器解析超链接的方式把一个目标 URL 解析成相对于一个基础 URL。
``` js
const url = require('url');
url.resolve('/one/two/three', 'four');         // '/one/two/four'
url.resolve('http://example.com/', '/one');    // 'http://example.com/one'
url.resolve('http://example.com/one', '/two'); // 'http://example.com/two'
```

## 十四、	querystring模块
提供了一些实用函数，用于解析与格式化 URL 查询字符串
### (一)	querystring.escape(str) 对给定的 str 进行 URL 编码，一般是将querystring.stringify()生成的字符串进行编码
### (二)	querystring.parse(str[, sep[, eq[, options]]]) 方法会把一个 URL 查询字符串 str 解析成一个键值对的集合。sep <string> 用于界定查询字符串中的键值对的子字符串。默认为 '&'。eq <string> 用于界定查询字符串中的键与值的子字符串。默认为 '='。
### (三)	querystring.stringify(obj[, sep[, eq[, options]]]) 该方法通过遍历给定的 obj 对象的自身属性，生成 URL 查询字符串，参数跟parse一样，是parse的逆方法
### (四)	querystring.unescape(str) 通过给 querystring.unescape 赋值一个函数来重写解码的实现，默认使用 JavaScript 内置的 decodeURIComponent() 方法来解码，跟escape是逆方法

## 十五、	Zlib模块
提供通过 Gzip 和 Deflate/Inflate 实现的压缩功能
### (一)	基本使用：
``` js
const zlib = require('zlib');
const gzip = zlib.createGzip();
const fs = require('fs');
const inp = fs.createReadStream('file.txt');
const out = fs.createWriteStream('file.txt.gz');

inp.pipe(gzip).pipe(out);
```
### (二)	结合服务端实现压缩文件的请求
Zlib实现压缩时，具备两种算法，一种是gzip，一种是deflate；目前大多数浏览器支持zlib和deflate格式的文件渲染；
1.	服务端代码：
``` js
const zlib = require('zlib');
const http = require('http');
const fs = require('fs');
http.createServer((request, response) => {
    const raw = fs.createReadStream('index.html');
    let acceptEncoding = request.headers['accept-encoding'];
    if (!acceptEncoding) {
        acceptEncoding = '';
    }
    if (/\bdeflate\b/.test(acceptEncoding)) {
        response.writeHead(200, { 'Content-Encoding': 'deflate' });
	 //文件压缩
        raw.pipe(zlib.createDeflate()).pipe(response);
    } else if (/\bgzip\b/.test(acceptEncoding)) {
        response.writeHead(200, { 'Content-Encoding': 'gzip' });
	  //文件压缩
        raw.pipe(zlib.createGzip()).pipe(response);
    } else {
        response.writeHead(200, {});
        raw.pipe(response);
    }
}).listen(1337);
		2、客户端代码
// 客户端请求示例
const zlib = require('zlib');
const http = require('http');
const fs = require('fs');
const request = http.get({
    host: 'localhost',
    path: '/',
    port: 1337,
    headers: { 'Accept-Encoding': 'gzip,deflate' }
});
request.on('response', (response) => {
    const output = fs.createWriteStream('example.com_index.html');

    switch (response.headers['content-encoding']) {
        // 或者, 只是使用 zlib.createUnzip() 方法去处理这两种情况
        case 'gzip':
            //解压
            response.pipe(zlib.createGunzip()).pipe(output);
            break;
        case 'deflate':
            //解压
            response.pipe(zlib.createInflate()).pipe(output);
            break;
        default:
            response.pipe(output);
            break;
    }
});
```
十六、	Websocket协议：

(一)	基于http协议的websocket，实现客户端和服务端的实时通讯，在WebSocket标准没有推出之前，AJAX轮询是一种可行的方案
WebSocket是HTML5新增的一种通信协议，其特点是服务端可以主动向客户端推送信息，客户端也可以主动向服务端发送信息，是真正的双向平等对话，属于服务器推送技术的一种。
在WebSocket API中，浏览器和服务器只需要做一个握手的动作，然后浏览器和服务端之间就形成了一条快速通道，两者之间就直接可以数据相互传送，带来的好处是
1.	相互沟通的Header很小，大概只有2Bytes。
2.	服务器不再被动的接收到浏览器的请求之后才返回数据，而是在有新数据时就主动推送给浏览器。
为了建立一个WebSocket连接，浏览器首先要向服务器发起一个HTTP请求，这个请求和通常的HTTP请求不同，包含了一些附加头信息，其中附加头信息Upgrade: WebSocket表明这是一个申请协议升级的HTTP请求。服务端解析这些头信息，然后产生应答信息返回给客户端，客户端和服务端的WebSocket连接就建立起来了。双方就可以通过这个连接通道自由的传递信息，并且这个连接会持续直到客户端或者服务端的某一方主动关闭连接。
WebSocket与HTTP协议一样都是基于TCP的，所以它们都是可靠的协议，Web开发者调用的WebSocket的send函数在Browser的实现中最终都是通过TCP的系统接口进行传输的。，WebSocket在建立握手连接时，数据是通过HTTP协议传输的。但在建立连接之后，真正的数据传输阶段是不需要HTTP参与的
(二)	Socket.io模块
socket.io提供了基于事件的实时双向通讯，它同时提供了服务端和客户端的API
服务端代码：
1.	创建服务：
1)	基础用法
``` js
var httpServer = require('http').createServer();
var io = require('socket.io')(httpServer);
httpServer.listen(3000);
``` 
2)	Express中：（bin/www）
```js
var app = require('../app');
var debug = require('debug')('nodeclass:server');

var http = require('http');

var port = normalizePort(process.env.PORT || '3000');
var server = http.createServer(app);
//创建不同的端口
// var socketServer=http.createServer().listen("3001")
global.io = require("socket.io")(server)

server.listen(port);
server.on('error', onError);
server.on('listening', onListening);
``` 
2.	实际应用：
1)	服务端向客户端推送数据
/*服务端*/
``` js
// 服务端绑定HTTP服务器实例
let httpServer = require('http').Server();
let io = require('socket.io')(httpServer);
httpServer.listen(3000);

// 服务端监听连接状态：io的connection事件表示客户端与服务端成功建立连接，它接收一个回调函数，回调函数会接收一个socket参数。
io.on('connection', (socket) => {
    console.log('client connect server, ok!');

    // io.emit()方法用于向服务端发送消息，参数1表示自定义的数据名，参数2表示需要配合事件传入的参数
    io.emit('server message', { msg: 'client connect server success' });

    // socket.broadcast.emit()表示向除了自己以外的客户端发送消息
    socket.broadcast.emit('server message', { msg: 'broadcast' });

    // 监听断开连接状态：socket的disconnect事件表示客户端与服务端断开连接
    socket.on('disconnect', () => {
        console.log('connect disconnect');
    });

    // 与客户端对应的接收指定的消息
    socket.on('client message', (data) => {
        cosnole.log(data); // hi server
    });

    socket.disconnect();
});
``` 
/*客户端*/
``` js
<script src="http://cdn.socket.io/stable/socket.io.js"></script>
<script>
// socket.io引入成功后，可通过io()生成客户端所需的socket对象。
let socket = io('http://127.0.0.0:3000');

// socket.emmit()用户客户端向服务端发送消息，服务端与之对应的是socket.on()来接收信息。
socket.emmit('client message', {msg:'hi, server'});

// socket.on()用于接收服务端发来的消息
socket.on('connect',  ()=>{
  console.log('client connect server');
});
socket.on('disconnect', ()=>{
  console.log('client disconnect');
});
</script>

2)	客户端主动向服务端发送数据

/*客户端*/
<script src="http://cdn.socket.io/stable/socket.io.js"></script>
<script>
let socket = io('http://127.0.0.1:3000');

let interval = setTimeInterval(()=>{
  socket.emit('random', Math.random());
}, 500);

socket.on('warn', count=>{
  console.log('warning count : '+count);
});

socket.on('disconnect', ()=>{
  clearInterval(interval);
});
</script>

/*服务端*/
let httpServer = require('http').Server();
let io = require('socket.io')(httpServer);
httpServer.listen(3000);

io.on('connection', socket=>{
  socket.on('random', value=>{
    console.log(value);
    if(value>0.95){
      if(typeof socket.warnign==='undefined'){
        socket.warning = 0;// socket对象可用来存储状态和自定义数据
      }
      setTimeout(()=>{
        socket.emit('warn', ++socket.warning);
      }, 1000);
    }
  });
});
```

十七、	Process全局对象：

process 对象是一个全局变量，它提供当前 Node.js 进程的有关信息，以及控制当前 Node.js 进程。 因为是全局变量，所以无需使用 require()


### (一)	属性：


1、	process.argv：属性返回一个数组，这个数组包含了启动Node.js进程时的命令行参数

2、	process.env属性返回一个包含用户环境信息的对象
process.env.foo = 'bar';
console.log(process.env.foo);

3、	process.execArgv属性返回当Node.js进程被启动时，Node.js特定的命令行选项， 这些选项在process.argv属性返回的数组中不会出现，并且这些选项中不会包括Node.js的可执行脚本名称或者任何在脚本名称后面出现的选项

$ node --harmony script.js --version

返回[‘--harmony’]

4、	process.execPath：process的执行路径。　这个属性会返回启动进程程序的路径

5、	process.exitCode：进程默认的退出码， 或者process.exit(code)指定的退出码

6、	process.version：node编译时的版本号

7、	process.pid：当前node的进程id

8、	process.arch: 当前CPU的架构,如：X64

9、	process.platform ：当前进程的运行平台，如win32

(二)	方法：
1、	process.cwd()返回 Node.js 进程当前工作的目录
2、	process.exit([code])：终止当前进程
demo：
``` js
process.on('exit', function(code) {
    console.log('进程退出码是:%d', code); // 进程退出码是:1
});
  
process.exit(1);
```
3、	process.uptime()：返回node程序已经运行的秒数
4、	process.chdir()：用于改变进程的工作目录
``` js
console.log('当前目录：' + process.cwd());
try {
  process.chdir('/tmp');
  console.log('新目录：' + process.cwd());
}
catch (err) {
  console.log('chdir: ' + err);
}
``` 
//输出如下
当前目录：/Users/liuht/code/itbilu/demo
新目录：/private/tmp
(三)	事件：
process.on('exit', function(code) {
    console.log('进程退出码是:%d', code); // 进程退出码是:1
});

process.exit(1);
十八、	子进程：
node.js是基于单线程模型架构，这样的设计可以带来高效的CPU利用率，但是无法却利用多个核心的CPU，为了解决这个问题，node.js提供了child_process模块，通过多进程来实现对多核CPU的利用. child_process模块提供了四个创建子进程的函数，分别是spawn，exec，execFile和fork

(一)	Api：
1.	child_process.exec(command[, options][, callback]) 启动子进程来执行shell命令,可以通过回调参数来获取脚本shell执行结果
demo：
(1)正常模式：
``` js
index.js:
const { exec } = require('child_process');
exec('node index2.js', (error, stdout, stderr) => {
    if (error) {
        console.error(`exec error: ${error}`);
        return;
    }
    console.log(`stdout: ${stdout}`);
    console.log(`stderr: ${stderr}`);
});
Index2.js:
console.log(122)
``` 
（2）基于util的promise模式：
``` js
Index.js:
const util = require('util');
const exec = util.promisify(require('child_process').exec);

async function lsExample() {
  const { stdout, stderr } = await exec('ls');
  console.log('stdout:', stdout);
  console.log('stderr:', stderr);
}
lsExample();
``` 
2.	child_process.execfile(file[, args][, options][, callback])与exec类型不同的是，它执行的不是shell命令而是一个可执行文件,不衍生一个 shell。 而是，指定的可执行的 file 被直接衍生为一个新进程，这使得它比 child_process.exec() 更高效
demo:
(1)	正常模式：
``` js
Index.js
注意：这里的第一个参数是可运行程序，第二个参数为执行的文件（数组格式）
const { execFile } = require('child_process');
const child = execFile('node', ['index2.js'], (error, stdout, stderr) => {
    if (error) {
        throw error;
    }
    console.log(stdout);
});
Index2.js
console.log(122)
```
(2)	基于unit的promise模式：
``` js
Index.js
const util = require('util');
const execFile = util.promisify(require('child_process').execFile);
async function getVersion() {
    const { stdout } = await execFile('node', ['index2.js']);
    console.log(stdout);
}
getVersion();
``` 
3.	child_process.spawn(command[, args][, options])仅仅执行一个shell命令，不需要获取执行结果，此方法没有回调函数，所以对于执行的文件或者程序，无法通过回调函数获取结果，使用给定的 command 和 args 中的命令行参数来衍生一个新进程。 如果省略 args，则默认为一个空数组。
Demo:
Index.js
``` js
const { spawn } = require('child_process');
const ls = spawn('node', ['index2.js']);

ls.stdout.on('data', (data) => {
    console.log(`stdout: ${data}`);
});

ls.stderr.on('data', (data) => {
    console.log(`stderr: ${data}`);
});

ls.on('close', (code) => {
    console.log(`子进程退出码：${code}`);
});
Index2.js
console.log(122)
```
4.	child_process.fork(modulePath[, args][, options])可以用node执行的.js文件，也不需要获取执行结果。fork出来的子进程一定是node进程，child_process.fork() 方法是 child_process.spawn() 的一个特殊情况，专门用于衍生新的 Node.js 进程。衍生的 Node.js 子进程与两者之间建立的 IPC 通信信道的异常是独立于父进程的。 每个进程都有自己的内存，使用自己的 V8 实例。 由于需要额外的资源分配，因此不推荐衍生大量的 Node.js 进程。
Demo:

Index.js
``` js
var cp = require('child_process');
//只有使用fork才可以使用message事件和send()方法
var n = cp.fork('./index2.js');
n.on('message', function(m) {
    console.log(m);
})
n.send({ "message": "hello" });
index2.js
process.on('message', function(m) {
    console.log(m);
})
process.send({ "message": "hello I am child" })
```

(二)	创建子进程，解决cpu利用率(针对于cpu密集型项目，可以通过创建子进程来解决高量的运算)
``` js
Index.js
const http = require('http');
const [url, port] = ['127.0.0.1', 3000];
const fork = require('child_process').fork;

const server = http.createServer((req, res) => {
    console.log(req.url);

    if (req.url == '/compute') {
        const compute = fork('./index2.js');

        compute.send('开启一个新的子进程');

        /**
         * 当一个子进程使用 process.send() 发送消息时会触发 'message' 事件
         */
        compute.on('message', sum => {
            res.end(`Sum is ${sum}`);

            compute.kill();
        });

        /**
         * 子进程监听到一些错误消息退出
         */
        compute.on('close', (code, signal) => {
            console.log(`收到close事件，子进程收到信号 ${signal} 而终止，退出码 ${code}`);

            compute.kill();
        })
    } else {
        res.end(`ok`);
    }
});

server.listen(port, url, () => {
    console.log(`server started at http://${url}:${port}`);
});

Index2.js
const computation = () => {
    let sum = 0;

    console.info('计算开始');
    console.time('计算耗时');

    for (let i = 0; i < 1e10; i++) {
        sum += i
    };

    console.info('计算结束');
    console.timeEnd('计算耗时');

    return sum;
};

process.on('message', msg => {
    console.log(msg, 'process.pid', process.pid); // 子进程id

    const sum = computation();

    /**
     * 如果Node.js进程是通过进程间通信产生的，那么，process.send()方法可以用来给父进程发送消息
     */
    process.send(sum);
})
``` 
### (三)	子进程守护：
## 十九、	Cluser模块：

Node.js是单线程的，那么如果希望利用服务器的多核的资源的话，就应该多创建几个进程，由多个进程共同提供服务，如果直接采用对于同一端口启动多个服务的话，会提示端口占用，所以Node新增了一个内置模块——“cluster”，故名思议，它可以通过一个父进程管理一堆子进程的方式来实现集群的功能。

### (一)	Api：
1.	事件
1)	Disconnect: 在工作进程的IPC管道被断开后触发本事件。可能导致事件触发的原因包括：工作进程优雅地退出、被kill或手动断开连接（如调用worker.disconnect()）。'disconnect' 和 'exit'事件之间可能存在延迟。这些事件可以用来检测进程是否在清理过程中被卡住，或是否存在长时间运行的连接。
2)	Error：失败监听
3)	Exit: 当任何一个工作进程关闭的时候，cluster模块都将触发'exit'事件。
4)	Fork:当新的工作进程被fork时，cluster模块将触发'fork'事件
5)	listening: 当一个工作进程调用listen()后，工作进程上的server会触发'listening' 事件，同时主进程上的 cluster也会被触发'listening'事件。
6)	message: 当cluster主进程接收任意工作进程发送的消息后被触发, 这个事件仅仅接受两个参数：消息和handle，而没有工作进程对象。
7)	online:当新建一个工作进程后，工作进程应当响应一个online消息给主进程。当主进程收到online消息后触发这个事件。 'fork' 事件和 'online'事件的不同之处在于，前者是在主进程新建工作进程后触发，而后者是在工作进程运行的时候触发
2.	方法
1)	cluster.fork([env]) 衍生出一个新的工作进程。只能通过主进程调用。
2)	cluster.disconnect([callback]) 当所有工作进程都断开连接并且所有handle关闭的时候调用

3.	属性：
1)	cluster.workers：储存了活跃的工作进程对象，id作为key。有了它，可以方便地遍历所有工作进程。只能在主进程中调用。
2)	cluster.worker：当前工作进程对象的引用，对于主进程则无效。
3)	cluster.isWorker：当进程不是主进程时，返回 true
4)	cluster.isMaster：当该进程是主进程时，返回 true

4.	Worker对象（形成的进行对象）
获取：cluster.workers和cluster.worker；
进程方法：
1)	worker.disconnect()：在一个工作进程内，调用此方法会关闭所有的server，并等待这些server的 'close'事件执行，然后关闭IPC管道。在主进程内，会给工作进程发送一个内部消息，导致工作进程自身调用.disconnect()。
2)	worker.kill([signal='SIGTERM']) ：这个方法将会kill工作进程。在主进程中，通过断开与worker.process的连接来实现，一旦断开连接后，通过signal来杀死工作进程。在工作进程中，通过断开IPC管道来实现，然后以代码0退出进程。将导致.exitedAfterDisconnect被设置。为向后兼容，这个方法与worker.destroy()等义
3)	worker.send(message[, sendHandle][, callback])：发送一个消息给工作进程或主进程，也可以附带发送一个handle。
进程属性：
1)	worker.id：每一个新衍生的工作进程都会被赋予自己独一无二的编号，这个编号就是储存在id里面

(二)	借助cluster解决cpu利用：

1.	利用cluster创建服务：
``` js
var cluster = require("cluster");
var http = require("http");
var numCPUs = require('os').cpus().length;

if (cluster.isMaster) {
    console.log("[master] " + "start master......");
    var data = 0;
    for (var i = 0; i < numCPUs; i++) {
        var work_process = cluster.fork();
    }
    cluster.on("listening", function(worker, address) {
        console.log("[master] " + "listening: worker " + worker.id + ", pid:" + worker.process.pid + ",Address:" + address.address + ":" + address.port);
    });
    cluster.on("exit", function(worker, code, signal) {
        console.log('worker ' + worker.process.pid + ' died');
    });
} else {
    http.createServer(function(req, res) {
        console.log("worker" + cluster.worker.id);
        res.end("worker" + cluster.worker.id);
    }).listen(30001);
}
```

2.	利用进行siege压力测试
``` js
var siege = require('siege')
siege() // node server.js为服务启动脚本
    .wait(3000) //延迟时间 
    .on(30001) //被压测的服务端口
    .concurrent(10) //并发数
    .for(100).times //或者.seconds 
    .get('/') //需要压测的页面
    .attack() //执行压测
```
## 二十、	异步编程：
### (一)	Js的异步运行机制：详见h5课程的js运行机制
### (二)	Promise
1.	简介：

    Promise 是异步编程的一种解决方案，比传统的解决方案——回调函数和事件——更合理和更强大。它由社区最早提出和实现，ES6 将其写进了语言标准，统一了用法，原生提供了Promise对象。

2.	特点：

1)	对象的状态不受外界影响。Promise对象代表一个异步操作，有三种状态：pending（进行中）、fulfilled（已成功）和rejected（已失败）

2)	一旦状态改变，就不会再变，任何时候都可以得到这个结果

3.	使用：

1)	New Promise((resolve,reject)=>{})

2)	Then():then方法返回的是一个新的Promise实例（注意，不是原来那个Promise实例）。因此可以采用链式写法，即then方法后面再调用另一个then方法。

3)	Catch（）：方法是.then(null, rejection)的别名，用于指定发生错误时的回调函数。

4)	Promise.all（）：方法用于将多个 Promise 实例，包装成一个新的 Promise 实例，参数为数组和cb
5)	Promise.race()：Promise.race方法同样是将多个 Promise 实例，包装成一个新的 Promise 实例。有一个实例率先改变状态，p的状态就跟着改变。那个率先改变的 Promise 实例的返回值，就传递给p的回调函数。

6)	Promise.resolve：有时需要将现有对象转为 Promise 对象，Promise.resolve方法就起到这个作用
Promise.resolve('foo')// 等价于new Promise(resolve => resolve('foo'))


7)	Promise.reject(reason)方法也会返回一个新的 Promise 实例，该实例的状态为rejected。
### (三)	手写promise：
``` js
// 在第一个promise中进行reject，则后续所有的then函数都不在执行
function promiseTest(initFn){
    // resolve成功函数
    var count=0      //纪录then函数成功执行的次数
    var TestPM=null  //保存每次成功函数的返回值
    var state=true   //如果有一个then函数返回promise，则递归函数不再执行
    var resolveFn=function(data){
        // 递归执行回掉函数
        function run(i){
            var num=CallbackArr.length
            // 将后续所有的then方法都挂载新的promise上
            var arr=CallbackArr.slice(i)
            // 检测当前的返回值是否属于promiseTest类
            if(TestPM instanceof promiseTest){
                arr.forEach((j)=>{
                    TestPM.then(j.okCB,j.errCB)
                })
                state=false
                // 检测当前是否有返回值
            }else if(TestPM){
                CallbackArr[i].okCB(TestPM)
                // 进行正常的执行
            }else{
                // console.log(CallbackArr[i])
                TestPM=CallbackArr[i].okCB(data)
            }
            count++

                if(count<num&&state){
                    // console.log(count,num)
                    run(count)
                }
        }
        run(count)
    }
    // reject失败函数
    var rejectFn=function(err){
        // 1、如果有一个reject将会中断代码的执行
        CallbackArr[count].errCB(err)
        // 2、如果捕捉错误，但是不阻断代码的执行,继续执行下一个then方法中的成功函数
        count++
        var num=CallbackArr.length
        if(count<num){
                resolveFn()
        }
    }

    // promise函数进行实例化时出入的参数
    initFn(resolveFn,rejectFn)

    // then函数的存储
    var CallbackArr=[]
    // then方法的对外接口
    this.then=function(okCallback,errCallback){
        var watch={
            okCB:okCallback,
            errCB:errCallback
        }
        CallbackArr.push(watch)
        //then函数返回当前的promise对象
        return this
    }
}

// 扩展all方法
promiseTest.all=function(){
        // 将获取到的参数转换成数组
        var promiseArr=Array.prototype.slice.apply(arguments)
        var arrLength=promiseArr.length
        // 用count来纪录每一项的值
        var count=0; 
        var result=[]  //用来存储每个pormise的结果

        // 返回一个新的promise来进行状态的捕捉
        return new promiseTest((resolve,reject)=>{
            var run=function(){
                promiseArr[count].then((data)=>{
                    result.push(data)
                    test()
                },(err)=>{
                    reject(err)
                })  
                count++
                if(count<arrLength){
                    run()
                }
            }
            var test=function(){
                if(result.length==arrLength){
                    resolve(result)
                }
            }

            run()
        })
}

// 检测all方法
var pro1=new promiseTest(function(resolve,reject){
    setTimeout(()=>{
        // resolve(1)
        reject("失败了1")
    },2000)
})
var pro2=new promiseTest(function(resolve,reject){
    setTimeout(()=>{
        // resolve(2)
        reject("失败了2")
    },4000)
})
var pro3=new promiseTest(function(resolve,reject){
    setTimeout(()=>{
        // resolve(4)
        reject("失败了3")
    },3000)
})

// 
// promiseTest.all(pro1,pro2,pro3).then((data)=>{
//  console.log(data)
// },(err)=>{
//  console.log(err)
// })

// 扩展race方法
promiseTest.race=function(){
        // 将获取到的参数转换成数组
        var promiseArr=Array.prototype.slice.apply(arguments)
        var arrLength=promiseArr.length
        // 用count来纪录每一项的值
        var count=0; 
        var resultErr=[]  //用来存储每个pormise的错误的结果
        var resultOk=[]
        // 返回一个新的promise来进行状态的捕捉
        return new promiseTest((resolve,reject)=>{
            var run=function(){
                promiseArr[count].then((data)=>{
                    resultOk.push(data)
                    test()
                },(err)=>{
                    resultErr.push(err)
                    testErr()
                })  
                count++
                if(count<arrLength){
                    run()
                }
            }
            // 检测如果成功的数组中有一个结果，立即执行resolve函数，并且数组长度只有为1时，才会执行；
            var test=function(){
                if(resultOk.length==1){
                    resolve(resultOk[0])
                }
            }
            var testErr=function(){
                if(resultErr.length==arrLength){
                    reject(resultErr)
                }
            }

            run()
        })
}

promiseTest.race(pro1,pro2,pro3)
.then((data)=>{
    console.log(data)
},(err)=>{
    console.log(err)
})

// new promiseTest(function(resolve,reject){
//  setTimeout(()=>{
//      resolve(1)
//  },2000)

// }).then((data)=>{
//  console.log(data)
// })
``` 

### 二十一、	Nginx实现负载均衡
大型项目讲解：
1.	官网下载：http://nginx.org/en/download.html，此处下载版本为：1.12.2，然后解压
 
2.	打开conf文件夹下的config.conf文件进行如下配置：
``` js
将80端口代理到4个不同的端口
server {
    listen       80;
    server_name  localhost;
    location / {
        proxy_pass   http://127.0.0.1:3001/;
        proxy_pass   http://127.0.0.1:3002/;
        proxy_pass   http://127.0.0.1:3003/;
        proxy_pass   http://127.0.0.1:3004/;
    }
}
```
3.	运行程序

## 二十二、	异常处理：

因为nodejs是单线程的，所以一旦发生错误或异常，如果没有及时被处理整个系统就会崩溃。错误异常有两种场景的出现，一种是代码运行中throw new error没有被捕获，另一种是Promise的失败回调函数，没有对应的reject回调函数处理，针对这两种情况Nodejs都有默认的统一处理方式，就是给整个进程process对象监听相应的错误事件
(一)	Try{}catch{}(捕获同步代码)
``` js
try {
    throw new Error('Catch me');
} catch (e) {
    // error captured
    console.log(e)
}
```
### (二)	uncaughtException（基于process的全局监听事件）
由于 NodeJS 的异步特性在现实世界里，异常总是会产生在某个模块中。所谓模块就是能完成一个功能的单元，即使是一个简单的函数也可以被看做一个模块。随着项目代码行数增多，异步嵌套的复杂性加强，经常会有异常没捕获的情况发生。一个没有很强健壮性的 NodeJS 应用，会因为一个未捕获的异常就整个挂掉，导致服务不可用。
uncaughtException虽然能够捕获异常，但是此时错误的上下文已经丢失，即使看到错误也不知道哪儿报的错，定位问题非常的不利。而且一旦uncaughtException事件触发，整个node进程将crash掉，如果不做一些善后处理的话会导致整个服务挂掉，这对于线上的服务来说将是非常不好的
``` js
try {
    setTimeout(function my_app() {
        throw new Error('Catch me');
    })
} catch (e) {
    // never called
}
process.on('uncaughtException', (err) => {
})
``` 
(三)	如果基于async和promise的同步错误可以使用其内置的catch进行捕获，或者如果借助promise解决异步，当异步发生错误时，可以用reject进行状态标记，同样可以使用catch进行捕获
``` js
function a() {
    return new Promise((resolve, reject) => {
        throw new Error("这里出了一个错误")
        setTimeout(() => {
            resolve(122)
        })
    })
}
async function all() {
    await a()
}
all().catch((err) => {

})
``` 
(四)	Domain 模块
Node.js Domain(域) 简化异步代码的异常处理，可以捕捉处理try catch无法捕捉的异常，domain模块，把处理多个不同的IO的操作作为一个组。注册事件和回调到domain，当发生一个错误事件或抛出一个错误时，domain对象会被通知，不会丢失上下文环境，也不导致程序错误立即退出，与process.on('uncaughtException')不同。
1.	Domain 模块可分为隐式绑定和显式绑定：
•	隐式绑定: 把在domain上下文中定义的变量，自动绑定到domain对象
•	显式绑定: 把不是在domain上下文中定义的变量，以代码的方式绑定到domain对象
Demo：
``` js
var EventEmitter = require("events").EventEmitter;
var domain = require("domain");
var domain2 = domain.create(); //创建域
//域进行监听错误
domain2.on('error', function(err) {
    console.log("domain2 处理这个错误 (" + err.message + ")");
});
``` 
// 隐式绑定
domain2.run(function() {
    var emitter2 = new EventEmitter();
    setTimeout(function my_app() {
        throw new Error('Catch me');
    })
    emitter2.emit('error', new Error('通过 domain2 处理'));
});

2.	Api：
1)	domain.run(function) 在域的上下文运行提供的函数，隐式的绑定了所有的事件分发器，计时器和底层请求。
2)	domain.add(emitter) 显式的增加事件
3)	domain.remove(emitter) 删除事件
4)	domain.create()返回一个domain对象。

## 二十三、	express
express 是一个简洁而灵活的 node.js Web应用框架, 提供了一系列强大特性帮助你创建各种 Web 应用，和丰富的 HTTP 工具。使用 Express 可以快速地搭建一个完整功能的网站。

(一)	基于express搭建服务：
``` js
var express = require('express');
var app = express();
app.get('/', function(req, res) {
    res.send('Hello World');
})
var server = app.listen(3000, function() {
    var host = server.address().address
    var port = server.address().port
    console.log("应用实例，访问地址为 http://localhost:" + port)
})
```
(二)	Express路由的配置：
1.	路由方法：
``` js
//get请求
app.get('/', function(req, res) {
        console.log("主页 GET 请求");
        res.send('Hello GET');
    })
    //  POST 请求
app.post('/', function(req, res) {
    console.log("主页 POST 请求");
    res.send('Hello POST');
})
``` 
2.	请求路径：
``` js
//匹配所有的路径，此路由应该放在所有路由的下方，否则会拦截所有路由
app.get('/', function(req, res) {
        res.send('root')
    })
    //匹配/about
app.get('/about', function(req, res) {
        res.send('about')
    })
    //匹配/acd或者/abcd
app.get('/ab?cd', function(req, res) {
        res.send('ab?cd')
    })
    //匹配/abcd或/abbcd或/abbbcd(至少有一个b)
app.get('/ab+cd', function(req, res) {
        res.send('ab+cd')
    })
    //匹配/abcd,/absdcd等等，中间可以为任何字符，任何位数
app.get('/ab*cd', function(req, res) {
    res.send('ab*cd')
})

    // 匹配 /abe 和 /abcde
app.get('/ab(cd)?e', function(req, res) {
    res.send('ab(cd)?e');
});
// 匹配 butterfly、dragonfly，不匹配 butterflyman、dragonfly man等
app.get(/.*fly$/, function(req, res) {
    res.send('/.*fly$/');
});
```
3.	路由句柄
路由句柄有多种形式，可以是一个函数、一个函数数组，或者是两者混合
1)	使用一个回调函数处理路由：
``` js
app.get('/example', function(req, res) {
    res.send('Hello from A!');
});
```

2)	使用多个回调函数处理路由（记得指定 next 对象）：
``` js
app.get('/example/b', function(req, res, next) {
    console.log('response will be sent by the next function ...');
    next();
}, function(req, res) {
    res.send('Hello from B!');
});
```
3)	使用回调函数数组处理路由：
``` js
var cb0 = function(req, res, next) {
    console.log('CB0');
    next();
}
var cb1 = function(req, res, next) {
    console.log('CB1');
    next();
}
var cb2 = function(req, res) {
    res.send('Hello from C!');
}
app.get('/example/c', [cb0, cb1, cb2]);
```
4)	混合使用函数和函数数组处理路由：
``` js
var cb0 = function(req, res, next) {
    console.log('CB0');
    next();
}
var cb1 = function(req, res, next) {
    console.log('CB1');
    next();
}
app.get('/example/d', [cb0, cb1], function(req, res, next) {
    console.log('response will be sent by the next function ...');
    next();
}, function(req, res) {
    res.send('Hello from D!');
});
``` 
5)	区分next()和next(router)

Demo1:此时执行结果为1234
``` js
app.get('/', function(req, res, next) {
    console.log('1');
    next();
}, function(req, res, next) {
    console.log("2")
    next(router)
});
app.get('/', function(req, res, next) {
    console.log('3');
    next();
}, function(req, res, next) {
    console.log("4")
});
Demo2:此时执行结果为134
app.get('/', function(req, res, next) {
    console.log('1');
    next(router);
}, function(req, res, next) {
    console.log("2")
    next(router)
});
app.get('/', function(req, res, next) {
    console.log('3');
    next();
}, function(req, res, next) {
    console.log("4")
});
```
4.	Request的api：

1)	req.app：当callback为外部文件时，用req.app访问express的实例
2)	req.baseUrl：获取路由当前安装的URL路径
3)	req.body / req.cookies：获得「请求主体」/ Cookies（借助中间件）
4)	req.fresh / req.stale：判断请求是否还「新鲜」
5)	req.hostname / req.ip：获取主机名和IP地址
6)	req.originalUrl：获取原始请求URL
7)	req.params：获取路由的parameters
8)	req.path：获取请求路径
9)	req.protocol：获取协议类型
10)	req.query：获取URL的查询参数串
11)	req.route：获取当前匹配的路由
12)	req.subdomains：获取子域名
13)	req.accepts()：检查可接受的请求的文档类型
14)	req.acceptsCharsets / req.acceptsEncodings / req.acceptsLanguages：返回指定字符集的第一个可接受字符编码
15)	req.get()：获取指定的HTTP请求头
16)	req.is()：判断请求头Content-Type的MIME类型
5.	response的api：

1)	res.app：同req.app一样
2)	res.append()：追加指定HTTP头
3)	res.set()在res.append()后将重置之前设置的头
4)	res.cookie(name，value [，option])：设置Cookie
5)	opition: domain / expires / httpOnly / maxAge / path / secure / signed
6)	res.clearCookie()：清除Cookie
7)	res.download()：传送指定路径的文件
8)	res.get()：返回指定的HTTP头
9)	res.json()：传送JSON响应
10)	res.jsonp()：传送JSONP响应
11)	res.location()：只设置响应的Location HTTP头，不设置状态码或者close response
12)	res.redirect()：设置响应的Location HTTP头，并且设置状态码302
13)	res.render(view,[locals],callback)：渲染一个view，同时向callback传递渲染后的字符串，如果在渲染过程中有错误发生next(err)将会被自动调用。callback将会被传入一个可能发生的错误以及渲染后的页面，这样就不会自动输出了。
14)	res.send()：传送HTTP响应
15)	res.sendFile(path [，options] [，fn])：传送指定路径的文件 -会自动根据文件extension设定Content-Type
16)	res.set()：设置HTTP头，传入object可以一次设置多个头
17)	res.status()：设置HTTP状态码
18)	res.type()：设置Content-Type的MIME类型
6.	app.router和express.router的区别：
1)	app.router无法进行模块化分解，无法实现中间件的插入，适用于单例路由；
``` js
app.get('/example', function(req, res) {
    res.send('Hello from A!');
});
```
2)	express.router：
使用express.Router该类创建模块化，可安装的路由处理程序。一个Router实例是一个完整的中间件和路由系统; 因此，它通常被称为“迷你应用程序”。
``` js
var express = require('express')
var router = express.Router()

// middleware that is specific to this router
router.use(function timeLog(req, res, next) {
        console.log('Time: ', Date.now())
        next()
    })
    // define the home page route
router.get('/', function(req, res) {
        res.send('Birds home page')
    })
    // define the about route
router.get('/about', function(req, res) {
    res.send('About birds')
})

module.exports = router
``` 

7.	中间件的使用：
1)	应用级别的中间件：
A.	无路径：
``` js
var app = express()
    //无路径的中间件，会将所有的请求，都必须经过此中间件
app.use(function(req, res, next) {
    console.log('Time:', Date.now())
    next() //此中间件处理后进行相匹配的路径路由
})
B  有路径：

//此中间件具备两层处理函数
app.use('/user/:id', function(req, res, next) {
        console.log('Request URL:', req.originalUrl)
        next()
    }, function(req, res, next) {
        console.log('Request Type:', req.method)
        next()
    })
    //
app.get('/user/:id', function(req, res, next) {
    console.log('ID:', req.params.id)
    next()
}, function(req, res, next) {
    res.send('User Info')
})
```

2)	路由级别的中间件
``` js
var app = express()
var router = express.Router()

// a middleware function with no mount path. This code is executed for every request to the router
router.use(function (req, res, next) {
  console.log('Time:', Date.now())
  next()
})

// a middleware sub-stack shows request info for any type of HTTP request to the /user/:id path
router.use('/user/:id', function (req, res, next) {
  console.log('Request URL:', req.originalUrl)
  next()
}, function (req, res, next) {
  console.log('Request Type:', req.method)
  next()
})

// a middleware sub-stack that handles GET requests to the /user/:id path
router.get('/user/:id', function (req, res, next) {
  // if the user ID is 0, skip to the next router
  if (req.params.id === '0') next('route')
  // otherwise pass control to the next middleware function in this stack
  else next()
}, function (req, res, next) {
  // render a regular page
  res.render('regular')
})

// handler for the /user/:id path, which renders a special page
router.get('/user/:id', function (req, res, next) {
  console.log(req.params.id)
  res.render('special')
})

// mount the router on the app
app.use('/', router)
```
3)	错误处理中间件
``` js
app.use(function (err, req, res, next) {
    console.error(err.stack)
    res.status(500).send('Something broke!')
})
``` 
4)	内置中间件
A.	提供静态资源
``` js
app.use(express.static(path.join(__dirname, 'public')));
``` 
B.	使用JSON有效负载解析传入的请求
``` js
app.use(express.json());
``` 
C.	用URL编码的有效负载解析传入的请求
``` js
app.use(express.urlencoded({ extended: false }));
```

5)	第三方中间件

A.	安装和加载cookie解析中间件功能cookie-parser
``` js
var cookieParser = require('cookie-parser')

// load the cookie-parsing middleware
app.use(cookieParser())
``` 
B.	解析body的中间件
``` js
var bodyParser = require("body-parser")
app.use(bodyParser());
```
C.	解决跨域的中间件：
``` js
var cors = require("cors")
app.use(cors())
```
二十四、	模板引擎：

模板引擎（这里特指用于Web开发的模板引擎）是为了使用户界面与业务数据（内容）分离而产生的，它可以生成特定格式的文档，用于网站的模板引擎就会生成一个标准的HTML文档。

(一)	配置：
1.	下载
``` js
 npm install ejs –save-dev
``` 
2.	配置express的ejs中间件
``` js
app.set('views', path.join(__dirname, 'views'));
app.set('view engine', 'ejs');
```
3.	书写.ejs模板
``` js
<!DOCTYPE html>
<html>
  <head>
    <title><%= title %></title>
    <link rel='stylesheet' href='/stylesheets/style.css' />
  </head>
  <body>
    <h1><%= title %></h1>
    <p>Welcome to <%= title %></p>
  </body>
</html>
```
4.	路由配置
``` js
var express = require('express');
var router = express.Router();

/* GET home page. */
router.get('/', function(req, res, next) {
  res.render('index', { title: 'Express' });
});

module.exports = router;
```


(二)	借助ejs实现错误页面的搭建
``` js
app.use(function(err, req, res, next) {
    // set locals, only providing error in development
    res.locals.message = err.message;
    res.locals.error = req.app.get('env') === 'development' ? err : {};

    // render the error page
    res.status(err.status || 500);
    res.render('error');
});
``` 
Error.ejs:

<h1><%= message %></h1>
<h2><%= error.status %></h2>
<pre><%= error.stack %></pre>

## 二十五、	数据库操作：

### (一)	数据库的连接与配置
1、	链接数据库配置信息
Config.js
``` js
module.exports = {
    sql_config: {
        host: "127.0.0.1",
        user: "root",
        password: "123456",
        port: 3306,
        database: "exam"
    }
}
``` 
2、	node与mysql的普通连接
``` js
// 与mysql的普通链接
function query(sql, callback) {
    var connection = mysql.createConnection(config.mysql_dev);
    connection.connect(function(err) { console.log(err) });
    connection.query(sql, function(qerr, rows, fields) {
        //关闭数据库连接  
        connection.end(function(err) { console.log(err) });
        //事件驱动回调  
        callback(qerr, rows, fields);
    });	
}
```
3、	node链接mysql创建连接池
``` js
let connection=mysql.createPool(config.dev_sql_config)
const query=(sql)=>{
  return new Promise((resolve,reject)=>{
    connection.getConnection((err,connect)=>{
      connect.query(sql,(sqlerr,rows,fields)=>{
        if(err){
          console.log(err) 
          reject(err)
          return
        }
        resolve(rows)
//释放连接
        connect.release()
      })
    })
  })
}
```
### (二)	常用数据库操作语句
1、	表查询语句
字符串查询范围
``` js
select * from school where id=’aa’
```
数值查询范围
``` js
select id,name from school where num>1
```
模糊查询
``` js
select * from school where name like ‘% 陈%’
```
数据截取(截取前十条数据)
``` js
select * from school limit 0,10
```
数据查询排序(desc降序，asc升序)
``` js
select * from school limit 0,10 order by desc 
```
表连接查询（多表查询，表结构相同）
``` js
"select * from (select * from table_a UNION ALL select * from table_b)as tabel_all "
```
2、	表插入语句
单纯插入表数据
``` js
insert into tabel_a(id,name) values(1,”chen”)
```
创建新表，并复制另外一个表格中的数据
``` js
select id,name into tabel_b from table_a
```
复制另外一个表中数据进行插入
``` js
insert into tabte_b(id,name) select id，name from tabel_a
```
3、	删除表数据
``` js
delete from class_one_list where id=‘1’
```
4、	更新表数据
``` js
update class_one_list set twoListNum=’1’,artListNum=’2’ where id=’2’
```
5、	创建表
``` js
CREATE TABLE person (number INT(11), name VARCHAR(255), birthday DATE);
``` 
6、	修改表名
``` js
alter table table_name rename table_new_name
```
7、	删除表
``` js
DROP TABLE  tbl_name;
```

二十六、	api汇总：
(一)	fs：
5.  fs.access(path[, mode], callback)	判断用户是否有权限操作给定的目录或者是文件
6.  fs.accessSync(path[, mode])	判断用户是否有权限操作给定的目录或者是文件
7.  fs.appendFile(file, data[, options], callback)	将数据异步附加到文件
8.  fs.appendFileSync(file, data[, options])	将数据异步附加到文件
9.  fs.chmod(path, mode, callback)	修改文件夹权限
1.  fs.chownSync(path, uid, gid)	修改文件夹权限
2.  fs.chown(path, uid, gid, callback)	更改文件夹所有权
3.  fs.close(fd, callback)	关闭已打开的文件
4.  fs.closeSync(fd)	关闭已打开的文件
5.  fs.createReadStream(path[, options])	返回一个新的可读流对象
6.  fs.createWriteStream(path[, options])	返回一个新的可写流对象
7.  fs.fchmod(fd, mode, callback)	更改文件权限
8.  fs.fchmodSync(fd, mode)	更改文件权限
9.  fs.fchown(fd, uid, gid, callback)	更改文件所有权
10. fs.fchownSync(fd, uid, gid)	更改文件所有权
11. fs.fdatasync(fd, callback)	刷新数据到磁盘
12. fs.fdatasyncSync(fd)	刷新数据到磁盘
13. fs.fstat(fd, callback)	返回文件的详细信息
14. fs.fstatSync(fd)	返回文件的详细信息
15. fs.fsync(fd, callback)	同步缓存数据到磁盘
16. fs.fsyncSync(fd)	同步缓存数据到磁盘
17. fs.ftruncate(fd, len, callback)	截取文件内容
18. fs.ftruncateSync(fd, len)	截取文件内容
19. fs.futimes(fd, atime, mtime, callback)	更改一个文件所提供的文件描述符引用的文件的时间戳
20. fs.futimesSync(fd, atime, mtime)	更改一个文件所提供的文件描述符引用的文件的时间戳
21. fs.lchmod(path, mode, callback)	更改文件权限(不解析符号链接)
22. fs.lchmodSync(path, mode)	更改文件权限(不解析符号链接)
23. fs.lchown(path, uid, gid, callback)	更改文件所有权（不解析符号链接）
24. fs.lchownSync(path, uid, gid)	更改文件所有权（不解析符号链接）
25. fs.link(existingPath, newPath, callback)	创建硬链接(只能在本券中)
26. fs.linkSync(existingPath, newPath)	创建硬链接(只能在本券中)
27. fs.lstat(path, callback)	获取文件信息(不解析符号链接)
28. fs.lstatSync(path)	获取文件信息(不解析符号链接)
29. fs.mkdir(path[, mode], callback)	创建文件目录，如果目录已存在，将抛出异常
30. fs.mkdirSync(path[, mode])	创建文件目录，如果目录已存在，将抛出异常
31. fs.mkdtemp(prefix[, options], callback)	创建临时目录
32. fs.mkdtempSync(prefix[, options])	创建临时目录
33. fs.open(path, flags[, mode], callback)	打开文件
34. fs.openSync(path, flags[, mode])	打开文件
35. fs.read(fd, buffer, offset, length, position, callback)	读取文件内容
36. fs.readSync(fd, buffer, offset, length, position)	读取文件内容，返回字节数
37. fs.readdir(path[, options], callback)	读取文件目录
38. fs.readdirSync(path[, options])	读取文件目录
39. fs.readFile(file[, options], callback)	读取文件
40. fs.readFileSync(file[, options])	读取文件
41. fs.readlink(path[, options], callback)	读取软连接信息
42. fs.readlinkSync(path[, options])	读取软连接信息
43. fs.realpath(path[, options], callback)	获取真实路径
44. fs.realpathSync(path[, options])	获取真实路径
45. fs.rename(oldPath, newPath, callback)	重命名路径
46. fs.renameSync(oldPath, newPath)	重命名路径
47. fs.rmdir(path, callback)	删除文件目录
48. fs.stat(path, callback)	删除文件目录
49. fs.rmdirSync(path)	获取文件信息
50. fs.statSync(path)	获取文件信息
51. fs.symlink(target, path[, type], callback)	创建符号链接
52. fs.symlinkSync(target, path[, type])	创建符号链接
53. fs.truncate(path, len, callback)	文件内容截取操作
54. fs.truncateSync(path, len)	文件内容截取操作
55. fs.unlink(path, callback)	删除文件操作
56. fs.unlinkSync(path)	删除文件操作
57. fs.unwatchFile(filename[, listener])	解除文件监控
58. fs.utimes(path, atime, mtime, callback)	修改文件时间戳
59. fs.utimesSync(path, atime, mtime)	修改文件时间戳
60. fs.watch(filename[, options][, listener])	监控文件
61. fs.watchFile(filename[, options], listener)	监控文件
62. fs.write(fd, buffer, offset, length[, position], callback)	向文件写数据
63. fs.writeSync(fd, buffer, offset, length[, position])	向文件写数据
64. fs.write(fd, data[, position[, encoding]], callback)	想文件写数据
65. fs.writeSync(fd, data[, position[, encoding]])	想文件写数据
66. fs.writeFile(file, data[, options], callback)	想文件写数据
67. fs.writeFileSync(file, data[, options])	想文件写数据