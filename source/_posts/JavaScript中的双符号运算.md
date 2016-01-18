title: JavaScript中的双符号运算
date: 2016-01-16 10:45:52
categories:
tags:
---
__双波浪号__

```
var i = 5.1;
var j = 5.5;
console.log(~~i); // 5
console.log(~~j); // 5
```

作用类似Math.floor。

类似的意思是在处理正数的时候，如果处理负数就它俩就不同了：

```
~~-5.1 // 5
Math.floor(-5.1) // -6
~~-5.5 // 5
Math.floor(-5.5) // -6
```

注：

>Math.ceil(x)
Returns the smallest integer greater than or equal to a number.

>Math.floor(x)
Returns the largest integer less than or equal to a number.


__双感叹号__

```
var a = 1;
var b = null;
var c = '';
var d = 'code';
console.log(!!a); // true
console.log(!!b); // false
console.log(!!c); // false
console.log(!!d); // true
```

作用类似Boolean，把值转换为boolean值。

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/floor
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/floor