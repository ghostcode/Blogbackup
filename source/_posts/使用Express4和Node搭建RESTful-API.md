title: 使用Express4和Node搭建RESTful API
date: 2014-11-03 12:46:33
categories:
tags: Node Express
---
几天前随着Express 4.0的发布，我们许多Node项目的路由需要跟着更改。伴随Express Router的变化，我们在设置应用路由时更加灵活。

今天我们将使用Node,Express 4.0和它的路由来创建RESTful API，同时使用Mongoose与MongoDB交互。在Chrome中使用Postman来测试我们的API。

接下来看看我们要创建的API以及它的用途。

###我们的程序###

我们将要创建的API:

* 处理CRUD
* 标准的URL(`http://example.com/api/bears`和`http://example.com/api/bears/:bear_id`)
* 使用合适的HTTP来搭配RESTful(GET,POST,PUT和DELETE)
* 返回JSON数据
* 控制台打印出所有的请求

这些都是严格遵循[RESTful APIS](http://scotch.io/bar-talk/designing-a-restful-web-api)的规范的，可以灵活的转变为任何你想创建的程序（用户，超级英雄，啤酒等）

开始之前要确保你安装了Node环境然后我们才能开始！

###启程###

我们先整理一下创建API所需要的文件，我们需要定义Node packages，使用Express搭建服务、路由和模型，最后，测试我们的API。

下面是我们的目录结构，我们需要很多文件同时为了演示我们尽可能的精简。当真的要做一个产品或者一个大的应用时，你应该拥有一个更好的结构（比如剥离routes到一个单独的文件里）。

	- app/
		----- models/
		---------- bear.js 	// our bear model
		- node_modules/     // created by npm. holds our dependencies/packages
		- package.json 		// define all our node app and dependencies
		- server.js 		// configure our application and create routes

###声明Node依赖包###

我们所有的Node项目都会在`package.json`里面定义依赖的包。先创建一个包含下面内容的文件：

	// package.json
	{
		"name": "node-api",
		"main": "server.js",
		"dependencies": {
			"express": "~4.0.0",
			"mongoose": "~3.6.13",
			"body-parser": "~1.0.1"
		}
	}

__这些包的作用是啥？__ `express`是Node的Web框架，`mongoose`是和MongoDB数据库通信的对象关系映射，`body-parser`使我们能在HTTP请求中获得参数信息，方便我们做一些事情。

###安装Node包###

这或许是最简单的一步，在你的项目根目录打开终端输入：

	$ npm install

npm会拉去所有的依赖包到`node_module`中。

`npm`是Node的包管理工具，现在我们有了所有依赖的包，可以开始创建API了。因为`server.js`是我们在package.json中声明的程序入口，所以我们找到`server.js`文件（没有的创建）来配置程序。

###搭建我们的服务器###

Node启动程序时会检查一些配置信息。

我们会保持代码的简洁以及良好的注释，清晰的了解我们每一步完成了什么。

	// server.js

	// BASE SETUP
	// =============================================================================

	// call the packages we need
	var express    = require('express'); 		// call express
	var app        = express(); 				// define our app using express
	var bodyParser = require('body-parser');

	// configure app to use bodyParser()
	// this will let us get the data from a POST
	app.use(bodyParser.urlencoded({ extended: true }));
	app.use(bodyParser.json());

	var port = process.env.PORT || 8080; 		// set our port

	// ROUTES FOR OUR API
	// =============================================================================
	var router = express.Router(); 				// get an instance of the express Router

	// test route to make sure everything is working (accessed at GET http://localhost:8080/api)
	router.get('/', function(req, res) {
		res.json({ message: 'hooray! welcome to our api!' });	
	});

	// more routes for our API will happen here

	// REGISTER OUR ROUTES -------------------------------
	// all of our routes will be prefixed with /api
	app.use('/api', router);

	// START THE SERVER
	// =============================================================================
	app.listen(port);
	console.log('Magic happens on port ' + port);

哇哦，我们做了许多事，但都很简单。接下来我们挨个看看：

* __基础设置__ 在我们的设置中包含使用express初始化应用，通过bodyParser来轻松获取请求头信息以及设置应用的端口。

* __路由配置__ 我们使用Express的Router来定义应用的路由。

* __启动服务__ 我们会在先前定义的端口启动应用，然后就可以运行和测试它。

###开启服务并测试###

确保所有的工作已经就绪，接下来我们启动应用并在先前定义的路由上发一个请求确保能得到一个响应。

使用下面的命令启动服务：

	$ node server.js

你将会看到如下的界面：

![img](/assets/images/node-api-start-server.png)

看到程序已经启动，接着来测试一下。

###使用Postman测试我们的API###

Postman会帮助我们测试API，它基本就是在我们填写的路径上发送HTTP请求。我们甚至可以传递参数和权限（在这个应用中我们暂且不需要）。

打开Postman看看如何使用：

![img](http://img4.tuchuang.org/uploads/2014/11/postman_rest_client_node_api.png)

你所需要做的就是：__填写目的url，选择一个http方法最后发送请求__，够简单吧？

接下来就是期盼已久的时刻，在URL一栏中输入__http://localhost:8080/api__，由于我们仅从服务器获得数据，所以我们选择__GET__方法，接着点击发送。

![post man](http://img3.tuchuang.org/uploads/2014/11/node_api_postman_test.png)

不错，我们得到了预期的结果，现在我们可以为请求服务。接下来连上数据库方便我们执行CRUD操作。

###数据库和Beaer模型###

我们会使这部分简短留有足够的时间在创建API路由上。我们所需要做的就是创建MongoDB数据库以及使程序和它相连。为了简化和数据库的操作我们同样需要mongoose模型。

__创建数据库和链接__

我们使用Modulus提供的数据库，你在上面可以创建自己的数据库以及可以在本地使用，或者使用Mongolab，所有你所需要的就是一个链接，就像下面那样就你的程序就可以连接。

一旦创建好数据库以及有一个可以连接的地址，接着在__server.js__里面添加下面两行：

	// server.js

	// BASE SETUP
	// =============================================================================

	...

	var mongoose   = require('mongoose');
	mongoose.connect('mongodb://node:node@novus.modulusmongo.net:27017/Iganiq8o'); // connect to our database

	...

现在我们已经获取mongoose的包同时连上架设在远程Modulus的数据库。接下来就需要创建bears的模型。

__Bear Moder app/models/bear.js__

因为创建模型不是本教程的核心，所以我们只一个带有名字字段的模型。接下来创建并添加如下代码：

	// app/models/bear.js

	var mongoose     = require('mongoose');
	var Schema       = mongoose.Schema;

	var BearSchema   = new Schema({
		name: String
	});

	module.exports = mongoose.model('Bear', BearSchema);

创建完上面的文件，接着就要在我们的server.js中引入：

	// server.js

	// BASE SETUP
	// =============================================================================

	...

	var Bear     = require('./app/models/bear');

	...

现在应用后端已经完成，接下来才是本文的重点，奔跑吧！兄弟！

###Express路由模块和路由###

我们将会使用Express Router实例来管理我们的路由，下面是我们将会用到的路由以及对应的HTTP方法。

| Route               | HTTP Verb | Description                  |
|---------------------|-----------|------------------------------|
| /api/bears          | GET       | Get all the bears.           |
| /api/bears          | POST      | Create a bear.               |
| /api/bears/:bear_id | GET       | Get a single bear.           |
| /api/bears/:bear_id | PUT       | Update a bear with new info. |
| /api/bears/:bear_id | DELETE    | Delete a bear.               |

这已经覆盖API所需的基本路由，同时也保持了我们的行为和HTPP方法同步的良好格式。

###路由中间件###

我们已经定义了第一个路由以及看到了它的作用。Express Router为我们在定义路由时提供很大的灵活性。

我们希望在每次请求API时都可以得到反馈信息，我只需要在代码中添加__console.log()__。

	// server.js

	...

	// ROUTES FOR OUR API
	// =============================================================================
	var router = express.Router(); 				// get an instance of the express Router

	// middleware to use for all requests
	router.use(function(req, res, next) {
		// do logging
		console.log('Something is happening.');
		next(); // make sure we go to the next routes and don't stop here
	});

	// test route to make sure everything is working (accessed at GET http://localhost:8080/api)
	router.get('/', function(req, res) {
		res.json({ message: 'hooray! welcome to our api!' });	
	});

	// more routes for our API will happen here

	// REGISTER OUR ROUTES -------------------------------
	// all of our routes will be prefixed with /api
	app.use('/api', router);

	...

为了达到那个目的我仅需要声明__router.use(function())__。我们定义路由的顺序很重要，因为程序会根据我们定义的顺序执行。

我们返回的数据时JSON格式的，对于API来说这是标准的同样这也方便使用我们API的用户调用。

我们同样需要添加__next()__标识程序需要继续执行下面的路由，这点很重要防止程序在此终止。

__Middleware Uses__ 中间件的作用很强大，我们可以验证请求以此保证请求的安全性，同样可以在这里抛出异常。同时还可以做登陆分析，总之在这里可以很多事。

疯狂吧！

###测试我们的中间件###

现在使用Postman向我们的应用程序发送一个请求，在终端就会看到__Something is happening__。


通过中间件，可以做一些了不起的事情，可以对用户进行权限管理。

###创建基本路由###

我们首先需要__获取__和__创建__的接口，这两项都是通过__/api/bears__api完成的，我们要先创建一个实例以备后用。

###创建一个熊 POST /api/bears###

我们将会添加一个处理POST的路由接着使用Postman来测试。

	// server.js

	...

	// ROUTES FOR OUR API
	// =============================================================================

	... // <-- route middleware and first route are here

	// more routes for our API will happen here

	// on routes that end in /bears
	// ----------------------------------------------------
	router.route('/bears')

		// create a bear (accessed at POST http://localhost:8080/api/bears)
		.post(function(req, res) {
			
			var bear = new Bear(); 		// create a new instance of the Bear model
			bear.name = req.body.name;  // set the bears name (comes from the request)

			// save the bear and check for errors
			bear.save(function(err) {
				if (err)
					res.send(err);

				res.json({ message: 'Bear created!' });
			});
			
		});

	// REGISTER OUR ROUTES -------------------------------
	// all of our routes will be prefixed with /api
	app.use('/api', router);

	...

现在我们已经创建好POST路由，以后都会使用__router.route__来处理相同路由。可以处理所有以__/bears__结尾的请求。

接下来用Postman看看创建的熊。


注意我们以__x-www-form-urlencoded__的形式来发送__name__，这样就会以查询字符串的形式发送到Node服务器。
	
我们得到创建成功后的信息，接着来处理获取熊的接口。

###获取所有的熊###

通过router.route(),我们可以把不同的路由连接在一起，这保证了代码的简洁。

// server.js

	...

	// ROUTES FOR OUR API
	// =============================================================================

	... // <-- route middleware and first route are here

	// more routes for our API will happen here

	// on routes that end in /bears
	// ----------------------------------------------------
	router.route('/bears')

		// create a bear (accessed at POST http://localhost:8080/api/bears)
		.post(function(req, res) {
			
			...
			
		})

		// get all the bears (accessed at GET http://localhost:8080/api/bears)
		.get(function(req, res) {
			Bear.find(function(err, bears) {
				if (err)
					res.send(err);

				res.json(bears);
			});
		});

	// REGISTER OUR ROUTES -------------------------------
	// all of our routes will be prefixed with /api
	app.use('/api', router);

	...

一个简单的路由，仅仅向__http://localhost:8080/api/bears__发送了一个GET请求，我们就能得到以JSON格式数据。

###为处理单个熊创建路由###

我们以__/bears__结尾的路由来处理多个实例，现在来处理单个的实例，比如我们传递一个熊的ID时。

通过以__/bears/:bear_id__结尾的路由来出来单个实例：

＊ 获取一个实例
＊ 更新一个实例
＊ 删除一个实例

__:bear_id__可以从请求头中获取，这得益于__body-parser__。

###获取一个实例###

我们将会添加一个路由去处理以__:bear_id__结尾的URL。

	// server.js

	...

	// ROUTES FOR OUR API
	// =============================================================================

	...

	// on routes that end in /bears
	// ----------------------------------------------------
	router.route('/bears')
		...

	// on routes that end in /bears/:bear_id
	// ----------------------------------------------------
	router.route('/bears/:bear_id')

		// get the bear with that id (accessed at GET http://localhost:8080/api/bears/:bear_id)
		.get(function(req, res) {
			Bear.findById(req.params.bear_id, function(err, bear) {
				if (err)
					res.send(err);
				res.json(bear);
			});
		});

	// REGISTER OUR ROUTES -------------------------------
	// all of our routes will be prefixed with /api
	app.use('/api', router);

	...

通过上面获取全部实例的数据，可以看到单个熊的ID，用这个ID可以在Postman中得到单个的实例。

###更新实例###

用router.route()连接起类似的路由并添加__.put()__。

	// server.js

	...

	// on routes that end in /bears
	// ----------------------------------------------------
	router.route('/bears')
		...

	// on routes that end in /bears/:bear_id
	// ----------------------------------------------------
	router.route('/bears/:bear_id')

		// get the bear with that id (accessed at GET http://localhost:8080/api/bears/:bear_id)
		.get(function(req, res) {
			...
		})

		// update the bear with this id (accessed at PUT http://localhost:8080/api/bears/:bear_id)
		.put(function(req, res) {

			// use our bear model to find the bear we want
			Bear.findById(req.params.bear_id, function(err, bear) {

				if (err)
					res.send(err);

				bear.name = req.body.name; 	// update the bears info

				// save the bear
				bear.save(function(err) {
					if (err)
						res.send(err);

					res.json({ message: 'Bear updated!' });
				});

			});
		});

	// REGISTER OUR ROUTES -------------------------------
	// all of our routes will be prefixed with /api
	app.use('/api', router);

	...

我们还是使用前面的ID获得实例，然后通过PUT方法，在参数里面修改然后保存。

为了确定更改生效，我们需要再查一遍实例。

###删除一个实例###

当需要删除一个实例时，仅仅需要向__/api/bears/:bear_id__发送DELETE请求。

下面添加对应的代码：

	// server.js

	...

	// on routes that end in /bears
	// ----------------------------------------------------
	router.route('/bears')
		...

	// on routes that end in /bears/:bear_id
	// ----------------------------------------------------
	router.route('/bears/:bear_id')

		// get the bear with that id (accessed at GET http://localhost:8080/api/bears/:bear_id)
		.get(function(req, res) {
			...
		})

		// update the bear with this id (accessed at PUT http://localhost:8080/api/bears/:bear_id)
		.put(function(req, res) {
			...
		})

		// delete the bear with this id (accessed at DELETE http://localhost:8080/api/bears/:bear_id)
		.delete(function(req, res) {
			Bear.remove({
				_id: req.params.bear_id
			}, function(err, bear) {
				if (err)
					res.send(err);

				res.json({ message: 'Successfully deleted' });
			});
		});

	// REGISTER OUR ROUTES -------------------------------
	// all of our routes will be prefixed with /api
	app.use('/api', router);

	...

使用一个存在的bear_id通过DELETE方法向我们的API发送请求，将会删除一个对应的实例。

接着再去获得所有实例时，就会得到空值。

###结论###

现在我们已有自己的API来处理CRUD，有一个好的基础才能创建一个更大和更健壮的程序。

这只是一个简洁的使用Express4创建的Node API，其实还有好多需要做和可以做的。比如：添加权限控制，友好的错误提示等。

如果有任何疑问，可以在下面留言。


























































