---
kernelspec:
  name: julia-livro-1.11
  display_name: "Julia 1.11 — livro"
---

# Good Practices

This chapter presents three good programming practices. They come from a whole
field dedicated to software development called Software Engineering.

## Using contracts

Whenever possible, code should be modular, that is, split into files
and/or functions. Each function should make clear what its parameters
are and what it returns.
This can be done using types.


```{code-cell} julia
function fatorial(n::Int64)::Int64
    if n < 2 
        return 1
    else  
        return n * fatorial(n - 1)
    end
end
```

With this, it becomes clear what the function receives and returns, and if a
different type than expected is passed in, we get an immediate error.

## Good practice 1: Use types

## Automated tests

To prevent errors, or bugs, from creeping in, an effective approach is to write code
that checks whether the code works. If this is done automatically, we have
automated tests.

```{code-cell} julia
using Test
function testaFat()
  @test fatorial(3) == 6
  @test fatorial(5) == 120
  @test fatorial(1) == 1
  @test fatorial(0) == 1
  @test fatorial(4) == 24
end
```

## Good practice 2: Write tests whenever possible

## Write code for humans, not for computers

Even though computers are capable of reading code that isn't always well
formatted, it's quite hard for humans to read code that doesn't follow a standard.
So here are some important tips:

- Use indentation. This makes blocks clear and makes it easy to identify
loops, if blocks, and function bodies.

- Choose variable and function names carefully. This makes the code much
easier for others to read.

- Whenever you spot a chance to improve the code,
do it. It's even better if you have automated tests, so you can check that
the improvement didn't break the code.

## Good practice 3: Write code for others to read


## Applying good practices

We'll now solve the following problem, applying the practices above.
Given a vector of real numbers, determine which numbers appear in the vector and the
number of times each of them occurs in it.

Analyzing the problem, we see that the input is a vector of real numbers,
which may contain repetitions. To determine which numbers are in the vector, we can
use another vector as output. Both the input and the output vectors should be of
type Float64. In addition, for the vector that gives the count of each number we
need a vector of integers. With that, we already have the function's signature.

```{code-cell} julia
function contHist(v::Vector{Float64}, el::Vector{Float64}, qtd::Vector{Int64})
end 
```
With this signature in hand, we can already write the tests.



```{code-cell} julia
function verifica(v::Vector{Float64}, elementos::Vector{Float64}, 
     quant::Vector{Int64})
     el = Float64[]
     quan = Int64[]
     contHist(v, el, quan)
     if el == elementos && quan == quant
        return true
     else
        return false
     end
end

function testaLista()
  @test verifica([1.3, 1.2, 0.0, 1.3], [1.3, 1.2, 0.0], [2, 1, 1])
  @test verifica([1.0, 1.0, 1.0, 1.0], [1.0], [4])
  @test verifica([8.3], [8.3], [1])
  @test verifica([3.14, 2.78, 2.78], [3.14, 2.78], [1, 2])
end
 
```

Finally, we can write the code. The idea behind the solution is simple:
we'll go through the input vector. For each element, there are two possibilities.
If it hasn't appeared before, we add the number to the output vector and
record one occurrence. If it has already appeared, we just increase the occurrence count.


```{code-cell} julia
function contHist(v::Vector{Float64}, el::Vector{Float64}, qtd::Vector{Int64})
    for a in v
        if a in el
            i = 1
            while el[i] != a
               i += 1
            end
            qtd[i] += 1
        else
            push!(el, a)
            push!(qtd, 1)
        end
    end
end
```
