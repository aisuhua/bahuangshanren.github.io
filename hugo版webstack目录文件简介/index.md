# Hugo版WebStack目录文件简介


{{< admonition type=example title="八荒山人导航" open=true >}}
我的导航站：[https://nav.bahuangshanren.tech/](https://nav.bahuangshanren.tech/) 

导航站源码地址：[https://github.com/BahuangShanren/navigation](https://github.com/BahuangShanren/navigation) 
{{< /admonition >}}

## 目录树

```shell
navigation
├─ .github
│    └─ workflows
│           └─ gh-pages.yml
├─ LICENSE
├─ README.md
├─ archetypes
│    └─ default.md
├─ config.toml
├─ content
│    └─ about.md
└─ themes
       └─ webstack-hugo
              ├─ data
              │    └─ webstack.yml
              ├─ layouts
              │    ├─ 404.html
              │    ├─ _default
              │    │    └─ single.html
              │    └─ index.html
              ├─ static
              │    └─ assets
              │           ├─ css
              │           │    ├─ bootstrap.css
              │           │    ├─ fontawesome_v4.7.0
              │           │    │    ├─ css
              │           │    │    │    └─ font-awesome.min.css
              │           │    │    └─ fonts
              │           │    │           ├─ FontAwesome.otf
              │           │    │           ├─ fontawesome-webfont.eot
              │           │    │           ├─ fontawesome-webfont.svg
              │           │    │           ├─ fontawesome-webfont.ttf
              │           │    │           ├─ fontawesome-webfont.woff
              │           │    │           └─ fontawesome-webfont.woff2
              │           │    ├─ googleapis.css
              │           │    ├─ nav.css
              │           │    ├─ xenon-components.css
              │           │    ├─ xenon-core.css
              │           │    ├─ xenon-forms.css
              │           │    ├─ xenon-skins.css
              │           │    └─ xenon.css
              │           ├─ images
              │           │    ├─ favicon.svg
              │           │    ├─ logo@2x.svg
              │           │    ├─ logo_dark@2x.svg
              │           │    └─ logos
              │           │           ├─ Default.svg
              │           │           └─ Github.svg
              │           └─ js
              │                  ├─ TweenMax.min.js
              │                  ├─ bootstrap.min.js
              │                  ├─ joinable.js
              │                  ├─ jquery-1.11.1.min.js
              │                  ├─ lozad.js
              │                  ├─ resizeable.js
              │                  ├─ xenon-api.js
              │                  ├─ xenon-custom.js
              │                  └─ xenon-toggles.js
              └─ theme.toml
```

## 对于目录及文件的简介

###  /.github/workflows/gh-pages.yml

Github Actions自动构建和部署配置文件。

```yaml
name: Deploy Navigation #名字随便起

on:
  push:
    branches:
      - master #源码所在分支

jobs:
  deploy:
    runs-on: ubuntu-20.04
    steps:
      - uses: actions/checkout@v2

      - name: Setup Hugo #安装hugo
        uses: peaceiris/actions-hugo@v2.4.13 #使用peaceiris开发的actions-hugo
        with:
          hugo-version: 'latest' #可以指定版本号，也可以使用latest表示最新版
          extended: true #支持hugo的扩展版

      - name: Build #使用hugo构建网页
        run: hugo --gc --minify

      - name: Deploy #部署博客
        uses: peaceiris/actions-gh-pages@v3.7.3
        with:
          personal_token: ${{ secrets.navigation }} # 个人访问令牌
          publish_branch: gh-pages # 发布网站的分支
          publish_dir: ./public
          cname: # 你的域名
          commit_message: ${{ github.event.head_commit.message }}
```

{{< admonition type=abstract title="不重要的几个文件" open=true >}}
1. `archetypes/default.md` 没什么用处。

2. `config.toml` 是hugo网站中常用的基础配置文件，在这个主题中需要配置的项不多。

3. `content/about.md` 用于生成 `关于本站` 页面。
{{< /admonition >}}

### /themes/webstack-hugo

#### /data/webstack.yml

`/themes/webstack-hugo/data/webstack.yml` ,导航网站页面中的网址都在这里配置。

##### 一级导航栏示例

```yaml
- taxonomy: 常用 # 一级导航栏名称
  icon: fa-star # fontawesome图标
  links: 
    - title: Github # 网站名称
      logo: assets/images/logos/Github.svg 
      # logo地址，即 themes/webstack-hugo/static/assets/images/logos/XXX.svg
      url: https://github.com/ # 网址
      description:  # 网站名称下面对于网站的描述

    - title: Gitee
      logo: assets/images/logos/Gitee.svg
      url: https://gitee.com/
      description: 
```

##### 二级导航栏示例

```yaml
- taxonomy: 邮箱 # 一级导航栏名称
  icon: fa-envelope
  list: 
    - term: 普通邮箱 # 二级导航栏名称
      icon: fa-envelope
      links:
        - title: Gmail
          logo: assets/images/logos/Gmail.svg
          url: https://mail.google.com/
          description:  

    - term: 临时邮箱
      icon: fa-envelope-open
      links:
        - title: 临时邮箱
          logo: assets/images/logos/Linshiyouxiang.svg
          url: https://linshiyouxiang.net/
          description: 
```

![](https://gitee.com/BahuangShanren/picture/raw/master/article_2021-03-13/1.png)

#### /layouts

1. `themes/webstack-hugo/layouts/_default/single.html`

    网站默认模板，比如 `关于本站` 的页面。

2. `themes/webstack-hugo/layouts/index.html`

    网站首页模板。

#### /static

##### /assets

###### /css

1. `themes/webstack-hugo/static/assets/css/fontawesome_v4.7.0`

    原作者在 `themes/webstack-hugo/static/assets/css/fonts` 这里放了好几种图标，感觉用不太到，就只留下了 `fontawesome` 。又因其版本不高，于是升级，但以我现在的水平只能把 [fontawesome](http://www.fontawesome.com.cn/) 升到 `v4.7.0` ，本来想升到 `v5.0` ，但是 `v5.0` 的结构不同于 `v4.7.0` ，要是换的话，需要处理负责网站框架的CSS和JS。

{{< admonition type=note title="换用图标注意事项" open=true >}}
在 `single.html` 和 `index.html` 这两个网页模板中，
1.  `<head>` 中引入 `fontawesome` 的CSS路径。
2. 用新的图标名称替换掉旧的。
{{< /admonition >}}

2. 其他CSS是网站框架方面的CSS。

###### /images

1. `themes/webstack-hugo/static/assets/images/logos` 网站首页各个网址的logo都存放在这里。
2. `themes/webstack-hugo/static/assets/images/favicon.svg` 是网站首页左侧导航菜单在折叠状态的左上角图标。
3. `themes/webstack-hugo/static/assets/images/logo_dark@2x.svg` 是 `关于本站` 页面中网站左上角的图标。
4. `themes/webstack-hugo/static/assets/images/logo@2x.svg` 是网站首页左侧导航菜单在展开状态的左上角图标。

###### /js

网站框架方面的JS。

#### /theme.toml

主题配置文件，不用修改。
