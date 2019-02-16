---
title: 如何搭建git page博客
date: 2017-09-26 22:32:11
tags: gitpage
---

#如何搭建gitpage博客

使用github page服务可以快速搭建个人博客，在搭建过程中遇到了些问题，所以在此做一次完整的梳理，让后来人都能躲过这一个个坑。

搭建步骤如下:
1.申请github账号(哈哈，废话)，(国内也有其他的类似平台，比如coding.net这个也尝试过，coding.net搭建过程类似，但是必须在首页添加Hosted by Coding Pages文字或者图标，不添加的情况下会有一次的中间跳转页面，添加完成后并且审核通过就可以跳过跳转页面)。
2.Create a new repository,项目名称为username.github.io,比如我的就是zhaoxmf.github.io,username就是你的github用户名或者组织名称。
3.使用git将该项目clone到本地，
`git clone https://github.com/username/username.github.io`
4.切换进用户目录
`cd username.github.io`
5.创建一个index.html文件：
`echo "hello world" > index.html`
6.提交到github
<pre>git add -all
git commit -m "initial commit"
git push -u prigin master</pre>

9.现在去访问https://username.github.io, 是不是已经显示出hello,world，那么恭喜你，第一个博客搭建完毕

当然这只是初始版，进阶版请查看下一篇