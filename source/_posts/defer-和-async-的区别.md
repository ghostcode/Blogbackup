title: 'defer 和 async 的区别'
date: 2014-08-10 14:59:26
tags:
---
加载javascript一直是页面优化的重头戏，defer和async又可以帮助一些：

###async:###

>Set this Boolean attribute to indicate that the browser should, if possible, execute the script asynchronously. It has no effect on inline scripts (i.e., scripts that don't have the src attribute).

大意：

>设置此Boolean属性就是让浏览器异步的执行此脚本，它对内敛脚本无效（就是没有src属性的脚本）

###defer:###

>This Boolean attribute is set to indicate to a browser that the script is meant to be executed after the document has been parsed. 

大意：

>设置此Boolean属性就是告诉浏览器在DOM节点解析完毕之前执行。

文档说的很少，其实还有好多东西，这里直接记录其异同：

__同：__

* 不阻塞页面的渲染
* 只能应用在通过src属性连接的javascript
* 此脚本内部不能有document.write
* 加载完成后可以触发onload事件

__异：__

* 出现的版本不同defer属于HTML4,async属于HTML5
* 各浏览器支持程度不同（IE4开始支持defer）
* 执行时机不同（defer在页面渲染完成之后执行，async在加载完成之后就执行）
* 执行的顺序不同（defer会严格按照页面出现的顺序执行，async在能执行的时候就会执行，有可能会顺序颠倒）

参考图：

![加载于执行](http://img2.picbed.org/uploads/2014/08/execution2.jpg)

注：

__正是由于async的执行顺序不可控，在具有依赖的脚本加载时不可以使用。对于没有依赖的代码可以使用比如统计代码、或者一个页面的单一脚本等__

参考：

https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/script

http://stackoverflow.com/questions/10808109/script-tag-async-defer

https://developer.mozilla.org/zh-CN/docs/Mozilla_event_reference/DOMContentLoaded_(event)

http://ued.ctrip.com/blog/?p=3121
