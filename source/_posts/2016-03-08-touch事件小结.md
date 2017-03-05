title: touch事件小结
date: 2016-03-08 15:14:45
categories:
tags:
---
目前支持良好的三种触摸事件：1. touchstart 2. touchmove 3. touchend。

>ToucheEvent -> TouchList -> Touch

__触摸事件属性（TouchEvent）：__

> Represents an event that occurs when the state of touches on the surface changes.

![img](/assets/images/touchevent.png)


__触摸事件列表（TouchList）：__

>Represents a group of touches; this is used when the user has, for example, multiple fingers on the surface at the same time.

* touches 当前位于屏幕上的所有手指动作的列表。
* targetTouches 位于当前 DOM 元素上的手指动作的列表
* changedTouches 涉及当前事件的手指动作的列表。例如，在一个 touchend 事件中，这将是移开手指

__触摸事件（Touch）：__

>Represents a single point of contact between the user and the touch surface.

![img](/assets/images/touch.png)

__位置信息：__

* clientX/Y
* pageX/Y
* screenX/y

这些信息可以判断拖动的方向（下拉刷新／上拉加载）。