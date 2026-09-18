---
kernelspec:
  name: julia-livro-1.11
  display_name: "Julia 1.11 — livro"
---

# Vector Exercises  

Vectors let us implement much more complex algorithms. In this chapter, we'll
look at a few exercises.

## Permutation

Given a vector of integers, we want to check whether it contains a permutation.
To do this, we need to check whether a vector of size n contains the numbers from
1 to n, each appearing exactly once. The vector [3, 1, 2] is a permutation, since
it has size 3 and the elements from 1 to 3 each appear once.

One way to solve this problem is with a flag variable. We start by assuming the
vector is a permutation, then check whether all the numbers between 1 and n are
in the vector. This can be done with the in command, which checks whether an
element belongs to the vector.

```{code-cell} julia
function permutação(l)
    perm = true
    tamanho = length(l)
    i = 1
    while i <= tamanho
        if !(i in l)
            perm = false
        end    
        i += 1
    end
    return perm
end
```


Another alternative is to check, for each element of the vector, whether it is
between 1 and n and unique. In other words, we check whether the first element
is between 1 and n, and then scan the vector to see if it's unique. Then we do
the same for the following elements. The code looks like this:

```{code-cell} julia
function permutação(l)
    perm = true
    tamanho = length(l)
    i = 1
    while i <= tamanho
        if (l[i] > tamanho || l[i] <= 0)
            perm = false
        end
        j = i +1
        while j <= tamanho
            if l[j] == l[i]
                perm = false
            end
            j += 1
        end
        i += 1
    end
    return perm
end
```

Yet another alternative is to use an auxiliary vector where we count the
occurrences of each number between 1 and n. At the end, every element of this
auxiliary vector must equal 1. This time, we'll take the opportunity to add
automated tests as well.


```{code-cell} julia
using Test
function permutação(l)
    perm = true
    tamanho = length(l)
    aux = zeros(Int8, tamanho)
    for i in l
      if i < 1 || i > tamanho
        perm = false
      else
        aux[i] += 1
      end
    end
    for i in aux
      if i != 1
        perm = false
      end
    end  
    return perm
end

@testset "Verifica Permutação" begin
    @test permutação([1,2,3])
    @test permutação([3, 2, 1])
    @test permutação([1])
    @test permutação([2, 1])
    @test permutação([4, 2, 3, 1])
    @test !permutação([1, 1])
    @test !permutação([1, 3])
    @test !permutação([4, 2, 3, -1])
    @test !permutação([5, 2, 3, 1])
    @test permutação([])
    @test !permutação([0, 3, 3])
    @test !permutação([2, 2, 2])
end
```

## Histogram

Since we saw in the previous example how to "count" numbers, we can take it a
step further and compute the histogram of a vector containing numbers between
1 and 10.

```{code-cell} julia
using Test

function histograma(l)
    result = [0,0,0,0,0,0,0,0,0,0]
    i = 1
    while i <= length(l)
        valor_atual = l[i]
        if valor_atual >= 1 && valor_atual <= 10
           result[valor_atual] += 1
        end
        i += 1
    end
    return result
end

@testset "Verifica Histograma" begin
    @test [1,0,0,0,0,0,0,0,0,0] == histograma([1])
    @test [0,0,0,0,0,0,0,0,0,0] == histograma([-1])
    @test [0,0,1,0,0,0,0,0,0,0] == histograma([3])
    @test [0,0,0,0,0,0,0,0,0,1] == histograma([10])
    @test [0,0,0,0,0,0,0,0,0,0] == histograma([11])
    @test [1,4,0,2,5,1,0,1,0,0] == histograma([5,6,5,4,5,5,4,2,8,2,1,2,5,2])
    @test [0,0,0,0,0,0,0,0,0,0] == histograma([])
    end

```

## Modeling Problems with the Computer

The computer can be a powerful tool for modeling real-world problems.
Let's take the birthday problem as an example. This problem is also known as
the birthday paradox: calculate the probability that, in a room with n people,
at least two of them share the same birthday. This problem can be solved using
probability theory, which reveals that in a room of 23 people, the chance that
two of them share a birthday is just over 50%.

But we can also model this problem computationally. The first step is to
simplify the dates: instead of month and year, we can encode each day as a
number between 1 and 365, where 1 corresponds to January 1st. To solve the
problem, we can randomly draw n dates and check whether there's any
repetition; if there is, we've found two people with the same birthday.

This is represented in the experimento_niver function below. But to find out
the actual probability, we need to repeat the experiment many times. In the
main() function below, we ask for the number of experiments and the number of
people in order to run the simulation.


```julia
function experimento_niver(n)
    repetiu = false
    i = 1
    nivers = []
    while i <= n && (repetiu == false)
        niver = rand(1:365)
        if niver in nivers
            repetiu = true
        end
        push!(nivers, niver)
        i += 1
    end
    return repetiu
end

function main()
    print("Quantos experimentos? ")
    quantas = readline()
    print("Quantas pessoas? ")
    npessoas = readline()
    quantas = parse(Int64, quantas)
    npessoas = parse(Int64, npessoas)
    sucessos = 0
    i = 1
    while i <= quantas
        if experimento_niver(npessoas)
            sucessos += 1
        end
        i += 1
    end
    println("A probabilidade estimada é ", 100*sucessos/quantas, "%")
end
main()

```

With small variations, we can run other experiments, such as checking
whether more than two people share the same birthday. To do this, below
we count the number of repetitions.


```{code-cell} julia
function experimento_niver(n)
    repetiu = 0
    i = 1
    nivers = []
    while i <= n
        niver = rand(1:365)
        if niver in nivers
            repetiu += 1
        end
        push!(nivers, niver)
        i += 1
    end
    return repetiu >= 2
end
```
