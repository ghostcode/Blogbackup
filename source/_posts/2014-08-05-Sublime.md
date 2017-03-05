title: 'Sublime Text2 个人常用插件'
date: 2014-08-05 09:23:46
tags: Sublime
---
这是个人常用Sublime Text 2的一些常用插件，会陆续更新，这并不是什么必备插件。

Sublime Text 2的插件安装方法：

	1. 按Ctrl+`调出console

	2. 粘贴以下代码到底部命令行并回车：

> import urllib2,os;pf='Package Control.sublime-package';ipp=sublime.installed_packages_path();os.makedirs(ipp) if not os.path.exists(ipp) else None;open(os.path.join(ipp,pf),'wb').write(urllib2.urlopen('http://sublime.wbond.net/'+pf.replace(' ','%20')).read())


* Emmet (前端的最爱)
* HTML/CSS/JS Pretty (代码美化)
* DocBlockr (注释)
* AngularJS
* SublimeCodeIntel (代码自动提示Javascript、python)
* ternjs (支持Vim,Emacs等javascript提示)
* ConvertToUTF8 (乱码解决)
* Ctage
* FakeImg.pl(图片占位)
* Placeholders
* Code​Formatter
* FileHeader(添加文件头部)
* [Java​Script Patterns](https://sublime.wbond.net/packages/JavaScript%20Patterns) (妈妈再也不担心我没有办法装逼了)
* [Java​Script & Node​JS Snippets](https://sublime.wbond.net/packages/JavaScript%20%26%20NodeJS%20Snippets) (提高效率)

技巧：(默认是在Preference-> Settings-User 添加)

* vim 编辑模式

> __"ignored_packages": []__,ESC进入 VIM模式。

* 双击带中划线的代码选中

>Preference-> Settings-Default 找到 __"word_separators"__,删除里面的‘-’。

* 高亮当前行

>"highlight_line": true

* 设置tab的空格数（我这里设置的是四个）


    "tab_size": 2,

    "translate_tabs_to_spaces": true


* 去除行末尾的多余空格


    "trim_trailing_white_space_on_save": true


主题：

	{
	    "theme": "Soda Light.sublime-theme",
	    "soda_folder_icons": true
	}

现在我的设置：

    {
        "color_scheme": "Packages/Color Scheme - Default/Monokai.tmTheme",
        "font_size": 13.0,
        "highlight_line": true,
        "ignored_packages":
        [
        ],
        "soda_folder_icons": true,
        "theme": "Soda Light.sublime-theme",
        "tab_size":4,
        "translate_tabs_to_spaces": true,
        "trim_trailing_white_space_on_save": true
    }



参考：http://buymeasoda.github.io/soda-theme/

更多插件：

https://sublime.wbond.net/