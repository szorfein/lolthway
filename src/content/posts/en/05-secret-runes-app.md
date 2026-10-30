---
title: "05 - Secret Runes app"
description: "A simple encryption and decryption program using Go Runes. We will see how to use: switch/case, convert string to uint, functions and build a simple menu."
date: 2026-10-27
lang: en
category: learn
tags: ["go", "learn-go"]
cover: go
---

This is just an exercise for people learning Go, not a serious app :)

## How look the final app

```txt
========= SECRETS RUNES ==========

  1. Encrypt a message
  2. Decrypt a message

Your choice? 2
Paste your message: 84 111 100 97 121 32 105 115 32 84 117 101 32 79 99 116 32 50 55 10

Your decrypted message is: Today is Tue Oct 27
```

## Base app with a menu

```go
package main

import (
	"bufio"
	"fmt"
	"os"
)

func display_banner() {
	fmt.Println("========= SECRETS RUNES ==========")
	fmt.Println("")
	fmt.Println("  1. Encrypt a message")
	fmt.Println("  2. Decrypt a message")
}

func main() {
	display_banner()
	fmt.Print("\nYour choice? ")
	var input int
	_, err := fmt.Scanln(&input)
	if err != nil { // if err is not "nil"
		fmt.Println("Error reading input: ", err)
		os.Exit(-1)
	}
    fmt.Println("You have choose", input)
}
```

Next we going to use a `switch/case` instead of `if` to make different operation on 1 or 2.

## Control the flow with Switch and Case

In the main function, add this:

```go
// return a string
func encrypt_msg() string {
    return "encrypt"
}

// return a string
func decrypt_msg() string {
    return "decrypt"
}

func main() {
    ...
	switch input {
	case 1:
		msg := encrypt_msg()
		fmt.Println(msg)
	case 2:
		msg := decrypt_msg()
		fmt.Printf(msg)
	default:
		os.Exit(0)
	}
}
```

Now, we only have to works on the two new functions `encrypt_msg()` and `decrypt_msg()`.

## Encryption

It's a easy part, this is not really an encryption, just a string conversion to the `Runes` type. For example, the string `hello` become `[104 101 108 108 111]`.

```go
func encrypt_msg() []rune {
	fmt.Print("Write your message: ")
	inputMsg := bufio.NewReader(os.Stdin)
	input, _ := inputMsg.ReadString('\n')
	return []rune(input)
}
```

The function return an array of runes `[]rune`.

## Decryption

Decryption is the most critical part of this application, we need to convert the user input to the `uint8` type in order to display "runes" correctly.

```go
func decrypt_msg() []uint8 {
	fmt.Print("Paste your message: ")
	inputMsg := bufio.NewReader(os.Stdin)
	input, _ := inputMsg.ReadString('\n') // Close buffer on "\n"
	intArray := convertToUint(input)
	fmt.Printf("\nYour decrypted message is: ")
	for _, v := range intArray {
		fmt.Printf("%c", v)
	}
	fmt.Println()
	return intArray
}
```

We capture the user input with `bufio.NewReader(os.Stdin)`, we need to write the function `convertToUint()`.

```go
func convertToUint(msg string) []uint8 {
	input := strings.TrimSuffix(msg, "\n")    // Remove "\n" to avoid error during uint8 convertion
	inputToArray := strings.Split(input, " ") // Convert String to Array, split on "space"
	var intArray []uint8                      // Need to convert the inputToArray to uint8 (compatible Runes)
	for _, v := range inputToArray {
		ui, err := strconv.ParseUint(v, 10, 8) // uint8 format
		if err != nil {
			fmt.Println("err:", err)
			os.Exit(-1)
		}
		intArray = append(intArray, uint8(ui)) // Add each elements to the intArray
	}
	return intArray
}
```

## Final program

```go
package main

import (
	"bufio"
	"fmt"
	"os"
	"strconv"
	"strings"
)

func display_banner() {
	fmt.Println("========= SECRETS RUNES ==========")
	fmt.Println("")
	fmt.Println("  1. Encrypt a message")
	fmt.Println("  2. Decrypt a message")
}

// Encrypt a message in Runes format
func encrypt_msg() []rune {
	fmt.Print("Write your message: ")
	inputMsg := bufio.NewReader(os.Stdin)
	input, _ := inputMsg.ReadString('\n')
	return []rune(input)
}

// Decrypt a Runes message
func decrypt_msg() []uint8 {
	fmt.Print("Paste your message: ")
	inputMsg := bufio.NewReader(os.Stdin)
	input, _ := inputMsg.ReadString('\n') // Close buffer on "\n"
	intArray := convertToUint(input)
	fmt.Printf("\nYour decrypted message is: ")
	for _, v := range intArray {
		fmt.Printf("%c", v)
	}
	fmt.Println()
	return intArray
}

// Need to convert string from bufio in UInt8 to work with Runes
func convertToUint(msg string) []uint8 {
	input := strings.TrimSuffix(msg, "\n")    // Remove "\n" to avoid error during uint8 convertion
	inputToArray := strings.Split(input, " ") // Convert String to Array, split on "space"
	var intArray []uint8                      // Need to convert the inputToArray to uint8 (compatible Runes)
	for _, v := range inputToArray {
		ui, err := strconv.ParseUint(v, 10, 8) // uint8 format
		if err != nil {
			fmt.Println("err:", err)
			os.Exit(-1)
		}
		intArray = append(intArray, uint8(ui))
	}
	return intArray
}

func main() {
	display_banner()
	fmt.Print("\nYour choice? ")
	var input int
	_, err := fmt.Scanln(&input)
	if err != nil { // if err is not "nil"
		fmt.Println("Error reading input: ", err)
		os.Exit(-1)
	}
	switch input {
	case 1:
		msg := encrypt_msg()
		fmt.Println(msg)
	case 2:
		msg := decrypt_msg()
		fmt.Printf("%s\n", msg) // It work's too
	default:
		os.Exit(0)
	}
}
```
