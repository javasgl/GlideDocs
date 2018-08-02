# glide.yaml 文件

`glide.yaml`文件包含了项目和依赖包的信息。这里列出了`glide.yaml`文件中的元素。

    package: github.com/Masterminds/glide
    homepage: https://masterminds.github.io/glide
    license: MIT
    owners:
    - name: Matt Butcher
      email: technosophos@gmail.com
      homepage: http://technosophos.com
    - name: Matt Farina
      email: matt@mattfarina.com
      homepage: https://www.mattfarina.com
    ignore:
    - appengine
    excludeDirs:
    - node_modules
    import:
    - package: gopkg.in/yaml.v2
    - package: github.com/Masterminds/vcs
      version: ^1.2.0
      repo:    git@github.com:Masterminds/vcs
      vcs:     git
    - package: github.com/codegangsta/cli
      version: f89effe81c1ece9c5b0fda359ebd9cf65f169a51
    - package: github.com/Masterminds/semver
      version: ^1.0.0
    testImport:
    - package: github.com/arschles/assert

这些元素是：

- `package`：顶级包是`GOPATH`中的位置。这用于确保不会导入顶级包的内容。
- `homepage`：可以找到有关包或应用程序的详细信息的地方。例如http://k8s.io
- `license`：许可证是 [SPDX](http://spdx.org/licenses/) 许可证字符串或许可证的文件路径。这允许自动化和用户轻松识别许可证。
- `owners`：一个或者多个项目所有者列表。可以是个人或者组织，这对反馈安全问题给所有者很有用。
- `ignore`：需要Glide忽略导入的依赖包列表。它们是包名称而不是包目录。
- `excludeDirs`：本地代码库中的目录列表，用以从扫描依赖关系中排除。
- `import`：导入的依赖包列表。每个依赖包可以包含：
    - `package`：需要导入的包名称，这是唯一的必选项。包名称遵循`go`工具一样的模式。这意味着：
        - 映射到`VCS`远程位置的包名称，以 .git ，.bzr ，.hg 或者 .svn。例如，`example.com/foo/pkg.git/subpkg`。
        - GitHub，BitBucket，Launchpad，IBM Bluemix Services以及Google Source是不需要VCS扩展的特殊情况。
    - `version`：使用的语义版本、语义版本范围，`branch`，`tag`，或者 `commint id`。更多信息请看[版本文档](versions.md)。
    - `repo`：如果包名称不是一个仓库地址，或者是一个私有仓库，可以列在这里。包将从仓库检出，并存放到包名称指定的地方。这允许使用`forks`。
    - `vcs`：版本控制系统，例如 git，hg，bzr 或者 svn。这仅在不能从包名称推测出类型的时候需要。例如,一个以 .git 结尾或者 GitHub 的包可以被检测为 Git。对于 Bitbucket 的仓库，我盟可以调用 API 来检测类型。
    - `subpackages`：在仓库内部使用的包的记录。这并不包括仓库中的所有包，而是直包括正在使用的包。
    - `os`：用于过滤的操作系统列表。设置后它将比较当前运行的操作系统是否和其中一个匹配，只有在匹配的时候才会获取依赖包。不设置则会跳过过滤。名称和 构建参数、GOOS 环境变量中使用的名称相同。
    - `arch`：用户过滤的系统架构列表。设置后它将比较当前运行的系统架构是否和其中一个匹配，只有在匹配的时候才会获取依赖包。不设置则会跳过过滤。名称和 构建参数、GOARCH 环境变量中使用的名称相同。
- `testImport`：在前面`import`中未列出的测试中使用的包列表。每个包具有与`import`下列出的相同的细节。
