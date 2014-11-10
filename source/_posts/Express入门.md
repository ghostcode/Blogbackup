title: Express入门
date: 2014-11-01 20:41:46
tags: Express
---
此文为翻译，删改了一些东西。

1.Express安装

假设你对Node和npm已经有些熟悉，新建一个文件夹：node

1.1第一种方法：

首先在node文件夹里，我们新建__package.json__来配置Express应用的一些信息。

```
{
    "name": "demo1",
    "description": "First Express app",
    "version": "0.0.1",
    "dependencies": {
       "express": "^4.10.1",
    }
}
```

<!-- more -->

然后到终端里面输入：

```
npm install
```

然后程序就会自动安装Express。

1.2第二种方法：

在终端cd node然后输入：

	npm init

然后一路回车，最后输入__yes__，会自动生成__package.json__文件。

2.创建程序入口文件：app.js

```
var express = require('express');
var app = express();
app.listen(3000);
```

在我学习一个新框架时，总会把节奏放的很慢，以至于不把问题变得很复杂。

3.定义程序的路由

现在让我们来给程序定义一些路由，Express通过API方法可以相应许多HTTP请求，请看下面例子：

```
//Regular HTTP get
app.get(some url, do something);
 
//Some other page
app.get(some other url, do something else);
 
//I can respond to a form post
app.post(some url, do more stuff);
```

接下来我们添加一个真正的首页路由：

	app.get('/', function(request, response) {
	    response.send("This would be some HTML");
	});	

注意现在我们在response上添加来一个send方法。如果到现在没有任何异常，你可以在终端输入__node app.js__来启动程序，并访问http://127.0.0.1:3000/

此时在页面上你应该看到：

	This would be some HTML

send方法可以处理不同数据类型的数据，假设你需要为你的站点添加JSON-based API，你可以简单的返回一个对象而不是字符串，Express可以自动把结果转换为JSON同时设置正确的相应头部：

	app.get('/api', function(request, response) {
	    response.send({name:"Raymond",age:40});
	});

3.生成一个blog程序

我们的第一个程序是一个blog，它看起来没有什么但是应该有的基本知识都包含了。package.json保持原样不做修改，修改app.js如下：

	var express = require('express');
	var app = express();
	 
	app.get('/', function(req, res) {
	    res.sendfile('./views/index.html');
	});
	 
	app.get('/about', function(req, res) {
	    res.sendfile('./views/about.html');
	});
	 
	app.get('/article', function(req, res) {
	    res.sendfile('./views/article.html');
	});
	 
	app.listen(3000);

此时我们把send方法改为sendfile，并且在app.js里面引入了html。同时添加了首页，关于和文章页面。

接着我们在项目根目录添加views文件夹，并添加index.html,about.html和artical.html。index.html里面添加：

	<html>
	<head>
	    <title>Home Page</title>
	</head>
	 
	<body>
	<h1>Blog!</h1>
	 
	<footer>
	<p>
	    <a href="/">Home</a> ~ <a href="/about">About Me</a> ~ <a href="/article">Some Article</a>
	</p>
	</footer>
	 
	</body>
	</html>

到目前为止并没有什么特别的，就是一些静态文件。终端里面终止程序，然后再启动程序访问首页应该看到上面写的一些东西。

4.改为动态应用

Express支持很多模版引擎（Jade，EJS......），但是我很喜欢Hanlebars所以在接下来的程序中选择了它。在我们的package.json里面添加：

	{
	    "name": "blog2",
	    "description": "Blog app",
	    "version": "0.0.1",
	    "dependencies": {
	        "express": "3.x",
	        "hbs":"*"
	    }
	}

接下来我们需要在app.js中使用它：

	var express = require('express');
	var app = express();
	 
	var hbs = require('hbs');
	 
	app.set('view engine', 'html');
	app.engine('html', hbs.__express);
	 
	app.get('/', function(req, res) {
	    res.render('index');
	});
	 
	app.get('/about', function(req, res) {
	    res.render('about');
	});
	 
	app.get('/article', function(req, res) {
	    res.render('article');
	});
	 
	app.listen(3000);

通过“view engine”告诉Express使用HTML作为动态文件，但这并不是必须的。“app.engine”才是指定何种引擎来渲染页面的。最后我们使用了render方法Express默认去views文件夹去加载页面文件，同时也会忽略文件后缀名，所以__res.render('something')__等同于__views/something.html__，然后根据我们设置的模版引擎来渲染页面。

