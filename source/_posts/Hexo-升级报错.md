title: Hexo 升级报错
date: 2014-09-26 00:12:30
tags: hexo
---

在mac上好不容易装了node(这几天node一直没有安装成功，原来是安装包错误！！！)，等到安装hexo之后，运行：

	hexo server

然后就报错：

```
{ name: 'HexoError',
  reason: 'incomplete explicit mapping pair; a key node is missed',
  mark: 
   { name: null,
     buffer: 'categories: Kategorien\r\nsearch: Suche\r\ntags: Tags\r\ntagcloud: Tag-Cloud\r\ntweets: Tweets\r\nprev: Vorherige Seite\r\nnext: Nächste Seite\r\ncomment: Kommentare\r\narchive_a: Archiv\r\narchive_b: Archiv: %s\r\npage: Seite %d\r\nrecent_posts: Neueste Artikel\n\u0000',
     position: 189,
     line: 9,
     column: 17 },
  message: 'Process failed: languages/de.yml',
  domain: 
   { domain: null,
     _events: { error: [Function] },
     _maxListeners: 10,
     members: [ [Object] ] },
  domainThrown: true,
  stack: undefined }
```

看到第二句我就搜了一下，结果原因是hexo升级，__Yaml Parser做出了一定的修改__。

解决办法：

```
1. 将主题language下的有空格的项都需要加上双引号。tag cloud --> "tag cloud"

2. 如果用的默认主题light，可以直接拉取最新主题，覆盖language文件夹即可。$ git clone https://github.com/tommy351/hexo-theme-light themes/
```

参考：

https://github.com/wileam/code/blob/master/source/_posts/update-hexo.md

http://www.huangyunkun.com/2014/07/31/hexo-update-error-with-yaml-parser/





