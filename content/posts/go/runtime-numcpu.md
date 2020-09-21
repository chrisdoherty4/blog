---
title: "runtime.NumCPU"
date: 2020-05-22T20:52:59-06:00

tags:
    - go

categories:
    - Programming
    - Technology
---

A number of books and articles I've read recently claim that [`runtime.NumCPU()`](https://golang.org/pkg/runtime/#NumCPU) returns the physical processor count. This isn't accurate: `runtime.NumCPU()` has always returned the logical processor count. Perhaps the authors use the terms physical and logical interchangably given most/all mainstream processors have simaltaneous multithreading technology?

```go
package main

import (
    "runtime"
    "fmt"
)

func main() {
    fmt.Println(runtime.NumCPU())
}
```