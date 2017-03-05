title: 20行JavaScript代码实现Router
date: 2014-12-05 17:31:11
categories:
tags: JavaScript
---
上周我看到一篇 [20行代码实现模板引擎](http://krasimirtsonev.com/blog/article/Javascript-template-engine-in-just-20-line)，这是受[John Resig's post on the same topic](http://ejohn.org/blog/javascript-micro-templating/)启发的。我发现他们是如此简单，有趣同时也激发我产生一个__20行代码实现客户端Router__的想法。

__开始创建一个router__

首先我们需要一个html模板：

	<!DOCTYPE html>  
	<html>  
	<head>  
	  <meta charset="utf-8">
	  <title>Building a router</title>
	  <script>
	    // Put John's template engine code here...
	  </script>
	</head>  
	<body>

	</body>  
	</html>  

<!-- more -->

在模板中我使用带有 `__type="text/html"__`标识的`script`的标签，以防止浏览器来渲染它们里面的内容，就像我们想的，我把它们放到了现有script标签的后面。

	<script type="text/html" id="home">  
	  <h1>Router FTW!</h1>
	</script>  
	<script type="text/html" id="template1">  
	  <h1>Page 1: <%= greeting %></h1>
	  <p><%= moreText %></p>
	</script>  
	<script type="text/html" id="template2">  
	  <h1>Page 2: <%= heading %></h1>
	  <p>Lorem ipsum...</p>
	</script>

就像我们看的一样，它们是如此的基础，那是因为我们这篇文章专注于路由部门。

__Hash URL's__

为了实现路由我将使用URL's，就是在__#__后面的一串东西，例如：`http://example.com/#/our/url/here`，我可以通过使用__HTML5 History AP__来实现，但是我会在下次来使用它。

__处理route变化__

页面加载完之后通过__onhashchange__事件来处理路由变化同时使用__onload__事件来处理页面加载之后对应的处理。

__路由注册方法__

我们开始构建路由注册方法：

	// A hash to store our routes:
	var routes = {};  
	// The route registering function:
	function route (path, templateId, controller) {  
	  routes[path] = {templateId: templateId, controller: controller};
	}

__注册路由__

现在我们可以创建新的路由，注意我模仿了AngularJs的控制器定义方式：

	route('/', 'home', function () {});  
	route('/page1', 'template1', function () {  
	    this.greeting = 'Hello world!';
	    this.moreText = 'Bacon ipsum...';
	});
	route('/page2', 'template2', function () {  
	    this.heading = 'I\'m page two!';
	});

但是依旧什么也没发生，因为我们还没有处理路由的方法......

__真是的路由处理方法__

我们创建了路由处理方法，但是我们依旧不知道在哪里渲染我们的内容，现在我设置了一个id为__view__的元素来容纳我们的内容：

	var el = null;  
	function router () {  
	    // Lazy load view element:
	    el = el || document.getElementById('view');
	    // Current route url (getting rid of '#' in hash as well):
	    var url = location.hash.slice(1) || '/';
	    // Get route by url:
	    var route = routes[url];
	    // Do we have both a view and a route?
	    if (el && route.controller) {
	        // Render route template with John Resig's template engine:
	        el.innerHTML = tmpl(route.templateId, new route.controller());
	    }
	}
	// Listen on hash change:
	window.addEventListener('hashchange', router);  
	// Listen on page load:
	window.addEventListener('load', router);  

万事俱备，我们来测试一下！

__测试第一版本__

首先我们在页面添加一些可以导航的链接，来触发我们不同的路由：

	<ul>
	    <li><a href="#">Home</a></li>
	    <li><a href="#/page1">Page 1</a></li>
	    <li><a href="#/page2">Page 2</a></li>
	  </ul>
	  <div id="view"></div>

第一版本的代码在[这里](https://gist.github.com/joakimbeng/7918297/278619bd5ba9b4768eecb0020b09a43f2e8eacea)

保存完整的代码并在高级浏览器中打开，你回发现：

>Router FTW!

同时那些可以导航的链接一样可以使用，你可以在浏览器中直接输入指定路由，比如："path/to/your/router.html#/page1"同时你会看到内容"page1"。

原文：[A Javascript router in 20 lines](http://joakimbeng.eu01.aws.af.cm/a-javascript-router-in-20-lines/)