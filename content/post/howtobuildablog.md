+++
date = "2016-03-24T20:02:12+08:00"
title = "如何快速搭建个人博客hugo + github pages"
tags = [
    "go",
    "golang",
    "hugo",
    "development",
]
categories = [
    "development",
]
toc = true
+++
## 为什么搭建自己的blog
1.像新浪还是qq空间我都买不起会员,没面子
3.没广告,能自定义,而且一下子就有了一种我能修电脑的高贵气质
## hugo
https://gohugo.io

hugo是go语言写的,其实用起来跟golang没一点关系,只用到编译好的程序.
它是个生态静态页面的工具.能把md文件渲染成网页.还自带了http sever,有 watch 功能.生成速度很快,watch反映也很快.

hugo生成静态网站的好处明显,不用数据库,也不需要你写逻辑,啥玩意儿你也不用管,写文章就完了,写完了就用工具调一条命令,啪!,网站好了.

## github pages
免费,无限流量,国内外都能上,速度还快.
怕就怕哪天有司机用github pages 飙车,一下被墙了就不好使了
在此呼吁大家遵守交通规则,文明驾驶.

## 如何使用
### 安装hugo
mac 用户
```
brew update && brew install hugo
```
我系统10.10的,安装的时候brew坏了,卸了从新安装的

brew安装
```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

brew 卸载
```
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/uninstall)"
```

windows 用户

去官网下载hugo.exe使用就行了

### 新建项目

```
hugo new site /建站文件夹
```
建完以后文件夹结构如下
```
tree -a
.
|-- archetypes
|-- config.toml
|-- content
|-- data
|-- layouts
`-- static
5 directories, 1 file
```
tree 是个挺好的工具 安装方法
```
brew install tree
```

### 写文章
hugo新建文章
```
hugo new post/hello.md
```
然后在./content/post 里面会多一个文件,内容这样的
```
+++
date = "2016-03-25T00:33:03+08:00"
description = ""
title = "hello"
draft = true
+++
```
draft表示是草稿,不会渲染.把这一行删掉就好了

### 选主题
https://themes.gohugo.io/

这里有各种各样的皮肤可以选,我选了比较简洁的hugo_theme_beg

安装方法
在项目里面建一个themes的目录
```
mkdir themes && cd themes
```
然后把你喜欢的主题clone下来
```
git clone git@github.com:dim0627/hugo_theme_beg.git
```

### 用hugo跑服务看下效果

```
hugo server -w --theme=hugo_theme_beg
```
打开浏览器访问 http://localhost:1313/

就能看到效果了.

### 上传至github pages
运行
```
hugo --theme=hugo_theme_beg
```
hugo会自动把渲染好的静态页面放在public下面

在github上建一个项目,命名方式: 用户名.github.io
然后把public里面的内容push去就可以了
以后每次修改重新运行上面命令,然后再用git同步即可

>不会用git的朋友,可以下载一个sourceTree,在github上练习下,无非就是pull->add/remove->commit->push

项目根目录下的config.toml记得修改baseURL
```
baseURL=                    "http://用户名.github.io/"
```

### 其他
disqus 支持
只要在config.toml里面添加
```
disqusShortname = "你的disqus用户名"
```
就可以了,但是disqus被墙了,国内用不了,我就换成了中国的`多说`

把themes/你选的主题/layouts/_default/single.html 里面disqus的换成下面的就行了

```
<!-- 多说评论框 start -->
<div class="ds-thread" data-thread-key="{{ .URL }}" data-title="{{ .Title }}" data-url="{{ .Permalink }}"></div>
<!-- 多说评论框 end -->

<!-- 多说公共JS代码 start (一个网页只需插入一次) -->
<script type="text/javascript">
    var duoshuoQuery = {short_name:"{{.Site.Params.duoshuoShortname}}"};

    (function() {
     var ds = document.createElement('script');
     ds.type = 'text/javascript';ds.async = true;
     ds.src = (document.location.protocol == 'https:' ? 'https:' : 'http:') + '//static.duoshuo.com/embed.js';
     ds.charset = 'UTF-8';
     (document.getElementsByTagName('head')[0]
      || document.getElementsByTagName('body')[0]).appendChild(ds);
     })();
    </script>
<!-- 多说公共JS代码 end -->
```
