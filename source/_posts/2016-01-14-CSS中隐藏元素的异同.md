title: 'CSS中隐藏元素的异同'
date: 2016-01-14 10:50:49
categories:
tags:
---

| --- |  隐藏元素 | 可以阻止响应事件  | 文档中移除元素  | 可以过渡动画  | 是否继承  | 阻止元素接受焦点  |
|---|---|---|---|---|---|---|
| opacity         | 是  | 否  | 否 | 是  | 否  | 否 |
| visibility      | 是  | 是  | 否 | 是  | 是  | 是 |
| display         | 是  | 是  | 是 | 否  | 否  | 是 |
| pointer-events  | 否  | 是  | 否 | 否  | 是  | 否 |

http://fellowtuts.com/html-css/difference-between-displaynone-visibilityhidden-or-opacity0/

https://css-tricks.com/snippets/css/toggle-visibility-when-hiding-elements/

http://stackoverflow.com/questions/272360/does-opacity0-have-exactly-the-same-effect-as-visibilityhidden

