---
title: "03 - More on the basis of the Go language."
description: "We have to known more about Pointer, Function, Goroutine, Slice, Map and Structure."
date: 2026-10-15
lang: en
category: learn
tags: ["go", "learn-go"]
cover: go
---

Before creating the next game, we need to explore a bit about all these base features.

## Slice

A "slice" is like an array in other languages, they can be resized and passed to functions.

```go
s := []int{1, 2, 3, 4}
fmt.Println(s)
fmt.Printf("display %d\n", s[0]) // start at 0

s = append(s, 5) // add 5 to the end
fmt.Printf("display %d\n", s[4])
fmt.Println(len(s)) // display the length 5
fmt.Println(s[0:2]) // display 2 first elements 1, 2
```

## Map

A "map" is more commonly as an associative array; you may use it like `m["total_life"] = 9999`.

```go
m := map[string]int{
    "one": 1, "two": 2,
    "three": 3, "four": 4,
}
fmt.Println(m) // display the whole map
fmt.Printf("display %d\n", m["one"])

delete(m, "four") // remove four

// Iterating over map, the output is unordered, this use goroutine...
for key, v := range m {
    fmt.Println("key:", key, "value:", v)
}

clear(m) // clear the whole map
```

## Pointers

Pointers are important in compiled language, and become even more important in big programs. The syntax can be a bit "weird" if you don't know C, C++, Java, Rust, etc

```go
count := int(42) // declare a simple variable
ptr := &count // pointer linked to count, use & to record the memory address

fmt.Printf("Displaying count %d\n", count)
fmt.Printf("count is stored at the address %v\n", &count) // the & displays the memory address

fmt.Printf("pointer ptr shows the same address %v\n", ptr) // already declared with &
fmt.Printf("pointer ptr shows the value of count %d\n", *ptr) // use * to show content of the address
```

A variables may be updated with pointers.

```go
*ptr = 4200 // Change value of "count" with the pointer ptr
fmt.Printf("Value of count was change with pointer %d\n", count)
fmt.Printf("to %d\n", *ptr)

count = 1111                // Change value
fmt.Printf("value of count was change %d\n", count)
fmt.Printf("to %d\n", *ptr) // The pointer follow the change
```

## Structure

You can define "structure" to create your own type

```go
type Person struct {
	Name string
	Age  int
}

// Function with weird syntax
func (p *Person) SayHello() {
	fmt.Println("Hello", p.Name)
}

func main() {
	// use pointer with struct
	girl := new(Person)
	girl.Name = "Alice"
	girl.SayHello()
}
```

And the last important feature we need to see is "goroutine".

## Goroutine

A goroutine in Go is a lightweight thread of execution. To create a goroutine, you need to use the `go` keyword before the function to execute. When Go sees a goroutine, it skips to the next step, and the thread is executed in the background.

```go
count := 15

fmt.Printf("Going to create %d goroutines.\n", count)

for i := 0; i < count; i++ {
    go func(x int) {
        fmt.Printf("%d ", x)
    }(i) // pass parameter to anonymous function
}

// Program end directly unless you use Time.Sleep
// to wait until goroutine finish to execute
time.Sleep(time.Second)
fmt.Println("End...")
```

If you want to execute the program without use `time.Sleep`, you may use the `sync` library.

```go
import (
	"fmt"
	"sync"
)

func main() {
	count := 15
	var waitGroup sync.WaitGroup

	fmt.Printf("Going to create %d goroutines.\n", count)
	for i := 0; i < count; i++ {
		waitGroup.Add(1)
		go func(x int) {
			defer waitGroup.Done()
			fmt.Printf("%d ", x)
		}(i) // pass parameter to anonymous function
	}
	waitGroup.Wait() // replace Sleep
	fmt.Println("End...")
}
```

In Go >=1.25, Go introduce a new method in the library `sync`, `WaitGroup.Go`. the program above is transformed as:

```go
func main() {
	count := 15
	var waitGroup sync.WaitGroup

	fmt.Printf("Going to create %d goroutines.\n", count)
	for _, i := range count {
		waitGroup.Go(func(x int) {
			fmt.Printf("%d ", x)
		}(i)) // pass parameter to anonymous function
	}
	waitGroup.Wait() // replace Sleep
	fmt.Println("End...")
}
```

The `waitGroup.add()` and `defer waitGroup.Done()` are no longer necessary. Also `go func` become `waitGroup.Go func`.
