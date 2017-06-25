# 常见问题

## Q: 为什么 Glide 有子包的概念而 Go 没有?

Go 中每个目录都是一个包。这在你有一个包含了所有包的仓库时工作正常。当你有不同的包在不同的 VCS 时事情就有一点复杂了。包含包的集合的项目应使用与版本相同的信息进行处理。通过以这种方式对包进行分组，我们可以管理相关信息。

## Q: bzr (或者 hg) 没有按照我的预期工作。为什么？

这些是正在进行的工程，可能需要一些额外的调整。请看看[vcs包](https://github.com/masterminds/vcs)。如果您看到更好的处理方式，请告诉我们。

## Q: 我应该让`vendor/`进版本控制么？

这看你。这是个人或组织的决定。Glide 将帮助你根据需要安装外部依赖包，或帮你管理已进入你的版本控制系统的依赖包。

By default, commands such as `glide update` and `glide install` install on-demand. To manage a vendor folder that's checked into version control use the flags:
默认情况下，`glide update`和`glide install`等命令将按需安装。要管理进入到版本控制的`vendor/`文件夹，请使用标志：

* `--update-vendored` (别名：`-u`) 更新 `vendor`依赖.
* `--strip-vcs` (别名： `-s`) 从`vendor`中删除 VCS 元数据 (例如 `.git` 目录) 。
* `--strip-vendor` (别名：`-v`) 删除嵌套的`vendor/`目录。

## Q: 怎么从 GPM, Godep, Gom, or GB 导入配置？

导入有两步。

1. 如果你导入的软件包具有 GPM ，Godep，Gom 或 GB 配置，Glide将自动递归地安装依赖包。
2. 如果要从 GPM ，Godep，Gom 或 GB 导入配置到 Glide，请参阅`glide import`命令。例如，您可以运行Glide的`glide import godep`来检测项目`Godep`配置并为您生成`glide.yaml`文件。

它们中的每一个都会将您现有的`glide.yaml`文件与它为这些管理器找到的依赖关系进行合并，然后将该文件输出出来。**它不会覆盖你的`glide.yaml`文件。**

你可以这样将它写入文件：

    $ glide import godep -f glide.yaml


## Q: Glide 能够按照操作系统或架构来获取一个包么？

可以的。使用`os`和`arch`字段，可以指定应该为哪个操作系统和体系架构提取包。例如，以下软件包将只能获取64位的 Darwin / OSX 系统：

    - package: some/package
      os:
        - darwin
      arch:
        - amd64

其他体系架构或操作系统的将不会被获取。

## Q: Glide 是如何得名的？

除了好读，"glide" 是"Go Elide"的缩写。这个想法是将通常需要大量时间的任务压缩到短短的几秒钟内。
