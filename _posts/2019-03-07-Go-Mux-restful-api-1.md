---
layout:     post
title:      "Making a RESTful JSON API in Go with Mux"
subtitle:   ""
date:       2019-03-07po 12:00:00
author:     "Jam"
header-img: "img/in-post/post-golang-bg.png"
tags:
    - Code
    - Golang
    - Mux
    - RESTful API
---

# Making a RESTful JSON API in Go with Mux

## Making a RESTful JSON API in Go

> 原文 https://thenewstack.io/make-a-restful-json-api-go/

### A Basic Web Server

```go
package main

import (
	"net/http"

	"github.com/gorilla/mux"
)

func main() {
	r := mux.NewRouter().StrictSlash(true)

	r.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
		w.Write([]byte("Hello"))
	})

	http.ListenAndServe(":8080", nil)

}
```

> //与http.ServerMux不同的是mux.Router是完全的正则匹配,设置路由路径/index/，如果访问路径/idenx/hello会返回404
> //设置路由路径为/index/访问路径/index也是会报404的,需要设置r.StrictSlash(true), /index/与/index才能匹配


### Adding a Router

路径又称"终点"（endpoint），表示API的具体网址。在RESTful架构中，每个网址代表一种资源（resource）。 第三方组件（Gorilla Mux package）： “github.com/gorilla/mux”

```go
package main

import (
	"net/http"

	"github.com/gorilla/mux"
)

func main() {
	r := mux.NewRouter().StrictSlash(true)

	r.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
		w.Write([]byte("Hello"))
	})
	r.HandleFunc("/todos", func(w http.ResponseWriter, r *http.Request) {
		w.Write([]byte("todo"))
	})
	r.HandleFunc("/todos/{todoId}", func(w http.ResponseWriter, r *http.Request) {
		vars := mux.Vars(r)
		todoId := vars["todoId"]
		w.Write([]byte("todo id:" + todoId))
	})

	http.ListenAndServe(":8080", r)
}
```

### 数据模型

```go
type Route struct {
	Name        string
	Method      string
	Pattern     string
	HandlerFunc http.HandlerFunc
}

type Routes []Route
```

### 重构

- routes.go

```go
package main

import "net/http"

type Route struct {
	Name        string
	Method      string
	Pattern     string
	HandlerFunc http.HandlerFunc
}

type Routes []Route

var routes = Routes{
	Route{
		"Index",
		"GET",
		"/",
		IndexHandler,
	},
	{
		"todoIndex",
		"GET",
		"/todos",
		TodoHandler,
	},
	{
		"todoDetailIndex",
		"GET",
		"/todos/{todoId}",
		TodoDetailHandler,
	},
}
```

- router.go

```go
package main

import (
	"github.com/gorilla/mux"
)

func NewRouter() *mux.Router {
	router := mux.NewRouter().StrictSlash(true)
	for _, route := range routes {
		router.
			Methods(route.Method).
			Path(route.Pattern).
			Name(route.Name).
			Handler(route.HandlerFunc)
	}
	return router
}
```

- handler.go

```go
package main

import (
	"net/http"

	"github.com/gorilla/mux"
)

func IndexHandler(w http.ResponseWriter, r *http.Request) {
	w.Write([]byte("Hello"))
}

func TodoHandler(w http.ResponseWriter, r *http.Request) {
	w.Write([]byte("todo"))
}

func TodoDetailHandler(w http.ResponseWriter, r *http.Request) {
	vars := mux.Vars(r)
	todoId := vars["todoId"]
	w.Write([]byte("todo id:" + todoId))
}
```

- main.go

```go
package main

import (
	"log"
	"net/http"
)

func main() {
	router := NewRouter()
	log.Fatal(http.ListenAndServe(":8080", router))
}

```

### Applying the Logger Decorator

- router.go

```go
func NewRouter() *mux.Router {
 
    router := mux.NewRouter().StrictSlash(true)
    for _, route := range routes {
        var handler http.Handler
 
        handler = route.HandlerFunc
        handler = Logger(handler, route.Name)
 
        router.
            Methods(route.Method).
            Path(route.Pattern).
            Name(route.Name).
            Handler(handler)
    }
 
    return router
}
```

- logger.go

```go
package main

import (
	"log"
	"net/http"
	"time"
)

func Logger(inner http.Handler, name string) http.Handler {
	return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
		start := time.Now()

		inner.ServeHTTP(w, r)

		log.Printf(
			"%s\t%s\t%s\t%s",
			r.Method,
			r.RequestURI,
			name,
			time.Since(start),
		)
	})
}
```

可以看到

![img](/img/in-post/post-go-mux-logger.jpg)