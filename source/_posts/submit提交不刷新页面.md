title: submit提交不刷新页面
date: 2015-04-23 09:48:12
categories:
tags:
---
###form中点击submit不刷新页面，纯html。###

```
	<iframe style="display: none;" src="" name="target_submit"></iframe>
	<form action="target_submit">
		<input type="text"/>
		<input type="submit" value="提交"/>
	</form>
```

技巧：提交表单，目标刷新页是iframe。

###阻止默认的submit，使用JavaScript提交。###

```
	$('#formId').submit(function () {
		//code goes here
	 	return false;
	});
```


