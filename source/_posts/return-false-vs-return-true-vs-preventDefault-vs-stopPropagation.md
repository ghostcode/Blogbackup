title: return false vs return true vs preventDefault vs stopPropagation
date: 2015-09-10 13:38:46
categories:
tags:
---



ie8+ 支持 addEventListener

<a href="＃" onclick="showAlert()"></a>

等价于

a.onclick = function(){
    showAlert();
}

http://stackoverflow.com/questions/19166296/javascript-return-true-or-return-false-when-and-how-to-use-it

https://css-tricks.com/return-false-and-prevent-default/

http://stackoverflow.com/questions/128923/whats-the-effect-of-adding-return-false-to-an-onclick-event