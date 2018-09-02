---
layout:     post
title:      使用GitHub搭建自己的博客
subtitle:   快速使用GIthub搭建自己的博客系统
date:       2018-08-31
author:     wsccoder
header-img: img/post-bg-re-vs-ng2.jpg
catalog: true
tags:
    - Blog
    - GitHub

---

## 搭建过程

在搭建博客时候也踩了不少坑，之前写博客在博客园上面写的，无奈博客园的界面太……，自己原本写了过一个博客系统但是也部署在了阿里云的服务器上面，但是后期没有续费就GG了。在后面看各路大佬在Github上搭建博客，自己也学着搭建了一个，在这过程中也是踩了不少的坑。其中还是要谢谢几位博主的，我也是根据几位博主的博客自己一点点的搭建起来的。

**废话少说我们下面就开始搭建博客**

> 我们的博客系统是基于Jekyll开源的，部署GitHub上面。这样我们可以省去搭建服务器的过程。

## 展示效果 

首先我们看下博客的展示效果

**网页版本的展示效果**

![](https://ws1.sinaimg.cn/large/005S1Oyygy1fuviuej6z5j326m14iqv5.jpg)

**手机版本展示效果**

![](https://ws1.sinaimg.cn/large/005S1Oyygy1fuviywb2g2j30ly0zo7l9.jpg)



## 开始进入搭建的过程

**注册GitHub账号**

> 我们搭建博客的方式是Github Pages + jekyll 的方式。

我们注册一个**GitHub** 账号

>如果你不知道GitHub，那你就应该先google下GitHub。然后在自己注册下登陆下在Starr Fock 下项目 体验下

**拉取博客模版**

> 在GitHub 上搜索下我的博客系统  **wsccoder.github.io** 进入到[我的仓库](https://github.com/wsccoder/wsccoder.github.io)



![](https://ws1.sinaimg.cn/large/005S1Oyygy1fuvjs34zybj323g0sw18m.jpg)



**点击这里**

![](https://ws1.sinaimg.cn/large/005S1Oyygy1fuvjuvm8nsj31ha13edo3.jpg)



点击右上角的 **Fock** 将这个仓库 **Fock**到你自己的账号下面。等一会之后在刷新你的GitHub界面，你就会发现这个仓库已经在你的GitHub账号下面了

>**Star** 和 **Fock**的区别在于star的项目对其进行修改之后不能将其推到源仓库中，而**Fock**可以进行推送。推送之后源仓库的作者可以进行审核，然后将你的代码整合到源仓库中

**修改仓库名**

![](https://ws1.sinaimg.cn/large/005S1Oyygy1fuvk9293quj31e2146qbk.jpg)



**点击setting 进入修改**

![](https://ws1.sinaimg.cn/large/005S1Oyygy1fuvkac5h5vj31dw138gtl.jpg)



将这个仓库的名称修改为 `你的Github账号名.github.io`，然后 Rename保存

然后在浏览器界面输入 `你的Github账号名.github.io` 就会显示下面的界面

![](https://ws1.sinaimg.cn/large/005S1Oyygy1fuvkdv4vi1j31rc17m1ky.jpg)



>下面你已经离成功不远了，现在你的博客已经完成初步了。后续的配置可以使你的博客更加的丰富
>
>

**我们看下整个网站的结构**

```html
├── _config.yml
├── _drafts
|   ├── begin-with-the-crazy-ideas.textile
|   └── on-simplicity-in-technology.markdown
├── _includes
|   ├── footer.html
|   └── header.html
├── _layouts
|   ├── default.html
|   └── post.html
├── _posts
|   ├── 2007-10-29-why-every-programmer-should-play-nethack.textile
|   └── 2009-04-26-barcamp-boston-4-roundup.textile
├── _data
|   └── members.yml
├── _site
├── img
└── index.html
```



这里面的内容较多，要是想深入的了解的话google下文档。我们在基本配置的时候只需要记住下面几个就行了

- `_config.yml` 全局配置文件    //我们配置的主要地方
- _posts 放置博客文章的文件夹
- img 存放图片的文件夹

**修改配置**

在你们的项目根目录上面找到`_config.yml` 这个文件然后对其进行修改基本配置，我们博客的所有全局配置都在这个配置文件中修改

![](https://ws1.sinaimg.cn/large/005S1Oyygy1fuvku0d5klj31em0yuahi.jpg)



**然后点击修改**

![](https://ws1.sinaimg.cn/large/005S1Oyygy1fuvkvpux5zj31im10kdpy.jpg)



**那么现在我们就进入配置里面对基本的配置进行修改**

**基础配置分为几部分**

- 基础设置
- 侧边栏
- 社交账号

下面我们将对其分别进行配置和讲解

**基础设置**

```yaml
# Site settings
title: You Blog    				  	#博客的标题
SEOTitle: 你的博客 | You Blog    	 #显示在浏览器上搜索的时候显示的标题
header-img: img/post-bg-rwd.jpg  	#显示在首页的背景图片
email: You@gmail.com	
description: "You Blog"  			 #网站介绍
keyword: "wsc, wsccoder Blog, 王守昌的博客, Java, Go, iPhone, mac pro, book" 
url: "https://qiubaiying.github.io"          # 这个就是填写你的博客地址
baseurl: ""      # 目前不用填写
```

\**侧边栏**

```yaml
# Sidebar settings
sidebar: true                           # 是否开启侧边栏.
sidebar-about-description: "说点装逼的话。。。"
sidebar-avatar:/img/avatar-by.JPG      # 你的个人头像 这里你可以改成我在img文件夹中的两张备用照片 img/avatar-m 或 avatar-g
```



**社交账号**

>可以点击这个社交账号的图标跳转到你的社交网站的个人中心
>
>

![](https://ws1.sinaimg.cn/large/005S1Oyygy1fuvl1vy83yj30i405c0tg.jpg)



下面是填写社交账号的用户名，没有可以不用填写

```
# SNS settings
RSS: false
weibo_username:     username
zhihu_username:     username
github_username:    username
facebook_username:  username
jianshu_username:	jianshu_id
```





**评论系统**

我之前使用的是根据前博主的 [Disqus](https://disqus.com/) 评论系统，但是博主说了这个评论系统会被墙所以我们只着重的介绍另外的以恶搞评论系统 **Gitalk** 评论系统

具体配置评论系统可以参考另外一个博主的博客 [为博客添加 Gitalk 评论插件](http://qiubaiying.top/2017/12/19/%E4%B8%BA%E5%8D%9A%E5%AE%A2%E6%B7%BB%E5%8A%A0-Gitalk-%E8%AF%84%E8%AE%BA%E6%8F%92%E4%BB%B6/)

**网站统计**

因为我们这个博客系统是没有自己的访问统计的，所以我们需要自己进行配置访问统计然后借用第三方平台进行统计

我们可以使用 [Baidu Analytics](http://tongji.baidu.com/web/welcome/login) 和 [Google Analytics](http://www.google.cn/analytics/) 拿到在这两个网站注册的时候获取的track_id 进入到我们的配置文件中进行替换

![](https://ws1.sinaimg.cn/large/005S1Oyygy1fuvle7vbndj31l60d60vr.jpg)

因为我没有使用百度的，所以将百度的给注释掉了。切记要更换 track_id 否则的话你的网站的反问记录会记录在我的后台中（尴尬😅）

要是目前不想统计可以直接都给注释掉

```yaml
# Analytics settings
# Baidu Analytics
ba_track_id: 83e259f69b37d02a4633a2b7d960139c

# Google Analytics
ga_track_id: 'UA-90855596-1'            # Format: UA-xxxxxx-xx
ga_domain: auto
```



我们现在可以看下Google Analytics后台的界面是什么样的

![](https://ws1.sinaimg.cn/large/005S1Oyygy1fuvljs0khgj324g11ajy4.jpg)



**好友**

```yaml
# Friends
friends: [
    {
        title: "Tuya",
        href: "http://tuya.com"
    },{
        title: "cctv",
        href: "http://www.cctv.com"
    },{
        title: "Apple",
        href: "https://apple.com"
    },{
        title: "Apple Developer",
        href: "https://developer.apple.com/"
    }
]
```



**保存**

>
>
>各位大哥切记要保存啊，提交啊。要不然咱们配置的都没有用了。。

![](https://ws1.sinaimg.cn/large/005S1Oyygy1fuvllgw1exj31nk0ky41h.jpg)



**过几十秒 然后刷新下你的主页**



铛铛铛。恭喜你 你的个人博客搭建完成啦。



## 写文章

