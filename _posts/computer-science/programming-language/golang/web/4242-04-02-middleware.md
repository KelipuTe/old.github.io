---
title: "使用 Go 实现 Web 框架 -- 中间件"
date: 2022-09-15 08:00:00 +0800
tags: computer-science 计算机科学 programming-language 编程语言 golang web-framework 框架 middleware 中间件
comment: false
show_author_profile: true
show_subscribe: false
---

> go version go1.19

### 代码

- demo-golang/demo/web/router/...

### 中间件

中间件有两个重要的组成部分：路由的处理方法和中间件的处理方法

```go
type HTTPHandleFunc func(p7ctx *HTTPContext)
```

```go
type HTTPMiddleware func(next HTTPHandleFunc) HTTPHandleFunc
```

中间件

```go
func RecoveryMiddleware() HTTPMiddleware {
	return func(next HTTPHandleFunc) HTTPHandleFunc {
		return func(p7ctx *HTTPContext) {
			// before
			next(p7ctx)
			// after
		}
	}
}
```

假设有两个中间件

```go
func AMiddleware() HTTPMiddleware {
	return func(next HTTPHandleFunc) HTTPHandleFunc {
		return func(p7ctx *HTTPContext) {
			next(p7ctx)
		}
	}
}

func BMiddleware() HTTPMiddleware {
	return func(next HTTPHandleFunc) HTTPHandleFunc {
		return func(p7ctx *HTTPContext) {
			next(p7ctx)
		}
	}
}
```

```

```


