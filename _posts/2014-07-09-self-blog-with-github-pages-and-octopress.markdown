---
layout: post
title: "用GitHub Pages和Octopress创建个人博客"
date: 2014-07-09
categories:
- blog
permalink: self-blog-with-github-pages-and-octopress
description: My personal blog based on github pages and octopress
---

不久前了解到可以使用[Github Pages][Pages] 来搭建一个免费的不限流量和空间的静态网站，结合Octopress的便可以搭建一个博客系统。花了一些时间学习了使用方法，把具体的过程在这里记录下来，备忘同时可以给其他人作为参考。

<!--more-->

## 简介

首先简单介绍一下我们使用的工具。

#### 1. Github 简介

[Github][]是一个很好的云端代码管理仓库，能够与版本管理软件[Git][]完美结合。我们可以通过Git将本地的代码上传到云端，以便更好更方便的与他人合作与交流。 除了普通的代码管理功能之外，Github提供了一个用户可以自定义的项目首页，即[GitHub Pages][Pages]。通过GitHub Pages，我们可以编写一个自己的站。

使用Github Pages来编写个人网页，我们只需要将代码上传到Github，Github会生成并托管整个网站。这样便免去了我们自己来管理服务器的麻烦，同时可以使用Git来进行版本控制。

#### 2. Jekyll 简介

[Jekyll][]是一个静态网页生成器。由于GitHub Pages只支持静态网页，因此，我们需要使用Jekyll将动态网页装换为静态网页。Jekyll支持[Liquid][]和[Markdown][]，我们使用Liquid来写博客的模板，使用Markdown来写博客的内容，再使用Jekyll转换为静态网页，再推送到GitHub。

#### 3. Octopress 简介

[Octopress][]是一个为[Jekyll][]设计的博客框架。Jekyll需要用户自己编写博客的模板，Octopress则提供了一个良好的模板，免去了用户自己设计的过程。此外，Octopress还提供了一些实用的小工具，方便用户配置博客系统以及编写博客。

## 具体实例

下面是一个具体的实例来说明配置的过程。

#### 1. 需要的软件

首先是安装[Git][]，我们依赖它与Github通信，同时对代码进行版本管理。

其次是[Ruby][]，版本号需要大于等于1.93。

#### 2. 安装Octopress

选择一个存放代码的文件目录，将octopree从GitHub克隆下来。


{% highlight bash %}
cd Codes   #选择Codes这个文件夹来存放代码
git clone git://github.com/imathis/octopress.git
cd octopress   #进入克隆得到的octopress文件夹
{% endhighlight %}

安装bundler，并使用bundler安装一些Octopress需要的软件包。

{% highlight bash %}
gem install bundler
bundle install
{% endhighlight %}

然后便可以安装Octopress的默认的主题。

{% highlight bash %}
rake install
{% endhighlight %}

这一步Octopress会创建一些文件目录，包括`source`，`sass`，`source/_posts`，`public`。

#### 3. 创建代码管理仓库

使用GitHub Pages首先需要用户创建一个特殊的代码管理仓库，即仓库必须命名为*username*.github.io，其中*username*是你的GitHub用户名。如下图所示。

![CreateRepository]({{ site.baseurl}}/assets/images/BlogGithubPages/CreateRepository.png "Create Repository")

点击下方**Create repository**即可创建一个仓库，创建成功之后可以得到如下图红框中所示的一个链接。

![SSHLink]({{ site.baseurl }}/assets/images/BlogGithubPages/SSHLink.png "SSH Link")

Github Pages会认识别该仓库下的默认（default）分支，将该分支下的文件上传到服务器，创建一个静态的网页，页面的URL为`http://username.github.io`。`username`为用户名，因此上例中的URL便是`http://yuge.github.io`。

#### 4. 使用Octopress创建站点

为了使用同样的一个仓库来保存项目的源文件，我们可以创建新一个分支，Octopress提供了一个方便的配置命令。

{% highlight bash %}
rake setup_github_pages
{% endhighlight %}

该命令会询问你的Github仓库的URL，将上图红框中的链接复制粘贴即可。该命令实际上是一系列命令的批处理，主要完成以下几件事情。

1. 将imathis/octopress这一仓库的本地简称从'origin'改为'octopress'。
2. 添加你的你的Githut Pages仓库，命名为'origin'。
3. 将'origin'设为默认的远程仓库。
4. 将Master分支改名为`source`。
5. 删除\_deploy文件夹，再新建一个\_deploy文件夹。
6. 在\_deploy文件夹里面初始化一个Git仓库，该仓库为master分支。

**上一步中，Git与Github的传输协议是SSH，因此可能需要配置Github服务器的ssh keys，[这里][sshkey]有详细的说明。**

然后使用Octopress来生成静态网页，并上传的Github仓库。

{% highlight bash %}
rake generate
rake deploy
{% endhighlight %}

这一步，Octopress会使用Jekyll来生成静态网页，保存在\_deploy文件夹中。然后使用Git将_deploy文件夹中的文件上传到Github的master分支。几分钟之后，Github Pages便会发布的你站点。

