---
title: "04 - Simple note app in Go"
description: "We are going to reuse all the stuff learned in a simple note, We will also see how Go deals with files and create a basic Command Line Application."
date: 2026-10-17
lang: en
category: learn
tags: ["go", "learn-go"]
cover: go
---

In this minimalist note program, we are going to:

- Create a CLI app that takes one argument, the name of the note
- Capture (read) user input and write the output into the note
- Deal with File, create, write and close

Start by create a new file `simple-note/main.go`

## The command line argument

In Go, to capture the argument from the command line, we need to use `os.Args`

```go
package main
import (
"fmt"
"os"
)

// Global Variables, we would also use a global "slice"
var (
	WorkDir  = "/tmp"
	FileName string
)

func checkArguments() {
	arguments := os.Args
	if len(arguments) == 2 {
		FileName = arguments[1] + ".txt" // we add .txt as file extension
	} else {
		fmt.Printf("Usage: %s <filename>\n", arguments[0])
		os.Exit(0)
	}
}

func main() {
checkArguments()
f := WorkDir + "/" + FileName
fmt.Println("Note will be written at ->", f)
}
```

You can try this with: `go run main.go` and `go run main.go hello`

## Capture the user input

We capture the user input with `bufio` and `os.Stdin`. `bufio.NewReader(os.Stdin)` return a new Reader with a default buffer size of 4096 bytes.

```go
import (
...
"bufio"
)

// this function would return a string
func captureMsg() string {
	fmt.Println("Please, enter your note")
	// User input for file content
	inputNote := bufio.NewReader(os.Stdin)
	// Read input and close buffer on '\n' (aka Enter)
	input, _ := inputNote.ReadString('\n')
	return input
}

func main() {
...
note := captureMsg()
fmt.Println("You have written", note)
}
```

And last, we need to save the "note" in a file.

## Deal with files

To use files, we going to create two function `createFile` and `writeInFile`. We mainly use `os.Create()`

```go
func createFile(filename, text string) {
	file, err := os.Create(filename)
	if err != nil {
		fmt.Println("Err creating a file", err)
		os.Exit(-1)
	}
	// closing running after the main method has completed
	defer func() {
		if err := file.Close(); err != nil {
			fmt.Println("Error closing file", err)
			os.Exit(-1)
		}
	}() // defer in anonymous function
	writeInFile(file, text)
}

// "file" should be a type os.File
func writeInFile(file *os.File, text string) {
	len, err := file.WriteString(text)
	if err != nil {
		fmt.Println("Failed writing to file", err)
		os.Exit(-1)
	}
	fmt.Printf("\nFile name is: %s", file.Name())
	fmt.Printf("\nNote length is: %d bytes\n", len)
}
```

## Final program

```go
package main

import (
	"fmt"
	"os"
	"bufio"
)

// Global Variables
var (
	WorkDir  = "/tmp"
	FileName string
)

func checkArguments() {
	arguments := os.Args
	if len(arguments) == 2 {
		FileName = arguments[1] + ".txt"
	} else {
		fmt.Printf("Usage: %s <filename>\n", arguments[0])
		os.Exit(0)
	}
}

func createFile(filename, text string) {
	file, err := os.Create(filename)
	if err != nil {
		fmt.Println("Err creating a file", err)
		os.Exit(-1)
	}
	// closing running after the main method has completed
	defer func() {
		if err := file.Close(); err != nil {
			fmt.Println("Error closing file", err)
			os.Exit(-1)
		}
	}() // defer in anonymous function
	writeInFile(file, text)
}

// "file" should be a type os.File
func writeInFile(file *os.File, text string) {
	len, err := file.WriteString(text)
	if err != nil {
		fmt.Println("Failed writing to file", err)
		os.Exit(-1)
	}
	fmt.Printf("\nFile name is: %s", file.Name())
	fmt.Printf("\nNote length is: %d bytes\n", len)
}

func captureMsg() string {
	fmt.Println("Please, enter your note")
	// User input for file content
	inputNote := bufio.NewReader(os.Stdin)
	// Read input and close buffer on '\n' (aka Enter)
	input, _ := inputNote.ReadString('\n')
	return input
}

func main() {
	checkArguments()
	f := WorkDir + "/" + FileName
	note := captureMsg()
	createFile(f, note)
}
```

This program is very minimalist, we would to improve it by adding:

- Improve the CLI by using more arguments and make "more things"
- List all notes from "WorkDir"
- Organise note by date or whatever
- Search a note by using keywords
- Delete a note, it will become a "tasklist" program :)