4.首页展示文章

为了保持教程简单这里并没有使用数据库，但是我们会模拟一些数据。在项目根目录创建：blog.js

	var entries = [
	{"id":1, "title":"Hello World!", "body":"This is the body of my blog entry. Sooo exciting.", "published":"6/2/2013"},
	{"id":2, "title":"Eggs for Breakfast", "body":"Today I had eggs for breakfast. Sooo exciting.", "published":"6/3/2013"},
	{"id":3, "title":"Beer is Good", "body":"News Flash! Beer is awesome!", "published":"6/4/2013"},
	{"id":4, "title":"Mean People Suck", "body":"People who are mean aren't nice or fun to hang around.", "published":"6/5/2013"},
	{"id":5, "title":"I'm Leaving Technology X and You Care", "body":"Let me write some link bait about why I'm not using a particular technology anymore.", "published":"6/10/2013"},
	{"id":6, "title":"Help My Kickstarter", "body":"I want a new XBox One. Please fund my Kickstarter.", "published":"6/12/2013"}];
	 
	 
	exports.getBlogEntries = function() {
	    return entries;
	}
	 
	exports.getBlogEntry = function(id) {
	    for(var i=0; i < entries.length; i++) {
	        if(entries[i].id == id) return entries[i];
	    }
	}

一贯的我们应该有一些：添加／编辑／删除的方法，但是到现在这两个方法足够了。更新app.js如下：

	var express = require('express');
	var app = express();
	 
	var hbs = require('hbs');
	 
	var blogEngine = require('./blog');
	 
	app.set('view engine', 'html');
	app.engine('html', hbs.__express);
	app.use(express.bodyParser());
	 
	app.get('/', function(req, res) {
	    res.render('index',{title:"My Blog", entries:blogEngine.getBlogEntries()});
	});
	 
	app.get('/about', function(req, res) {
	    res.render('about', {title:"About Me"});
	});
	 
	app.get('/article/:id', function(req, res) {
	    var entry = blogEngine.getBlogEntry(req.params.id);
	    res.render('article',{title:entry.title, blog:entry});
	});
	 
	app.listen(3000);

注意__以上方法使用于Express3.0，要是Express4.0则要把：

	app.use(express.bodyParser());

换为：

	var bodyParser = require('body-parser');
	app.use(bodyParser.json());
	app.use(bodyParser.urlencoded({
	  extended: true
	}));

接着我们要修改首页的代码，语法要根据你选择的模版引擎：(可能是hexo也编译大括号,所以我使用＋＋代替了大括号,大家如果复制代码记得改为大括号)

	<h1>Blog!</h1>
	++#each entries++
	    <p>
	        <a href="/article/++id++">++title++</a><br/>
	        Published: ++published++
	    </p>
	++/each++

即使你对Hanlebars不熟悉，也应该对上面的代码猜的差不多，就是循环文章列表。

5.添加页面框架

我敢说你现在应该疑惑其他页面结构去哪里了，当你在Express中使用模版引擎时会自动有框架支持。意味着我可以把我站点通用的东西整合到框架中同时Express会自动的在特定的地方输出我们的内容。在views里面创建layout.html:

	<html>
	<head>
	    <title>++title++</title>
	</head>
	<body>
	    +++body+++
	    <footer>
	        <p>
	            <a href="/">Home</a> ~ <a href="/about">About Me</a>
	        </p>
	    </footer>
	</body>
	</html>

6.显示文章内容

更新artical.html:

	<h1>++blog.title++</h1>
	<p>
		Published: ++blog.published++
	<p/>
	++blog.body++

到此我们已经构建完成了我们的应用，重启应用：node app.js。

如果看不到文章列表或者报错：

	module.js:340
	    throw err;
	          ^
	Error: Cannot find module '/Users/zhuxy/work/node/views/app.js'
	    at Function.Module._resolveFilename (module.js:338:15)
	    at Function.Module._load (module.js:280:25)
	    at Function.Module.runMain (module.js:497:10)
	    at startup (node.js:119:16)
	    at node.js:906:3

问题时没有找到__node_path__路径，在终端输入：

	Linux(Mac): export NODE_PATH=../node_modules
	Windows: set NODE_PATH=.

我的是：

	export NODE_PATH=/usr/local/lib/node_modules

到此我们的教程就完了！如有问题请回复 － －

原文：http://code.tutsplus.com/tutorials/introduction-to-express--net-33367