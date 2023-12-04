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

Here are some of the common functions and my trivial implementation of them.

**Slice Contains Int**

Check to see if a slice contains an input value.

```go
func contains(slice []int, input int) bool {
  for _, el := range slice {
    if el == input {
      return true
    }
  }
  return false
}
```
