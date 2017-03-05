title: Promise reject and throw Error
date: 2016-03-27 21:49:13
categories:
tags: promise
---
以Promise为基础的方法应该reject的Promise，而不该抛出异常。

原因是如果这样做将需要两种错误处理方式：

```
function asyncFunc() {
    return doSomethingAsync() // (A)
    .then(result => {
        ···
    })
    .catch(error => { // (B)
        ···
    });
}
```
若在A行执行的异步方法抛出异常，但是B行并不会做出反应。

__Promise方法中处理异常__

如果异常在then和catch的回调函数中抛出并不会有问题，因为这两个方法会转变异常为reject。
可是若在异步方法中添加了同步方法：

```
function asyncFunc() {
    doSomethingSync(); // (A)
    return doSomethingAsync()
    .then(result => {
        ···
    });
}
```
如果在A行抛出异常，整个方法都会抛出一个异常，有两个方法可以处理此问题：

1.返回rejected Promise

可以捕获异常，然后作为rejected Promises返回：

```
function asyncFunc() {
    try {
        doSomethingSync();
        return doSomethingAsync()
        .then(result => {
            ···
        });
    } catch (err) {
        return Promise.reject(err);
    }
}
```

2.在回调中执行同步代码

以Promise.resolve()开启一个then的链式调用然后在回调中执行同步代码：

```
function asyncFunc() {
    return Promise.resolve()
    .then(() => {
        doSomethingSync();
        return doSomethingAsync();
    })
    .then(result => {
        ···
    });
}
```
译：http://www.2ality.com/2016/03/promise-rejections-vs-exceptions.html