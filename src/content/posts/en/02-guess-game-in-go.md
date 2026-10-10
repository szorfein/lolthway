---
title: "02 - Guess game in Go"
description: "Time to build a little guess game, we going to learn how to control flow, use loop and declare variables."
date: 2026-10-10
lang: en
category: learn
tags: ["go", "learn-go"]
cover: go
---

We going to build a "Guess game" for the next step. It's a classic game when you learn how to code in different language. You can see an example of what is it [here](https://c-lang-learning.com/en/game-projects.html#game1).

Before coding anything, just things to all steps the program's will need

- Create a random number, something between 0-100 for start.
- The program will ask the user for a number, so we need to capture the answer
- Create a loop for the program, the game quit when you found the right number.
- Compare the answer with the random number, the program tell if the number is greater or lower

## Variable declaration
In Go, you have two way to declare a variable

```go
var chaosNumber = 10; // normal way
chaosNumber := 10; // or shorter
```

Avoid to mix between `=` or `:=`, `:=` is used to declare a variable while `=` assign a value to a variable.

## Declare a random number

There are two library in Go for this, `math/rand` and `crypto/rand`. I think the `math/rand` is the most simple for this program, so we going to use that.

```go
go doc math/rand
go doc rand.intl
```

So at this step, your program `main.go` would look like this:

```go
package main
import ( "fmt"; "math/rand"; )

func main() {
randomNumber := rand.Intn(100) // generate a number between 0 and 100 (max)
fmt.Printf("Number is %d\n", randomNumber)
}
````

If you run the program two time, you should have two different number unless you are very lucky

```go
go run main.go
go run main.go
````

## Ask and capture the answer

For capture the answer, you need `fmt.Scanln`. Don't hesitate to check with `go doc`

```go
fmt.Println("Please enter a number between 1 and 100")
var input
fmt.Scanln(&input)
fmt.Printf("you enter %d", input)
```

In Go, when you start using random value (like the user input), you should check than the "Type" is correct or check for error. Here we want an Integer (positive number) and nothing other so instead, do this:

```go
fmt.Println("Please enter a number between 1 and 100")
var input int
_, err := fmt.Scanln(&input) // use "_" when you don't need to use the variable
if err != nil { // if err is not "nil"
    fmt.Println("Error reading input: ", err)
    os.Exit(-1) // exit(-1) is bad, yeah really bad
}
if input > 100 || input < 0 {
    fmt.Println("Superior to 100 or a negative number")
    os.Exit(-1)
}
```

## Compare the input with the random number

This part is "easy", you just need some condition `if {}`

```go
if input == chaosNumber { // == mean "equal"
    fmt.Printf("===You found the number %d===\n", chaosNumber)
    os.Exit(0) // Exit without error
}

if input > chaosNumber { // superior
    fmt.Printf("%d is too high\n", input)
}

if input < chaosNumber { // u know
    fmt.Printf("%d is too low\n", input)
}
```

## The loop

For the loop, you can use a `for {}`:

```go
for {
// You program logic here
}
```

## Final program

Solution in 39 lines:

```go
package main

import (
	"fmt"
	"math/rand"
	"os"
)

func main() {
	chaosNumber := rand.Intn(100)

	fmt.Println("===Hello th the GuessGame===")
	//fmt.Printf("Number is %d\n", chaosNumber)

	for {
		fmt.Println("Please enter a number between 1 and 100")
		var input int
		_, err := fmt.Scanln(&input)
		if err != nil { // if err is not "nil"
			fmt.Println("Error reading input: ", err)
			os.Exit(-1)
		}
		if input > 100 || input < 0 {
			fmt.Println("Superior to 100 or not Integer")
			os.Exit(-1)
		}

		if input == chaosNumber {
			fmt.Printf("===You found the number %d===\n", chaosNumber)
			os.Exit(0)
		}
		if input > chaosNumber {
			fmt.Printf("%d is too high\n", input)
		}
		if input < chaosNumber {
			fmt.Printf("%d is too low\n", input)
		}
	}
}
```

You can try to improve this program, count how munch tentatives you try, rewrite the code by using functions, etc...
