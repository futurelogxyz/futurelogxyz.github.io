---
author: wallezen
title: 利器（一）
date: 2023-08-15
description:  博客、文档、API、图库、幻灯片的创作与管理工具
tags: ["tools", "blog", "document", "API", "gallery", "slides"]
thumbnail: https://futurelog-1251943639.cos.accelerate.myqcloud.com/img/iX8Eva-20230815.png
photoCredits: <a href="https://twitter.com/wallezen007" target="_blank">wallezen</a>
photoSource: <a href="https://www.imdb.com/title/tt0062622/" target="_blank">「2001:A Space Odyssey」</a>
draft: true
---

> 人创造工具，工具改变人。
{.blockquote}

## 1. 博客
搭建博客的工具较多，从经典的功能丰富的 `WordPress` 到简单易用主题丰富的 `Hexo`、`Jekyll`、`Hugo` 等等。
本博客使用的是 [Hugo](https://gohugo.io/)，`Hugo` 具备优异的性能，配置灵活、主题丰富、功能扩展方便。
关于 `Hugo` 的安装与使用，可以参考 [Hugo 官方文档](https://gohugo.io/documentation/)。

下面重点介绍下本博客的安装部署过程，以及对博客主题的修改定制。

### 1.1 博客安装
本博客使用了 [Hinode](https://themes.gohugo.io/themes/hugo-theme-hinode/) 主题，推荐基于其 `template` 的方式进行安装。

Step 1. 访问 Github [Hinode template](https://github.com/gethinode/template) 仓库，点击仓库右上角的 `Use this template` -> `Create a new repository`。

Step 2. 填写 `Repository name` 和 `Description` 等相关信息，然后点击 `Create repository`。
{{< carousel class="col-sm-12 col-lg-8 mx-auto w-90" >}}
  {{< img src="https://futurelog-1251943639.cos.accelerate.myqcloud.com/img/4Z8JfR-20230820.png" caption="Step 1" >}}
  {{< img src="https://futurelog-1251943639.cos.accelerate.myqcloud.com/img/qzum3z-20230820.png" caption="Step 2" >}}
{{< /carousel >}}

Step 3. 在本地 clone 刚刚创建的仓库，并安装相关依赖。
{{< command user="wallezen" host="localhost" shell="bash" >}}
git clone https://github.com/futurelogxyz/futurelogxyz.github.io.git
(out)Cloning into 'futurelogxyz.github.io'...
(out)remote: Enumerating objects: 39, done.
(out)remote: Counting objects: 100% (39/39), done.
(out)remote: Compressing objects: 100% (33/33), done.
(out)remote: Total 39 (delta 1), reused 26 (delta 1), pack-reused 0
(out)Receiving objects: 100% (39/39), 119.98 KiB | 777.00 KiB/s, done.
(out)Resolving deltas: 100% (1/1), done.
cd futurelogxyz.github.io && npm install
(out)...
{{< /command >}}

Step 4. 启动 local server，预览博客。
{{< command user="wallezen" host="localhost" shell="bash" >}}
hugo server -D # OR npm run start
(out)Watching for changes in /Users/wallezen/Codehub/github/futurelogxyz/futurelogxyz.github.io/{archetypes,assets,content,(out)data,i18n,layouts,package.json,static}
(out)Watching for config changes in /Users/wallezen/Codehub/github/futurelogxyz/futurelogxyz.github.io/config/_default, /(out)(out)Users/wallezen/Codehub/github/futurelogxyz/futurelogxyz.github.io/config/_default/menus
(out)Start building sites … 
(out)hugo v0.116.0-5a7e0da84e6e3cb74108abedc0b3331cbe2c9c68+extended darwin/arm64 BuildDate=2023-07-31T10:28:28Z VendorInfo=brew
(out)
(out)                   | ZH  | EN   
(out)-------------------+-----+------
(out)  Pages            |  57 |  36  
(out)  Paginator pages  |   1 |   1  
(out)  Non-page files   |   0 |   0  
(out)  Static files     | 111 | 111  
(out)  Processed images | 198 |   0  
(out)  Aliases          |   3 |   2  
(out)  Sitemaps         |   2 |   1  
(out)  Cleaned          |   0 |   0  
(out)
(out)Built in 7711 ms
(out)Environment: "development"
(out)Serving pages from memory
(out)Running in Fast Render Mode. For full rebuilds on change: hugo server --disableFastRender
(out)Web Server is available at http://localhost:1313/ (bind address 127.0.0.1)
(out)Press Ctrl+C to stop
{{< /command >}}

### 1.2 博客部署
本博客基于 [Github Pages](https://pages.github.com/) 进行部署。

Step 1. 修改 `config/_default/hugo.toml` 文件，将 `baseURL` 修改为 `https://futurelogxyz.github.io`。

Step 2. 在 `config/_default/hugo.toml` 文件 中添加一行配置 `publishDir = "docs"`。

Step 3. 运行 `hugo` 命令，生成静态文件。
{{< command user="wallezen" host="localhost" shell="bash" >}}
hugo
{{< /command >}}

Step 4. 将生成的静态文件推送到 Github 仓库。
{{< command user="wallezen" host="localhost" shell="bash" >}}
git add .
git commit -m "deploy"
git push
{{< /command >}}

Step 5. 在 Github 仓库的 `Settings` -> `Pages` 中，将 `Source` 修改为 `Github Actions`, 然后选择 `Static HTML`
{{< image src="https://futurelog-1251943639.cos.accelerate.myqcloud.com/img/rBxWP7-20230820.png" class="w-80" >}}

Step 6. 在 `Static HTML` 的配置文件 `static.yml` 中，将 `path` 修改为 `./docs/`。
{{< image src="https://futurelog-1251943639.cos.accelerate.myqcloud.com/img/qnN7UT-20230820.png" class="w-80" >}}

Step 7. 点击 `Commit changes`，Github 会自动构建并部署博客，部署完成后，Github pages 会自动分配域名 `https://futurelogxyz.github.io`。根据需要，可以在 `Settings` -> `Pages` 的 `Custom domain` 中绑定自定义域名，按照说明配置即可，根据这里不再详述。

### 1.3 主题定制
`Hinode` 主题简洁美观，并包含了丰富的功能。但默认的 footer 样式不太好看，因此参考了 [gethinode](https://gethinode.com/) 站点的 footer 样式，对进行了定制修改。

Step 1. 新增 `layouts/partials/footer/footer.html` 和 `layouts/partials/footer/social.html` 文件。
{{< file path="layouts/partials/footer/footer.html" id="file-footer-html" show="false">}}
{{< file path="layouts/partials/footer/social.html" id="file-social-html" show="false">}}

Step 2. 新增 `assets/scss/theme.scss` 文件。
{{< file path="assets/scss/theme.scss" id="file-theme-scss" show="false">}}

Step 3. 修改 `config/_default/menus/menus.en.toml` 文件，新增 `footer` 菜单。
{{< docs name="menus-footer" file="config/_default/menus/menus.en.toml" id="docs-menus-footer" show="false" >}}

Step 4. 启动 local server 即可预览定制的 footer 样式效果。


## 2. 文档
文档相比于博客文章是一种更具结构化作为一名程序员，文档是必不可少的，不管是技术文档还是个人笔记，都需要一个好用的文档工具。文档


## 3. API


## 4. 图库


## 5. 幻灯片


