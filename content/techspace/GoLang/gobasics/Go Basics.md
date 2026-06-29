
## Package Declaration

  

Every Go program starts with a package declaration. The `main` package is special — it defines a standalone executable program.

  

```go

package main

```

  

---

  

## Imports

  

The `fmt` package provides formatted I/O functions, like printing to the console.

  

```go

import "fmt"

```

  

---

  

## The Main Function

  

Go searches for the `main` function in the `main` package to start execution. Without it, the compiler cannot generate a binary executable.

  

- Defined with the `func` keyword

- Takes no parameters

- Returns no values

- All program logic starts here

  

```go

func main() {

    fmt.Println("hello World!!")

}

```

  

---

  

## go build vs go run

  

### `go build`

  

- Compiles source code and generates a persistent executable binary

- Use this when you want to distribute or deploy the program

- Once built, you can run the binary multiple times without recompiling

- Example use cases: web servers, command-line tools

  

### `go run`

  

- Compiles and runs the source code in a single step

- Does **not** generate a binary file

- Useful during development for quickly testing small programs

- No need to manage separate binary files while iterating

  

> **Rule of thumb:** Use `go run` during development, `go build` for distribution.

  

---

  

## The Go Compiler

  

The Go compiler translates Go source code into machine code. During compilation it:

  

1. Performs **lexical analysis** — breaks source into tokens

2. **Parses** the tokens into a structure

3. Does **type checking** — ensures code follows Go's type rules

4. Performs **code generation** — produces optimized machine code

  

The compiler checks for syntax errors and generates fast, efficient binaries.

  

### Tree Shaking

  

The compiler builds an **Abstract Syntax Tree (AST)** from your source code and uses it to apply optimizations, including tree shaking:

  

- Analyzes dependencies between different parts of the code

- Identifies which functions, variables, and packages are actually used

- Removes unused ("dead") code from the final binary

- Results in smaller binaries and faster execution

  

---

  

## The Go Runtime

  

The Go runtime is a set of libraries and components that support executing Go programs. It handles:

  

- **Garbage collection** — automatic memory management

- **Goroutine scheduling** — managing concurrent execution

- **Memory management** — allocation and deallocation

- **Runtime services** — the environment your program runs in

  

---

  

## Memory Management

  

Go handles memory automatically through its runtime garbage collector. Key points:

  

- The garbage collector finds and reclaims memory no longer in use

- Prevents memory leaks without manual intervention

- Runs **concurrently** in the background — the program keeps running while GC does its work

- Developers don't manage memory manually, which reduces memory-related bugs

  

---

  

## Is Go Platform Independent?

  

Partially. Go is designed for cross-platform development, but with an important distinction:

  

- The Go compiler generates **machine code specific to the target platform**

- The same `.go` source files can be compiled for Windows, macOS, Linux, and more

- The resulting binary is **platform-specific** — a Windows binary won't run on Linux

  

### Cross-Compilation

  

Go supports cross-compilation via `GOOS` and `GOARCH` environment variables:

  

```bash

# Compile for Windows from any platform

GOOS=windows GOARCH=amd64 go build -o myprogram.exe

```

  

The generated `.exe` will only run on Windows. To run on Linux, you'd recompile with `GOOS=linux`.

  

---

  

## The Go Standard Library

  

A collection of pre-written packages covering a wide range of functionality:

  

- Input/output (`fmt`, `os`, `bufio`)

- Networking (`net`, `net/http`)

- Data manipulation (`strings`, `strconv`, `encoding/json`)

- Cryptography (`crypto/...`)

- And much more

  

The standard library is designed to be efficient, reliable, and easy to use — you get a lot done without reaching for third-party packages.