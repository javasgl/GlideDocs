# Glide 插件

Glide 支持一个类似于Git的简单的插件系统。

## Existing Plugins
## 已有的插件

Glide 已经包含一些插件：

* [glide-vc](https://github.com/sgotti/glide-vc) - 构建应用时允许你跳过`vendor/`目录中你不需要的文件。
* [glide-brew](https://github.com/heewa/glide-brew) - 帮助你转换通过Go deps管理的项目到`Homebrew`，便于为你的 Go 程序构建 brew 版本。
* [glide-hash](https://github.com/mattfarina/glide-hash) - 生成与Glides内部哈希兼容的`glide.yaml`文件的哈希值。
* [glide-cleanup](https://github.com/ngdinhtoan/glide-cleanup) - 从`glide.yaml` 文件中移除没有使用的包。
* [glide-pin](https://github.com/multiplay/glide-pin) - 从`glide.lock`文件中取出所有的依赖包，并把它们和`glide.yaml`中正确的对应起来。

_注意，往这个列表添加插件请提交一个`pull request`。_

## 插件如何工作

当 Glide 遇到一个未知的子命令时，会根据以下规则尝试将其委​​托给另一个可执行文件。

例子：

```
$ glide install # 已知的命令，所以直接执行它 
$ glide foo     # 未知的命令，所以会寻找一个合适的插件
```

In the example above, when glide receives the command `foo`, which it does not know, it will do the following:
上面的例子中，当 glide 接收到不认识的命令`foo`，它将执行以下操作：

1. 将`foo`变为`glide-foo`
2. 在系统的`$PATH`中寻找`glide-foo`，如果找到则执行它。
3. 否则在当前项目的根目录下寻找`glide-foo`。(也就是说，在与`glide.yaml`相同的目录中)。如果找到则执行它。
4. 如果没有找到适当的命令，则错误退出。

## 编写 Glide 插件

Glide 插件可以用任何你想要的语言编写，只要它可以作为Glide的子进程从命令行执行。Glide包含的示例是一个简单的Bash脚本。我们可以很容易地用 Go，Python，Perl，甚至是Java（带包装器）来编写。

Glide插件必须位于两个位置之一：

1. `PATH`路径中
2. 和`glide.yaml`相同的目录 

建议系统级别的插件放在`/usr/local/bin`或者`$GOPATH/bin`中，而项目特定的插件放在和`glide.yaml`所在的目录里。

### 参数和标志

说 Glide 是这样执行的：

```
$ glide foo -name=Matt myfile.txt
```

Glide会将此解释为使用`-name = Matt myfile.txt`参数来执行`glide-foo`的请求。它不会试图解析这些参数或以任何方式修改它们

假设，如果 Glide 有自己的`-x`标志，你可以这样调用：

```
$ glide -x foo -name=Matt myfile.txt
```

这种情况下，glide 将会解释并私吞`-x`，并将其余部分传递给`glide-foo`，如上例所示。

## 插件例子

文件： glide-foo

```bash
#!/bin/bash

echo "Hello"
```
