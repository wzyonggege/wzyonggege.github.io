---
layout:     post
title:      "Go搭建HTTP服务器 "
subtitle:   "In this example you will learn how to create a basic HTTP server in Go. First, let’s talk about what our HTTP server should be capable of. A basic HTTP server has a few key jobs to take care of."
date:       2019-03-05 12:00:00
author:     "Jam"
header-img: "img/post-golang-bg.jpg"
tags:
    - Code
    - Golang
---

# 创建 http 服务端

## 服务端

<pre>
package main

import (
	"fmt"
	"net/http"
)

func sayHello(w http.ResponseWriter, r *http.Request)  {
	fmt.Println(r.URL.Path)
	w.Write([]byte("Hello"))
}

func main() {
	http.HandleFunc("/", sayHello)
	http.ListenAndServe(":8080", nil)
}
</pre>

- HandleFunc 将 handle function 注册给一个pattern（地址）
    - ServeMux，http 请求的多路复用器
    - 将请求的URL和已注册的所有pattern匹配
    - 因此，根目录 "/" 不仅仅能匹配以 "/" 为路径的 URL，当没有其他能匹配的 pattern 时，也会匹配到根目录。
    - 实现简单 基于字典查询 
- ListenAndServe 监听端口
    - 第一个参数是端口号
    - 第二个参数是 multiplexer，nil为默认
    - 核心代码 ln, err := net.Listen("tcp", addr)
    - 处理新链接 func (srv *Server) Serve(l net.Listener)， 开启一个for（while）每个连接建立一个goroutine，实现高并发
        - 选择 multiplexer，如果是 nil 则使用 DefaultServeMux
        - 使用 multiplexer 找到请求 uri 所对应的 handler
        - 写入 ResponseWriter 并回复给客户端