---
kernelspec:
  name: julia-livro-1.11
  display_name: "Julia 1.11 — livro"
---

# Going Beyond One Dimension (Matrices)

So far we have worked with structures that have more than one dimension, but
without looking closely at their type. In this lesson we will try to understand
the differences between them and how this can be used to our advantage.

Let's start with lists:

```{code-cell} julia
v = [1, 2, 3]
typeof(v)
```

The type returned is: Vector{Int64} (alias for Array{Int64, 1}). In this case
this means that v is a vector of integers, or a one-dimensional array. Likewise

```{code-cell} julia
v = zeros(Int64, 3)
typeof(v)
```

But vectors can be more flexible, as in the example below:

```{code-cell} julia
v = [1, 2.0, "três"]
typeof(v)
```

In this case the vector's type is no longer integers and becomes "Any",
that is, Vector{Any} (alias for Array{Any, 1}).

Furthermore, imagine the following situation:

```{code-cell} julia
a = [1, 2, 3]
push!(v, a)
typeof(v)
```

In this case, the vector remains of type Any, but in the fourth position we
now have a vector with three integers.
This shows us that vector structures can be quite flexible.
However, this flexibility with different types usually comes at some cost,
generally in performance.

On the other hand, structures can also have more than one dimension. These
are called matrices, and they can be created with the zeros function
that we already used above.

```{code-cell} julia
m = zeros(Int64, 3, 2)
typeof(m)
```

Above we created a two-dimensional matrix with three rows and two columns.
Its elements can be accessed just like in a vector, but now with two indices.

```{code-cell} julia
m[1, 2]  = 10
```
```{code-cell} julia
function imprime(m::Array{Int64,2})
    println(m)
end
```

```{code-cell} julia
function imprime(m::Vector{Vector{Int64}})
    println(m[1])
    println(m[2])
end
```


```{code-cell} julia
function imprime(m::Vector{Vector{Int64}})
    for i in m
        println(i)
    end
 end

```


```{code-cell} julia
function imprime(m::Vector{Vector{Int64}})
    for i in m
        for j in m[i]
            println(j,"  ")
        end   
    end
end
```


```{code-cell} julia
function imprime(m::Vector{Vector{Int64}})
    for i in m
        print("|")
        for j in i
            print(j,"  ")
        end
        println("|")   
    end
end
```

```{code-cell} julia
function imprimeMatriz(m::Matrix{Int64})
    println(m)
end 
```


```{code-cell} julia
function imprimeMatriz(m::Matrix{Int64})
    i = 1
    while i < size(m)[1]
        println(m[1])
        i += 1
    end
end
```

```{code-cell} julia
function imprimeMatriz(m::Matrix{Int64})
    i = 1
    while i < size(m)[1]
        j = 1
        while j < size(m)[2]
            print(m[i, j], " ")
            j += 1
        end
        println()   
        i += 1
    end
end

```

```{code-cell} julia
function preencheMatriz(m::Matrix{Int64})
    i = 1
    while i <= length(m)
        m[i] = rand(Int) % 10
        i += 1
    end
end

```

```{code-cell} julia
function criaIdentidate(tam::Int64)
    m = zeros(Int64, tam, tam)
    i = 1
    while i <= tam
        m[i, i] = 1
    end
    return m  
end
```

Direct operations on matrices such as +, - and *
