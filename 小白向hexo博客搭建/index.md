# 小白向Hexo博客搭建


## 前言

1. 操作环境为Windows 10。
2. 本文只是进行了一些基础配置，一些我没用上的功能就没有写。
3. 主要作为自己搭建Hexo博客入门时的总结。

## 配置环境

### 安装Node.js

[Node.js](http://nodejs.cn/) 安装完成后，打开**cmd**，输入`node -v`和`npm -v`，若出现版本号，即安装成功。

### 安装Git

[Git](https://git-scm.com/downloads) 安装路径可以更改，其他安装选项一路Next即可。

- 进行到第2步的时候，确保选中Git Bash here（默认选中），这样就可以在资源管理器的右键菜单中随时使用Git Bash here打开Git Bash。

<center><img src= "https://cdn.jsdelivr.net/gh/BahuangShanren/picture@master/article_2020-05-21/2.png" /></center>

- 进行到第6步的时候，确保选中第二项（默认选中），这样就可以在Git Bash、cmd、Power Shell和其他第三方软件中（比如VS Code中的Terminal里）使用Git。

<center><img src= "https://cdn.jsdelivr.net/gh/BahuangShanren/picture@master/article_2020-05-21/3.png" /></center>

安装完成后在**cmd**中输入`git --version`，若出现版本号，即安装成功。

### 更改npm仓库地址（建议）

在**Git Bash**中进行以下操作，**以后大部分的操作都在Git Bash内进行，而不用cmd了**（当然也可以用，上图不是选中了Git from the command line and also from 3rd-party software嘛）

- 随便什么地方空白处右键点击`Git Bash Here`即打开Git Bash。
- 或者直接找到Git Bash.exe打开。

#### 法一：改变默认地址

输入以下命令查看当前npm的仓库地址：

```bash
npm config get registry
```

控制台会显示npm的默认仓库地址：

```bash
https://registry.npmjs.org/
```

使用以下命令来改变默认仓库地址为淘宝镜像地址：

```bash
npm config set registry https://registry.npm.taobao.org
```

然后使用命令查看当前npm的仓库地址验证是否成功更改。

使用这种方法，以后使用`npm`命令时，仍然输入`npm`。

#### 法二：安装cnpm

使用以下命令安装cnpm：

```bash
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

安装完成后输入以下命令，若出现版本号，即安装成功。

```bash
cnpm -v
```

使用这种方法，以后要输入`cnpm`代替`npm`来执行命令。

## 安装Hexo

在合适的地方新建一个文件夹，名称任意。我的这个文件夹是`E:/Blog`，对于这个文件夹我的理解是：

> 它其实就是常用的、所谓的“Hexo目录”，Hexo就安装在这里，博客的所有数据都存在这里。
>
> GitHub仓库里的博客文件其实是Hexo目录下的public文件夹里的内容：
>
> 每次用Git Bash输入`hexo g`命令时，静态博客都会被生成到`E:/Blog/public`中，输入`hexo d`命令后，再被推送到仓库的`master`分支（默认为`master`）

> 不要在安装Hexo之前，先行把创建的仓库克隆到本地，然后又在该仓库文件夹下安装Hexo，这是行不通的，Hexo只能在空文件夹下安装并初始化（初始化其实就是把Hexo的项目仓库克隆到本地）。

> 我把创建仓库的内容往后排了，为了避免有人像我刚接触这些概念时，因为没有理清楚GitHub仓库与Hexo文件夹的关系而犯同样的错。

- 在Hexo目录下空白处右键点击`Git Bash Here`打开Git Bash。
- 或者打开Git Bash.exe，在Git Bash输入cd命令定位到Hexo目录下（大小写无所谓），比如：

```bash
cd e:/blog
```

输入以下命令安装Hexo。会有几个报错，无视就行。

```bash
npm install hexo-cli -g
```

安装完后输入`hexo -v`，若出现版本号，即安装成功。

输入`hexo init`初始化文件夹（其实就是克隆Hexo的GitHub项目仓库到此文件夹），所以速度有点慢。

输入`npm install`安装必备的组件。

输入

```bash
npm i hexo-deployer-git
```

安装推送博客到仓库要用到插件（不安装它就不能使用`hexo d`命令）

输入`hexo g`生成静态网页，然后输入`hexo s`打开本地服务器，用浏览器打开http://localhost:4000/ ，可以看到Hexo默认的博客网页：

![](https://cdn.jsdelivr.net/gh/BahuangShanren/picture@master/article_2020-05-21/4.png)

按`ctrl+c`关闭本地服务器。

## 连接Git与本地

### 全局设置Git用户名和邮箱

在Git Bash中输入以下命令（把`your_name`改为自己的GitHub用户名）：

```bash
git config --global user.name "your_name"
```

输入以下命令（把`your_email@example.com`改为自己的GitHub的注册邮箱）

```bash
git config --global user.email "your_email@example.com"
```


输入以下命令看是否显示自己的用户名和邮箱：

```bash
git config --global --list
```

### 使用SSH连接到GitHub

输入以下命令（把`your_email@example.com`改为自己的GitHub的注册邮箱）

```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

按照提示完成三次回车（使用默认设置），即可生成 ssh key。

在Github个人设置中点击`SSH and GPG keys`，点击`New SSH Key`新建SSH密钥：

![](https://cdn.jsdelivr.net/gh/BahuangShanren/picture@master/article_2020-05-21/5.png)

![](https://cdn.jsdelivr.net/gh/BahuangShanren/picture@master/article_2020-05-21/6.png)

- 在Git Bash输入`cat ~/.ssh/id_rsa.pub` 查看刚刚生成的个人公钥。复制输出的内容，粘贴到上图`key`框中,`Title`任意。

- 或者用记事本打开`C:/Users/用户名/.ssh`中的`id_rsa.pub`文件。复制全部内容，粘贴到上图`key`框中,`Title`任意。

点击Add SSH Key。

在Git Bash中输入以下命令，

```bash
ssh -T git@github.com
```

首次使用需要确认并添加主机到本机SSH可信列表——当出现下面这句提示时，输入`yes`回车即可：

```bash
Are you sure you want to continue connecting (yes/no)?
```

若出现带有自己用户名的语句即为连接成功：

```bash
Hi your_name! You've successfully authenticated, but GitHub does not provide shell access.
```

### 使用SSH连接到Coding

> 如果和我一样，**GitHub和Coding的注册邮箱`相同`**的话，这一步继续按照这篇文章来操作

> 如果**GitHub和Coding的注册邮箱`不同`**的话，这一步需要参考这两个教程操作：
>
> [ssh-key git多账户配置](https://www.jianshu.com/p/0406cf5701e8)
>
> [Git配置多个SSH-Key](https://gitee.com/help/articles/4229#article-header0)

Coding新增SSH的步骤和GitHub差不多

![](https://cdn.jsdelivr.net/gh/BahuangShanren/picture@master/article_2020-05-21/7.png)

<p align="center"><img src="https://cdn.jsdelivr.net/gh/BahuangShanren/picture@master/article_2020-05-21/8.png" style="zoom: 50%;" /></p>

因为我的**GitHub和Coding的注册邮箱相同**，所以上面生成的那个SSH也可以用于Coding。

- 在Git Bash输入`cat ~/.ssh/id_rsa.pub` 查看个人公钥。复制输出的内容，粘贴到上图`公钥内容`框中。

- 或者用记事本打开`C:/Users/用户名/.ssh`中的`id_rsa.pub`文件。复制全部内容，粘贴到上图`公钥内容`框中。

点击添加。

在Git Bash中输入以下命令，

```bash
ssh -T git@e.coding.net
```

首次使用需要确认并添加主机到本机SSH可信列表——当出现下面这句提示时，输入`yes`回车即可：

```bash
Are you sure you want to continue connecting (yes/no)?
```

若出现带有自己用户名的语句即为成功：

```bash
Coding 提示: Hello your_name, You've connected to Coding.net via SSH. This is a personal key.
your_name，你好，你已经通过 SSH 协议认证 Coding.net 服务，这是一个个人公钥.
公钥指纹：************************
```

## 创建GitHub Pages

1. 新建一个仓库，仓库名称任意。

> GitHub可以为任意仓库开启GitHub Pages，只是URL有区别。
>
> 仓库名称若是 `username.github.io` ，URL就是 http://username.github.io ;
>
> 仓库名称若是其他，比如 `blog` ，URL就是 http://username.github.io/blog 

2. 选择“通过README文件初始化仓库”，GitHub会为此仓库开启GitHub Pages，如下图所示，提示用户“你的站点已经发布”，点进去会显示一个网页，就是刚才的README文件。

![](https://cdn.jsdelivr.net/gh/BahuangShanren/picture@master/article_2020-05-21/9.png)

3. 或者手动开启GitHub Pages，如下图，需要选择 `source` ，比如选择 `master` 然后点击 `save` 

![](https://cdn.jsdelivr.net/gh/BahuangShanren/picture@master/article_2020-05-21/GitHubPages.png)

> 创建Github仓库时，如果不手动设置分支，博客会默认推送到`master`或`main`分支。
>
> 当然可以建立几个分支，一个用来发布博客（就是`E:/Blog/public`中的内容），一个用来存储整个博客基础数据（即整个Hexo目录），这样可以多设备同步写博客，只是自己的一些appid就会暴露在源码中了。
>
> 使用多分支的话，要记得在Hexo目录下的`_config.yml`文件中改变发布仓库的分支。

## 创建Coding Pages

> Coding的静态网站服务偶尔会出现问题，所以看自己需要，没必要非得同时部署博客到GitHub和Coding。
>
> 其实开启了jsDelivr加速的GitHub Pages足够支撑大部分地区的浏览，要追求更快的速度，用Gitee Pages也行，它在三者之间速度最快，只是免费版本的不能自定义域名罢了。
>
> Coding工作台界面更新频繁，所以文章有些地方可能对不上实际情况。

1. 首先在Coding工作台新建一个项目

![](https://cdn.jsdelivr.net/gh/BahuangShanren/picture@master/article_2020-05-21/11.png)

> 因为Coding工作台的更新，模板会变动，而且不同模板的功能和设置都不同，不过影响不大，功能和设置后续可以自己在项目设置里更改。

2. 新建的项目地址名称也是随便填。

![](https://cdn.jsdelivr.net/gh/BahuangShanren/picture@master/article_2020-05-21/12.png)

> 选中`启用 README.md 文件初始化项目`，如果不，等会儿发布静态网站时还得手动在`代码仓库`功能中选择`快速初始化仓库`。

3. 建好项目后，确保项目有`持续集成`和`持续部署`这两个功能，如果没有，就在左下角的`项目设置`里，打开这两个功能的开关（所以新建项目时，选择哪个模板不重要）：

![](https://cdn.jsdelivr.net/gh/BahuangShanren/picture@master/article_2020-05-21/13.png)

4. 然后回到项目控制台，选择`持续部署`里的`静态网站`，点击`立即发布静态网站`，前提是已经实名认证过账号：

![](https://cdn.jsdelivr.net/gh/BahuangShanren/picture@master/article_2020-05-21/14.png)

5. Coding会生成一个随机的网址作为此静态网站的地址。当然现在什么内容也没有，需要后续部署。

## 下载matery主题

- 下载[主题源码](https://github.com/blinkfox/hexo-theme-matery/releases)，解压缩后，将`hexo-theme-matery`文件夹放到Hexo目录下的`themes`文件夹中即可。
- 也可以在Hexo目录下的`themes`文件夹中通过Git Bash输入以下命令克隆主题仓库：

```bash
git clone https://github.com/blinkfox/hexo-theme-matery.git
```

## 配置主题

### 配置Hexo的`_config.yml`文件

#### 切换主题

在Hexo目录下的`_config.yml`文件中，修改`theme`的值（此值即为主题文件夹的名字）：

```yaml
theme: hexo-theme-matery
```

#### 基础设置

在Hexo目录下的`_config.yml`文件中，根据自身情况修改。

| 参数          | 描述                                          |
| :------------ | :-------------------------------------------- |
|`title`      | 网站标题                                      |
|`subtitle`   | 网站副标题                                    |
|`description`| 网站描述，主要用于SEO                         |
|`keywords`   | 网站的关键词。使用半角逗号`,`分隔多个关键词 |
|`author`     | 作者名字                                      |
|`language`   | 网站使用的语言。简体中文用户设为`zh-CN`      |
|`timezone`   | 网站时区。Hexo 默认使用电脑的时区             |

#### 修改URL

在Hexo目录下的`_config.yml`文件中，将url: http://yoursite.com 改为自己的GitHub Pages链接。

#### 屏幕适配优化（建议）

在Hexo目录下的`_config.yml`文件中，修改两个`per_page`的分页条数值为`6`的倍数，如：`12`、`18`等，这样文章列表在各个屏幕下都能较好的显示。

```yaml
index_generator:
 path: ''
 per_page: 12
 order_by: -date
```

```yaml
per_page: 12
pagination_dir: page
```

#### 修改deploy地址

在Hexo目录下的`_config.yml`文件中：

```yaml
deploy:
	type: git
	repository:
        github: git@github.com:your_name/your_project_name.git
        coding: git@e.coding.net:your_name/your_project_name.git
	branch: master
```

将`repository`修改为自己的GitHub仓库和Coding仓库的SSH地址：

![](https://cdn.jsdelivr.net/gh/BahuangShanren/picture@master/article_2020-05-21/15.png)

![](https://cdn.jsdelivr.net/gh/BahuangShanren/picture@master/article_2020-05-21/16.png)

这样部署博客时就会同时部署到这两个仓库。

#### 修改代码高亮

默认的Hexo代码高亮不好分辨，十分影响阅读，matery主题中使用 [hexo-prism-plugin](https://github.com/ele828/hexo-prism-plugin) 的Hexo插件处理代码高亮，在Git Bash中输入以下命令安装：

```bash
npm i -S hexo-prism-plugin --save
```

在Hexo目录下的`_config.yml`文件中，修改`highlight`的值为`false`，然后新增`prism`插件相关的配置：

```yaml
prism_plugin:
  mode: 'preprocess'
  theme: 'tomorrow'
  line_number: true
  custom_css:
```

#### 增加搜索模块（可选）

matery主题中使用 [hexo-generator-search](https://github.com/wzpan/hexo-generator-search) 的Hexo插件来做内容搜索，在Git Bash中输入以下命令安装：

```bash
npm install hexo-generator-search --save
```

在Hexo目录下的`_config.yml`文件中，新增以下的配置项：

```yaml
search:
  path: search.xml
  field: post
```

#### 添加RSS订阅支持模块（可选）

matery主题中使用 [hexo-generator-feed](https://github.com/hexojs/hexo-generator-feed) 的Hexo插件来支持RSS，在Git Bash中输入以下命令安装：

```bash
npm install hexo-generator-feed --save
```

在Hexo目录下的`_config.yml`文件中，新增以下的配置项：

```yaml
feed:
  type: atom
  path: atom.xml
  limit: 20
  hub:
  content:
  content_limit: 140
  content_limit_delim: ' '
  order_by: -date
```

执行 `hexo clean && hexo g` 重新生成博客文件，在Hexo目录下的 `public` 文件夹中看到 `atom.xml` 文件，说明安装成功。

#### 增加中文链接转拼音模块（可选）

如果文章名称是中文的，那么Hexo默认生成的永久链接也会有中文，这样不利于`SEO`，而且`gitment`评论对中文链接也不支持。可以用 [hexo-permalink-pinyin](https://github.com/viko16/hexo-permalink-pinyin) 的Hexo插件使生成文章时自动生成中文拼音的永久链接。

在Git Bash中输入以下命令安装：

```bash
npm i hexo-permalink-pinyin --save
```

在Hexo目录下的`_config.yml`文件中，新增以下的配置项：

```yaml
permalink_pinyin:
  enable: true
  separator: '-'    # 默认分隔符为 '-'，按需要自定义
```

效果就是：

```html
https://duduibahuang.github.io/2020/05/13/搭建博客
```

变成：

```html
https://duduibahuang.github.io/2020/05/13/da-jian-bo-ke
```

#### 增加Emoji表情支持（可选）

matery主题中使用 [hexo-filter-github-emojis](https://npm.taobao.org/package/hexo-filter-github-emojis) 的 Hexo 插件来支持`emoji`表情的生成，把对应的`markdown emoji`语法（`::`,例如：`:smile:`）转变成`emoji`表情，在Git Bash中输入以下命令安装：

```bash
npm install hexo-filter-github-emojis --save
```

在 Hexo 目录下的`_config.yml`文件中，新增以下的配置项：

```yaml
githubEmojis:
  enable: true
  className: github-emoji
  inject: true
  styles:
  customEmojis:
```

### 配置matery主题的`_config.yml`文件

#### 更改图片

相关图片（比如赞赏码、文章特色图、logo等），在文件夹中替换为自己的即可，注意名称、路径、大小和分辨率。

#### 修改社交链接

在matery主题的`_config.yml`文件中，将社交连接改为自己的。

另外，可以在matery主题的`/layout/_partial/social-link.ejs`文件中，新增、修改自己需要的社交链接地址，增加链接可参考如下代码：

```html
<% if (theme.socialLink.github) { %>
    <a href="<%= theme.socialLink.github %>" class="tooltipped" target="_blank" data-tooltip="访问我的GitHub" data-position="top" data-delay="50">
        <i class="fab fa-github"></i>
    </a>
<% } %>
```

社交图标（如：`fa-github`）可以在 [Font Awesome](http://www.fontawesome.com.cn/) 中找到，复制图标名称在这里替换即可。

> 不过要注意Font Awesome的版本和主题使用的是否一致，新版本中新增的图标在旧版本中是无法显示的。

#### jsDelivr的CDN加速服务（建议）

> 第一次使用此功能，要先配置url，再`hexo clean && hexo g && hexo d`部署到GitHub的仓库（必须是GitHub的仓库）。

URL配置规则：`https://cdn.jsdelivr.net/gh/GitHub用户名/仓库名`，在matery主题的`_config.yml`文件中最后两行，更改为自己的链接：

```yaml
jsDelivr:
  url: https://cdn.jsdelivr.net/gh/duduibahuang/duduibahuang.github.io
```

> 1. 这样配置后生成的博客推送到GitHub的仓库中，然后jsDelivr访问该仓库，为这些文件提供CDN服务（具体可以到[jsDelivr官网](https://www.jsdelivr.com/)查看详细规则）。
>
> 2. 只要将上面的URL配置好就算开启了jsDelivr加速服务，别的文件引用链接不用改为jsDelivr要求的格式，会自动更改的。
>
> 3. 虽然理论上jsDelivr会引用最新版本的文件，但是更新时间有点长，可能会导致网页显示上一版本的内容。
>
> 4. 配置了此项，就代表着本地调试的时候，网站依然会去GitHub请求资源（原来的资源），本地调试的时候记得将此项配置注释或者删除掉。

- 不启用加速时，博客网站会从GitHub的服务器中加载需要的资源，因为GitHub的服务器在国外，所以网站加载速度慢。

不启用加速时，查看网站源码，文件的引用是这样的：

![](https://cdn.jsdelivr.net/gh/BahuangShanren/picture@master/article_2020-05-21/17.png)

- 启用加速后，博客网站会从jsDelivr的服务器中加载需要的资源，因为jsDelivr在中国有服务器，所以网站加载速度会有提升。

启用加速时，查看网站源码，文件的引用是这样的：

![](https://cdn.jsdelivr.net/gh/BahuangShanren/picture@master/article_2020-05-21/18.png)

#### 百度统计（可选）

在matery主题的`_config.yml`文件中，找到百度统计模块，填写自己的ID：

```yaml
baiduAnalytics:
  enable: true
  id: 
```

在 [百度统计](https://tongji.baidu.com/) 中新建网站，完成后会让你复制代码粘贴到自己的文件中，在`hm.src = "https://hm.baidu.com/hm.js?`之后的字符串就是你的ID：

![](https://cdn.jsdelivr.net/gh/BahuangShanren/picture@master/article_2020-05-21/BaiduAnalytics.png)

其实打开`matery/layout/_partial/baidu-analytics.ejs`就会发现，百度统计让你复制的代码已经在这里了，只是没有ID，意思就是matery主题的作者已经把这个功能集成了。

#### 谷歌统计（可选）

在matery主题的`_config.yml`文件中，找到谷歌统计模块，填写自己的ID：

```yaml
googleAnalytics:
  enable: true
  id: 
```

在 [谷歌统计](https://analytics.google.com) 中添加自己的网站，找到自己的跟踪ID：

![](https://cdn.jsdelivr.net/gh/BahuangShanren/picture@master/article_2020-05-21/GoogleAnalytics.png)

### 新增博客功能页面（可选）

#### 新建分类 categories 页

`categories`页是用来展示所有分类的页面，如果hexo目录下的`source`目录中还没有`/categories/index.md`文件，使用Git Bash运行以下命令新建：

```bash
hexo new page "categories"
```

编辑刚刚新建的页面文件`/source/categories/index.md`，至少需要以下内容：

```yaml
---
title: categories
date: 2020-05-13 22:06:28
type: "categories"
layout: "categories"
---
```

#### 新建标签 tags 页

`tags`页是用来展示所有标签的页面，如果hexo目录下的`source`目录中还没有`/tags/index.md`文件，使用Git Bash运行以下命令新建：

```bash
hexo new page "tags"
```

编辑刚刚新建的页面文件`/source/tags/index.md`，至少需要以下内容：

```yaml
---
title: tags
date: 2020-05-13 22:06:28
type: "tags"
layout: "tags"
---
```

#### 新建留言板 contact 页

`contact`页是用来展示留言板信息的页面，如果hexo目录下的`source`目录中还没有`/contact/index.md`文件，使用Git Bash运行以下命令新建：

```bash
hexo new page "contact"
```

编辑刚刚新建的页面文件`/source/contact/index.md`，至少需要以下内容：

```yaml
---
title: contact
date: 2020-05-14 10:40:01
type: "contact"
layout: "contact"
---
```

> 本留言板功能依赖于第三方评论插件，需要启用自己的的评论系统才有效果。评论模块设置在matery目录下的`_config.yml`文件中。

#### 新增评论插件

评论插件的选择可以看云游君的这篇[第三方评论系统之我见](https://www.yunyoujun.cn/share/third-party-comment-system/)。

首先来到[Valine官网](https://www.leancloud.cn/)，创建新应用，之后在应用的设置里复制AppId 和 AppKey。

![](https://cdn.jsdelivr.net/gh/BahuangShanren/picture@master/article_2020-05-21/19.png)

在matery目录下的`_config.yml`文件中，填写刚刚复制的AppId 和 AppKey。

```yaml
# Valine 评论模块的配置，默认为不激活，如要使用，就请激活该配置项，并设置 appId 和 appKey.
valine:
  enable: true
  appId: 
  appKey: 
  notify: true
  verify: true
  visitor: true
  avatar: 'mm' # Gravatar style : mm/identicon/monsterid/wavatar/retro/hide
  pageSize: 10
  placeholder: '说点什么' # Comment Box placeholder
  background: #放上图片的地址
```

在该文件的400行左右，可以按需要更改引用Valine插件的地址：

```yaml
valine: /libs/valine/Valine.min.js
#若想保持最新版，请替换为https://unpkg.com/valine/dist/Valine.min.js，默认为/libs/valine/Valine.min.js
```

> 然而如果使用https://unpkg.com/valine/dist/Valine.min.js 这个链接，同时jsDelivr的CDN加速服务处于启用状态，就会导致Valine加载失败（具体可以看加载失败的网页源代码，会发现引用Valine插件的地址被jsDelivr变成了错误的地址）。
>
> 解决方案是，手动下载最新版Valine.min.js覆盖的本地`/themes/matery/source/libs/valine/Valine.min.js`

Valine的留言管理是通过操作数据库完成的：

![](https://cdn.jsdelivr.net/gh/BahuangShanren/picture@master/article_2020-05-21/20.png)

> Valine目前使用的是 [Gravatar](http://cn.gravatar.com/) 作为评论列表头像。请自行登录或注册 [Gravatar](http://cn.gravatar.com/)，然后修改自己的头像。
>
> 评论的时候，留下在 [Gravatar](http://cn.gravatar.com/) 注册时所使用的邮箱即可。
>
> 目前非自定义头像有以下8种默认值可选:
>
> |    参数值    |                           表现形式                           | 备注                             |
> | :----------: | :----------------------------------------------------------: | :------------------------------- |
> | 空字符串`''` | ![Gravatar官方图形](https://gravatar.loli.net/avatar/d41d8cd98f00b204e9800998ecf8427e?s=40) | Gravatar官方图形                 |
> |     `mp`     | ![神秘人(一个灰白头像)](https://gravatar.loli.net/avatar/d41d8cd98f00b204e9800998ecf8427e?s=40&d=mp) | 神秘人(一个灰白头像)             |
> | `identicon`  | ![抽象几何图形](https://gravatar.loli.net/avatar/d41d8cd98f00b204e9800998ecf8427e?s=40&d=identicon) | 抽象几何图形                     |
> | `monsterid`  | ![小怪物](https://gravatar.loli.net/avatar/d41d8cd98f00b204e9800998ecf8427e?s=40&d=monsterid) | 小怪物                           |
> |  `wavatar`   | ![用不同面孔和背景组合生成的头像](https://gravatar.loli.net/avatar/d41d8cd98f00b204e9800998ecf8427e?s=40&d=wavatar) | 用不同面孔和背景组合生成的头像   |
> |   `retro`    | ![八位像素复古头像](https://gravatar.loli.net/avatar/d41d8cd98f00b204e9800998ecf8427e?s=40&d=retro) | 八位像素复古头像                 |
> |  `robohash`  | ![机器人](https://gravatar.loli.net/avatar/d41d8cd98f00b204e9800998ecf8427e?s=40&d=robohash) | 一种具有不同颜色、面部等的机器人 |
> |    `hide`    |                                                              | 不显示头像                       |

修改matery目录下`_config.yml`文件中的Valine模块即可更改默认非自定义头像：

```yaml
Valine
...
avatar: 'mp' # Gravatar style : ''/mp/identicon/monsterid/wavatar/retro/robohash/hide
```

#### 新建友情链接 friends 页

`friends`页是用来展示友情链接信息的页面，如果hexo目录下的`source`目录中还没有`/friends/index.md`文件，使用Git Bash运行以下命令新建：

```bash
hexo new page "friends"
```

编辑刚刚新建的页面文件`/source/friends/index.md`，至少需要以下内容：

```yaml
---
title: friends
date: 2020-05-14 10:45:11
type: "friends"
layout: "friends"
---
```

在hexo目录下的`source`目录中新建`_data`目录，在`_data`目录中新建`friends.json`文件，文件内容示例如下所示：

```yaml
[{
    "avatar": "https://cdn.jsdelivr.net/gh/duduibahuang/photo/avatar.jpg",
    "name": "闪烁之狐",
    "introduction": "Matery主题作者",
    "url": "https://blinkfox.github.io/",
    "title": "前去学习"
}]
```

#### 新建404页

matery主题没有404页面，在hexo目录下的`source`目录中新建一个`404.md`，内容如下：

```yaml
---
title: 404
date: 2020-05-14 10:45:11
type: "404"
layout: "404"
description: "你来到了没有知识的荒原 "
---
```

然后在matery主题文件夹的`layout`目录下新建一个`404.ejs`文件，内容如下：

```html
<style type="text/css">
    /* don't remove. */
    .about-cover {
        height: 75vh;
    }
</style>

<div class="bg-cover pd-header about-cover">
    <div class="container">
        <div class="row">
            <div class="col s10 offset-s1 m8 offset-m2 l8 offset-l2">
                <div class="brand">
                    <div class="title center-align">
                        404
                    </div>
                    <div class="description center-align">
                        <%= page.description %>
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>

<script>
    // 每天切换 banner 图.  Switch banner image every day.
    $('.bg-cover').css('background-image', 'url(/medias/banner/' + new Date().getDay() + '.jpg)');
</script>
```

#### 新增分享插件

在matery目录下的`_config.yml`文件中，默认有两种选择，sharejs和addthis：

1. [sharejs](https://github.com/overtrue/share.js/)

```yaml
# 支持顺序，可选项目为twitter, facebook, google, qq, qzone, wechat, weibo, douban, linkedin.
sharejs:
  enable: false
  sites: twitter,facebook,google,qq,qzone,wechat,weibo,douban,linkedin
```

> matery的sharejs文件在matery目录下的`source/libs/share/js`中，不过是压缩后的`min.js`文件。

2. [AddThis](https://www.addthis.com/ )

首先到官网创建新应用，之后复制应用的pubid：

![](https://cdn.jsdelivr.net/gh/BahuangShanren/picture@master/article_2020-05-21/21.png)

```yaml
addthis:
  enable: false
  pubid: #输入应用的pubid
```

## 文章设置

### Front-matter设置（建议）

为了新建文章方便，将Hexo目录下`/scaffolds/post.md`、`/scaffolds/page.md`、`/scaffolds/draft.md`都修改为：

```yaml
---
title: {{ title }}
date: {{ date }}
img: 
top: true
cover: true
coverImg: 
toc: true
mathjax: false
summary: 
categories: 
tags:
keywords:
---
```
这样以后新建文章后不用自己补充了，修改信息即可，每篇文章的这些属性可以单独设置。

### Front-matter选项解释

这些内容均为非必填的。但至少填写`title`和`date`的值。

| 配置选项      | 默认值                                | 描述                                                         |
| ------------- | ------------------------------------- | ------------------------------------------------------------ |
| title         |`Markdown`的文件标题                 | 文章标题                                                     |
| date          | 文件创建时的日期时间                  | 发布时间。尽量保证每篇文章的该值是唯一的，因为本主题中`Gitalk`和`Gitment`识别`id`是通过`date`的值来作为唯一标识的 |
| author        | Hexo目录下`_config.yml`中的`author`| 文章作者                                                     |
| img           |`featureImages`中的某个值            | 文章特征图，推荐使用图床                                     |
| top           |`true`                               | 是否作为首页推荐文章                                         |
| cover         |`false`                              | 是否将文章加入到首页轮播封面中                               |
| coverImg      | 无                                    | 该文章在首页轮播封面需要显示的图片，如果没有，则默认使用文章的特色图片 |
| password      | 无                                    | 是否设置本文章的阅读验证密码。该值必须是用`SHA256`加密后的密码，若启用，前提是在主题的`config.yml`中激活了`verifyPassword`选项 |
| toc           |`true`                               | 本文章是否开启是否开启TOC。若启用，前提是在主题的`config.yml`中激活了`toc`选项 |
| mathjax       |`false`                              | 本文章是否开启数学公式支持。若启用，前提是在主题的`config.yml`中激活了`mathjax`选项 |
| summary       | 无                                    | 如果这个属性有值，文章摘要就显示这段文字，否则程序会自动截取文章的部分内容作为摘要 |
| categories    | 无                                    | 文章分类                                                     |
| tags          | 无                                    | 文章标签，一篇文章可以多个标签                               |
| keywords      | 文章标题                              | 文章关键字，SEO 时需要                                       |
| reprintPolicy | cc_by                                 | 文章转载规则， 可以是 cc_by, cc_by_nd, cc_by_sa, cc_by_nc, cc_by_nc_nd, cc_by_nc_sa, cc0, noreprint 或 pay 中的一个 |

- tags多标签时示例

```yaml
tags:
  - Typora
  - Markdown
```

## 新建文章、PicGo配置图床与发布

### 新建文章

在Hexo目录下，Git Bash Here，输入`hexo new post "文章名称"`，新建一篇文章。

然后打开Hexo目录下`source/_posts`，发现多了一个markdown文件，这就是刚刚新建的文章。

### 配置图床（建议）

众所周知，Markdown文件是不能像Word那样把图片嵌入文件里的，即图片不是跟着文件走的。如果插入的是本地图片，发布了博客之后，就会发现文章里没有图片。所以一般选择插入网络图片的形式。

这时要有一个方便又快速的上传工具，会加快写作的效率。

- 注意不要把隐私图片上传到图床中。
- 以下方法基于 [PicGo2.3.0-beta.0](https://github.com/Molunerfinn/PicGo/releases/tag/v2.3.0-beta.0) 和 [Typora0.9.86(beta)](https://typora.io/#windows) 
- PicGo默认支持SM.MS图床、腾讯云COS、Github图床、七牛图床、Imgur图床、阿里云OSS、又拍云图床，更多支持可以根据 [PicGo插件](https://github.com/PicGo/Awesome-PicGo) 自行配置，下面我只列举了几个适合新手的。
- Typora自动上传需要PicGo(app)保持在后台运行。
- 如果习惯用命令行的话，[PicGo-Core](https://picgo.github.io/PicGo-Core-Doc/) 更舒适。


#### 法一

[PicGo+Gitee图床+Typora](https://zhuanlan.zhihu.com/p/102594554)，**稳定**。

> **以下是注意事项**

1. Gitee中超过1MB的文件无法通过外链获取。
2. 在Typora中引用Gitee的图片，格式是这样的（PicGo默认自动复制的也是这个格式）：

```yaml
https://gitee.com/用户名/仓库名/raw/master/图片名
```

而不是用直接在网页地址栏复制的链接：

```yaml
https://gitee.com/用户名/仓库名/blob/master/图片名
```

**即`仓库名`与`master`之间的是`raw`而不是`其他字符`**，不然图片显示不出来。

3.  如果在Typora中插入中文名的图片上传到**Gitee**，虽然PicGo也能自动上传成功，但是在Typora中显示不出来，解决办法是把Typora的`偏好设置`→`图像`→`插入图片时自动转义图片URL功能`打开。这样以后再插入中文名图片就一切正常了，不过在这之前插入的中文名图片还是不能显示，需要重新插入（自动转义、上传）。

> 至于PicGo+Coding图床+Typora，我各种操作，无法使用（可能是我菜了）

#### 法二

PicGo+[SM.MS图床](https://sm.ms/)+Typora，**稳定**，普通用户免费空间有5GB，在PicGo里配置很简单，只要粘贴上自己的token即可。

#### 法三

[PicGo+GitHub图床+Typora](https://picgo.github.io/PicGo-Doc/zh/guide/config.html#github%E5%9B%BE%E5%BA%8A)，**非常不稳定**，经常上传失败。

**上传失败的几个原因**

1. Typora提示：`{"success",false}`，这是因为同一名称的图片重复上传。
2. Typora提示：`Failed to fetch`，网络原因（比如端口的问题），具体可以在PicGo的设置里打开日志，查看原因。
3. 据说图片上传时间的间隔太短会受到Github服务器限制（不知真假）。
4. PicGo自身的bug。
5. Typora自身的bug。

> 有人说图片名称里有`+`，也会导致上传失败。我试验了一下，（这个版本，别的版本没试验）可以正常上传，他说的可能是过时的情况了。

#### 法四

既然PicGo上传GitHub图床不稳定，干脆不用它了。直接在本地Git仓库文件夹里增删图片，然后通过GitHub Desktop或者Git等工具批量Pull、Push，用图片的时候在Markdown里粘贴图片链接即可：

```Markdown
![](https://cdn.jsdelivr.net/gh/用户名/仓库名/图片名)
```

缺点是图片名称容易搞混。

> 注意：虽然理论上jsDelivr会引用最新版本的文件，但是更新时间有点长，可能会导致网页显示同名称的上一版本图片。

### 发布文章

#### Hexo常用命令

##### hexo clean

- 清除缓存文件 (`db.json`) 和已生成的静态文件 (`public`)。在某些情况（尤其是更换主题后），如果发现对站点的更改无论如何也不生效，可能需要运行该命令。

- 该命令可以简写为`hexo cl`

##### hexo generate

- 生成静态文件。


- 该命令可以简写为`hexo g`

##### hexo server

- 启动服务器，开启本地预览，默认情况下，访问网址为：http://localhost:4000/


- 该命令可以简写为`hexo s`

##### hexo deploy

- 部署之前预先生成静态文件，然后部署。

- 该命令可以简写为`hexo d`

#### 快速发布

编辑完文章后，

- 在Git Bash输入以下命令即可本地预览：

```bash
hexo cl && hexo g && hexo s
```

- 在Git Bash输入以下命令即可部署博客到静态网站（即推送到相应仓库）：

```bash
hexo cl && hexo d
```

> 推送成功后，需要稍等几秒，网站才会应用刚刚部署的相关博客或设置，如果多次刷新、清理缓存都未达到效果，就要检查自己的各方面配置了。

## 自定义域名（可选）

### 域名解析

首先注册一个域名，很多人都推荐在国外的服务商注册，原因大概有：

1. 可以不用实名认证
2. 有大量便宜的域名注册商
3. 不会像国内的一些服务商那样因为某些原因停止解析自己的域名

但是，“**部分**国外网站注册域名便宜”说的是域名价格**平均**比国内便宜，指的是**平均情况**。所以对于个别域名，还是要货比三家。我是在 [阿里云](https://wanwang.aliyun.com/domain/) 注册的域名，[腾讯云](https://dnspod.cloud.tencent.com/) 也不错。

注册之后解析域名，可以参考下图这样添加：

![](https://cdn.jsdelivr.net/gh/BahuangShanren/picture@master/article_2020-05-21/22.png)

这样，阿里云的国外服务器为境外浏览者解析`shan-ren.cn`指向我的GitHub Pages，阿里云的国内服务器为国内浏览者解析`shan-ren.cn`指向我的Coding Pages，这就实现了分流。

> 有人说，这种情况可以两条记录都使用默认解析线路，阿里云会智能分流。我试了，不怎么稳定。

DNS分流解析效果图：

![](https://cdn.jsdelivr.net/gh/BahuangShanren/picture@master/article_2020-05-21/23.png)

> 我用的是国外[whatsmydns](https://www.whatsmydns.net/)查询的，用阿里云自己的DNS查询工具查不出来这种效果。

因为我还使用了jsDelivr的CDN加速服务，网站具体加载速度依赖jsDelivr的全球节点，不管是GitHub Pages还是Coding Pages，速度差别不大。

> 但是，还是不如部署到Gitee Pages上加载速度快。只不过Gitee Pages免费版本每次在本地部署后，还得在Gitee Pages页面手动更新，而且免费版本不能自定义域名，所以我就没用Gitee Pages。

### 域名绑定

只是完成在域名服务商那里解析的步骤，还不能直接访问自己的域名转到相应的Github/Coding Pages，因为网站（Github/Coding Pages）上还需要有CNAME文件（如果域名解析类型是CNAME型）来对应。

#### 法一

> 前提是像我一样，把一个域名同时解析为两个域名（GitHub Pages和Coding Pages的域名）。

在本地Hexo目录下的`source`目录里新建名为`CNAME`的文件（没有后缀名），输入自己的解析的域名，比如我输入`shan-ren.cn`，这样`hexo d`之后，会把CNAME文件推送到我的GitHub和Coding的仓库，这样网站上就有CNAME文件来对应服务商的域名解析，输入`shan-ren.cn`就会跳转到相应的网站。

#### 法二

> 前提是解析了两个不同的域名分别指向GitHub Pages和Coding Pages。

这样就不能用法一了，因为一个CNAME文件放到两个网站上，两个网站都想对应解析成CNAME文件里的那个域名，这就产生了冲突导致解析失败。可以在每次`hexo d`部署之后，手动到网站上设置。

1. 在GitHub Pages仓库的设置里参考下图设置：

![](https://cdn.jsdelivr.net/gh/BahuangShanren/picture@master/article_2020-05-21/24.png)

2. 在Coding Pages仓库的设置里参考下图设置：

![](https://cdn.jsdelivr.net/gh/BahuangShanren/picture@master/article_2020-05-21/25.png)

要记得重新点击“立即部署”按钮：

![](https://cdn.jsdelivr.net/gh/BahuangShanren/picture@master/article_2020-05-21/26.png)

这样就不冲突了。只是每次都得手动设置，可能还有更好的解决方案，但我用不上，就没去琢磨。

## 参考链接

[Hexo文档](https://hexo.io/zh-cn/docs/)

[Matery文档](https://github.com/blinkfox/hexo-theme-matery/blob/develop/README_CN.md)

[韦阳的博客](https://godweiyang.com/2018/04/13/hexo-blog/)
