title: JavaScript技巧揭秘
date: 2014-09-27 16:41:07
tags: Javascript
---
Javascript中有许多技巧被有经验的开发者广泛使用。它们并不显而易见，特别是对初级开发者。这些技巧利用了语言特点，从侧面实现了语言本身不能直接实现的目标。下面我对这些技巧做了一个解释说明的专辑。

你应该知道其中多数技巧是hack,并不是用在日常的开发中的。这篇文章的目的是解释它们是如何工作的，并不赞同使用它们。

__使用!!来进行Boolean值转换__

JavaScript中的一切变量都可以转换为true或false，这意味着你如果把变量放到if表达式中，会根据转换结果自动选择执行真分支还是假分支。

0，false，""，null，undefined，NaN可以转换为false，其他值会转换为真。有时你希望一个值转换为boolean值，你可以使用“||”。

另一方面你可以使用if(x)来代替if(x == 'test')。当x是空值时，它会执行else分支。

__字符串变为数字___

字符串变为数字最常用的方法当属parseInt(x,10)和parseFloat(),还有一种不常用的就是 "3" - 0。判断一个值是否为数字有个技巧：

	x === +x

__使用||提供默认值__

<!-- more -->

在JavaScript中||时一个“[short-circuit evaluation](http://en.wikipedia.org/wiki/Short-circuit_evaluation)”，同样也广泛应用于其他语言当中。这个操作符首先会执行左侧表达式，当值为假时，执行右侧的表达式。考虑下面的一个例子：

	function setAge(age){
		this.age = age || 10;
	}
	setAge()

我们没有提供age参数，因此 age || 10 会返回 10，这是提供默认值的好方法，其实上面的例子等价于：

	var age;

	if(age){
		this.age = age;
	}else{
		this.age = 10;
	}

第一个例子很简明，以至于你会经常看到。

我个人非常喜欢这个模式的简明，但是上面的例子有个问题：不能给年龄设置为0。改进方法：

原作者的：

	this.age = (typeof age !== 'undefined') ? age : 10;

但是虽然上面的方法可以设置为0，但是此时依旧可以设置为null。所以下面是我的方法：

	this.age = (age === +age) ? age : 10;

__使用void 0 来代替undefined__

关键词void接受一个参数并且一直返回undefined，但为何不直接使用undefined呢？因为在一些浏览器中可以重写undefined，同样在一些框架中也可以看到。在[EC5](http://stackoverflow.com/questions/8783510/javascript-how-dangerous-is-it-really-to-assume-undefined-is-not-overwritten)中浏览器是不可以重写undefined的。

__使用(function(){ })()实现封装模式__

当你希望需要封装时，你可以将代码包裹在一个匿名函数并可以立即执行的函数里面。Javascript里面只有两种作用域：全局和函数作用域，看看[ECMA6 block scopes](http://ariya.ofilabs.com/2013/05/es6-and-block-scope.html)。你写在全局的变量，可以在任何地方使用；正常情况下你希望使用函数将多数代码封装同时对全局作用域只提供接口。所以这种模式会很好用，注意下面的例子：

	(function() {
	  function div(a, b) {
	    return a / b;
	  }

	  function divBy5(x) {
	    return div(x, 5);
	  }

	  window.divBy5 = divBy5;
	})()

	div // => undefined
	divBy5(10); // => 2

在以上提到的所有技巧里面，这是一个副作用最小的推荐使用，以此来减少内部逻辑代码暴露到全局作用域中。

综诉：我想提醒的是你的代码对于其他开发者一定要简介明了。语言本身提供的结构一般优于人造的方法。

上面提到的一些问题在即将到来的ES6中将得到解决。例如在将来你不需要使用复杂的__age = age || 10__模式，因为ES6允许使用更好的模式来提供默认值：

	function(age = 10){
		...
	}

另一个例子是，你将不再需要(function(){})()模式，当高级浏览器实现 [EC6 modules](http://eviltrout.com/2014/05/03/getting-started-with-es6.html)

原文：http://blog.mdnbar.com/javascript-common-tricks?utm_source=javascriptweekly&utm_medium=email


资源：

如果你希望了解更多技巧，请看下面的链接：

1. [Byte Saving Techniques](https://github.com/jed/140bytes/wiki/Byte-saving-techniques)

2. [JavaScript Idiosyncrasies (kinda)](https://github.com/miguelmota/javascript-idiosyncrasies)

3. [JavaScript language advanced Tips & Tricks](https://code.google.com/p/jslibs/wiki/JavascriptTips)

4. [45-useful-javascript-tips-tricks-and-best-practices](http://modernweb.com/2013/12/23/45-useful-javascript-tips-tricks-and-best-practices/)














