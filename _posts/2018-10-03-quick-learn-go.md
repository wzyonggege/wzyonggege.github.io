---
layout:     post
title:      "Learn Go in X Minutes"
subtitle:   "A Tour of Go"
date:       2018-10-03 12:00:00
author:     "Jam"
header-img: "img/post-bg-universe.jpg"
tags:
    - Code
    - Golang
---

#### Hello World

<pre>
package main

import "fmt"

func main() {
    fmt.Printf("Hello, world or 你好，世界 or Καλημέρα κόσμε or こんにちは世界\n")
}
</pre>

It prints following information.

<pre>Hello, world or 你好，世界 or Καλημέρα κόσμε or こんにちは世界
</pre>

#### Define variables

<pre>
var variableName type = value
var vname1, vname2, vname3 type = v1, v2, v3

// Define three variables without type "type"
var vname1, vname2, vname3 = v1, v2, v3

vname1, vname2, vname3 := v1, v2, v3

_, b := 34, 35
</pre>

#### Constants

<pre>
const constantName = value
// you can assign type of constants if it's necessary
const Pi float32 = 3.1415926
const Pi = 3.1415926
const i = 10000
const MaxThread = 10
const prefix = "astaxie_"
</pre>