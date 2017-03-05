title: JavaScript最佳实践一
date: 2014-11-13 09:37:38
tags: JavaScript
categories:
---

__变量和函数名有意义__

选择易理解和简短的名字作为变量和方法。

__糟糕的命名__

	x1 fe2 xbqne

__同样糟糕的命名__

	incrementerForMainLoopWhichSpansFromTenToTwenty
	}
	createNewMemberIfAgeOverTwentyOneAndMoonIsFull

尽量避免使用变量和方法的名字去解释值。

<!-- more -->

__或许在其他一些国家是没有意义__

	isOverEighteen()

__通用__

	isLegalAge()

你的代码就是一个故事，请保持故事情节深入人心（容易阅读）。

__避免全局__

全局变量是一个相当可怕的。

__原因：__你代码中的全局变量即有可能被覆盖。

__变通：__使用闭包和模块模式。

问题：所有的全局变量在其他地方都可以访问，这是不可避免的。

	var current = null;
	var labels = {
	   'home':'home',
	   'articles':'articles',
	   'contact':'contact'	
	};
	function init(){
	};
	function show(){
	   current = 1;
	};
	function hide(){
	   show();
	};

__对象字面量__：其中的任何东西都需要通过对象的名字获得。

问题：重复的模块名字会产生大量的代码而且令人厌烦。

	demo = {
	   current:null,
	   labels:{
	      'home':'home',
	      'articles':'articles',
	      'contact':'contact'
	   },
	   init:function(){
	   },
	   show:function(){
	      demo.current = 1;
	   },
	   hide:function(){
	      demo.show();
	   }
	}

模块模式：你需要指出哪些可以全局使用哪些不可以－－在这两之间取舍。

问题：重复的模块名字，内部函数使用不同的语法。

	module = function(){
	   var labels = {
	      'home':'home',
	      'articles':'articles',
	      'contact':'contact'
	   };
	   return {
	      current:null,
	      init:function(){
	      },
	      show:function(){
	         module.current = 1;
	      },
	      hide:function(){
	         module.show();
	      }
	   }
	}();

揭露模块模式：保持相同的代码风格同时暴露全局方法和变量。

	module = function(){
	   var current = null;
	   var labels = {
	      'home':'home',
	      'articles':'articles',
	      'contact':'contact'
	   };
	   var init = function(){
	   };
	   var show = function(){
	      current = 1;
	   };
	   var hide = function(){
	      show();
	   }
	   return{init:init, show:show, current:current}
	}();
	module.init();

__使用严格模式__

浏览器的JavaScript解析引擎有很强大的容错能力，当你需要移植代码或者交接代码给同事时，松散的代码就会给你带来麻烦。所以代码检查才能保证代码的安全。

检查你的代码：[http://www.jslint.com/](http://www.jslint.com/)

__代码注释适中为好__

>好的代码不需解释

这是一个夸张的迷信。

注释那些你认为需要解释的地方，但是记住不要讲解你的人生故事（不要太长）。

避免使用单行注释__//__，使用__/* */__更安全，当换行时不会产生错误。

如果你使用注释来调试代码，这是一个很好的技巧：

	module = function(){
	   var current = null;
	/*
	   var init = function(){
	   };
	   var show = function(){
	      current = 1;
	   };
	   var hide = function(){
	      show();
	   }
	// */
	   return{init:init, show:show, current:current}
	}();

注释不能出现在用户面前。__开发代码不是活的代码(开发代码不是面向用户的代码)__

__避免技术混用__

JavaScript很擅长计算，获取外部数据（Ajax）以及根据事件做出相应的回应。

例如：

在类名为mandatory且其值为空时在外围加上红色边框。

	var f = document.getElementById('mainform');
	var inputs = f.getElementsByTagName('input');
	for(var i=0,j=inputs.length;i<j;i++){
	   if(inputs[i].className === 'mandatory' && inputs.value === ''){
	      inputs[i].style.borderColor = '#f00';
	      inputs[i].style.borderStyle = 'solid';
	      inputs[i].style.borderWidth = '1px';
	   }
	}

两个月后，产品改版不要加红色边框，要在在字段后面增加一个提示图标。

其他开发会改变你的JavaScript代码为如下：

	var f = document.getElementById('mainform');
	var inputs = f.getElementsByTagName('input');
	for(var i=0,j=inputs.length;i<j;i++){
	   if(inputs[i].className === 'mandatory' && inputs.value === ''){
	      inputs[i].className+=' error';
	   }
	}

__使用短路符号__

你一旦习惯使用短路符号，就会发现你的代码简洁并且容易阅读。

原代码：

	var lunch = new Array();
	lunch[0]='Dosa';
	lunch[1]='Roti';
	lunch[2]='Rice';
	lunch[3]='what the heck is this?';

改后代码：

	var lunch = [
	   'Dosa',
	   'Roti',
	   'Rice',
	   'what the heck is this?'
	];

原代码：

	if(v){
	   var x = v;
	} else {
	   var x =10;
	}

改后代码：

	var x = v || 10;

__模块化__

保持代码模块化和职责单一性。

很容易写出一个方法来处理好多事情，可是当你要扩展功能时，你应该拆分多个方法去实现相应的功能。

为了防止出现类似的情况，你应该保证代码的简单和通用去完成功能，而不是大而全的方法去实现。

在另一个功能时，你可以使用揭示模式暴露方法以此来扩展方法。

好的代码应该容易扩展而不需要重写核心代码。

__渐进增强__

避免JavaScript代码的高度耦合。

DOM的生成耗费时间并且代价昂贵。

有些元素的生成依赖JavaScript，但当JavaScript代码被禁用时，用户看到的页面就不会那么友好。

__允许配置和转移__

你的代码会随时被改变。

包括标签，类名和IDs以及外观。

把这些放到可配置项中，使你的代码易维护并可定制。

例如：

	carousel = function(){
	   var config = {
	      CSS:{
	         classes:{
	            current:'current',
	            scrollContainer:'scroll'
	         },
	         IDs:{
	            maincontainer:'carousel'
	         }
	      },
	      labels:{
	         previous:'back',
	         next:'next',
	         auto:'play'
	      },
	      settings:{
	         amount:5,
	         skin:'blue',
	         autoplay:false
	      },
	   };
	   function init(){
	   };
	   function scroll(){
	   };
	   function highlight(){
	   };
	   return {config:config,init:init}
	}();

__总结__

在这一节，我们讲到来命名规范，注释以及一些代码组织的建议。

原文：[javascript-best-practices-1](http://www.thinkful.com/learn/javascript-best-practices-1/Modularize)



































































