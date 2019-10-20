---
layout:     post
title:      Concurrency
subtitle:   "Concurrent Computing Lecture 02"
date:       2019-10-20
author:     "Elvis"
catalog:    true
comments:   true
tags:
    - Cuncurrency
	- Go
---

# Concurrency



A goroutine is a green thread.



### Goroutine

It is an application-level thread.

It is much more lightweight than an OS-thread.

Many goroutines can run on one OS-thread.



### Go Concurrent Execution: `go` keyword (Hello World 2.0)

```go
package main

import (
  "fmt"
  "time"
)

func say(some string) {
  fmt.Println(something)
}

func main() {
  go say("Hello " + "World")
  time.Sleep(1 * time.Second)
}
```



### Goroutine

- A groutine is a green thread.
  - Scheduled by a runtime library or VM instead of natively by the host OS.
- It is an application-level thread.
- It is much more lightweight than an OS-thread.
- Many groutines can run on one OS-thread.



### Goroutines continued

- It an independently executing function, launched by a `go` statement.
- It not thread.
- There might be only one thread in program with thousands of goroutines.
- Instead, goroutines are multiplexed dynamically onto threads as needed to keep all the goroutines running.
- But if you think of it as very cheap thread, you won't be far off.



### Memory

- Goroutines run in the same address space,
  - So access to shared memory must be synchronized.
- However, ususally we can avoid sharing memory at all!



### Channels

Channel provides a way to send a message between two goroutines.

```go
channel := make(chan int)
```

Send `someInt` to the channel

```go
v := <-channel
```

Receive from channel and assign the received value to v.



### Parallel Sum

```go
func sum(s []int, c chan int) {
  sum :=0
  for _,v := range s {
    sum += v
  }
  c <- sum // 把 sum 传到 channel c 里
}

func main() {
  s := []int{7, }
}
```

