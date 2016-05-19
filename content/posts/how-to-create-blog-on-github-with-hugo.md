+++
date = "2016-05-17T14:24:57+08:00"
draft = false
title = "怎样使用Hugo和Github创建个人博客(Mac OS)"

+++

[Hugo][1]使得我们更容易的生成一套可部署在[Github][2]上的博客系统，其简单易用的特点广受欢迎，其提供的大量主题模板可以让你的个人博客彰显个性和品位。

### 本地测试
- 安装[Brew][3]

```
$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
- 安装[Hugo][4]

```
$ brew install hugo
```
- 生成[Hugo][5]博客目录

```
$ hugo new site blog
```
此时生成了一个名字为blog的目录，目录结构如下：

```
├── archetypes
├── content
├── data
├── layouts
├── static
```
- 下载一个主题

```
$ mkdir themes
$ cd themes
$ git clone https://github.com/spf13/hyde.git
```
- 创建一篇博客文章

```
$ hugo new post/HelloWorld.md
```
- 本地测试博客

```
$ hugo server --theme=hyde
```
浏览器打开[http://localhost:1313][6]，一个全新的博客页面展示出来。

### 部署到Github

- 注册[Github][7]账号
新建一个仓库并命名为`username.github.io`，username为你的Github用户名。

```
$ cd blog
$ hugo --theme=hyde --baseUrl="http://username.github.io"
```
顺利的话，blog目录下会生成了一个public文件夹，这个文件夹包含了Hugo为你生成的静态页面。目录结构大概是下面这个样子：

```
zengdaqiandeMacBook-Pro:Blog zengdaqian$ tree public/
public/
├── 404.html
├── CNAME
├── apple-touch-icon-144-precomposed.png
├── css
│   ├── hyde.css
│   ├── poole.css
│   └── syntax.css
├── favicon.png
├── index.html
├── index.xml
├── posts
│   ├── HelloWorld
│   │   └── index.html
│   ├── index.html
│   └── index.xml
└── sitemap.xml
```
接下来我们把public文件夹的内容上传到你刚刚生成的仓库中：

```
$ cd public
$ git init
$ git remote add origin https://github.com/username/username.github.io.git
$ git add -A
$ git commit -m "first commit"
$ git push -u origin master
```
大功告成，让我们在浏览器中试一下`http://username.github.io`（username改成你的Github用户名）


[1]: https://gohugo.io/
[2]: https://github.com/
[3]: http://brew.sh/
[4]: https://gohugo.io/
[5]: https://gohugo.io/
[6]: http://localhost:1313
[7]: https://github.com/
