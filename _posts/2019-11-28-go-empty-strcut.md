---
layout:     post
title:      "GO empty struct{}"
subtitle:   "<- struct{}{}"
date:       2019-11-28 12:30:00
author:     "Jam"
header-img: "img/post-bg-universe.jpg"
tags:
    - Code
    - Golang
---

# GO empty struct{}

- 空struct 节省内存

```
a := struct {}{}
fmt.Println(unsafe.Sizeof(a))
// 0
```

- channel struct{}

```
func main()  {
	done := make(chan struct{})

	go func() {
		fmt.Println("hello 111")
		done <- struct{}{}
	}()

	<- done
}

```

与channel一起使用，可做信号通知管道，其本身无实际价值，空struct{} 对内存更加友好，空struct类数据内存申请部分，返回都是固定地址，避免滥用。

```
type notify struct{}

func main()  {
	done := make(chan notify)
	go func() {
		fmt.Println(“hello 111”)
		done <- notify{}
	}()

	<- done
}
```