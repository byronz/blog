---
title: "Go"
date: 2017-12-14T17:45:54-05:00
tags: [go, programming, concurrency]
---
# Doumentation

Go has well balanced the documentation and code by integrating a powerful eco-system within the tool sets.

1. `godoc -http=:8888` offline documentation site
2. `godoc fmt Fprintf` or `godoc builtin append` quick terminal check
3. comments prefix as document stub, but no specific requirements for parameters and return value, as go document tool will dynamically parse it from the source code

# Context

## Paradigm

- compiled
- concurrent
- imperative
- structured

## Authors

original
- Robert Griesemer
- Rob Pike (his spouse French Renee)
- Ken Thompson

## Tools

delve debugging server

godoc

## Types

> static vs abstract

static are basically all the common *builtin* types

`interface{}` is any type or `object` in other OOP-oriented language

## Struct

struct pointer has c-like syntax sugar for `(*p).X => p.X`

## Array

```go
var a [10]int
```

array cannot be resized

## Slice

match the python way for lower and higher bound

slice literal `a := []int{1,2,3,4}`
1. create an array of fixed size 4
2. create the slice references the array

*length* and *capacity*

[usage and internals](https://blog.golang.org/go-slices-usage-and-internals)

![diagram](https://blog.golang.org/go-slices-usage-and-internals_slice-2.png)

```go

func Pic(dx, dy int) [][]uint8 {

	buf := make([][]uint8, dy)

	for i := range buf {
		buf[i] = make([]uint8, dx)
		for j := range buf[i] {
			buf[i][j] = uint8(j ^ i)
		}
	}
	return buf
}
```

## strings

"" is string
'' is a character or char, but it's not 8-bit ascii

in **go**, the word **rune** is used

>type rune = int32
>
>    rune is an alias for int32 and is equivalent to int32 in all ways. It is
    used, by convention, to distinguish character values from integer
    values.

```go
for pos, char := range "Gő!汉字熱" {
    fmt.Printf("character '%c' starts at byte position %d\n", char, p汉字熱os汉字熱)
}
//character 'G' [type => 0] starts at byte position int32
//character 'ő' [type => 1] starts at byte position int32
//character '!' [type => 3] starts at byte position int32
//character '汉' [type => 4] starts at byte position int32
//character '字' [type => 7] starts at byte position int32
//character '熱' [type => 10] starts at byte position int32
```

## map

```go

//solution to WordCount

package main

import (
	"strings"
	"golang.org/x/tour/wc"
)

func WordCount(s string) map[string]int {
	counter := make(map[string]int)
	for _, word := range strings.Fields(s) {
		counter[word]++
	}
	return counter
}


func main() {
	wc.Test(WordCount)
}
```

## defer

similar to `with` context management in python

## function

## methods

> Method is a function with special *receiver* argument

```go

type Vertex struct {
	X, Y float64
}

// (v Vertex) is the receiver
func (v Vertex) Abs() float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func Abs(v Vertex) float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}


v := Vertex{3, 4}

v.abs() // method
Abs(v) // function

```

## enum

const iota = 0 // Untyped int.

*iota* is a predeclared identifier representing the untyped integer
ordinal number of the current const specification in a (usually
parenthesized) const declaration. It is zero-indexed.

![iota](https://camo.githubusercontent.com/a375bc9aaf7f25c99104936003d3a72f28da4225/68747470733a2f2f63646e2d696d616765732d312e6d656469756d2e636f6d2f6d61782f323030302f312a7366414854337a6b2d576a7853445249444d706461412e676966)
[visual guide to enums](https://blog.learngoprogramming.com/golang-const-type-enums-iota-bc4befd096d3)

## range

range can iterate over *slices, arrays, strings, maps and channels*

## make vs new

- `new` allocates memory, zeros it and returns its address as *T (*abstract types*)

- make: creates *Slices, Maps and Channel* only and returns **Initialized** values as T

	builtin types

## composite literal

```go
return &File{fd, name, nil, 0}
return &File{fd: fd, name: name}
```

## fmt

- %v
- %+v => all fields for struct
- %#v => go syntax `main.stack{pos:2, data:[10]int{20, 40, 0, 0, 0, 0, 0, 0, 0, 0}}`
