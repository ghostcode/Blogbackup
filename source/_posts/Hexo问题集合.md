title: Hexo问题
date: 2014-09-26 00:12:30
tags: hexo
---

1.__Yaml Parser做出了一定的修改__

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

2.__hexo 3.1.1更新后代码高亮失效__

原因：

>Add option to disable highlight auto-detect

解决方法：

增加 auto_detect: true 到站点 _config.yml 的 hilight 部分，否则默认自动检测语言高亮是关闭的

```
  highlight:
  enable: true
  auto_detect: true
  line_number: true
  tab_replace: ''
```

参考：
https://github.com/hexojs/hexo/releases/tag/3.1.1

https://github.com/iissnan/hexo-theme-next/issues/186

3.__添加Travis CI__

以前发布Hexo博文的步骤：

	1.本地写好博文 hexo new "xxx"
	2.生成、发布 hexo -d -g
	3.同步博客原文到github

流程太机械，而且有时候忘记同步博客原文到仓库。机械的流程就应该自动化，就像现在前端代码的合并、压缩、打包、拼接雪碧图、CSS代码添加前缀等等机械的动作就应该流程化、自动化，省下更多的时间去做别的事情。然后就搜了一些 hexo 自动化的东西，找到了 Travis CI 这个东西，然后就愉快的集成了，现在本地写完博客只要同步就行了，生成发布的任务 Travis CI 帮你搞定了。

	1.本地写博文 hexo new "xxx"
	2.同步博文到github

参考：http://www.jianshu.com/p/e22c13d85659

4.hexo 添加实例

我的初衷需求是在 hexo 博客上添加一些代码实例，就是一些demo。然后就搜了一些方法，找到官网提供了一个博客配置项:

	skip_render: Paths not to be rendered. You can use glob expressions for path matching

大体意思就是 hexo 在生成博客时，就是忽略此目录（博客的配置文件_config.yml里的 source_dir 下的指定目录）

参考：https://hexo.io/docs/configuration.html
