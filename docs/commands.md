# 命令

以下是 Glide 的命令，大部分是帮助管理工作区的。

## glide create (别名：glide init)

初始化新的工作区。除此之外，它会在尝试猜测要放入的包和版本时创建一个`glide.yaml`文件。例如，如果你的项目使用 Godep，它将其使用指定的版本。Glide 足够聪明地扫描代码库并检测正在使用的导入，无论它们是否由另一个程序包管理器指定。

    $ glide create
    [INFO]	Generating a YAML configuration file and guessing the dependencies
    [INFO]	Attempting to import from other package managers (use --skip-import to skip)
    [INFO]	Scanning code to look for dependencies
    [INFO]	--> Found reference to github.com/Masterminds/semver
    [INFO]	--> Found reference to github.com/Masterminds/vcs
    [INFO]	--> Found reference to github.com/codegangsta/cli
    [INFO]	--> Found reference to gopkg.in/yaml.v2
    [INFO]	Writing configuration file (glide.yaml)
    [INFO]	Would you like Glide to help you find ways to improve your glide.yaml configuration?
    [INFO]	If you want to revisit this step you can use the config-wizard command at any time.
    [INFO]	Yes (Y) or No (N)?
    n
    [INFO]	You can now edit the glide.yaml file. Consider:
    [INFO]	--> Using versions and ranges. See https://glide.sh/docs/versions/
    [INFO]	--> Adding additional metadata. See https://glide.sh/docs/glide.yaml/
    [INFO]	--> Running the config-wizard command to improve the versions in your configuration

这里提到的配置向导(`config-wizard`)可以在这里运行，也可以在以后手动运行。此向导可帮助找出可用于依赖关系的版本和范围

### glide config-wizard

这将运行一个向导，扫描依赖关系并检索其上的信息，以提供可交互选择的建议。例如，它可以发现依赖关系是否使用语义版本，并帮助你选择要使用的版本范围。

## glide get [package name]

你可以将一个或多个软件包下载到你的`vendor/`目录，并将其添加到你的`glide.yaml`文件中。

    $ glide get github.com/Masterminds/cookoo

当使用`glide get`时，它会检测列出的包来解决它的依赖关系，包括使用 Godep，GPM，Gom 和GB 的配置文件。

`glide get`命令可以给包名传递一个[版本或范围](versions.md)。例如，

    $ glide get github.com/Masterminds/cookoo#^1.2.3

包名和版本通过一个`#`来分隔。如果没有指定版本或范围，并且依赖关系使用语义版本，那么 Glide 将询问是否要使用它们。

## glide update (别名： glide up)

下载或更新`glide.yaml`文件中列出的所有库，并将它们放在`vendor/`目录中。它还将递归地遍历依赖包以获取所需的任何配置文件。

    $ glide up

这将会覆盖由Glide，Godep，gb，gom和GPM管理的其他项目。当找到这些包将根据需要安装。

将使用固定到特定版本的依赖关系创建或更新`glide.lock`文件。例如，如果在`glide.yaml`文件中将版本指定为范围（例如，^ 1.2.3），则`glide.lock`文件中将其设置为特定的`commit id`。这允许可重复的安装（参见 `glide install`）。


从获取的包中移除任何嵌套的`vendor/`目录请看`-v`参数。

## glide install

当你想从`glide.lock`文件安装特定版本的包时使用`glide install`。

    $ glide install

这将会读取`glide.lock`文件，如果它没有绑定到`glide.yaml`文件则会发出警告，并安装特定`commit id`的版本。

当`glide.lock`文件没有绑定到`glide.yaml`文件，例如有更改，它将提示警告。运行`glide up`将在更新依赖关系树时重新创建`glide.lock`文件。

如果没有`glide.lock`文件，`glide install`将执行`update`并生成一个锁文件。

从获取的包中移除任何嵌套的`vendor/`目录请看`-v`参数。

## glide novendor (别名： glide nv)

当您运行`go test ./..`命令时，它将遍历包括`vendor/`目录在内的所有子目录。当测试应用程序时，你可能只需要测试应用程序文件，而无需运行依赖包及其依赖包的所有测试。这时`novendor`命令派上了用场，它列出了除`vendor/`之外的所有目录。

    $ go test $(glide novendor)

这将会在你项目除`vendor`外所有目录中运行运行`go test`。

## glide name

当您使用 Glide 进行脚本编写时，您需要知道您正在处理的包的名称。 `glide name`返回`glide.yaml`文件中列出的的名称。

## glide list

Glide的`list`命令按字母顺序显示项目导入的所有包。

    $ glide list
    INSTALLED packages:
    	vendor/github.com/Masterminds/cookoo
    	vendor/github.com/Masterminds/cookoo/fmt
    	vendor/github.com/Masterminds/cookoo/io
    	vendor/github.com/Masterminds/cookoo/web
    	vendor/github.com/Masterminds/semver
    	vendor/github.com/Masterminds/vcs
    	vendor/github.com/codegangsta/cli
    	vendor/gopkg.in/yaml.v2

## glide help

打印 glide 帮助信息。

    $ glide help

## glide --version

打印 glide 版本并退出。

    $ glide --version
    glide version 0.12.0

## glide mirror

镜像提供了将一个仓库替换为其镜像仓库的能力。当您希望为连续集成（CI）系统设置缓存或者想要在本地依赖包上工作时，这很有用。

镜像存放在你`GLIDE_HOME`中的一个`mirrors.yaml`文件中。

管理镜像有三个命令：`list`、`set` 和 `remove`。

使用`set`：

    glide mirror set [original] [replacement]

或者：

    glide mirror set [original] [replacement] --vcs [type]

例如,

    glide mirror set https://github.com/example/foo https://git.example.com/example/foo.git

或

    glide mirror set https://github.com/example/foo file:///path/to/local/repo --vcs git

使用`remove`：

    glide mirror remove [original]

例如,

    glide mirror remove https://github.com/example/foo
