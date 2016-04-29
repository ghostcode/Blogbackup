title: 静默一切console.log和alert
date: 2016-04-29 12:19:30
categories:
tags:
---

开发的时候可以打开静默，上线可以静默：

```
// 备份以备后用
var __log = console.log;
var __alert = window.alert;
 
//静默 console.log alert
function mute() {
    console.log = function(){};
    window.alert = function(){};
}

//打开静默
function unmute() {
    console.log = __log;
    window.alert = __alert;
}
```