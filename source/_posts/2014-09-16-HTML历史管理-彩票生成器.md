title: HTML历史管理-彩票生成器
date: 2014-09-16 14:38:29
tags:
---
一般的彩票网站都有__机选__（随机选择）功能：

> [网易彩票](http://caipiao.163.com/order/dlt/#from=syks)

> [好123彩票](http://caipiao.hao123.com/)

下面就用javascript简单模拟一下

__HTML__

```
<div id="caipiao"></div>
<button id="J_btn">彩票生成</button>
```

<!-- more -->

__Javascript__


```
var btn = document.getElementById('J_btn'),
    caipiao = document.getElementById('J_caipiao');
btn.addEventListener('click',function(){
    var nums = randomNum(35,7);
    caipiao.innerHTML = nums;
});

/**
 * 从指定数组中随机抽取指定个数的数字
 * @param  {[Number]} alls     [总个数]
 * @param  {[Number]} selected [要选择的个数]
 * @return {[Array]}          [返回的指定个数的数组]
 */
function randomNum(alls,selected){
    var numArr = [],
        resultArr = [],
        index;
    for(var i = 1; i <= alls; i++){
        numArr.unshift(i);
    }
    for(var j = 0; j < selected; j++){
        index = Math.floor( Math.random() * alls );
        resultArr.push(numArr[index]);
    }
    return resultArr;
}
```
<a class="jsbin-embed" href="http://jsbin.com/binom/2/embed">JS Bin</a><script src="http://static.jsbin.com/js/embed.js"></script>

但是有时候我感觉前一组数字会更好，想返回，可是上面的没有此功能。

下面就来完成上面提到的需求：

```
var btn = document.getElementById('J_btn'),
    caipiao = document.getElementById('J_caipiao');
    btn.addEventListener('click',function(){
        var numKey = Math.random();
        var nums = randomNum(35,7);
        caipiao.innerHTML = nums;
        window.location.hash = nums;
    });
    /**
     * 从指定数组中随机抽取指定个数的数字
     * @param  {[Number]} alls     [总个数]
     * @param  {[Number]} selected [要选择的个数]
     * @return {[Array]}          [返回的指定个数的数组]
     */
    function randomNum(alls,selected){
        var numArr = [],
            resultArr = [],
            index;
        for(var i = 1; i <= alls; i++){
            numArr.unshift(i);
        }
        for(var j = 0; j < selected; j++){
            index = Math.floor( Math.random() * alls );
            resultArr.push(numArr[index]);
        }
        return resultArr;
    }
    window.onhashchange = function(){
        var num = window.location.hash.substr(1) || '';
        caipiao.innerHTML = num;
    };
```

预览地址：[demo1](http://jsbin.com/batocu/1/)

打开上面的连接，看地址栏的变化，会有一串数字。当生成几组之后，你可以通过浏览器的后退按钮选择前面生成的数。

<a class="jsbin-embed" href="http://jsbin.com/batocu/1/embed">JS Bin</a><script src="http://static.jsbin.com/js/embed.js"></script>

进一步优化：

__此时URL中的数字就是生成的数字，所以让人感觉不专业__，所以我们做下处理

```
var btn = document.getElementById('J_btn'),
    caipiao = document.getElementById('J_caipiao'),
    obj = {};

btn.addEventListener('click',function(){
    var nums = randomNum(35,7);
    caipiao.innerHTML = nums;
    var numKey = Math.random();
    obj[numKey] = nums;
    window.location.hash = numKey;
});
/**
 * 从指定数组中随机抽取指定个数的数字
 * @param  {[Number]} alls     [总个数]
 * @param  {[Number]} selected [要选择的个数]
 * @return {[Array]}          [返回的指定个数的数组]
 */
function randomNum(alls,selected){
    var numArr = [],
        resultArr = [],
        index;
    for(var i = 1; i <= alls; i++){
        numArr.unshift(i);
    }
    for(var j = 0; j < selected; j++){
        index = Math.floor( Math.random() * alls );
        resultArr.push(numArr[index]);
    }
    return resultArr;
}
window.onhashchange = function(){
    var num = obj[window.location.hash.substr(1)] || '';
    caipiao.innerHTML = num;
};
```

解析：

__新增了一个对象 obj，存储生成的数字，关键字是 随机生成的数字numKey，从hash取的时候也是根据numKey。__

预览：[demo2](http://jsbin.com/bakep/1/#0.8101516056340188)

此时你会看到浏览器后面有一串小数，貌似很专业。