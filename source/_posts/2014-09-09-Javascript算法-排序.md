title: Javascript算法-排序
date: 2014-09-09 13:54:45
tags: Javascript
---
笔试会遇到一些算法问题，为了不被虐，也是为了增加知识，开始一个系列的算法学习。

__交换排序的基本思想是：两两比较待排序记录的关键字，发现两个记录的次序相反时即进行交换，直到没有反序的记录为止。__

__冒泡排序__


```
function sort(array){

    // 是否为数组
    var isArray = Object.prototype.toString.call(array).slice(8,-1).toLowerCase() === 'array';

    if(!isArray){
        return array + ' is not an Array!';
    }

    var n = array.length,
        tmp;

    for(var i = 0; i < n; i++){
        for(var j = i + 1; j < n - i; j++){
            if(array[j - 1] > array[j]){
                tmp = array[j - 1];
                array[j - 1] = array[j];
                array[j] = tmp;
            }
        }
    }
    return array;
}
```

调用：

    sort([119,20,10,3]) //[3,10,20,119]

    sort(123) //123 is not an Array.
