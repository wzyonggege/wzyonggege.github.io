---
layout:     post
title:      "Golang Signal"
subtitle:   "信号处理"
date:       2019-09-20 12:00:00
author:     "Jam"
header-img: "img/in-post/post-golang-bg.png"
tags:
    - Code
    - Golang
---

# Golang Signal

> 信号(Signal)是Linux, 类Unix和其它POSIX兼容的操作系统中用来进程间通讯的一种方式。
> 
> 一个信号就是一个异步的通知，发送给某个进程，或者同进程的某个线程，告诉它们某个事件发生了。
>
> 当信号发送到某个进程中时，操作系统会中断该进程的正常流程，并进入相应的信号处理函数执行操作，完成后再回到中断的地方继续执行。

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

Go信号通知机制先是需要一个os.Signal 的channel，通过往channel 发送signal实现，signal.Notify 注册需要接受的信号。例如停止运行的ctrl-C就是发送了SIGINT信号。

> Ctrl-C 发送 INT signal (SIGINT)，通常导致进程结束
>
> Ctrl-Z 发送 TSTP signal (SIGTSTP); 通常导致进程挂起(suspend)
>
> Ctrl-\ 发送 QUIT signal (SIGQUIT); 通常导致进程结束 和 dump core.
>
> Ctrl-T (不是所有的UNIX都支持) 发送INFO signal (SIGINFO); 导致操作系统显示此运行命令的信息
>
> SIGHUP 终端控制进程结束(终端连接断开)
>
> SIGTERM 结束程序(可以被捕获、阻塞或忽略)