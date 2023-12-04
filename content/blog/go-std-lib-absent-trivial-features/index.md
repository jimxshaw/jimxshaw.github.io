---
title: "Go Std Lib Absent Trivial Features"
date: "2017-10-28T13:54:30"
draft: false
comments: false
socialShare: true
toc: false
tags:
  - Go
  - Golang
---

The Go standard library (std lib) was deliberately created to be minimalistic compared to the standard libraries of other languages. This design choice fits with Go's philosophy of simplicity and the preference for small, independent features that can be combined in different ways. Knowing this, there are many commonly used functions that you might expect to find in the Go std lib but are absent because they were deemed "too trivial" to write.

Here are some common absent functions and my trivial implementations for them.

#### Slice Contains Value

Check to see if a slice contains an input value.

```go
func Contains(slice []int, input int) bool {
  for _, el := range slice {
    if el == input {
      return true
    }
  }
  return false
}
```

As of **Go 1.18 (March 2022)**, it's possible to use generics.

```go
func Contains[T comparable](slice []T, input T) bool {
  for _, el := range slice {
    if el == intput {
      return true
    }
  }
  return false
}

```

As of **Go 1.21 (August 2023)**, the previously experimental *slices* package has been added to the std lib and it has a *Contains* function.

```go
import "slices"

sports := []string{"football", "baseball", "basketball"}
slices.Contains(sports, "football") // true
```
