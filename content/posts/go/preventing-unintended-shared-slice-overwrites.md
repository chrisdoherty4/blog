---
title: "Preventing unintended overwrites with shared slices"
date: 2020-09-19T09:51:23-06:00
tags: 
    - go
    - programming
categories:
    - Programming
    - Technology
---

## Slices

Go _slices_ are small constructs backed by a traditional array. The slice construct is light weight because it contains only a pointer to it's [first element, a length and a capacity](https://github.com/golang/go/blob/master/src/runtime/slice.go#L13). The slice pointer can point to any element in the backing array.

The length denotes the number of elements currently in the slice and the capactiy denotes the maximum number of elements allowed in the slice. Generally, the capacity of a slice is the size of the underlying array. When we add a new element to a slice that's reached it's capacity a new backing array is created that is typically [twice the size](https://github.com/golang/go/blob/master/src/runtime/slice.go#L146) of the current capactiy.

## Passing slices around

When a slice is passed as a function argument or used as a return value it maintains a reference to the same backing array because the copy of a slice contains copies of the pointer, length and capactiy. Subsequently, changing an element in a slice causes all slices pointing to the same backing array to be updated.


```go
func update(s []string) {
    s[0] = "Banana"
}

func main() {
    originSlice := []string{"Tomato", "Squash", "Lettuce", "Spinach"}
    update(newSlice)

    fmt.Println(originSlice) // "Tomato" has changed to "Banana"
}
```

Similarly, creating a new slice containing a subset of elements with the `newSlice := originSlice[startIndex:endIndexExclusive]` syntax and manipulating the new slice results in data in the `originSlice` being updated. This includes [appending](https://golang.org/pkg/builtin/#append) data to the `newSlice`, that has the same capacity as the `originSlice`.

```go
func main() {
    originSlice := []string{"Tomato", "Squash", "Lettuce", "Spinach"}

    newSlice := originSlice[0:2] // {"Tomato", "Squash"}
    newSlice = append(newSlice, "Banana") // {"Tomato", "Squash", "Banana"}

    // Origin slice inadvertently updated 
    fmt.Println(originSlice) // {"Tomato", "Squash", "Banana", "Spinach"}
}
```

## Protecting against append overwrites

We can protect against `append(...)` specifically by explicitly setting the `newSlice` capacity to the same as the length using `newSlice := originSlice[startIndex:endIndexExclusive:expectedNewSliceLen]` syntax. When we try appending to the `newSlice` that has reached it's capacity Go creates a new backing array with increased capacity, copies the elements in `newSlice` to the new backing array and returns a new slice pointing to the new backing array.

```go
func main() {
    originSlice := []string{"Tomato", "Squash", "Lettuce", "Spinach"}

    // New slice with capacity
    newSlice := originSlice[0:2:2]

    // Len > Capacity resulting in new backing array being created.
    newSlice = append(newSlice, "Banana")

    // Origin slice inadvertently updated 
    fmt.Println(newSlice) // {"Tomato", "Squash", "Banana"}
    fmt.Println(originSlice) // {"Tomato", "Squash", "Lettuce", "Spinach"}
}
```