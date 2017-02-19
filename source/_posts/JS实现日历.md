title: JS实现日历
date: 2017-02-19 18:35:25
categories:
tags:
---

最近在捣鼓一个chrome插件，包括两个功能：1.倒计时，2.日历。

大致是下面这个样子：

![倒计时模式](/assets/images/countdown.png)

![日历模式](/assets/images/calendar.png)

倒计时模式倒还简单，日历刚开始以为困难点，仔细分析后也还挺顺利。

写过日历的一般都知道有三点很重要：

>1.一个月中有几天
2.一个月中第一天星期几
2.一个月中第后一天星期几
3.正确的处理换行

从我写这个日历的思路开始：

1.刚开始思路有点乱，想着既要输出月份，又要输出月中的天数，还要处理各种换行，好多事。后来我就想倒退吧，先输出一个月的，如果指定月份的日历输出了，下面就是循环遍历月份就行了。

下面是三月份的日历输出代码：

```
    let today = new Date(),
        year = today.getFullYear(),
        month = today.getMonth();

    // ❌

    let dayStr = '';
    // 3月有31天
    let dayInMonth = 31;

    // 每个月的第一天
    let firstDay = new Date(year,month,1);
    // 每个月的最后一天
    let lastDay = new Date(year,month,31);

    //第一天星期几(0-6)
    let weekday = firstDay.getDay();
    // 当月的最后一天星期几
    let lastDayWeekDay = lastDay.getDay(); 

    // 每一个都是从1号开始
    let date = 1;

    // ❌

    // 补齐前面的空格
    for(let i = 0; i < weekday; i++){
        dayStr += '+ ';
    }

    for(;date <= dayInMonth;date++){

        dayStr += date + ' ';
        weekday++

        // 换行处理
        if(weekday%7 == 0){
            weekday = 0;
            dayStr += '\n';
        }

    }

    // 补齐后面的空格
    for(let j = 0; j < (7 - lastDayWeekDay - 1); j++){
        dayStr += '+  ';
    }
```
可以把上面代码拷贝到控制台执行，看下效果。

可能看上去还不像日历，日历是有头的，周几，周几，把下面代码分别加入上面代码❌处。

    let weeks = ['日','一','二','三','四','五','六'];

    dayStr += weeks.join(' ') + '\n';

这样就比较像了。其实讲到这里基本日历就差不多了，还有一个重要的方法，就是算出指定月份的天数，其实JS的 Date 里没有给出api，所以需要自己算。
好忧伤，我查了一下python里的日历相关的给了方法。但是JS里有一个 getDate() 方法获取月份中的第几天。经过网上的搜索，得到如下方法：

    function daysInMonth(month, year) {
      return new Date(year, month + 1, 0).getDate();
    }

我来解释一下：当我们尝试传入0（月份是 0-11 ）一月份，2017，得到 new Date(2017,1,0)。这代表是2月份的第0天，好奇怪，哪有第0天的？其实某月的第0天就是前一个月的最后一天，所以就得到了某月的天数。

完整代码如下：

```
    function daysInMonth(month, year) {
      return new Date(year, month + 1, 0).getDate();
    }

    let weeks = ['日','一','二','三','四','五','六'];

    let year = new Date().getFullYear();
    let dayStr = '';

    for(let month = 0; month <=11; month++){
        // 每个月的第一天
        let firstDay = new Date(year,month,1); 
        let dayInMonth = daysInMonth(month,year);
        // 每个月的最后一天
        let lastDay = new Date(year,month,dayInMonth);

        // 第一天星期几(0-6)
        let weekday = firstDay.getDay(); 
        // 最后一天星期几
        let lastDayWeekDay = lastDay.getDay();
        // 每一个都是从1号开始
        let date = 1;

        dayStr += weeks.join(' ') + '\n';

        // 补齐前面的空格
        for(let i = 0; i < weekday; i++){
            dayStr += '+ ';
        }

        for(;date <= dayInMonth;date++){
            dayStr += date + ' ';
            weekday++

            if(weekday%7 == 0){
                weekday = 0;
                dayStr += '\n';
            }
        }

        // 补齐后面的空格
        for(let j = 0; j < (7 - lastDayWeekDay - 1); j++){
            dayStr += '+ ';
        }

        dayStr += '\n\n';
    }
```
参考：

1.http://stackoverflow.com/questions/315760/what-is-the-best-way-to-determine-the-number-of-days-in-a-month-with-javascript
2.https://github.com/lishengzxc/bblog/issues/5





