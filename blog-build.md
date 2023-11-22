---
title: 搭建这个网页的一些记录
date: 2023-11-04 02:16:00
tags:
- 技术
- 网页
- 博客
- Hexo
- GitHub Pages
categories:
- 技术
---

# 搭建这个网页的一些记录

最开始本人想一切从简，就使用了 GitHub Pages 的默认服务，随手选择了一个`jekyll`主题；结果在折腾的过程中发现其存在一些问题，而且关键是主题也都其实不太好看，于是考虑更换主题，先尝试了使用`hugo`。

## 使用 Hugo（一些简单的尝试）

本人使用的是 Windows 系统，先尝试使用 Scoop 安装 Hugo，并且使用 Git Clone 了一个主题。

```powershell
scoop install hugo
git clone $THEME_URL $THEME_DIR
# 本人安装了 Busy box win32
cd $THEME_DIR
rm.exe -rf .\.git\
git init -b main
git add -A
git commit -m "init"
# 初始化之后，运行一下
hugo server --port=11313
```

默认的端口有可能被占用，我们换个端口。这个时候报错，想起来自己还没有安装 Go ，于是使用 `scoop install go`。

然后依然出现问题，我猜测可能是某些依赖没有被编译在 Windows 下的二进制文件中，于是我尝试了使用 WSL2 来安装 Hugo 和 Go，最后成功部署起来了。

本来应该就结束了的工作，但是发现一些问题。由于 Hugo 目前对于 Markdown 的支持有些已知的问题，比如多行公式无法正常渲染。这对于本人目前的需求来说是致命的，于是我又开始寻找其他的解决方案。

> 在此我强调一下，不是说 Hugo 有什么严重的问题，而是我目前的需求不满足。如果你对于 **多行公式** 的需求不算很高，那么 Hugo 是一个非常好的选择。你必须放弃使用`\\` 来换行，而是使用 `\\\\`；还有各种奇奇怪怪的小问题——看得出来 Hugo 使用的 Goldmark 渲染器，根本没有针对于 Katex 做过什么优化，只能说勉强兼容了这个 JavaScript 渲染器。
> 多行公式类似下面这种：
>
> ```latex
> \begin{align}
>    a &= b + c \\
>      &= d + e
> \end{align}
> ```
>
> 或者
> 
> ```latex
> \begin{pmatrix}
>    a & b \\
>    c & d
> \end{pmatrix}
> ```
>

这种非标准的 tex 语法，仅仅为了兼容一个 JavaScript 渲染器，这能忍？换个渲染器！

## 使用 Hexo

在查阅 Hugo 解决公式渲染问题的时候，看到有人提到了 Hexo，于是我就去尝试了一下。[Hexo](https://hexo.io/zh-cn/) 是一个基于 Node.js 的静态博客框架，使用起来非常简单，而且它的插件生态也非常丰富。

由于 Node 我本就安装好了，我只需要安装 Hexo 即可。使用 `npm install hexo-cli -g` 即可安装 Hexo。

```powershell
hexo init $DIR
cd $DIR
npm install
cd themes
```

所以重点是寻找一个足够好看的模板。Hexo 的[模板网站](https://hexo.io/themes/index.html)里面，大多数的模板都太正式、简约，和我想要的风格不太一样。于是我又去 GitHub 上面寻找，最后找到了一个模板[Bamboo](https://github.com/yuang01/hexo-theme-bamboo)。看他的介绍是基于 Vue 做的，但是我大概看了一眼代码感觉可维护性不太好。或许后面我还得自己写一个模板了。

看到好项目，先 Star 一下，然后 Fork 到自己的仓库，然后 Clone 到本地。

```powershell
git clone $THEME_URL $THEME_DIR
cd $THEME_DIR
```

完成之后，修改 `/themes/bamboo/_config.yml` 文件，将 `url` 改为自己的网站地址，并调整一下其他的配置。随后修改根目录下的 `_config.yml` 文件，将 `theme` 改为 `bamboo`。

接下来是一个大工程：配置好 git 仓库。

本人目前的想法是，既然`themes`下面的文件夹实际上是一个 Git 仓库，就不妨使用 Git 的 submodule 功能来管理；顺便把我一开始的老博客全部文件夹移动到对应推文的文件夹下面，一并管理：

```powershell
git init
git add -A
git commit -m "init"
git submodule add $THEME_URL $THEME_DIR
git submodule add $BLOG_URL $BLOG_DIR
git commit
git remote add origin $REPO_URL
git push --set-upstream origin main
```

这样就完成了一个基本的配置，接下来就是写文章了。

首先我们发现了 Hugo 默认就带了一个样例文件，而这个给我第一次commit的时候提交了。现在我们需要删除它：

```powershell
git rm -r --cached $BLOG_DIR/source/_posts/hello-world.md
git add -A
git commit -m "remove default files"
git push
```

然后去 `$BLOG_DIR/source/_posts/`，把样例文件提交到这个仓库里面，这样就不影响了。

随后，我修改了一下样例文件，发现这个博客模板没有支持 Mermaid 语法，需要手动安装：

```powershell
npm install --save hexo-filter-mermaid-diagrams
```

然后发现，这个模板也不支持公式，需要手动安装：

```powershell
npm remove hexo-renderer-marked
npm install --save hexo-renderer-markdown-it-katex
```

这样就基本完成了……

等下，这个版本的 Katex 太老了，好多新特性无法使用，需要升级。但是因为 Katex 是 `hexo-renderer-markdown-it-katex` 的依赖的依赖`@abreto/markdown-it-katex`的依赖，这个想改好麻烦。好在 Katex 的工作原理我还是相对熟悉，所以我做了个大胆的尝试：直接修改`node_modules`下面的文件。

先安装Katex的最新版本：

```powershell
npm install --save katex
```

然后把`node_modules/@abreto/markdown-it-katex/node_modules/katex/`直接删了，换成`node_modules/katex/`。

```bash
rm -rf node_modules/@abreto/markdown-it-katex/node_modules/katex/
cp -r node_modules/katex/ node_modules/@abreto/markdown-it-katex/node_modules/
```

然后本地测试一下，发现居然就完成了。

随后需要给每个推文加个头，标注一下时间、标签、分类等等。有点麻烦，所以现在还没干完。

## 部署 GitHub Pages

首先把项目推到了`szw0407.github.io`这个仓库里面，然后在 GitHub Pages 里面选择了对应的分支，选择使用Action来部署。

部署的时候，将`npm install`命令后面，加上了`&& rm -rf node_modules/@abreto/markdown-it-katex/node_modules/katex/ && cp -r node_modules/katex/ node_modules/@abreto/markdown-it-katex/node_modules/`，这样就可以在部署的时候自动更新 Katex 的版本了。

目前暂时还没遇到什么问题，不过好像，这个网站已经给我搞成了屎山！
