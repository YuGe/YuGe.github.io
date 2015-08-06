---
layout: post
title: Setting git server and hooks
date: 2015-08-06 21:08
categories:
- blog
permalink: setting-git-server-and-hooks
description: setting a git server and auto deploy with git hooks.
---

Git server is used to set personal Git repository. It can be used to share
source code with others. On the other side, it can be used to do auto deploy
with git hooks. This makes it very convenient when you want to deploy
automatically after post, such as web application server.

<!--more-->

## Add an server user

The first thing you need to deal with is the authentication problem, if you
need to write to the server. The most common and easiest method is to use
SSH protocol. You only need to add an user in the server and use it to authority
check and manage source code.

{% highlight bash %}
sudo adduser git
{% endhighlight %}

## Add SSH public key

If you want to access the server by SSH with this user. Your SSH public key need
to be added to the user's authorized_keys.

You can upload your local SSH public by `scp` to the sever.
{% highlight bash %}
scp ~/.ssh/id_rsa.pub user@sever:/tmp/
{% endhighlight %}

At the server side, append it to the `authorized_keys`.
{% highlight bash %}
cat /tmp/id_rsa.pub >> ~/.ssh/authorized_keys
{% endhighlight %}

## Make a directory as repository

First we make a directory to save all source code, and then make a sub directory
to save the code of a specific project. We use `git init` with the `--bare`
option to initialize an repository without a working directory.

{% highlight bash %}
sudo mkdir /var/git
chown git:git /var/git
su git
cd /var/git
mkdir project.git
git init --bare project.git
{% endhighlight %}

## Set remote repository for local project

If you don't have a working project, you need to initialize one. Then add the
server git repository as remote repository.

{% highlight bash %}
mkdir myproject
cd myproject
git init
touch README
git add .
git commit -m "initial commit"
git remote add origin git@server:/var/git/project.git
git push origin master
{% endhighlight %}

If you `commit` with nothing in the project, you may meet a error `src refspec
master does not match any`. You can add a `README` file and `commit`, `push`
again.

## Set git hooks for auto deploy

Git hooks are actually files contain shell scripts. Git will run these scripts
pre or post the push. The name of the file determined the time order to be run.
The `post-receive` hook runs after the entire process is completed, and thus can
be used to do auto deploy.

{% highlight bash %}
vim /var/git/project.git/hooks/post-receive
{% endhighlight %}

As it is actually shell script, you can write any code you want. An simple
example of `post-receive` file.

{% highlight bash %}
#! /bin/bash

while read oldrev newrev ref
do
    if [[ $ref =~ .*/master$ ]];
    then
        echo "Master ref received.  Deploying master branch to production..."
        git --work-tree=/var/www/html --git-dir=/var/git/project.git checkout -f
    else
        echo "Ref $ref successfully received.  Doing nothing: only the master branch may be deployed on this server."
    fi
done
{% endhighlight %}

This script update the working project on the server while a master branch was
successfully received. If another branch was received, it will no nothing.

You need to add the execute permission to the file.

{% highlight bash %}
chmod +x /var/git/project.git/hooks/post-receive
{% endhighlight %}

Now, the working project will be automatically updated while you push master
branch to the server.

<hr/>
Thanks for your read, please email me if you found any error.