当然，我们也可以将source分支中的源文件提交上传的Github。

{% highlight bash %}
git add .
git commit -m 'your message'
git push origin source
{% endhighlight %}

对于一个新的仓库，Github会将最先上传的一个分支设为默认分支，并且使用默认分支的文件来构建站点。如果你的默认分支不是master分支，可以到Github页面进行设置。在该分支主页面的右侧有一个`settings`按钮，点击进去在页面的上方便可以设置。如下图。


![Settings]({{ site.baseurl }}/assets/images/BlogGithubPages/Settings.png "Settings")

![Set Default Branch]({{ site.baseurl }}/assets/images/BlogGithubPages/SetDefaultBranch.png "Set Default Branch")

几分钟以后我们便可以访问自己的站点，浏览器输入`username.github.io`，便可以访问自己站点了。如下图所示。

![Initial Site]({{ site.baseurl }}/assets/images/BlogGithubPages/InitialSite.png "Initial Site")

#### 5. 写第一篇博客

Octopress提供了一个简单地创建一个博客的命令

{% highlight bash %}
rake new_post["title"]
{% endhighlight %}

其中`title`是你想创建的博客的名字。如：

{% highlight bash %}
rake new_post["First Blog"]
{% endhighlight %}

然后输入命令`rake preview`即可在本地预览页面。在浏览器中输入`localhost:4000`，即可看到如下图所示界面。

![First Blog]({{ site.baseurl }}/assets/images/BlogGithubPages/FirstBlog.png "First Blog")

该命令实际上是创建了一个Markdown文件，存放在`source/_post`目录，如`2014-07-12-first-blog.markdown`。我们修改该文件即可编写自己的博客。如。

{% highlight bash %}
layout: post
title: "First blog"
date: 2014-07-12 13:15:38 +0800
comments: true
categories:

Hello! 这是第一篇博客!
{% endhighlight %}

这样在预览界面可以看到博客的内容发生了变化。

![First Modification]({{ site.baseurl }}/assets/images/BlogGithubPages/FirstModification.png "First Modification")

#### 6. 生成静态页面并推送到Github

{% highlight bash %}
rake generate
rake deploy
{% endhighlight %}

上面两行命令即可方便的生成静态页面并将静态页面推送到Github。之后在`username.github.io`便可以看到啦。

到这里我们便完成一个博客站点的创建，并且成功地创建了第一篇博客。

#### 7. 编写一个独立的页面

Octopress提供了一个创建独立页面的命令，页面文件放在`source`目录中，URL为`username.github.io/pagename`，其中`pagename`是你创建的页面名称。

{% highlight bash %}
rake new_page[super-awesome]
# creates /source/super-awesome/index.markdown

rake new_page[super-awesome/page.html]
# creates /source/super-awesome/page.html
{% endhighlight %}


## Octopress 文件说明

这一部分是Octopress的文件说明。

#### 1. 存放代码的文件夹的根目录

在存放代码的文件夹的根目录里，`_config.yml`和`Gemfile`是两个我们会用到的配置文件。以及`source`，`public`，`_deploy`，`sass`这几个需要常用的文件夹。

`-config.yml`文件可以对Octopress生成的站点的一些固定信息进行配置，如`url`，`title`，`subtitle`，`author`等等，这些信息在生成站点的时候会用到。还有包括Markdown解析插件的选择，代码高亮的方式等，都需要在这个文件里进行配置。

`Gemfile`文件里是我们需要的插件的名字，这样我们可以简单地使用`bundle install`来安装我们需要的插件。

`source/`文件夹里面存放的是生成静态文件的源代码，包括Liquid格式的模板文件，以及Markdown格式的文件。后面会有详细的说明。

`public/`用来存放由`source`文件夹里面的文件生成的的静态网页，每次运行`rake generate`，`source` 文件夹里面的文件都会被删除，再生成一次。

`_deploy/`文件夹里面存放的是`origin/master`分支的文件，该文件夹里面的文件在使用`rake deploy`命令时会被清空，再将`public/`文件夹里面的文件复制过来。然后提交推送到Github。

`sass/`文件夹里面存放的是sass格式的样式文件，用来对模板进行配置。

#### 2. source文件夹

`source`文件夹里面包括`_includes`，`_layouts`，`_posts`，`images`这些文件夹。

`_includes`文件夹存放的是模板文件。`_deploy`存放的是页面的模板。`_post`是用些用markdown写的博客，`images`存放的图片文件。



[github]: https://github.com/
[git]: http://git-scm.com/
[pages]: https://pages.github.com/
[octopress]: http://octopress.org/
[jekyll]: http://jekyllrb.com/
[markdown]: http://daringfireball.net/projects/markdown/
[liquid]: http://docs.shopify.com/themes/liquid-documentation/basics
[sshkey]: https://help.github.com/articles/generating-ssh-keys
[Ruby]: https://www.ruby-lang.org/en/
