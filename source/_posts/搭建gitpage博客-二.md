---
title: 搭建gitpage博客(二)
date: 2017-09-27 19:32:38
tags:
---
上一篇我们已经可以搭建出hello world博客，但是看起来不够炫酷，接下来我们就要使用hexo来给我们的博客添加一些特效。
英文好的同学直接戳这里：https://hexo.io/docs/

安装hexo之前需要安装node.js以及Git

安装完毕之后使用npm命令安装hexo,-g表示全局安装

`npm install -g hexo-cli`

使用以下命令安装Hexo到指定文件夹
<pre>
hexo init &lt;folder&gt;
cd &lt;folder>&gt;
npm install
</pre>

安装成功后
新建一篇文章，
<pre>
hexo init &lt;folder&gt;
hexo new [layout] &lt;title&gt;
hexo generate
</pre>

启动本地服务：

`hexo s`
相当于
`hexo server`

出现如下界面即表示服务启动成功，这个服务是用来做本地调试用

<pre>
$ hexo s
INFO  Start processing
INFO  Hexo is running at http://localhost:4000/. Press Ctrl+C to stop.
</pre>

浏览器访问http://localhost:4000/ 便可以看到效果，

那么问题来了，为什么全是英文的，并且博客标题也不对啊

接下来我们来配置自己的博客：

使用你喜欢的任意一款编辑器打开_config.yml文件
修改title为自己的博客名,注意yml文件对格式要求比较严格，冒号和后面的内容必须用一个空格隔开，
修改subtile为副标题
修改author为自己的名字
language比较重要，中文博客修改为zh-Hans
url是gitpage地址

在本地预览下是不是已经更符合你的期待了，可是现在是运行在本地，我们需要部署到gitpage上公网访问，hexo提供了自动部署到github或者其他平台的功能，我们需要修改配置文件告诉hexo部署的目标信息
修改_config.yml文件
对于repository需要是ssh形式的url而不是https，如何配置github的ssh请戳这里http://www.cnblogs.com/superGG1990/p/6844952.html ，我们在此我们只需要完成前两步即可，
然后点击clone or download 选择use ssh，把文本框的内容拷贝下来放到repository即可。

注意：这里type是git而不是github
<pre>
deploy:
	type: git
    repository: git@github.com:username/blog.git
    branch: master
</pre>

然后执行以下命令安装 hexo-deployer-git工具，
`npm install hexo-deployer-git --save`
执行以下命令部署到github，
`hexo d`

稍等片刻就可以使用你的usename.github.io访问你的博客了。

新建一篇博客记录下来写写你的感受吧。
`hexo new "title"`
在source/_posts找到你标题对应的.md文件使用一款自己喜欢的markdown编辑器开始写博客吧，我用的是haroopad，工具不重要，重要的是博客的内容，所以不用太纠结。
完成后
`hexo g`
`hexo d`
就成功部署到github上了，是不是很方便。