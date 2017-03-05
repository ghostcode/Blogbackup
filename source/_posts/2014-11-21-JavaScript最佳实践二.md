title: JavaScript最佳实践二
date: 2014-11-21 22:09:20
categories:
tags:
---
__避免多层嵌套__

>多层嵌套会降低易读性

在循环中嵌套循环是一个不好的做法，同时还要维护多个迭代变量（i,j,k,l,m...）。

	function renderProfiles(o){
	   var out = document.getElementById('profiles');
	   for(var i=0;i<o.members.length;i++){
	      var ul = document.createElement('ul');
	      var li = document.createElement('li');
	      li.appendChild(document.createTextNode(o.members[i].name));
	      var nestedul = document.createElement('ul');
	      for(var j=0;j<o.members[i].data.length;j++){
	         var datali = document.createElement('li');
	         datali.appendChild(
	            document.createTextNode(
	               o.members[i].data[j].label + ' ' + 
	               o.members[i].data[j].value
	            )
	         );
	         nestedul.appendChild(detali);
	      }
	      li.appendChild(nestedul);
	   }
	   out.appendChild(ul);
	}

<!-- more -->

你可以避免多层嵌套，使用特定的方法在循环中完成循环：

	function renderProfiles(o){
	   var out = document.getElementById('profiles');
	   for(var i=0;i<o.members.length;i++){
	      var ul = document.createElement('ul');
	      var li = document.createElement('li');
	      li.appendChild(document.createTextNode(data.members[i].name));
	      li.appendChild(addMemberData(o.members[i]));
	   }
	   out.appendChild(ul);
	}
	function addMemberData(member){
	   var ul = document.createElement('ul');
	   for(var i=0;i<member.data.length;i++){
	      var li = document.createElement('li');
	      li.appendChild(
	         document.createTextNode(
	            member.data[i].label + ' ' +
	            member.data[i].value
	         )
	      );
	   }
	   ul.appendChild(li);
	   return ul;
	}

 想像一下不好的编辑器和小屏幕。

 __优化循环__

 >循环在JavaScript中很消耗性能

好多时间浪费在做无用功。

不要在每次循环时去读取数组长度，应该将其存储在另一个变量中。

	var names = ['George', 
	'Ringo', 
	'Paul', 
	'John'];
	for(var i=0;i<names.length;i++){
	   doSomethingWith(names[i]);
	}

优化：

	var names = ['George', 
	'Ringo', 
	'Paul', 
	'John'];
	for(var i=0,j=names.length;i<j;i++){
	   doSomethingWith(names[i]);
	}

避免在循环中进行过多的计算，包括正则表达式和先前的DOM操作。

你可以在循环中创建DOM节点，但是请不要在其中进行插入文档操作。

__尽可能少操作DOM__

>如果你可以避免，那就不要进行DOM操作

原因：首先DOM操作很耗费资源同时在不同浏览器中总有一些问题。

方法：自己写或者使用别的工具来进行数据到HTML的转化。

找到尽可能多的数据然后一次渲染完。

__不要屈服于浏览器的特性__

>不要依赖浏览器的古怪行为并且希望都兼容。

避免Hack而是要分析问题细节。

当你需要额外功能时，多数时候是因为你糟糕的接口设计。

__不要迷信任何数据###

>好的代码都不信任任何输入的数据

* 不要相信HTML文档

任何人都可以对文档瞎搞比如使用Firebug

* 不要相信数据总是以正确的格式传入函数

使用Typeof进行判断然后继续执行程序

* 不要相信DOM中的元素总是可获取的

在改变DOM之前要测试确保相应元素正确存在

* 不要使用JavaScript去保护任何东西

因为它非常脆弱

__使用JavaScript完成功能而不是生产内容__

>如果在JavaScript中写了许多HTML代码，这表明你做错了

使用DOM接口生成HTML不是很方便并且难以保证生成HTML的质量。

如果你有大量的接口只有当JavaScript开启时才会有效，这时应该使用Ajax来接收静态页面文件。

这种方法使你更易维护HTML片段同时可以进行定制它。

__站在巨人的肩膀上__

>JavaScript很有趣，但是对多个浏览器写代码时体验就不是很好......所以使用一个好的框架

JavaScript框架就是用来修复浏览器的差异性以使浏览器行为和你的代码按预期运行。

好的框架让你专注于业务代码而不是浪费时间去解决各种浏览器的内部差异。

__开发代码不是活得的代码__

>活得代码是面向机器的，开发代码是面向人类的

* 在构建阶段校验，压缩和优化你的代码

* 不要过早优化和批评你的同事，这些会使他们从中吸收经验

* 如果减少编写代码的时间，我们可以有更多的时间来完善向机器码的转换

原文：[javascript-best-practices-2](http://www.thinkful.com/learn/javascript-best-practices-2/Avoid-Heavy-Nesting)