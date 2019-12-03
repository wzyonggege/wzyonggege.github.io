---
layout:     post
title:      "Graceful Restart in Golang"
subtitle:   "优雅的重启"
date:       2019-09-20 12:00:00
author:     "Jam"
header-img: "img/in-post/post-golang-bg.png"
tags:
    - Code
    - Golang
---

# 主程序的关闭重启

### Bilibili 案例

```
func main() {
    // ... 
    signalHandler()
}

func signalHandler() {
	var (
		ch = make(chan os.Signal, 1)
	)
	signal.Notify(ch, syscall.SIGHUP, syscall.SIGQUIT, syscall.SIGTERM, syscall.SIGINT)
	for {
		si := <-ch
		log.Info("get a signal %s, stop the server", si.String())
		switch si {
		case syscall.SIGQUIT, syscall.SIGTERM, syscall.SIGINT:
			s.Close()
			time.Sleep(time.Second)
			return
		case syscall.SIGHUP:
		default:
			return
		}
	}
}
```