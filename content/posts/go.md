---
title: "Go"
date: 2017-12-14T17:45:54-05:00
tags: [go, programming, concurrency]
---
---

### Doumentation

Go has well balanced the documentation and code by integrating a powerful eco-system within the tool sets.

1. `godoc -http=:8888` offline documentation site
2. `godoc fmt Fprintf` or `godoc builtin append` quick terminal check
3. comments prefix as document stub, but no specific requirements for parameters and return value, as go document tool will dynamically parse it from the source code

# Basics

## Static Types (Built In)

[builtin](https://golang.org/pkg/builtin) pkg

```go
bool

string

int int8 int16 int32 int64
uint uint8 uint16 uint32 uint64 uintptr

byte //is an alias for uint8 
rune //is an alias for int32

float32 float64
complex64 complex128

```

### Type Casting

The expression T(v) converts the value v to the type T

### Type inference

```go
v := 'a'
//v is of type int32  97
fmt.Printf("v is of type %T  %v\n", v, v) 
v := "a"  //v is of type string  a
```

### error
```go
type error interface {
        Error() string
}
```

### Strings

`""` is string

`''` is a character or char, but it's not 8-bit ascii 
in **go**, the word **rune** is used

```go
func main() {
	for pos, char := range "Gő!汉字熱" {
		fmt.Printf("character '%c' starts at byte position %d\n", char, pos)
	}
	// character 'G' starts at byte position 0
	// character 'ő' starts at byte position 1
	// character '!' starts at byte position 3
	// character '汉' starts at byte position 4
	// character '字' starts at byte position 7
	// character '熱' starts at byte position 10
}
```

## Dynamic Types

### Pointers

```go
// has no pointer arthimetic
var p *int
i := 42
p = &i

fmt.Println(*p)
*p = 21
```

### Struct

```go
type Point struct{
	x int
	y int
}
// Struct fields can be accessed through a struct pointer.
// struct pointer has c-like syntax sugar for `(*p).x => p.x`

v := Point{1,2}  // struct literals
p = &v
p.x = 100

w := Point{x:1}  // implies y:0
```

### Array

```go
var a [10]int
primes := [6]int{2, 3, 5, 7, 11, 13}
primes := [...]int{2,3,5,7}
```
array cannot be resized, array variable is not the pointer to the first element. 
each re-assignment is a copy of the whole content


### Slice

> a slice is three-item descriptor (pointer to array, length, capacity)

```go
//type, len, cap
make([]int, 10, 100) //[0 0 0 0 0 0 0 0 0 0]
```
slice literal `a := []int{1,2,3,4}`:
1. create an array of fixed size 4
2. create the slice references to the array

A slice does not store any data, it describes a section of an underlying array

slice range *matches the python way for lower and higher bound*

slice *length* and *capacity*

[usage and internals](https://blog.golang.org/go-slices-usage-and-internals)

![diagram](https://blog.golang.org/go-slices-usage-and-internals_slice-2.png)

#### append (built-in)

> `func append(slice []T, elements ...T) []T`

```go
x := []int{1,2,3}
y := []int{4,5,6}
x = append(x, y...)
// the y... is like the unpack in python where you have a tuple x, and *x is to unpack the tuple into elements
```


#### 2 dimensions

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

### make vs new

-  `new` allocates memory, zeros it and returns its address as *T (*abstract types*)

- `make` (built-in func): creates *Slices, Maps and Channel* only and returns **Initialized** (not zeroed) values as T (not *T)

	these three types represent **references** to data structure underneath. recall that the example of slice, it needs an array initialized to reference

### map

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




## Interface{}

`interface{}` is any type or `object` in other OOP-oriented language


# Control Structure 

```go
// loop
for i:=1; i<100; i++ {

}
for i < 100 {

}
for {

}

// if 
//Variables declared by the statement are only in scope until the end of the if.

if err := scanner.Err(); err != nil {
		fmt.Fprintln(os.Stderr, "reading input:", err)
	}

//switch
//Go only runs the selected case, not all the cases that follow

//Switch without a condition is the same as switch true
switch os := runtime.GOOS; os {
case "darwin":
	fmt.Println("OS X.")
case "linux":
	fmt.Println("Linux.")
default:
	// freebsd, openbsd,
	// plan9, windows...
	fmt.Printf("%s.", os)
}

//goto



//defer
//Deferred function calls are pushed onto a stack. When a function returns, its deferred calls are executed in LIFO order
func main() {
	defer fmt.Println("world")
	fmt.Println("hello")
}



```








## functions


```go
// parameter shortened types
func add(x, y int) int {
	return x + y
}

//naked return
func split(sum int) (x, y int){
	x = sum * 4 / 9
	y = sum - x
	return
}

```


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

//pointer receiver 
func (v *Vertex) Scale(coef int) {
	v.x = v.x * coef
	v.y = v.y * coef
}
v := Vertex{3, 4}

v.abs() // method
Abs(v) // function

```


## constants

- created at compile time
- numbers, characters(runes), strings and boolean
- math.Sin(math.Pi/4) is not as Sin is func call

### iota

const iota = 0 // Untyped int.

*iota* is a predeclared identifier representing the untyped integer
ordinal number of the current const specification in a (usually
parenthesized) const declaration. It is zero-indexed.

![iota](https://camo.githubusercontent.com/a375bc9aaf7f25c99104936003d3a72f28da4225/68747470733a2f2f63646e2d696d616765732d312e6d656469756d2e636f6d2f6d61782f323030302f312a7366414854337a6b2d576a7853445249444d706461412e676966)
[visual guide to enums](https://blog.learngoprogramming.com/golang-const-type-enums-iota-bc4befd096d3)
 



## range

range can iterate over *slices, arrays, strings, maps and channels*

```go
for idx := range nums //drop ,value
for _, val := range nums
```

## composite literal

```go
return &File{fd, name, nil, 0}
return &File{fd: fd, name: name}
```

## fmt

- %v
- %+v => all fields for struct
- %#v => go syntax `main.stack{pos:2, data:[10]int{20, 40, 0, 0, 0, 0, 0, 0, 0, 0}}`


##

```go

// word count
package main

import (
	"golang.org/x/tour/wc"
	"strings"
)

//WordCount returns stats of string words
func WordCount(s string) map[string]int {
	words := strings.Fields(s)
	stats := make(map[string]int)
	for _, word := range words {
		stats[word]++
	}
	return stats
}

func main() {
	wc.Test(WordCount)
}


```