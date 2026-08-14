---
title: "Go Programming Language"
date: "2016-08-20T09:20:00"
draft: false
comments: false
socialShare: true
toc: false
tags:
  - Go
  - Golang
---

![Go Gopher][gogopher]

Go was created by a team inside Google back in 2009. Three big languages that were used at Google at that time were C/C++, Java and Python. The creators of Go saw that each of those three languages was powerful but had limitations. For example, Python was easy to pickup and learn but ran into trouble when running apps at the scale a massive company like Google needed. Java started out simple but its type system grew increasingly complex. C/C++ was wickedly fast but it also had a complex type system and it suffered from notoriously slow compile-times. The three languages were also created at a time when multi-threaded applications were rare. Concurrency was added to them at a later date. Google's often highly concurrent and parallel environments posed challenges for them. Go was the answer to many of the concerns Python, Java and C/C++ had.

### Go Features

**Simplified and Efficient Concurrency**

Go’s approach to concurrency is one of its standout features. Unlike languages where concurrency feels like an afterthought, Go was designed with concurrency at its core, thanks to goroutines and channels.

* *Goroutines*: Lightweight threads managed by the Go runtime. They are less resource-intensive than traditional threads and are easy to create.

    ```go
    func printCounts(label string, count int) {
        for i := 0; i < count; i++ {
            fmt.Println(label, ":", i)
        }
    }

    func main() {
        go printCounts("asyncCount", 5) // Executed Asynchronously
        printCounts("syncCount", 5)     // Executed Synchronously
    }
    ```

    In this example, printCounts is called as a goroutine and runs asynchronously alongside the main function's execution.

* *Channels*: Pipes that connect concurrent goroutines, allowing them to communicate with each other safely.

    ```go
    func compute(values chan int) {
        for i := 0; i < 5; i++ {
            values <- i * i // Send i*i to the channel
        }
        close(values)
    }

    func main() {
        values := make(chan int)
        go compute(values)

        for value := range values {
            fmt.Println(value)
        }
    }
    ```

    Here, compute sends squared values to the values channel, which are then read and printed in the main function.

**Interface Implementation**

In Go, interfaces are implicitly implemented. This means that a type implements an interface by just implementing its methods, without needing to explicitly declare it.  

```go
type Vehicle interface {
    StartEngine() bool
}

type Car struct {
    Model string
}

func (c Car) StartEngine() bool {
    return true
}

func main() {
    var myCar Vehicle = Car{Model: "Tesla Model S"}
    fmt.Println("Engine started:", myCar.StartEngine())
}
```

Here, Car implicitly implements the Vehicle interface by defining the StartEngine method.

**Garbage Collection**

Go offers a modern garbage collector that provides a good balance between performance and convenience. It efficiently manages memory, freeing developers from manual memory management, common in languages like C++.

**Cross-Platform Binary Compilation**

Go compiles down to a single binary, simplifying deployment. This binary contains all dependencies, making distribution and versioning more straightforward.

**Standard Library**

Go's standard library is another strong suit, offering a wealth of utilities ranging from file handling to networking. It's designed to encourage the creation of reliable and efficient programs.

```go
// Sample to demonstrate HTTP server creation
func main() {
    http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
        fmt.Fprintln(w, "Welcome to my Go server!")
    })

    log.Fatal(http.ListenAndServe(":8080", nil))
}
```

This example shows how simple it is to set up a basic HTTP server using Go's standard library.

**Growing Go Community**

Go has a strong and passionate community supporting the language. A language lives or dies based on its community support as it's the community who spreads the word and makes sure new developers wanting to learn the language have as an easy time as possible to learn it. Go certainly has a fervent community.

**Philosophy of Simplicity**

Go benefits from simplicity and simplicity ought to be recognized as a language feature. Simplicity as in the creators consciously left out features that are popular in other languages because they felt they would add complexity to Go.

Features like generics (only recently added in 2022 in a limited form), assertions, and method overloading are omitted to keep the language straightforward and uncluttered.

### Resources

Be sure to check out the official Go [website][godocs] for more information.

[gogopher]: https://www.vertica.com/wp-content/uploads/2019/07/Golang.png

[godocs]: https://golang.org/