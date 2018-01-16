---
title: "Why Go?"
date: 2017-12-22T09:27:52-05:00
tags: [go, programming]
draft: true
---

My few days' experience with `GO` was quite amazing.

# Gopher

![mascot](https://seeklogo.com/images/G/gopher-logo-5BDED1E91D-seeklogo.com.png)

Designed by Reneé French

# Go is built for team work

## Documentation

Go has its own **OFFICIAL** convention for package, exported func/methods, examples (which at the same time can be a valid test). It also includes a built-in web server, and a handy terminal command tool `godoc`.

## Compiler

- build time is truly fast
- in go community, you don't need have a *style guideline*, because everything is controlled by *gofmt*
- the `lint` and `gofmt` for go is well integrated with all the popular IDEs

## Simple Syntax => Producitive

- programmer with any background (C/C++/Java or sriptings) will feel confortable to start coding in few days
- learning curve is important for team to grow

## Interface


## Concurrency

goroutine is about **2~4kb** of memory, which is considerably lightweight, might not be as small as **BEAM** (Erlang VM) <1kb

*running 100 http requests with goroutines takes about 3s in my old mac book*
```go
package main

import (
	"fmt"
	"io/ioutil"
	"net/http"
)

func fetch(ch chan int) {
	if resp, err := http.Get("https://byronz.github.com"); err == nil {
		if body, err := ioutil.ReadAll(resp.Body); err == nil {
			s := len(string(body))
			println(s)
			ch <- s
		}
	}
}

func main() {
	ch := make(chan int)
	defer close(ch)
	sum := 0

	for i := 1; i < 100; i++ {
		go fetch(ch)
	}

	for j := 1; j < 100; j++ {
		sum += <-ch
	}
	fmt.Println(sum)

}
```





