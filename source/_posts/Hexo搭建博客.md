---
title: Hexo搭建博客
date: 2017-04-19 16:21:11
tags:
- 工具
- Hexo
categories: [Hexo]
---

第一次通过Hexo搭建博客
<!-- more -->

# 环境搭建

------

## 学习地址

http://wensibo.top

## Git安装
	
> * [下载](https://git-scm.com/download/win)安装GIT
> * 命令创建SSH Key
```
$ ssh-keygen -t rsa -C "youremail@example.com"
```
> * 如果一切顺利的话，可以在用户主目录里找到.ssh目录，里面有**id_rsa**和**id_rsa.pub8**两个文件，这两个就是SSH Key的秘钥对，**id_rsa**是私钥，不能泄露出去，**id_rsa.pub**是公钥，可以放心地告诉任何人.
> * 登陆GitHub，打开**Account settings**，**SSH Keys**页面，然后，点**Add SSH Key**，填上任意Title，在Key文本框里粘贴**id_rsa.pub**文件的内容，点**Add Key**，你就应该看到已经添加的Key


## Node下载

安装[node](https://nodejs.org/en/)


## 搭建Hexo

### Hexo安装

所有的环境搭建好之后就可以打开git bash，并使用下面的命令进行安装。
```
$ npm install -g hexo-cli
```

### 搭建网站

安装 Hexo 完成后，执行下列命令，Hexo 将会在指定文件夹中新建所需要的文件。

```
$ hexo init <folder>
$ cd <folder>
$ npm install
```

这个过程需要一定的时间，请耐心等待。
建成之后的目录结构大致如下，可能会有所差异，并无大碍。
```
├── _config.yml
├── package.json
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
└── themes
```

* **_config.yml** 是网站的配置信息，大部分时候你会使用到他。
* **source** 目录下面存放的是你编写的博客文章，在 _posts 目录下为你已经发表的文章，在 _drafts 目录下为你存放的草稿文章，当然你可能没有这个文件夹，因为一开始你还没有新建草稿文章。
* **themes** 目录为你的博客网站使用的主题，程序默认的主题landscape，他长这样：

### 启动网站


首先进入你的博客网站目录，例如我的目录为/hexo/FristBlog。
```
cd /hexo/FristBlog
```
**启动网站**
```
hexo server
或者 开启并启动网站
hexo server -reload 
```

### 更换主题 indigo

安装主题，首先要切换到博客的根目录下

```
git clone git@github.com:yscoder/hexo-theme-indigo.git themes/indigo
```
在根目录下的themes中会生成一个indigo的文件夹
并在_config.yml中 修改
```
themes:indigo
```


### 依赖

> less 主题默认使用less作为css预处理工具
```	
$ npm install hexo-renderer-less --save
```

> feed 生成RSS
```
$ npm install hexo-generator-feed --save
```

> 开启站内搜索Json-content
```
$ npm install hexo-generator-json-content --save
```

> QRCode 用于分享二维码
```
$ npm install hexo-helper-qrcode --save
```

### 开启菜单栏

> 开启标签页

```	
hexo new page tags
```

修改blog/source/tags/index.md

```
layout: tags
comments: false
```

> 开启分类页

```
hexo new page categories
```

修改blog/source/categories/index.md

```
layout: categories
comments: false
```

### 将网站部署到GitHub上

#### 新建一个仓库

名叫yourname.github.io，`注意:yourname 必须是你的GitHub账户名`

#### 修改_config.yml

博客根目录下的_config.yml

```
deploy:
  type: git
  repo: https://github.com/yourname/yourname.github.io.git
  branch: master
```

#### 安装Hexo支持的Git插件
```
$ npm install hexo-deployer-git --save
```

#### 生成静态文件，每次修改都会用到

```
hexo generate
```

#### 部署网站

通过Git插件将刚才生成的静态文件上传至刚才新建的仓库中。

```
hexo deploy
```

### 新建文章

#### 新建文章

```
hexo new <title>
例如
hexo new  第一篇文章
```
当然你也可以直接在**blog\source\_posts**目录下直接新建一个**yourtitle.md**文件，这样就算是新建了一篇文章，但是这样创建的文章需要在最上方添加如下说明：
```
title: hello world
date: 2017-01-12 20:37:15
tags:  标签
- hello world
- hello
categories: [first]  种类
```

> ** 注意 **
> * 请注意hexo的格式比较严格，必须要在属性后面的冒号之后紧跟一个空格，后面跟内容。
> * 如果是标签的话，在 - 后紧跟一个空格，再填写标签内容。
> * 如果有多个标签，可以换行以此按照原来的格式继续填写。
> * 分类的话，我尝试了多次，只有再添加一个分类的时候才能正常显示，如果你想在一片文章中添加多个分类的话，好像是会有如下图所示的问题的，当然我觉得每篇文章属于一个分类就够了吧！

#### 删除或修改 文章

与上面类似

#### 生成和发布文章

```
hexo generate
hexo deploy
```

## 评论功能

常见的有：
> * 多说：<http://duoshuo.com/>，要关闭了，慎入！
> * Disqus：<https://disqus.com/>，国外网站优选
> * 畅言：<http://changyan.kuaizhan.com/> ，搜狐（需要备案，要有自己的主机）
> * 友言：<http://www.uyan.cc/> 小，不稳定
> * 网易云跟帖：<https://gentie.163.com/index.html>， 网易，稳定，集成方便

### 网易云跟帖集成步骤（适用于Indigo主题）

每个页面的布局其实在_partial/post.ejs 中，comment 评论就是其中一部分。

* 在主题根目录下的_onfig.yml 中 添加
```
wangyi: true
```
* 在_partial/plugins添加wangyi.ejs文件，复制网易云跟帖web代码
```
<% if (theme.wangyi){ %>
<div id="cloud-tie-wrapper" class="cloud-tie-wrapper"></div>
<script src="https://img1.cache.netease.com/f2e/tie/yun/sdk/loader.js"></script>
<script>
	var cloudTieConfig = {
	  url: document.location.href, 
	  sourceId: "",
	  productKey: "715e7508ff6c4889ae209a2acbf24b9d",
	  target: "cloud-tie-wrapper"
	};
	var yunManualLoad = true;
	Tie.loader("aHR0cHM6Ly9hcGkuZ2VudGllLjE2My5jb20vcGMvbGl2ZXNjcmlwdC5odG1s", true);
</script>
<% } %>
```
* 在_partial/post/comment.ejs 中添加
```
<%- partial('../plugins/wangyi') %>
```
	
## 流量统计

常见的有：
> * Google Analysis：<https://www.google.com/intl/zh-CN/analytics/> ，功能强大不用多说，由于国内Google的服务用不了，所以推荐海外站点使用。
> * CNZZ：<http://web.umeng.com> ，中文网站统计分析平台，口碑不错，目前和友盟合并被阿里收购。
> * 百度统计：<http://tongji.baidu.com> 
	
## 高级功能

### 申请域名

<https://wanwang.aliyun.com/>


### 在虚拟主机中搭建你的网站



#### Nginx反向代理

* [下载地址](http://nginx.org/en/download.html)
* 启动Nginx。进入c:\nginx，双击nginx.exe，或者打开cmd，进入Nginx目录，执行命令start nginx。默认的端口号为80。
> 直接访问浏览器localhost,正常情况下，就能看到Nginx的欢迎界面了。如果不对，90%的可能是因为80端口占用问题，打开配置Nginx配置文件，修改一下默认端口就行了。
* 修改端口号。修改c:\nginx\conf\nginx.conf，将其中的server节点修改如下，记得要把最后一行错误页也修改了，这样当你访问你的网站的时候，出现路径不存在的情况时，会自动跳转到该页面
```
server {
    listen       80;
    server_name  localhost;
    #charset koi8-r;
    #access_log  logs/host.access.log  main;
    location / {
        root   html/public;
        index  index.html;
    }
    error_page  404              /404.html;
```
* 通过Hexo g命令生成的静态站点，默认就是Hexo站点目录中的public文件夹。将生成好的静态站点（也就是public/目录），拷贝至Nginx目录下的html文件夹中。然后修改Nginx配置文件。记得将public文件夹全部拷贝哦！

* 重启nginx。在任务管理器关闭nginx.exe再重新开启或者在命令行中使用如下命令重启nginx。
```
nginx -s reload
```
* 重新访问localhost，就可以看到Hexo静态站点了。这里要注意浏览器缓存的问题

### 网站推广

网站推广其实质就是**SEO(搜索引擎优化)**，提高你网站在网络的可见度，那么可以通过很多方式去增加网站的访问量
* 向搜索引擎提交你的网站地址。这是至关重要的，也是十分有效的
	
	
------

# 其它

## 网站图标

工具[faviconer](http://www.faviconer.com)

## 获取图片URL

[图床工具](http://tuchuang.org/)用来上传图片获取URL
	
## Markdown 语法
[Markdown语法](https://www.zybuluo.com/mdeditor)