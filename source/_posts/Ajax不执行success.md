title: Ajax不执行success
date: 2015-05-22 11:10:28
categories:
tags: Ajax
---
最近老是遇到：Ajax返回状态码是200,但是就不执行success代码。

经过排查、总结，会有几个原因导致此问题：

1. 返回的数据类型一定要符合定义的数据类型。如果你的dataType是json类型，那么返回的数据一定要符合json,不然就会执行error。特别注意：含有 __\ 空格 回车 数据要转义__ 

2. done要和 __$.ajax__平级，而不是和success一样嵌套在里面（我就是这样错的）

3. ajax请求跨域（解决方法是在两个文件里都添加一段 js: __document.domain__，或者采用Jsonp的方式）

如果存在状态正确，但是执行error时，可以如下调试：

```
	.error(error){
		console.log(error)
	}
}
```

打印出error信息