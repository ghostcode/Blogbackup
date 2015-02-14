title: 忘却jQuery与“原配”JavaScript重归于好
date: 2015-02-08 14:58:42
categories:
tags:
---
JavaScript就在那里不离不弃，你却对她视而不见。为何不用jQuery？因为它太慢而且在确认你站点不需要而外的负担时。

我无意与在框架和原生代码之前争辩，有时抛弃使用那些神奇的东西很难。但是在这我想说的是仅仅为了一个字符选择器而加载一大坨代码，例如__$__或者其他类似的。

假定简写并不是它的唯一特点，每个使用框架的人是因为支持IE，动画处理和单一的选择器功能。

###原生等价###

选择元素

	// jQuery
	var els = $('.el');

	// Native
	var els = document.querySelectorAll('.el');

	// Shorthand
	var $ = function (el) {
	  return document.querySelectorAll(el);
	}

	var els = $('.el');

	// Or use regex-based micro-selector lib
	// http://jsperf.com/micro-selector-libraries

<!-- more -->

创建元素

	// jQuery
	var newEl = $('<div/>');

	// Native
	var newEl = document.createElement('div');

添加事件监听

	// jQuery
	$('.el').on('event', function() {

	});

	// Native
	[].forEach.call(document.querySelectorAll('.el'), function (el) {
	  el.addEventListener('event', function() {

	  }, false);
	});

设置／获取属性

	// jQuery
	$('.el').filter(':first').attr('key', 'value');
	$('.el').filter(':first').attr('key');

	// Native
	document.querySelector('.el').setAttribute('key', 'value');
	document.querySelector('.el').getAttribute('key');

添加／删除／切换 类名

	// jQuery
	$('.el').addClass('class');
	$('.el').removeClass('class');
	$('.el').toggleClass('class');

	// Native
	document.querySelector('.el').classList.add('class');
	document.querySelector('.el').classList.remove('class');
	document.querySelector('.el').classList.toggle('class');

添加元素

	// jQuery
	$('.el').append($('<div/>'));

	// Native
	document.querySelector('.el').appendChild(document.createElement('div'));

复制元素

	// jQuery
	var clonedEl = $('.el').clone();

	// Native
	var clonedEl = document.querySelector('.el').cloneNode(true);

删除元素

	// jQuery
	$('.el').remove();

	// Native
	remove('.el');

	function remove(el) {
	  var toRemove = document.querySelector(el);

	  toRemove.parentNode.removeChild(toRemove);
	}

获取父元素

	// jQuery
	$('.el').parent();

	// Native
	document.querySelector('.el').parentNode;

获取前一个／后一个元素

	// jQuery
	$('.el').prev();
	$('.el').next();

	// Native
	document.querySelector('.el').previousElementSibling;
	document.querySelector('.el').nextElementSibling;

XHR又称Ajax

	// jQuery
	$.get('url', function (data) {

	});
	$.post('url', {data: data}, function (data) {

	});

	// Native

	// get
	var xhr = new XMLHttpRequest();

	xhr.open('GET', url);
	xhr.onreadystatechange = function (data) {

	}
	xhr.send();

	// post
	var xhr = new XMLHttpRequest()

	xhr.open('POST', url);
	xhr.onreadystatechange = function (data) {

	}
	xhr.send({data: data});

这里只是一部分，你可以在浏览器控制台里找到更到或者[ MDN’s JS API reference ](https://developer.mozilla.org/ru/docs/Web/API)或者[WPD’s DOM docs](http://docs.webplatform.org/wiki/dom)。

你依旧可以使用框架或者库，在你确定没有库不能达到你的目的时，可以在[此处](http://microjs.com/)找到完成特定任务的库，否则还是使用原生JavaScript。