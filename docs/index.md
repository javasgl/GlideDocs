# Glide: Go的包版本管理工具

[Glide](https://glide.sh) 从概念上来讲是一个 [Go](https://golang.org) 的包管理工具，类似于 Rust 的 Cargo， Node.js 的 NPM，Python 的 Pip，Ruby 的 Boundler 等等...

Glide 提供了以下功能：

* 在 `glide.yaml` 文件中记录依赖信息。包括了名称、版本或版本范围、私有仓库或者类型不能识别时的版本控制信息等等
* 通过`glide.lock`文件来追踪每个包的具体修改，这使得能够重用依赖树。
* 适用于语义版本和语义版本范围。
* 支持 Git、Bzr、HG 和 SVN。和 `go get` 支持的版本控制系统一致。
* 利用 `vendor/`目录使得不同项目可以拥有相同依赖的不同版本。
* 允许使用包别名，这对 `forks` 非常有用。
* 支持从 Godep、GPM、Gom 和 GB 导入配置

## 安装 Glide

有以下几种方式安装 Glide：

1. 使用脚本自动安装。`curl https://glide.sh/get | sh`
2. 下载[发布版本](https://github.com/Masterminds/glide/releases)来安装。Glide 发布语义版本。
3. 使用系统包管理工具来安装Glide。例如，如果你在 Mac 上使用 [Homebrew](http://brew.sh)，则可以使用 `brew install glide` 来安装。
4. 使用`go get`来安装最新的开发快照版本（这不是发布版本）。例如，`go get -u github.com/Masterminds/glide`。
