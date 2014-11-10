title: JavaScript最佳实践一
tags: JavaScript
---
###变量和函数名有意义###

选择易理解和简短的名字作为变量和方法。

__不好的名字__

	x1 fe2 xbqne

__同样糟糕的名字__

	incrementerForMainLoopWhichSpansFromTenToTwenty
	}
	createNewMemberIfAgeOverTwentyOneAndMoonIsFull

尽量避免使用变量和方法的名字去解释值。

__或许在其他一些国家是没有意义__

	isOverEighteen()

__通用__

	isLegalAge()

你的代码就是一个故事，请保持故事情节深入人心（容易阅读）。

###避免全局###

全局变量是一个相当可怕的坏注意。

__原因：__你的代码极有可能被添加到你后面的代码所覆盖。

__变通：__使用闭包和模块模式。

问题：所有的全局变量可以在任何地方被访问；这是无法避免的，任何在页面上的代码都可以重写你将要做的。

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

暴露模块模式：保持相同的代码风格同时暴露全局方法和变量。

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











































































