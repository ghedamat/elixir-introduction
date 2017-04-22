name: inverse
layout: true
class: inverse

---
class: center, middle

# Introduction to Elixir

---

### About me

<img src="images/me.jpg" width="100%" height="100%">

---
# $ whoami

## Mattia - @ghedamat (github/twitter/slack)
## Developer @ [Precision Nutrition](https://precisionnutrition.com)
## Co-Organizer of ElixirTo ([@torontoelixir](https://twitter.com/torontoelixir))

---
# What is Elixir

<img src="http://i3.kym-cdn.com/photos/images/facebook/000/173/575/25810.jpg" width="100%" height="100%">

---

# A new(ish) programming language

## Based on the Erlang VM (BEAM)

<img src="images/elixiris.png" width="100%" height="100%">

---
# **Features**

# Functional

# Immutability

# Concurrency

# Metaprogramming (macros)

# Tooling (iex, hex, mix)

# Interop

---
# **Designed to build apps that are:**

# Distributed

# Fault-tolerant

# Scalable (horizontal and vertical)

# Highly-resilient

---

# **Why should I care?**

# Vertical Scaling has a limit

## increasing number of cores
## Fork model doesn't work that well
## Enables using all resources

# It's FAST and has great ergonomics

# Easy to learn/get started

# Big ecosystem

---

class: center, middle

# Let's dive in

## Warning: code ahead


---

# Basic Types

```elixir

iex> 42                        # integer

iex> 4.2                       # float

iex> true                      # boolean

iex> :ghedamat                 # atom

iex> "Toronto Elixir"          # string

iex> [1, 2, 3]                 # (linked) list

iex> {1, 2, 3}                 # tuple

iex> %{a: "foo", b: "bar"}     # map

iex> %{"a" => "foo", "b" => "bar"}

```

---

# Basic Operators

```elixir

iex> 10 + 5                    # 15

iex> "string" <> "concat"      # "stringconcat"

iex> "string #{5 - 1}"         # "string 4"

iex> true and false            # false

iex> [1, 2, 3] ++ [4, 5]       # [1, 2, 3, 4, 5]

iex> [1, 2, 3] -- [2]          # [1, 3]

iex> [1 | [2]]                 # [1, 2]

```

---

# Pattern Matching

```elixir

iex> x = 1
1

iex> x
1

iex> 1 = x
1

iex> 2 = x
** (MatchError) no match of right hand side value: 1

iex> 1 = unknown
** (CompileError) iex:1: undefined function unknown/0
```

---

# Pattern Matching (cont'd)

```elixir

iex> {a, b, c} = {:hello, "world", 42}
{:hello, "world", 42}

iex> a
:hello

iex> b
"world"

iex> {a, b, c} = {:hello, "world"}
** (MatchError) no match of right \
hand side value: {:hello, "world"}

iex> {a, b, c} = [:hello, "world", 42]
** (MatchError) no match of right \ 
hand side value: [:hello, "world", 42]

```

---

# Pattern Matching (common uses)

```elixir
iex> {:ok, result} = {:ok, 42}
{:ok, 42}
iex> result
42

iex> [head | tail] = [1, 2, 3]
iex> head
1
iex> tail
[2, 3]

iex> {a, _, c, _} = {:a, :b, :c, :d}

```

---

# Control Flow

```elixir
iex> case {1, 2, 3} do
...>   {4, 5, 6} ->
...>     "this won't match"
...>   {1, x, 3} ->
...>     "this will bind x to 2"
...>   _ ->
...>     "this matches all the time"
...> end

"this will bind x to 2"

iex> cond do
...>   2 + 2 == 5 ->
...>     "this won't be true"
...>   2 * 2 == 3 ->
...>     "this won't be true"
...>   1 + 1 = 2 ->
...>     "this will"
...>   true ->
...>     "always true"
...> end

"this will"
```

---

# Control Flow

```elixir

iex> if true do
...>   "this"
...> else
...>   "that"
...> end

"this"

iex> if true, do: 1 + 2
3

iex> if false, do: :this, else: :that
:that

```

---

# Modules 

## Functions are grouped in modules

```elixir
defmodule Area do
  def rectangle(l, w) do
    mult(l, w)
  end
  
  def square(l) do
    rectangle(l, l)
  end
  
  defp mult(a, b) do
    a * b
  end
end

iex> Area.rectangle(2, 3)
6
```

---

# Function Capturing

```elixir

defmodule Area do
  def square(l) do
    l * l
  end
end

iex> list = [1, 2, 3]

iex> Enum.map list, fn l -> Area.square(l) end

iex> Enum.map list, &Area.square/1

```

---

# Multiple Arities

```elixir

defmodule Area do
  def rectangle(l) do
    rectangle(l, l)
  end

  def rectangle(l, w) do
    l * w
  end
end

iex> Area.rectangle(2, 3)
6

iex> Area.rectangle(2)
4

```

---

# Multiple Patterns & Recursion

## as always matching happens in order of definition

```elixir

defmodule Math do
  def sum_list([head | tail], accumulator) do
    sum_list(tail, head + accumulator)
  end

  def sum_list([], accumulator) do
    accumulator
  end
end

iex>  Math.sum_list([1, 2, 3], 0)
6
```

---

# Pipe Operator

```elixir
odd? = &(rem(&1, 2) != 0)

list = 1..100_000
tripled_list = Enum.map(list, &(&1 * 3))
only_odds = Enum.filter(tripled_list, odd?)
total = Enum.sum(only_odds)

# or 
Enum.sum(Enum.filter(Enum.map(1..100_000, &(&1 * 3)), odd?))

# much nicer
1..100_000
  |> Enum.map(&(&1 * 3))
  |> Enum.filter(odd?)
  |> Enum.sum

```

---

class: center, middle

# What about this concurrency thing?

---

class: center, middle

# Announcements

## We have a new newsletter

## http://torontoemberjs.com

## Next month, final meetup of the year

## Demo December, Pharmacy Bar, Parkdale

## Please reach out with your demos!

## NO MEETUP IN JANUARY
