title: JavaScript事件整理
date: 2015-09-10 13:38:46
categories:
tags: javascript
---
在JavaScript中事件是一个很重要的概念，一切的业务处理都是基于用户与界面交互产生事件。

事件模型
---

当我们谈论事件的时候肯定要先知道事件有两种模型：事件冒泡（IE）、事件捕获（Netscape）

这两个是完全相反的事件模型概念：

事件冒泡：

>事件开始于最具体的元素（目标元素），然后逐级向上传播

事件捕获：

>事件开始于最外层元素，然后出击向下穿透，直到目标元素

而W3C组织做了一个明智的决定：__w3c的事件模型开始于事件捕获然后到达目标元素之后开始事件冒泡，并且给了开发者选择何时处理事件。__
<!-- more -->
```
    element1.addEventListener('click',doSomething2,true)

    element2.addEventListener('click',doSomething,false)
```

>true代表事件捕获，false代表事件冒泡

__注：很少人使用事件捕获，除非特殊要求。__

事件处理模型
---

有了事件之后我们就要处理，当然浏览器的竞争的硝烟肯定会在这里大干一场。

__DOM 0级事件处理模型__
这种模型是一个很久远的事了，但至今所有现代浏览器都支持，原因一是简单，二是具有跨浏览器的优势。有时
候这种事件处理模型也被认为是元素的方法，所以程序中的this指向的是当前元素。

```
    var btn = document.getElementById('myBtn')
    btn.onclick = function(){
        alert(this.id)//"myBtn"
    }
```
删除事件绑定：
```
btn.onclick = null
```

__DOM 2级事件处理模型__

此模型提供两个方法：addEventListener,removeEventListener，并且接受三个参数：事件名，处理函数，布尔值。
通过addEventListener添加的事件只能通过removeEventListener移除，而且匿名函数无法移除。

```
element1.addEventListener('click',function(){
    //doSth
},false)

element1.removeEventListener('click',function(){
    //doSth
},false)

```
上面方法是无法移除事件的，因为上面两个方法看似相同，其实是声明了两个不同的方法。所以如果要起效果必须像下面这样：

```
var handler = function(){
    //doSth
}
element1.addEventListener('click',handler,false)

element1.removeEventListener('click',handler,false)

```
可以使用addEventListener添加多个事件，而且执行顺序就是添加顺序。

__IE事件处理模型__

IE有两个方法和DOM 2的类似：attachEvent、detachEvent，只接收两个参数：事件名，处理函数。没有第三个参数的原因是：IE8-的浏览器只支持冒泡，所以事件都在冒泡阶段处理的。

```
    var btn = document.getElementById('myBtn')
    btn.attachEvent('onclick',function(){
        alert('Clicked')
    })
```
同样IE模型也可以添加多个事件，但是执行顺序与添加顺序相反。还有移除事件和DOM2类似，不能是匿名函数。最后有一点要注意IE事件模型的处理函数中的this指向的是window。

解决方法：

```
var handler = function(){
    
}
var element = //the element, doesn't matter how it is obtained
element.attachEvent("onclick", function() {
    //call handler with 'this' == 'element'
    return handler.apply(element, arguments);
});
```
还有一个方法，事件处理方法中使用this，无非就是想得到绑定的元素，可以另辟蹊径：
```
var element = //the element, doesn't matter how it is obtained
element.attachEvent("onclick", function(event) {
    if(!event){
        event = window.event
    }
    var callerElement = event.target || event.srcElement
    //doSth
});
```

__跨浏览器事件处理__
```
var EventUtil = {
    addHandler:function(element,type,handler){
        if(element.addEventListener){
            element.addEventListener(type,handler,false)
        }else if(element.attachEvent){
            element.attachEvent('on'+type,function(){
                return handler.apply(element,arguments)
            })
        }else{
            element['on'+type] = handler
        }
    },
    removeHandler:function(element,type,handler){
        if(element.removeEventListener){
            element.removeEventListener(type,handler,false)
        }else if(element.detachEvent){
            element.detachEvent('on'+type,handler)
        }else{
            element['on'+type] = null
        }
    }
}
```
__浏览器默认事件（行为）__

a标签的跳转，submit提交表单，reset重置表单。

__阻止默认事件__

有时候你不需要元素的默认事件，就得阻止它。此时你会想到preventDefault、return false，他们都可以做到阻止但是有何不同呢？
return false只能作用于：
HTML事件属性

```
<a href="http://www.iwjw.com" onclick="alert('iwjw');return false;" id="iwjw">iwjw</a>
```
DOM 0级事件
```
var element = document.getElementById('iwjw')
element.onclick = function(){
    //doSth
    return false
}
```
还有需要注意return false只能放在函数里的最后一行，而preventDefault可以放在函数体的任意位置。这就导致一个问题：
```
element.onclick = function(e){
    //doSth
   throw "Error";  // 此处JS抛出一个错误，就会忽略后面的代码，即浏览器会执行默认行为
   return false;
});
element.onclick = function(e){
   e.preventDefault();
   //doSth
   throw "Error"  // 此处JS抛出一个错误，同样会忽略后面的代码，但是已经调用过peventDefault方法，浏览器不会执行默认行为
});
```
对于addEventListener添加的事件，return false是无效的：
```
element.addEventListener('click',function(e){
    alert(123)
    return false
},false)
```
只能使用addEventListener，这或许就是peventDefault灵活的地方,但是如果你对return false一见钟情、不离不弃，还是想用它，如之奈何？提供一个方法：
```
element.onclick = function(e){
    try{
        alert('onclick')
        //或许有错误
        return false
    }catch(e){
        return false
    }
}
```
如果有错误，就会捕获错误执行catch里的return false，若没有错误则执行try里面的return 

__阻止冒泡__
因为事件有冒泡的阶段所以事件会造成“污染”，即影响父级元素，那如何阻止呢？
IE：
```
window.event.cancelBubble = true
```
W3C:
```
e.stopPropagation()
```
跨浏览器：
```
function doSomething(e)
{
    if (!e){
        e = window.event;
        e.cancelBubble = true;
    } 
    if (e.stopPropagation) e.stopPropagation();
}
```
参考：

1. http://www.quirksmode.org/js/events_order.html
2. http://stackoverflow.com/questions/4924036/return-false-on-addeventlistener-submit-still-submits-the-form
3. http://www.w3school.com.cn/js/js_errors.asp