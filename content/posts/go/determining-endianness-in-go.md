---
title: "Determining Endianness in Go"
date: 2020-09-26T17:05:08-06:00

tags:
    - go
    - endianness

categories:
    - Programming

draft: true
---

In most languages you can do some variation of the same algorithm to determine endianness. The algorithm generally involves writing `0x0100` to a 16-bit integer and checking if the `0x01` or `0x00` byte was written at the first address.

```go
func Endian() string {
    var i int = 0x0100

    ptr := unsafe.Pointer(&i)

	if 0x01 == *(*byte)(ptr) {
		return "Big Endian"
	} else if 0x00 == *(*byte)(ptr) {
		return "Little Endian"
	} else {
		return "Unknown"
	}
}
```
