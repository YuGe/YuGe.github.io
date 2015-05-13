---
layout: post
title: "我的vimrc文件"
date: 2014-07-12 12:40:34 +0800
categories:
- blog
permalink: my-vimrc-file
description: My personal vimrc file
---

这里贴出了我的vimrc文件，一些设置后面会有注释，但是没有特别详细的说明。

这个配置文件能够通用与Mac和目前流行的发行版Linux，在Windows下能够绝大部分通用，但是可能会需要根据具体情况进行一下小的修改。

我使用的是[Vundle][]来管理插件，使用之前需要下载安装一下Vundle，然后`Bundle
Install`便可以安装所有的插件。

**[YouCompleteMe][]**是一个非常优秀的代码补全插件，且提供了多种语言的语义补全，但是安装的时候需要编译，有一点复杂。

<!-- more -->

{% highlight vim %}
{% include dotfiles/vimrc %}
{% endhighlight %}

[YouCompleteMe]: https://github.com/Valloric/YouCompleteMe
[Vundle]: https://github.com/gmarik/Vundle.vim
