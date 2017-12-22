---
title: Hello Hexo
date: 2017-09-08 02:32:22
---

## 前言

今晚花了一整晚的事件搭起了这个博客网站。要不第一篇博客，就来分享一下这个过程吧！

其实之前一直想用Github Pages做个人博客网站，但是由于时间和记忆力的关系就一直搁置着。

是这学期的《算法设计与分析》课程，老师要求我们每周用写一篇算法博客，作为平时作业。老师推荐用CSDN来写，但是我觉得太丑了，还是想折腾一下自己的博客网站。

所以呢，最后在张圆圆同学的推荐下，我选择了Hexo + Github Pages来做这件事情。

## 要点

1. 创建自己的github.io仓库

2. 安装Hexo，配置网站参数

3. 选择主题，配置主题参数

4. 添加标签

## 具体过程

### 创建自己的github.io仓库

这一步很简单，直接在Github上面New repository就好了。
不过记得repository的名字一定要是username.github.io，其中username是你在Github的用户名。
创建完之后也不用把仓库拉到本地。

### 安装Hexo，配置网站参数

Hexo是用Node写的，这对我来说很熟悉，直接用npm安装就可以了。

- 安装Hexo手脚架

  `npm install hexo-cli -g`

- 初始化Hexo博客项目

  `hexo init blog_project_name`

- 进入项目目录后，安装Node依赖包：

  `npm install`

- 跑起来！

  `hexo server`

然后就可以在浏览器访问localhost:4000来进入本地的博客网站啦。
在博客项目根目录下的_config.yml里面可以配置参数，过了四级应该能看懂。
另外，在_config.yml的deploy一项中，修改为：

    deploy:
        type: git
        repo: https://github.com/usernmae/username.github.io.git
        branch: master

现在就可以试下把本地的博客网站push到Github Pages上面了：

- 清除本地缓冲（重要，不然push到Github Pages上与本地的效果可能会相差很大）
  `hexo clean`

- 生成本地博客网站

  `hexo g`

- 部署到Github Pages

  `hexo d`

期间会提示你输入Github的用户名和密码，然后就可以真正访问username.github.io了！

### 选择主题，配置主题参数

我选择的是[clean-blog](https://github.com/klugjo/hexo-theme-clean-blog)，先把主题项目clone到本地，再进行参数配置即可。

- clone主题项目到本地（记得在博客项目根目录下运行该命令）

  `git clone https://github.com/klugjo/hexo-theme-clean-blog.git themes/clean-blog`

- 配置主题参数

其实这一步不是必要的，详见该主题项目的Github。

### 添加标签

- 添加标签页

  `hexo new page "tags"`

- 在source/tags/目录下，编辑刚刚生成的index.md，添加：

  `type: "tags"`

- 最后，写博客的时候直接使用标签即可

  `tags: [tag1, tag2, tag3]`

## 最后

希望这篇文章对想在Github Pages上搭建自己网站的同学有帮助！
也希望大家以后没事可以多来看看，假装讨论算法问题。

最后，告诉你一个小秘密，你看到主页的那张很好看的照片没有？没错，是我拍的。
Wow，是时候说一声，晚安。
