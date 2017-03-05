title: 低版本Webkit的popstate事件问题
date: 2015-12-22 15:43:14
categories:
tags:
---
对于popstate事件大家应该有个印象：活动历史项改变时触发。

以下几种情况会触发：

* 点击浏览器的前进，后退按钮
* Javascript代码中调用history.back(),forward(),go()

来个简单的例子：

```
    window.onpopstate = function(event) {
      alert("location: " + document.location + ", state: " + JSON.stringify(event.state));
    };
    history.pushState({page: 1}, "title 1", "?page=1");
    history.pushState({page: 2}, "title 2", "?page=2");
    history.replaceState({page: 3}, "title 3", "?page=3");
    history.back(); // alerts "location: http://example.com/example.html?page=1, state: {"page":1}"
    history.back(); // alerts "location: http://example.com/example.html, state: null
    history.go(2);  // alerts "location: http://example.com/example.html?page=3, state: {"page":3}

```

<!-- more -->

>Browsers tend to handle the popstate event differently on page load. 
Chrome (prior to v34) and Safari always emit a popstate event on page load, but Firefox doesn't.

>浏览器对于页面加载时处理popstate事件的方式有些不同，Chrome(34版本之前)和Safari当浏览器加载时触发popstate事件，但是火狐
并不会。

到目前我测试的Safari（9.0）浏览器加载页面时依旧会触发popstate。

测试代码：

```
    window.addEventListener('popstate',function(event){
        console.log('popstate');
    },false);

```
Safari（9.0-）下测试，会打印出:__popstate__。

如何避免？

在时间队列中添加事件，等到进程空闲时则会尽快执行：

```
    setTimeout(function(){
        window.addEventListener('popstate',function(){
            console.log('popstate')
        },false);
    },0)

```

初次加载时，event.state值为null:

```
    window.addEventListener('popstate',function(event){
        if(event.state == null){
            return 
        }
        console.log('popstate')
    },false)
```

参考：

1. https://github.com/visionmedia/page.js/pull/243
2. https://github.com/rackt/react-router/blob/7f60ea666a5791f7d22b645f0a513f5dd89f580e/build/global/ReactRouter.js#L725
3. http://html5doctor.com/history-api/