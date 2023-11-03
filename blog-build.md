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

## 使用 Hexo

在查阅 Hugo 解决公式渲染问题的时候，看到有人提到了 Hexo，于是我就去尝试了一下。[Hexo](https://hexo.io/zh-cn/) 是一个基于 Node.js 的静态博客框架，使用起来非常简单，而且它的插件生态也非常丰富。

```powershell
git clone $THEME_URL $THEME_DIR
cd $THEME_DIR
```
