title: Pub/Sub模式
date: 2014-10-19 09:31:53
tags:
---
Javascript设计模式那本书也稀里糊涂的看了一遍，并没有在工作中很好的应用。这次公司的项目用到了Pub/Sub模式，最近就仔细看了一下。

由于自己知识有限，为了严谨还是引用了维基百科的解释：

>發布/訂閱（Publish/subscribe 或pub/sub）是一種消息範式，消息的發送者（發布者）不是計劃發送其消息給特定的接收者（訂閱者）。而是發布的消息分為不同的類別，而不需要知道什麼樣的訂閱者訂閱。訂閱者對一個或多個類別表達興趣，於是只接收感興趣的消息，而不需要知道什麼樣的發布者發布的消息。這種發布者和訂閱者的解耦可以允許更好的可擴展性和更為動態的網路拓撲.

发布者和订阅者中间的关系纽带是__消息__（又称主题topic），不需要直接关联。

其实现实生活中有很多这样的例子：

* 订阅彩信
* 邮件订阅

这就带来了其优点：

__解藕__：双方不需要知道对方的存在，由于__主题__是枢纽，双方的对程序／系统的修改都不会影响对方。

同时由于其优点也带来了副作用：

__死板的语义链接__:发布者发出的数据格式必须和订阅者约定，这就造成修改时的成本。

概念讲完了，现在看看如何实现：

```
var pubsub = {};
(function(q){
	//维护主题和订阅者的对象
	var topics = {},
		subUid = -1;
	q.publish = function(topic,args){
		//指定主题不存在则返回
		if(!topics[topic]){
			return false;
		}
		//取出指定主题下的订阅者和个数
		var subscribers = topics[topic],
			len = subscribers ? subscribers.length : 0;
		//逐个执行该主题上订阅者的回调方法
		while(len--){
			subscribers[len].func(topic,args);
		}
		return this;
	};
	q.subscribe = function(topic,func){
		//指定主题不存在则创建
		if(!topics[topic]){
			topics[topic] = [];
		}
		var token = (++subUid).toString();
		//指定主题中添加订阅者
		topics[topic].push({
			token:token,
			func:func
		});
		return token;
	};
	q.unsubscribe = function(token){
		for(var m in topics){
			if(topics[m]){
				for(var i = 0, j = topics[m].length; i < j; i++){
					//根据token删除订阅者
					if(topics[m][i].token === token){
						topics[m].splice(i,1);
						return token;
					}
				}
			}
		}
		return this;
	};
}(pubsub));
```

订阅：

```
var messageLoger = function(topics,data){
	console.log("Loggin:" + topics + ":" + data);
};

var subscription = pubsub.subscribe('inbox/newMessage',messageLoger);

pubsub.publish('inbox/newMessage','HelloWorld');
```
退订：

```
pubsub.unsubscribe(subscription);

pubsub.publish('inbox/newMessage','HelloWorld');

```

<a class="jsbin-embed" href="http://jsbin.com/fidagu/1/embed">JS Bin</a><script src="http://static.jsbin.com/js/embed.js"></script>

 参考：

 1. http://msdn.microsoft.com/en-us/magazine/hh201955.aspx

 2. http://addyosmani.com/resources/essentialjsdesignpatterns/book/#observerpatternjavascript

 3. http://en.wikipedia.org/wiki/Publish%E2%80%93subscribe_pattern