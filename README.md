## 搭建过程

在搭建博客时候也踩了不少坑，之前写博客在博客园上面写的，无奈博客园的界面太……，自己原本写了过一个博客系统但是也部署在了阿里云的服务器上面，但是后期没有续费就GG了。在后面看各路大佬在Github上搭建博客，自己也学着搭建了一个，在这过程中也是踩了不少的坑。其中还是要谢谢几位博主的，我也是根据几位博主的博客自己一点点的搭建起来的。

**废话少说我们下面就开始搭建博客**

> 我们的博客系统是基于Jekyll开源的，部署GitHub上面。这样我们可以省去搭建服务器的过程。
>
> 转载修改自  [BYBlog](http://qiubaiying.top/) 感谢这位同学。因为他的博客少走了很多弯路 
> 下面是博客的搭建教程，这个教程修改自 Hux 。


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



点击右上角的 **Fork** 将这个仓库 **Fork**到你自己的账号下面。等一会之后在刷新你的GitHub界面，你就会发现这个仓库已经在你的GitHub账号下面了

>**Star** 和 **Fork**的区别在于star的项目对其进行修改之后不能将其推到源仓库中，而**Fork**可以进行推送。推送之后源仓库的作者可以进行审核，然后将你的代码整合到源仓库中

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

在这里写文章的话就不像一般的博客网站你在后台编写或者在自己的文本编辑器上面写让后复制到后台编辑器里面。我们这里需要自己创建文件然后将你的日志文件push到你GitHub上面。

**创建文章**

我们创建的文章都保存在这个文件夹得下面。

![](https://ws1.sinaimg.cn/large/005S1Oyygy1fuyx27qromj31eu12eds0.jpg)





*每次我们要创建发表新文章的时候止需要在这个文件夹下面创建一个新的md的文件就ok了*

![](https://ws1.sinaimg.cn/large/005S1Oyygy1fuyx5benixj31d614ek4x.jpg)



我们在GItHub的界面上创建一个文章

![](https://ws1.sinaimg.cn/large/005S1Oyygy1fuyx61iq51j31f20yagz4.jpg)





然后在编辑这个文章

![](https://ws1.sinaimg.cn/large/005S1Oyygy1fuyx85e6uxj31do0y8ao6.jpg)



之后点击保存，估计过个十几秒，你再刷新你的博客首页就能看见你发布的新文章了。

**格式**

每一篇文章文件命名采用的是`2017-02-04-Hello-2017.md`时间+标题的形式，空格用`-`替换连接。

文件的格式是 `.md` 的 [**MarkDown**](http://sspai.com/25137/) 文件。

我们的博客文章格式采用是 **MarkDown**+ **YAML** 的方式。

[**YAML**](http://www.ruanyifeng.com/blog/2016/07/yaml.html?f=tt) 就是我们配置 `_config`文件用的语言。

[**MarkDown**](http://sspai.com/25137/) 是一种轻量级的「标记语言」，很简单。[花半个小时看一下](http://sspai.com/25137)就能熟练使用了

大概就是这么一个结构。

```
---
layout:     post   				    # 使用的布局（不需要改）
title:      My First Post 				# 标题 
subtitle:   Hello World, Hello Blog #副标题
date:       2017-02-06 				# 时间
author:     BY 						# 作者
header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - 生活
---

## Hey
>  这是一遍博客

 哈哈哈哈哈  博客完成了
```

按格式创建文章后，提交保存。进入你的博客主页，新的文章将会出现在你的主页上.

![](https://ws1.sinaimg.cn/large/005S1Oyygy1fuyxgfys7fj31fo16onpd.jpg)

到这里，恭喜你！）。

到这里的话，一个博客的全部是大功告成了。

#### 首页标签

在首页可以看到这些特色标签，当你的文章出现相同标签（默认相同的**标签数量大于1**），才会自动生成。

所以当你只放一篇文章的时候是不会出现标签的。

![](https://ws1.sinaimg.cn/large/005S1Oyygy1fuyxiet5hdj31681524qp.jpg)

建站的初期，博客比较少，若你想直接在首页生成比较多的标签。你可以在 `_congfig.yml`中找到这段：

```
# Featured Tags
featured-tags: true                     # 是否使用首页标签
featured-condition-size: 1              # 相同标签数量大于这个数，才会出现在首页
```

将其修改为`featured-condition-size: 0`, 这样只有一个标签时也会出现在首页了。

相反，当你博客比较多，标签也很多时，这时你就需要改回 `1` 甚至是 `2` 了。



——————————— 后面部分是完全将前博主的文章转载过来了------------------------------------

# 自定义域名

搭建好博客之后 你可能不想直接使用 [baiyingqiu.github.io](http://baiyingqiu.github.io/) 这么长的博客域名吧, 想换成想 [qiubaiying.top](http://qiubaiying.top/) 这样简短的域名。那我们开始吧！

#### 购买域名

首先，你必须购买一个自己的域名。

我是在[阿里云](https://wanwang.aliyun.com/domain/?spm=5176.8006371.1007.dnetcndomain.q1ys4x)购买的域名

![img](http://upload-images.jianshu.io/upload_images/2178672-ef3844cab15e35ff.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

用**阿里云** app也可以注册域名，域名的价格根据后缀的不同和域名的长度而分，比如我这个 `qiubaiying.top` 的域名第一年才只要4元~

域名尽量选择短一点比较好记住，注意，不能选择中文域名，比如 `张三.top` ,GitHub Pages **无法处理中文域名**，会导致你的域名在你的主页上使用。

注册的步骤就不在介绍了

#### 解析域名

注册好域名后，需要将域名解析到你的博客上

管理控制台 → 域名与网站（万网） → 域名

![img](http://upload-images.jianshu.io/upload_images/2178672-9a75bba50d1b14d7.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

选择你注册好的域名，点击解析

![img](http://upload-images.jianshu.io/upload_images/2178672-0968a8dd2045f4fd.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

添加解析

分别添加两个`A` 记录类型,

一个主机记录为 `www`,代表可以解析 `www.qiubaiying.top`的域名

另一个为 `@`, 代表 `qiubaiying.top`

记录值就是我们博客的IP地址，是 GitHub Pagas 在美国的服务器的地址 `151.101.100.133`

![img](http://upload-images.jianshu.io/upload_images/2178672-0769a93bc487e9d8.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可以通过 [这个网站](http://ip.chinaz.com/) 或者直接在终端输入`ping 你的地址`，查看博客的IP

```
ping qiubaiying.github.io
```

细心地你会发现所有人的博客都解析到 `151.101.100.133` 这个IP。

然后 GitHub Pages 再通过 CNAME记录 跳转到你的主页上。

#### 修改CNAME

最后一步，只需要修改 我们github仓库下的 **CNAME** 文件。

选择 **CNAME** 文件

![img](http://upload-images.jianshu.io/upload_images/2178672-a422f3dab436dfb7.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

使用的注册的域名进行替换,然后提交保存

![img](http://upload-images.jianshu.io/upload_images/2178672-6e613004fb410b44.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这时，输入你自己的域名，就可以解析到你的主页了。

大功告成！

# 进阶

若你对博客模板进行修改，你就要看看 Jekyll 的[开发文档](http://jekyll.com.cn/),是中文文档哦，对英语一般的朋友简直是福利啊（比如说我😀）。

还要学习 **Git** 和 **GitHub** 的工作机制了及使用。

你可以先看看这个[git教程](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/)，对git有个初步的了解后，那么相信你就能将自己图片传到GitHub仓库上，或者可以说掌握了 **使用git管理自己的GitHub仓库** 的技能呢。

对于轻车熟路的程序猿来说，这篇教程就算就结束了，因为下面的内容对于你们来说 so eazy~

但相信很多小白都一脸懵逼，那我们继续👇。

# 利用GithHub Desktop管理GitHub仓库

[GithHub Desktop](https://desktop.github.com/) 是 **GithHub** 推出的一款管理GitHub仓库的桌面软件，换句话说就是将你在**Github**上的文件同步到本地电脑上，并将修改后的文件同步到**Github**远程仓库。

#### 下载

点击图片进入下载页面，选择对应的平台进行下载

[![img](http://upload-images.jianshu.io/upload_images/2178672-6022ba3938b3088e.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](https://desktop.github.com/)

下面以**Mac**平台为例：

#### 安装

将下载好的文件解压，将这只小猫拖到应用程序文件夹中

![img](http://upload-images.jianshu.io/upload_images/2178672-8f8c27f4e5c72276.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

就可以在**Launchpad**找到这只小猫咪~

![img](http://upload-images.jianshu.io/upload_images/2178672-0f2da4717361459c.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 登录

点开应用,会弹出**登录**框，

![img](http://upload-images.jianshu.io/upload_images/2178672-adb7d6824e471ef5.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

输入你的**GitHub**账号和密码进行登录

![img](http://upload-images.jianshu.io/upload_images/2178672-2d7c407ebddbb44f.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

登录后关闭窗口

![img](http://upload-images.jianshu.io/upload_images/2178672-93cdccc42024914b.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然后返回引导窗，一直按 **Continue** 继续

![img](http://upload-images.jianshu.io/upload_images/2178672-450ccef6b1ab7b0a.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**Continue**

![img](http://upload-images.jianshu.io/upload_images/2178672-06b6e6792472ecae.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

还是**Continue**~![img](http://upload-images.jianshu.io/upload_images/2178672-681a6c455f6b512f.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

进入主界面，先 **右键Remve** 删除这个用户指导，贼烦~

![img](http://upload-images.jianshu.io/upload_images/2178672-604f6f23b8fab6f3.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 克隆仓库

选择你的仓库克隆到本地

![img](http://upload-images.jianshu.io/upload_images/2178672-45ddcd27e2f858a1.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![img](http://upload-images.jianshu.io/upload_images/2178672-625be1220fea36b6.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 管理仓库

现在文件夹中打开

![img](http://upload-images.jianshu.io/upload_images/2178672-92c1616af56b501a.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

打开后你会的发现文件结构和你在Github上的一模一样~

![img](http://upload-images.jianshu.io/upload_images/2178672-bf3580ae1cd9a29e.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

你最先关心的可能是你的头像~在**img**文件夹中把替换我的头像就好了。

![img](http://upload-images.jianshu.io/upload_images/2178672-c9421d64538c3ba6.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

不仅是图片，所有在Github上的的操作都可以进行。

#### 保存修改

当你对仓库文件夹的文件下进行修改、添加或删除时，都可以在 **GitHub Desktop** 中看到

例如我在 `img` 中添加了一张图片 `avatar-demo.png` 添加了一张图片

就可以在看到**GitHub Desktop**显示了我的修改

保存修改只要按 **Commit to master**，然后可以写上你的修改说明

![img](http://upload-images.jianshu.io/upload_images/2178672-4bfbfec37cbb8eb6.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 同步

将修改同步到 **GitHub** 远程仓库上只需要一步：点击右上角的**同步按钮**

![img](http://upload-images.jianshu.io/upload_images/2178672-3c2ee8234a7f1832.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 完成

打开你的GitHub上的仓库，你就可以看到已经和本地同步了

可以看到你提交的详情： `add img`

![img](http://upload-images.jianshu.io/upload_images/2178672-293bdd4cbee0e9e3.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这样，你已经能轻松管理自己的博客了。

想上传头像，背景，或者是删掉你不要的图片（我的头像😏）已经是 so eazy了吧~

#### 注意

你在 **GitHub** 网站上进行 **Commit** 操作后，需要在**GitHub Desktop**上按一下 **同步按键** 才能同步网站上的修改到你的本地。

# 修改个人介绍

![img](https://ws4.sinaimg.cn/large/006tNc79gy1fme0poz7gqj30vq0l8whh.jpg)

修改个人介绍需要修改根目录下的 `about.html` 文件

![img](https://ws2.sinaimg.cn/large/006tNc79gy1fme0rna33tj30bw0bntah.jpg)

看不懂 HTML 标签？没关系，对照着修改就好了~ 还有注意这个有中英介绍

![img](https://ws1.sinaimg.cn/large/006tNc79gy1fme0sbvmmcj30zp0os7ap.jpg)

# 常见问题

最近有很多人给我提问题，我这边总结一下

#### 配置文件修改后没有效果

刷新几遍浏览器就好了~

不行的话，先清除浏览器缓存再试试。

#### 404错误

1. 检查你的仓库名是否有按照要求填写
2. 确定 **Fork** 的是不是我的仓库~

#### 修改CNAME文件，域名还是不变

清除浏览器缓存就OK~

#### 其他问题

直接在评论中提出来或私信我，我会一一替大家解决的😀

# 其他

最近有人往我的远程仓库不停的 **push**，一天连收几十封邮件！例如像这样的

![img](http://upload-images.jianshu.io/upload_images/2178672-1347f2cc9a4a8dc8.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

原因大多是直接Clone了我的仓库到本地，**没有删除我的远程仓库地址**，添加完自己的仓库地址后，一口气推送到所有远程仓库（包括我的😂）~

打扰了我的工作和生活~

所以，**请不要往我的仓库上推送分支**！

我发现一个问题是，很多人每次修改博客的内容都commit一次到远程仓库，然后再查看修改结果，这样效率非常低！

#### 来，上车！

## 在本地调试博客

> 注：下面的操作是在 **Mac** 终端进行的。 **Windows** 环境下的配置请参考 [@梦幻之云](http://www.jianshu.com/u/a13e7484dc21) 提供的 [这篇文章](https://agcaiyun.cn/2017/09/10/%E6%90%AD%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/)。

有心的同学在 [jekyll官网](http://jekyllcn.com/) 就会发现 `jekyll` 的 提供的实例代码。

```
~ $ gem install jekyll bundler
~ $ jekyll new my-awesome-site
~ $ cd my-awesome-site
~/my-awesome-site $ bundle install
~/my-awesome-site $ bundle exec jekyll serve
# => 打开浏览器 http://localhost:4000
```

这段命令创建了一个默认的 `jekll` 网站，然后在本机的 4000 窗口展示。聪明的你应该发现怎么做了吧~

安装 `jekyll`和 `jekyll bundler`

```
$ gem install jekyll
$ gem install jekyll bundler
```

进入你的 **Blog 所在目录**，然后创建本地服务器

```
$ jekyll s
```

然后会显示

```
 Auto-regeneration: enabled for '/Users/baiying/Blog'
Configuration file: /Users/baiying/Blog/_config.yml
    Server address: http://127.0.0.1:4000/
  Server running... press ctrl-c to stop.
```

你就可以在 <http://127.0.0.1:4000/> 看到你的博客，你对本地博客的修改都会在这个地址进行显示，这大大提高了对博客的配置效率。

使用`ctrl+c`就可以停止 **serve**

# Star

若本教程顺利帮你搭建了自己的个人博客，请不要 **害羞**，给我的 [github仓库](https://github.com/qiubaiying/qiubaiying.github.io) 点个 **star** 吧！

因为最近发现 Fork 将近破百，加上直接 Clone 仓库的，保守估计已经帮助上百人成功的搭建了自己的博客，~~可是 Star 却仅仅只有 **12**！可能还是做的不够好吧！~~现在已经破百了，感谢大家的Star！

### **别无他求，点个 Star 吧**！

![img](http://upload-images.jianshu.io/upload_images/2178672-768a38ee9fb0df28.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**心满意足！**

# 补充

#### 修改网站的 **icon**

![img](https://ws2.sinaimg.cn/large/006tNc79gy1flgh6k23ppj30ad00uq2t.jpg)

要修改如图所示的网站 **icon**：

在博客 `img` 目录下找到并替换 `favicon.ico` 这个图标即可，图标尺寸为`32x32`。

![img](https://ws2.sinaimg.cn/large/006tNc79gy1flghahch1oj30gu09y419.jpg)

#### 修改主页的座右铭

最近有不少小伙伴私信我：**如何修改主页的座右铭？**

就是这个：

![img](http://upload-images.jianshu.io/upload_images/2178672-31dc0068f256aca3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

很简单，找到博客目录下的 **index.html** 文件，修改这句话就可以了。

![img](http://upload-images.jianshu.io/upload_images/2178672-9e4785654523bf07.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 如何在博客文章中上插入图片

博客的文章用的是 MarkDown 格式，如果没用过 MarkDown 真的 强烈推荐 [花半个小时学习一下](http://sspai.com/25137)。

MarkDown 中添加图片的形式是 :`![](图片的URL)`

例如：

`![MarkDown示例图片](http://upload-images.jianshu.io/upload_images/2178672-eb2effd6b942a500.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)`就会显示下面这张图片

![MarkDown示例图片](http://upload-images.jianshu.io/upload_images/2178672-98965f66db8f5856.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

`https://ws3.sinaimg.cn/large/006tNc79gy1fj9xhjzobbj30yg0my75z.jpg`就是这张图片的URL，我们可以在浏览器输入这个URL找到或下载这张图片。

所以，要在 MacDown 中插入图片，这张图片就需要上传到图床（网上），然后在引 用这张图片的URL。

##### 将图片上传到图床

Mac 上的图床神器：iPic

直接在App Store上下载，谁用谁知道！

使用方法很简单，直接拖动图片到 P 图标上，或者选中图片按快捷键 `⌘+U`，就能请示上传。

上传成功就能直接粘贴图片的URL。

![iPic](http://upload-images.jianshu.io/upload_images/2178672-7399aeaced6f1e29.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

用 iPic 上传图片后，获取URL插入文章中就可以了。

![iPic上传图片](http://upload-images.jianshu.io/upload_images/2178672-4be76fb02708de5e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 推荐几个好用软件

##### MarkDown编辑器

[MacDown](https://macdown.uranusjr.com/)：可能是Mac上最好的MacDown编辑器了

![img](http://upload-images.jianshu.io/upload_images/2178672-2226239a63278302.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##### 图片压缩工具

[ImageOptim](https://imageoptim.com/)

对于我们的博客来说，图片越大，加载速度越慢。

不信你用手机打开你的博客试试~

所以有必要对我们上传到博客网站中的图片：指的是你的头像，首页背景图片，文章背景图片等。对于博客文章中插入的图片，其实也可以压缩了再上传。

对博客中的所有图片进行压缩：

看看压缩结果，最高的一张压缩了78.7%，这简直是太可怕了！

![ImageOptim压缩图片](http://upload-images.jianshu.io/upload_images/2178672-0f8e643fa1da8674.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

好了，现在个人博客的加载速度估计要起飞了~



