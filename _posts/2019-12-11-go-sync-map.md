---
layout:     post
title:      "线程安全的map"
subtitle:   "go map"
date:       2019-12-11 12:00:00
author:     "Jam"
header-img: "img/in-post/post-golang-bg.png"
tags:
    - Code
    - Golang
---

> Maps are not safe for concurrent use: it's not defined what happens when you read and write to them simultaneously. If you need to read from and write to a map from concurrently executing goroutines, the accesses must be mediated by some kind of synchronization mechanism. One common way to protect maps is with sync.RWMutex.


```
package main

import "sync"

type SafeMap struct {
    sync.RWMutex
    Map map[int]int
}

func newSafeMap(size int) *SafeMap {
    sm := new(SafeMap)
    sm.Map = make(map[int]int)
    return sm

}

func (sm *SafeMap) get(key int) int {
    sm.RLock()
    value := sm.Map[key]
    sm.RUnlock()
    return value
}

func (sm *SafeMap) set(key int, value int) {
    sm.Lock()
    defer sm.Unlock()
    sm.Map[key] = value
}
```