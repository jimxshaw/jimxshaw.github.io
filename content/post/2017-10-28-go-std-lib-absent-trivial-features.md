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

The Go [standard library (std lib)][std lib] was deliberately created to be minimalistic compared to the standard libraries of other languages. This design choice fits with Go's philosophy of simplicity and the preference for small, independent features that can be combined in different ways. 

There are many commonly used functions that you might expect to find in the Go std lib but are absent because they were thought to be "too trivial" to write.

Here are a few missing ones and my trivial implementations for them.

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

numbers := []int{1, 2, 3}
result := Contains(numbers, 3) // true
```

As of **Go 1.18 (March 2022)**, it's possible to use generics.

```go
func Contains[T comparable](slice []T, input T) bool {
  for _, el := range slice {
    if el == input {
      return true
    }
  }
  return false
}

numbers := []int{1, 2, 3}
result := Contains(numbers, 3) // true

fruits := []string{"apple", "banana", "peach"}
result := Contains(fruits, "grape") // false

```

As of **Go 1.21 (August 2023)**, the previously experimental [slices package][slices package] has been added to the std lib and it has a *Contains* function.

```go
import "slices"

sports := []string{"football", "baseball", "basketball"}
slices.Contains(sports, "football") // true
```

#### Map over a Slice

Map means to apply a function to each element in a slice and then create a new slice with the results.

```go
func Map(slice []int, fn func(int) int) []int {
  result := make([]int, len(slice))
  for i, el := range slice {
    result[i] = fn(el)
  }
  return result
}

numbers := []int{1, 2, 3}
doubled := Map(numbers, func(n int) int { return n * 2 }) // [2, 4, 6]
```

As of **Go 1.18 (March 2022)**, it's possible to use generics.

```go
func Map[T, V any](slice []T, fn func(T) V) []V {
  result := make([]V, len(slice))
  for i, el := range slice {
      result[i] = fn(el)
  }
  return result
}

numbers := []int{1, 2, 3}
plusOne := Map(numbers, func(n int) int { return n + 1 }) // [2, 3, 4]
strSlice := Map(numbers, func(n int) string { return fmt.Sprint("Item:%d", n) }) // ["Item:1", "Item:2", "Item:3"]
```

#### Filter a Slice

Filter means to extract slice elements that meet a certain criteria from an input function and return a new slice with elements that meet the criteria.

```go
func Filter(slice []int, fn func(int) bool) []int {
    var result []int
    for _, el := range slice {
        if fn(el) {
            result = append(result, el)
        }
    }
    return result
}

ages := []int{14, 18, 21, 16, 25, 5}
adults := Filter(ages, func(age int) bool { return age >= 18 }) // [18, 21, 25]
```

As of **Go 1.18 (March 2022)**, it's possible to use generics.

```go
func Filter[T any](slice []T, fn func(T) bool) []T {
  var result []T
  for _, el := range slice {
      if fn(el) {
          result = append(result, el)
      }
  }
  return result
}

numbers := []int{1, 2, 3}
filteredNum := Filter(numbers, func(n int) bool {
  return n > 1
}) // [2, 3]

words := []string{"engine", "architecture", "home"}
filteredStr := Filter(weather, func(s string) bool {
  return len(s) > 7
}) // ["architecture"]
```

#### Reduce a Slice

Reduce means to take a slice and combine its elements into a single value using an input combining function.

```go
func Reduce(slice []int, initial int, fn func(int, int) int) int {
  result := initial
  for _, el := range slice {
      result = fn(result, el)
  }
  return result
}

prices := []int{100, 20, 30}
total := Reduce(prices, 0, func(accumulation, price int) int { return accumulation + price }) // 150

```

As of **Go 1.18 (March 2022)**, it's possible to use generics.

```go
func Reduce[T, M any](slice []T, fn func(M, T) M, initial M) M {
  result := initial
  for _, el := range slice {
      result = fn(result, el)
  }
  return result
}

words := []string{"This", "is", "Go!"}
concatenated := Reduce(words, func(accumulation, next string) string {
  return accumulation + " " + next
}) // "This is Go!"
```
___

My functions list is by no means exhaustive and neither is my implementation for them. 

What other absent functions have you written implementations for?

Feel free to share!

#### Resources

[Go std lib documentation][std lib]

[std lib slices package][slices package]

[Go 1.18 Release Notes][Go 1.18]

[Go 1.21 Release Notes][Go 1.21]

[std lib]: https://pkg.go.dev/std

[slices package]: https://pkg.go.dev/slices

[Go 1.18]: https://tip.golang.org/doc/go1.18

[Go 1.21]: https://tip.golang.org/doc/go1.21