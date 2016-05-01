# Static files

## Static files

Serve a static directory

```go
// first parameter is the request url path (string)
// second parameter is the system directory (string)
// third parameter is the level (int) of stripSlashes
// * stripSlashes = 0, original path: "/foo/bar", result: "/foo/bar"
// * stripSlashes = 1, original path: "/foo/bar", result: "/bar"
// * stripSlashes = 2, original path: "/foo/bar", result: ""

iris.Static("/public", "./static/assets/", 1)
//-> /public/assets/favicon.ico
```

Serve static individual file

```go

iris.Get("/txt", func(ctx *iris.Context) {
	ctx.ServeFile("./myfolder/staticfile.txt")
}

```

Putting all together, serve static individual files dynamically

```go
package main

import (
	"strings"
	"github.com/kataras/iris"
	"github.com/kataras/iris/utils"
)

func main() {

	iris.Get("/*file", func(ctx *iris.Context) {
	  	   requestpath := ctx.Param("file")

			path := strings.Replace(requestpath, "/", utils.PathSeperator, -1)

			if !utils.DirectoryExists(path) {
				ctx.NotFound()
				return
			}

			ctx.ServeFile(path)
	}
}

iris.Listen(":8080")

```