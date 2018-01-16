---
title: "Elixir Part I - Playbook"
date: 2017-11-30T17:55:57-05:00
draft: true
tags: ['programming', 'elixir', 'erlang']
---

---
https://joearms.github.io/published/2013-05-31-a-week-with-elixir.html

# Pattern Matching

equal sign = is not an assignment, is a **match operator**

^ **pin operator**  forces using existing values

# Immutable

```elixir
iex(13)> l1 = [1,2,3]
[1, 2, 3]
iex(14)> l2 = [ l1 | 'a']
[[1, 2, 3], 97]
iex(16)> l2 = [ 'a'| l1]
['a', 1, 2, 3]
```

# Basics

Value Types

```elixir
iex(23)> 0o3407
1799
iex(24)> 0b111100
60
iex(25)> 0x33ff
13311
iex(28)> :var@2
:var@2
iex(29)> 1..100
1..100
iex> Regex.run ~r{[aeiou]}, "caterpillar"
["a"]
iex> Regex.scan ~r{[aeiou]}, "caterpillar"
[["a"], ["e"], ["i"], ["a"]]
```

System Types

```elixir
iex(31)> self
#PID<0.84.0>
iex(32)> make_ref
#Reference<0.2792161163.4208984065.108413>
```

Collection Types

```elixir
#tuple
iex(34)> {3,3,44,'adada',"adada", :adad, 33.33, 1..20}
{3, 3, 44, 'adada', "adada", :adad, 33.33, 1..20}
# typical funcition call
iex(38)> {status, file} = File.open('load.jmx')
{:ok, #PID<0.142.0>}
# list (lisp-like)

iex(40)> [1,2]++[3,4]
[1, 2, 3, 4]
iex(41)> [1,3,4]--[1,]
[3, 4]
iex(43)> 2 in [3,4]
false

# keyword list
iex(46)> [1,name: "Dave", city: "Dallas", likes: "Programming"]
[1, {:name, "Dave"}, {:city, "Dallas"}, {:likes, "Programming"}]
iex(47)> {1, fred: 1, dave: 2}
{1, [fred: 1, dave: 2]}
```

Maps
```elixir
iex(48)> states = %{ "AL" => "Alabama", "WI" => "Wisconsin" }
%{"AL" => "Alabama", "WI" => "Wisconsin"}

iex(50)> colors = %{ red: 0xff0000, green: 0x00ff00, blue: 0x0000ff }
%{blue: 255, green: 65280, red: 16711680}
iex(51)> colors['red']
nil
iex(52)> colors["red"]
nil
iex(53)> colors[:red]
16711680
iex(54)> colors.red
16711680
iex(55)> colors.black
** (KeyError) key :black not found in: %{blue: 255, green: 65280, red: 16711680}
```
Binaries

```elixir
iex(64)> bin = <<3 :: size(2), 5 :: size(4), 1 :: size(2)>>
<<213>>
iex(58)> :io.format("~-8.2b~n", :binary.bin_to_list bin)
11010101
:ok
iex(63)> 0b11010101
213
```

Dates & Times

```elixir
iex(65)> d1 = Date.new(2016, 12, 25)
{:ok, ~D[2016-12-25]}
iex(76)> t1 = Time.new(12, 34, 56)
{:ok, ~T[12:34:56]}
```

Operators

```elixir
iex(100)> div(3,1)
3
iex(101)> 3/1
3.0

iex(108)> [1,3,4] -- [2,1]
[3, 4]
iex(109)> [1,3,4] ++ [2,1]
[1, 3, 4, 2, 1]
```

# Anonymous Functions

```elixir
iex(1)> sum = fn (a,b) -> a+b end
#Function<12.99386804/2 in :erl_eval.expr/5>
iex(2)> sum.(1,2)
3

iex(3)> o99 = fn -> 99 end
#Function<20.99386804/0 in :erl_eval.expr/5>
iex(4)> o99.()
99

iex(6)> greet = fn msg -> IO.puts msg end
#Function<6.99386804/1 in :erl_eval.expr/5>
iex(10)> greet.([80, 77])
PM
:ok
```

function is first class

& notation

```elixir
iex(32)> add_one = &(&1+1)
#Function<6.99386804/1 in :erl_eval.expr/5>
iex(33)> add_one.(2)
3

iex(36)> speak = &(IO.puts &1)
&IO.puts/1
# elixir figures out it's a direct wrapper of built in call, so it associate the anonymous func directly

iex(40)> Enum.map [1,2,3,4], &(&1 + 1)
[2, 3, 4, 5]

iex(44)> Enum.each [1,3,4,5], & IO.inspect &1
1
3
4
5
:ok
iex(45)> Enum.each [1,3,4,5], &IO.inspect/1
1
3
4
5
:ok
```


# Modules and Functions

do...end is syntactic sugar for do: ()

```elixir
# it's common pattern in elixir

defmodule Maths do
  def sum(0), do: 0
  def sum(n), do: n + sum(n-1)

  def gcd(x, 0), do: x
  def gcd(x, y), do: gcd(y, rem(x, y))

  def fact(0), do: 1
  def fact(n) when n > 0, do: n * fact(n-1)
end


```

default parameter `param \\ val`

defp is private function

Pipe operator `|>`

```elixir
iex(10)> (1..25) |> Enum.map(&(&1*&1)) |> Enum.filter(&(rem(&1, 2) == 0))
[4, 16, 36, 64, 100, 144, 196, 256, 324, 400, 484, 576]

```

Module Directives

- import Module [, only: | except: ]
- alias My.Import.Module.{Parser, Runner}
- require for marcros

Module Attributes

```Elixir
@author "dave"
def get_author do
  @author
end
```

# Operators


- list `++` and `--`
- string `<>`
- boolean `and or not`


# List and Recursion

```elixir
# head and tail with recursion
iex(26)> [1 | [2 | [3 | []]]]
[1, 2, 3]

# pattern match
iex(29)> [head | [2,3]] = [1,2,3]
[1, 2, 3]
iex(30)> [head | [3]] = [1,2,3]
** (MatchError) no match of right hand side value: [1, 2, 3]

# count lenght
defmodule Listxx do
  def len([]), do: 0
  def len([_ | tail]), do: 1 + len(tail)
end

```

# Map, Keyword lists, Sets and Structs

Keyword

- keys must be atoms
- keys are ordered
- keys can be redundant

```elixir
iex(63)> if 0, do: :this, else: :that
:this
iex(64)> if false, do: :this, else: :that
:that
# this can be considered the `if` function using keyword list as parameters
```

Map

```elixir

iex(88)> map = %{:a => 1, 2 => :b}
%{2 => :b, :a => 1}
# map update using pipe
iex(89)> %{map | 2 => "two"}
%{2 => "two", :a => 1}
```

Struct

```elixir
defmodule Ski do
  defstruct name: "", distance: 0, eclaire: false, vertical: 0
end

iex(97)> bromont = %Ski{name: "Ski Bromont", distance: 100}
%Ski{distance: 100, eclaire: false, name: "Ski Bromont", vertical: 0}
iex(98)> bromont.name
"Ski Bromont"
iex(99)> bromont.distance
100
iex(100)> orford = %{bromont | name: "Orford",  distance: 110, eclaire: true}
%Ski{distance: 110, eclaire: true, name: "Orford", vertical: 0}
```
