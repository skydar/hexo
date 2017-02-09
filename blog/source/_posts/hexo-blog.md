---
title: 使用hexo并替换material主题
date: 2017-02-09 20:32:32
tags: github 博客 
---

# 使用hexo并替换material主题

*博客的发展有很长的历史了，常见的从老牌的网易、新浪博客到专注技术的CSDN、博客园，再到目前比较火的掘金、简书，相信很多人从新老交替的博客平台来回了很多次了，真心累，我非常赞同**阮一峰**大神的观点：*

>喜欢写Blog的人，会经历三个阶段。
　　第一阶段，刚接触Blog，觉得很新鲜，试着选择一个免费空间来写。
　　第二阶段，发现免费空间限制太多，就自己购买域名和空间，搭建独立博客。
　　第三阶段，觉得独立博客的管理太麻烦，最好在保留控制权的前提下，让别人来管，自己只负责写文章。[查看更多](http://www.ruanyifeng.com/blog/2012/08/blogging_with_jekyll.html)

使用github Pages来构建博客，真的很省心，阮大大介绍的是Jekyll，对比之后，我选择使用更强大更简便的hexo来构建静态博客系统

------

## 使用hexo

首先从fork一份[hexo](https://github.com/hexojs/hexo)作为当前开发的hexo版本，并clone到本地。
然后按照README：
### 安装

``` bash
$ npm install hexo-cli -g
```

### 快速开始

``` bash
$ hexo init blog
$ cd blog
```

这样blog文件夹下的`source/_posts`里面就有一个hello-world.md文件，告诉你如何本地开发以及部署等内容。
```
$ hexo server
```
这样本地就可以看到了。
界面是不是有点难看，不急我们先安装一个美观的主题，就是我推荐的material主题。

-----

## 安装material主题

*有几种方式，推荐github方式*

```
$ git clone https://github.com/viosey/hexo-theme-material.git themes/material
```
*同样如果你有定制的需求，先fork一份，然后在clone自己的repository*

## 使用material主题

修改`blog/_config.yml`
```xml
#site下包含很多个性化的内容
...
# Extensions
theme: material
...
# Deployment
type: git
repository: https://github.com/xxx/xxx.github.io.git
branch: xxx
```

## 保存你的设置

修改`blog/.gitignore`，增加：
```
themes/
```
然后
```bash
$ git add blog
$ git commit -am ''
# 或者 git push
```

下次只需要clone项目下的`blog`文件夹，npm install依赖包，以及同样的办法，clone你二次开发过的的material到`themes`文件夹下，就可以开始真正的写博客的工作了，最后再介绍。

## 部署到github
**配置github pages的过程省略**
剩下的就很简单了：

安装部署工具，并部署
```bash
$ npm install hexo-deployer-git --save
$ hexo generate
$ hexo deploy
```

## 开始你的第一篇博文
上文提到，其实自带了一篇hello-world的博文，不算，我们重新来写一篇：
```
$ hexo new "first"
```
hexo生成的标记语言是基于markdown的，所以会在`source/_posts`生成对应的first.md，然后编辑他；
执行 hexo server 可以一边开效果，一边编辑。

*有时候要放图片什么的，记得放在主题文件夹对应的目录下，然后hexo generate，分析路径再添加，这个自己研究吧*

## 删除博文
* 首先，在`source/_posts`找到你要删除的博文md文件，
* 然后
```bash
$ hexo clean
$ hexo generate
```

还有更多可玩的[推荐给你](http://blog.csdn.net/wizardforcel/article/details/40684575)

[1]: http://