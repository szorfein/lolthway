---
title: "01 - Introduction to Go"
description: "Short introduction for Go, build and run your first app, install and use go doc."
date: 2026-10-09
lang: en
category: learn
tags: ["go", "learn-go"]
cover: go
---

## Why start to code in Go in 2026

Go exist since 2009 and is always evolving. There are a lot of useful programs written in Go like Kubernetes, Docker, Terraform, CockroachDB and more.

Interested applications to build

- Desktop app (AppImage) with [wails](https://wails.io/docs/introduction/), better than Electron?
- A Go backend for Wayland and [Quickshell](https://quickshell.org/) which returns all the information you need, saves user preferences, etc...
- Microservices, gRPC, API, Prometheus, Grafana, etc...
- Command line applications with [cobra](https://cobra.dev/) or [viper](https://github.com/spf13/viper)

There are a ton of libraries that help to interact with GNU/Linux like [sys/unix](https://pkg.go.dev/golang.org/x/sys/unix), [procfs](https://github.com/jandre/procfs).

## Configure your shell

```sh
if command -v go >/dev/null 2>&1; then
    export GOBIN=$(go env GOPATH)/bin
    PATH="$PATH:$(go env GOBIN):$(go env GOPATH)/bin"
fi
```

With this in place, all the dependencies we need will be installed properly.

## Change directory

Create a new directory to start coding

```sh
mkdir first-app
cd first-app
```

## Generate the go.mod

```sh
go mod init hello_world
```

Output

```txt
go: creating new go.mod: module hello_world
go: to add module requirements and sums:
go mod tidy
```

Every Go program has a `go.mod` in the root directory. It's like a `package.json` for Node.js or a `Gemfile` for Ruby.

Don't edit the go.mod yourself; instead use `go get` or `go mod tidy` to update dependencies.

You can also use a Git repository as a source like:

```sh
go mod init github.com/YOUR-NAME/hello
```

But we will see that later.

## First app

Create a new file `hello.go`

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello World");
}
```

The `package main` should always be present with the `func main() {}`. The `import` is used to list all your dependencies and external libraries.

## Build and run your app

```sh
go build
./hello_world
```

The name of the app matches with the module name in the `go.mod`.

You can also just run `go run hello.go` which compiles and runs when you want to execute only one program.

## Look for Documentation

Need information about a function, package. Use `go doc`

```sh
go doc fmt.Printf
go doc fmt
```

If `go doc` is not available for you, install it with `go install golang.org/x/tools/cmd/godoc@latest`
