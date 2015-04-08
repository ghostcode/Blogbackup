title: Gulp+Browserify
date: 2015-03-17 23:18:33
categories:
tags:
---
> 更新
2014年，9月8日
[Gulp start](https://github.com/greypants/gulp-starter)在这篇文章写之后经过了很大的发展。可下面的内容依旧是连贯的，只有少数部分在第一次写过之后进行了更改。比如：
* [自动加载task](https://github.com/greypants/gulp-starter/blob/master/gulpfile.js#L17)
* 通过[gulp/config](https://github.com/greypants/gulp-starter/blob/master/gulp/config.js)配置任务
* 用BrowserSync替换LiveReload

###开始###

周末，我决定使自己沉浸于Grunt和RequireJs中，   接着到周一时，看到一篇 [and just like that Grunt and RequireJS are out, it’s all about Gulp and Browserify now.](http://www.100percentjs.com/just-like-grunt-gulp-browserify-now/)整个人都不好了！

为了让你避免googling，文档中遨游以及我碰到的问题，我搜集了一些认为对你开始有用的资源和信息

###Gulp + Browserify starter repo###

我已经创建了[Gulp + Browserify starter repo](https://github.com/greypants/gulp-starter)并配有如何完成一些常规任务和工作流的例子。

<!-- more -->

* [Compile CoffeeScript](https://github.com/greypants/gulp-starter/blob/master/package.json#L15)(带有[source map](https://github.com/greypants/gulp-starter/blob/master/gulp/tasks/browserify.js#L13))
* [Compile Handlebars Templates](https://github.com/greypants/gulp-starter/blob/master/package.json#L16)
* [Compile SASS with Compass](https://github.com/greypants/gulp-starter/blob/master/gulp/tasks/compass.js#L8)
* [LiveReload](https://github.com/greypants/gulp-starter/blob/master/gulp/tasks/compass.js#L17)
* [Browserify-shim: require non-CommonJS code, with dependencies](https://github.com/greypants/gulp-starter/blob/master/package.json#L19)
* [Set up module aliases](https://github.com/greypants/gulp-starter/blob/master/package.json#L10)
* [Run a static Node server (with logging)](https://github.com/greypants/gulp-starter/blob/master/gulp/tasks/serve.js)
* [Pop open your app in a Browser](https://github.com/greypants/gulp-starter/blob/master/gulp/tasks/open.js)
* [Report Errors through Notification Center](https://github.com/greypants/gulp-starter/blob/master/gulp/util/handleErrors.js)
* [Image processing](https://github.com/greypants/gulp-starter/blob/master/gulp/tasks/images.js#L11)

###[常见问题集合](https://github.com/greypants/gulp-starter/wiki)###

[Node](http://nodejs.org/),[npm](https://www.npmjs.org/),[CommonJS](http://wiki.commonjs.org/wiki/Modules/1.1),[package.json](https://www.npmjs.org/doc/json.html)...是什么？当我一头扎进这些内容时，多数文档不够详尽，都假设用户对其有些了解的情况下编写的文档。我已经将一些背景知识融入到Wiki中，以此消除我上面提到的Repo的知识隔阂。

* [What is a Module?](https://github.com/greypants/gulp-starter/wiki/What-is-a-Module%3F)
* [What is Node?](https://github.com/greypants/gulp-starter/wiki/What-is-Node%3F)
* [What is npm?](https://github.com/greypants/gulp-starter/wiki/What-is-npm%3F)
* [What is package.json?](https://github.com/greypants/gulp-starter/wiki/What-is-package.json%3F)
* [What is Browserify?](https://github.com/greypants/gulp-starter/wiki/What-is-Browserify%3F)
* [What is Gulp?](https://github.com/greypants/gulp-starter/wiki/What-is-Gulp%3F)

###为何Gulp很好？###

__它是有意义的。__

在学了Grunt几天后我就学习了Gulp，无论什么原因，我发现它很容易掌握并且工作起来很享受。通过不同的过程输出文件流的想法很有赞。

>gulp's use of streams and code-over-configuration makes for a simpler and more intuitive build. - gulpjs.com

下面是一个图片处理的过程：

	var gulp       = require('gulp');
	var imagemin   = require('gulp-imagemin');
	 
	gulp.task('images', function(){
	    return gulp.src('./src/images/**')
	        .pipe(imagemin())
	        .pipe(gulp.dest('./build/images'));
	});

首先，`gulp.src`定位到资源文件用于接下来的各种任务。在这个例子中，我使文件经过`gulp-imagemin`，然后通过`gulp.dest()`输出到我的`build`目录。若想添加额外的操作（重命名，改变尺寸，自动加载等），只需要在任务流中加载更多的管道。

###速度###

Gulp是相当的快，我刚刚完成了一个相当复杂的app，它编译Sass，SourceMap的CoffeeScript，Handlebars的模板以及自动加载。这些看起来貌似都不是问题。

>By harnessing the power of node's streams you get fast builds that don't write intermediary files to disk. - gulpjs.com

###打散任务配置文件###

gulpfile就是在使用gulp时的任务配置，如果你使用过grunt，它就像gruntfile。在经历一些实验，收到一些提交建议和学习Node/CommonJS多棒之后，我拆分了所有的任务到单个文件中，然后通过gulpfile.js组合起来。

	var gulp = require('./gulp')([
	    'browserify',
	    'compass',
	    'images',
	    'open',
	    'watch',
	    'serve'
	]);
	 
	gulp.task('build', ['browserify', 'compass', 'images']);
	gulp.task('default', ['build', 'watch', 'serve', 'open']);

200个字符，很简洁，对吧？这里发生了什么：我引入了在`./gulp/index.js`中创建的gulp模块，然后以一个列表的形式将在`./gulp/tasks`文件夹下创建的任务文件。

	var gulp = require('gulp');
	 
	module.exports = function(tasks) {
	    tasks.forEach(function(name) {
	        gulp.task(name, require('./tasks/' + name));
	    });
	 
	    return gulp;
	};

我们传给这个方法的每一个任务名，都是在`.／tasks／`文件夹下的独立文件暴露出的方法。现在每一个任务都被注册，可以在一个大任务里面使用，例如像定义在gulpfile尾部的default。

![img](/assets/images/gulp-struct.png)

这样就可以重用而且在新项目中很容易建立，阅读[gulp doc](https://github.com/gulpjs/gulp/blob/master/docs/README.md)获得更多知识。

###为何Browserify很好？###

>“Browserify lets you require('modules') in the browser by bundling up all of your dependencies.” - Browserify.org

Browserify就像一个单独的文件，遵守require依赖原理同时把他们合并为一个新文件，你可以在命令行里使用或者通过Node中的API在gulp中调用。

####基础的API例子####

__app.js__

	var hideElement = require('./hideElement');
	 
	hideElement('#some-id');

__hideElement.js__

	var $ = require('jquery');
	module.exports = function(selector) {
	    return $(selector).hide();
	};

__gulpfile.js__

	var browserify = require('browserify');
	var bundle = browserify('./app.js').bundle()

通过Browserify调用app.js经历如下过程：

1. app.js 依赖 hideElement.js
2. hideElement.js需要jquery.js模块
3. 把jQuery，hideElement.js,app.js捆绑在一起，在使用之前确保没有依赖的文件都存在

###CommonJs到AMD###

我们的团队已经开始以Require.js和Almond.js的模块化开发，他俩其实是实现了AMD规范。我们喜欢这样组织代码并从中获益，可是...

__AMD和Require.js感觉很笨重和不便__

	require([
	    './thing1',
	    './thing2',
	    './thing3'
	], function(thing1, thing2, thing3) {
	    // Tell the module what to return/export
	    return function() {
	        console.log(thing1, thing2, thing3);
	    };
	});

__第一次使用CommonJs模块的时候感觉很自然__

	ar thing1 = require('./thing1');
	var thing2 = require('./thing2');
	var thing3 = require('./thing3');
	 
	// Tell the module what to return/export
	module.exports = function() {
	    console.log(thing1, thing2, thing3);
	};

确保阅读[ how require calls resolve to files, folders, and node_modules](http://nodejs.org/api/modules.html#modules_file_modules)

###Browserify很棒是因为Node和NPM很棒###

Node使用CommonJs模式来提供模块管理，真正使其强大的是通过NPM可以快速安装，更新和管理依赖。你一旦尝试过此种组合，你将会一直需要它。Browerify就是把这种模式带到浏览器端。

若需要jQuery，一般情况，你会打开浏览器在jQuery.com上找到最新的版本的，然后下载到一个文件夹里，然后在你的布局里添加script标签，同时使其附加到window对象上作为全局对象。

通过NPM和Browserify，你只需要：

	npm install jquery --save

app.js

	var $ = require('jquery');
	$('.haters').fadeOut();

这些代码表示从NPM下载最新的jQuery版本，并保存到你项目根目录下的node_modules文件夹中。`--save`参数代表在package.json中的dependencies添加此包。现在你可以在当前项目下的任何文件中`require("jquery")`。jQuery对象通过var $ 暴露在局部作用域内，而不是在window下的全局作用域。当我在第三方网站引入我的代码同时该网站有其他版本的jquery时，这种模式会很好，避免版本冲突的可能。

###Transforms的能力###

在打包Js文件之前，Browserify通过一些[transform](https://github.com/substack/node-browserify/wiki/list-of-transforms)可以很轻松的预处理你的文件然后打包到一个文件里。就是编译.coffee或.hbs文件到合并的文件里。

常规的做法是在package.json的`browserify.transform`下列出你的transforms。Browserify会按照你列的顺序执行。这都假设已经通过npm install 安装了它们。

	"browserify": {
	    "transform": ["coffeeify", "hbsfy" ]
	  },
	 "devDependencies": {
	    "browserify": "~3.36.0",
	    "coffeeify": "~0.6.0",
	    "hbsfy": "~1.3.2",
	    "gulp": "~3.6.0",
	    "vinyl-source-stream": "~0.1.1"
	  }

注意：因为transforms只在预处理中使用并且不需要在输出JS中使用所以我在`devDependencies`中列出了它们。你可以使用`--save-dev`或`-D`自动添加到`devDependencies`中。

现在我们可以使用`require('./view.coffee')`和`require('./template.hbs')`就像使用其他JS文件一样。我们可以使用`extentions`选项来使Browserify辨认出扩展，所以我们不需要在require中指明后缀。

	browserify({
	    entries: ['./src/javascript/app.coffee'],
	    extensions: ['.coffee', '.hbs']
	})
	.bundle()
	...

在这里看[事例](https://github.com/greypants/gulp-starter/blob/master/gulp/tasks/browserify.js)

###同时使用Gulp和Browserify###

起初，我使用`gulp-browserify`插件，但是几礼拜之后发现Gulp把它加入黑名单了，原来插件不是必须的，可以直接使用node-browserify API,借助[vinyl-source-stream](https://www.npmjs.org/package/vinyl-source-stream)，可以使合并的文件变为gulp需要的流。直接使用Browserify可以100%获得最新版本的特性。

__基础方法__

	var browserify = require('browserify');
	var gulp = require('gulp');
	var source = require('vinyl-source-stream');
	 
	gulp.task('browserify', function() {
	    return browserify('./src/javascript/app.js')
	        .bundle()
	        //Pass desired output filename to vinyl-source-stream
	        .pipe(source('bundle.js'))
	        // Start piping stream to tasks!
	        .pipe(gulp.dest('./build/'));
	});

__高级用法__

查看一下[browserify.js task](https://github.com/greypants/gulp-starter/blob/master/gulp/tasks/browserify.js)和[package.json](https://github.com/greypants/gulp-starter/blob/master/package.json)中是如何为CoffeeScript和Handlebars预编译的。通过[browserify-shim](https://github.com/thlorenz/browserify-shim)j创建没有共同的JS模块同时使用消息中心来处理编译错误。阅读这些[API](https://github.com/substack/node-browserify#api-example)来了解Browserify能做的东西。

















