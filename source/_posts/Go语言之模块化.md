---
title: Go语言之模块化
categories: Go
tags:
	- Go
	- Module
date: 2022-03-13
---

最近在学习 Go，看了一个视频，讲模块化的，但是我重现不了视频的操作，经过折腾之后终于明白了 Go 的模块化，现在记录下来。

## 建立模块

Go 是以模块来组织的代码的，通过模块之间的合作，来组织一个大型的程序，比如标准库中的 `fmt` `time` 等就是一个个的模块，我们利用这些模块来实现自己的功能。

我们也可以写一个自己的模块，新建一个文件夹 `math`，然后在其中新建一个 `go.mod` 文件，就有一个自己的模块了

```
math
└── go.mod

0 directories, 1 file
```

我们为 `go.mod` 添加一点内容

```
module math

go 1.17
```

这个文件指定了两个内容：

- `module math`：当别人引入你的模块时应该如何引入，这里我们指定我们的模块名为 `math`，所以别人引入我们的模块时应该通过 `import "math"` 来引入
- `go 1.17`：指明了我们使用的 go 的版本

新建 `utils`文件夹，并且在 `math/utils` 下面新建一个 `sqrt.go` 的文件，它属于 `utils` 包，注意包名可以与目录名不一致，但是我们一般保持一致。`sqrt.go` 中包含一个 `Sqrt` 方法，它可以对一个浮点数求平方根。

```go
package utils

func Sqrt(x float64) float64 {
	z := 1.0
	for i := 0; i < 10; i++ {
		z -= (z*z - x) / (2 * z)
	}
	return z
}
```

现在我们想在项目中引用这个模块，如何引用？

分为两种情况：

- 模块内引用
- 模块外引用

## 模块内引用

新建 `math/main` 文件夹，并新建 `math/main/main.go`，我们希望在 `main.go` 中使用 `math/utils/sqrt.go` 中 `Sqrt` 方法

```go
package main

import (
	"math/utils"
	"fmt"
)

func main() {
	x := 5.0
	fmt.Println(utils.Sqrt(x))
}
```

通过  `import "math/utils"` 就可以使用 `math/utils` 包中暴露出去的方法了（以大写开头的方法）。

## 模块外引用

如果我们在另一个模块中想引入 `math` 这个模块应该怎么办，新建另一个文件夹 `another-modules`，并新建 `another-modules/go.mod`，内容如下

```
module another-modules

go 1.17
```

新建文件 `main.go`

```go
package main

import "math/utils"

func main() {
	x := 5.0
	println(utils.Sqrt(x))
}
```

如果你尝试去运行这个程序的化，它会有如下输出

```powershell
# go run main.go
main.go:3:8: package math/utils is not in GOROOT (C:\Program Files\Go\src\math\utils)
```

它提示无法找到 `package math/utils`，这是因为它会试图在 `GOROOT` 这个环境变量指定的路径去寻找，因为我们的 `math` 模块不在 `GOROOT` 指定的目录中，所以它当然找不到，我们标准库如 `fmt` `time` 就在 `GOROOT/src` 文件夹中。

为了使用 `math` 模块，我们需要修改 `another-modules.go.mod`

```go
module another-modules

go 1.17

require "math" v1.0.0
replace "math" => "../math"
```

我们添加了两行代码，`require "math" v1.0.0` 表示 `another-modules` 模块依赖于 `math` 模块，`replace "math" => "../math"` 表示 `math` 模块你得去 `"../math"` 这个路径查找，因为 `math` 模块与 `another-modules` 模块在同一个路径中，所以得去上级目录寻找 `math` 模块。

再次运行 `another-modules/main.go`，就可以得到正确的输出了

```powershell
# go run main.go
+2.236068e+000
```

## 引用第三方模块

上面我们使用的都是本地的模块，我们看看如何引用第三方模块，在引用模块之前需要先下载，`Go` 提供了 `go get` 来下载第三方的模块，我们可以从 `Github` 上下载第三方模块

```powershell
go get "github.com/orcaman/concurrent-map"
```

第三方模块会被缓存到 `GOPATH/pkg/mod` 文件夹中，当我们引用第三方模块时，会从这个文件夹中导入。

> 当使用 `go get` 下载模块时会出现下载问题，可以修改 `GOPROXY` 来解决，在 `PowerShell` 中输入如下命令
>
> ```powershell
> $env:GOPROXY = "https://goproxy.io"
> ```
> 

当我们在模块中运行 `go get "github.com/orcaman/concurrent-map"`，会发现 `go.mod` 中会添加这么一行文本

```
require github.com/orcaman/concurrent-map v1.0.0 // indirect
```

表示我们的项目依赖于下载的第三方模块，将依赖写入 `go.mod` 的好处就是当别人拿到我们的项目时，可以通过 `go mod download` 下载这个项目所依赖的所有模块。
