---
title: "Integer Conversion in Go"
date: 2020-09-26T14:56:58-06:00

tags:
    - programming
    - go

categories:
    - TIL
    - Programming
---

Go's integer type conversion behaves differently dependent on whether you're declaring a variable and assigning a value to it or applying a type conversion to existing data.

Declarig a variable and assigning a value outside the representable bounds of the variable type results in an error.

```go
// Fails because int8s bounds are -128 to 127
var i int8 = 128
```

Converting a data to a type with less bits resuts in data with the most significant bits being dropped.

```go
var i int16 = 0x0100

j := int8(i) // 0x00 as the most significant bits in i are dropped.
```