## Go-basics



#### installating Go

```
https://go.dev/doc/install
```
configure GOROOT, GOPATH & GOBIN


```
go help

go version

go env

go mod init <module-name> - create a module

go get <module-name> -  Get third party packages from SCM

go run <filename>

go build - compile the executable file and put in same directory

go build -o <outpout-file-name> - save executable with custom name

go install -  compile the executable file and move to $GOPATH/bin or $GOBIN if set

```
**sample go code**


```
package main

import "fmt"

func main() {
    fmt.Println("Hello, World!")
}

```

### Go basics

- Data types
1. bool
2. Numeric types
3. byte
4. rune : is an alias of int32
5. string

- variables & constants
- Shorthand declarations
- fmt package 
- variable type & size
- function & named return 
- Map
- Importing local packages
    1. import alias
    2. Dot import
    3. Use of blank identifier
- cross compilation

```
env GOOS=linux GOARCH=arm64 go build main.go

```
