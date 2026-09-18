---
kernelspec:
  name: julia-livro-1.11
  display_name: "Julia 1.11 — livro"
---

# Data Input and the Beginning of Lists
In this lesson, we have two main topics: how to perform data input, through
input commands and through command-line arguments. We will also see how to
work with a special type of variable, one that can store more than one
value.

## The Input Command
When we want to insert data in Julia, we can simply enter the data. But how
can we get a regular program to accept data as input?

For this, we have the readline() command, which pauses the program's
execution and waits for a String to be entered, which happens when the
"enter" key is pressed.

```julia

println("Digite o seu nome")
resposta = readline()
println("O seu nome é: ", resposta)
```

If, when running the program, you type `Maria` and press enter, the final output of your program will be `O seu nome é: Maria`.

Since readline() reads Strings, if we want to read numbers, we need to use
the parse command. In its simple form the parse command 
takes two parameters: the first is the type we want to convert to, 
and the second is the original value.

```julia

println("Digite um inteiro")
valor = parse(Int64, readline())
println("O numero digitado foi ", valor)
```


Now that we know how to read numbers from the keyboard, let's try a simple
exercise: read a sequence of integers terminated by zero and return their
sum.

```julia

function somaVarios()
    soma = 0.0
    println("Digite um número")
    n = parse(Float64, readline())
    while  n!=0
        soma = soma + n
        println("Digite um número")
        n = parse(Float64, readline())
    end
    println("A soma é: ", soma)
end
``` 

Look at the following example, which calculates the squares of the numbers
in a list terminated by zero.

```julia

function leQ()
  x = readline()
  n = parse(Float64, x)
  while n != 0
    println("$n ao quadrado é ", n * n)
    x = readline()
    n = parse(Float64, x)
  end
end
```

Note that readline can also take a file variable so that data is read
directly from it. In this case, however, we need to be careful to open
(open()) and close (close()) the file, as shown below:

```julia

function leQ()
    println("Digite um número")
    f = open("numeros.txt", "r+")
    x = readline(f)
    n = parse(Float64, x)
    while n != 0
        println("$n ao quadrado é ", n * n)
        println("Digite outro número")
        x = readline(f)
        n = parse(Float64, x)
    end
    close(f)
end
```

## Reading Through the Command Line

The other way to read input is through the ARGS constant, which is set up
when a program is called. To understand this better, let's look at the
following program.

```julia

println(ARGS)
```

If the line above is in the file args.jl, calling julia args.jl with different parameters will produce different results.

For example, when calling:

julia args.jl 1 2 3 abc

We will get the following response:

```julia

["1", "2", "3", "abc"]
```

Let's take a closer look at this response, noting that each parameter
occupies a position.

```julia

tam = length(ARGS)
println("O tamanho dos argumentos é: ", tam)
for i in 1:tam
    println(ARGS[i])
end
```

 Looking at the code above, we can see that the length() function returns
the number of arguments, that is, the size of the ARGS list. In addition,
using square brackets we can access each position of the list
individually.


The example below adds up the integer parameters given as arguments. It
also illustrates a good practice: always organizing code into modules, in
this case into functions:

```julia

function SomaEntrada()
    tam = length(ARGS)
    s = 0
    i = 1
    while i <= tam
        valor = parse(Int, ARGS[i])
        println(valor)
        s = s + valor
        i = i + 1
    end
    println("A soma foi: ", s)
end
SomaEntrada()
```

Lists give us a lot of flexibility. For this reason, lists, or arrays,
deserve a topic of their own.

## Lists

Let's first play around a bit in the console.

```{code-cell} julia
vetor = [1, 2, 3]
println(vetor[1])
println(length(vetor))
vetor[2] = vetor[2] + 1
vetor[1] = 2 * vetor[3]
println(vetor)
``` 

As mentioned before, the for loop was made to work with arrays. Let's look
at a few functions, the first one prints the elements of an array one per
line.

```julia

function imprimeVetor(v)
    for el in v
        println(el)
    end
end
```

This can also be done using the array's indices:

```julia

function imprimeVetor(v)
    for i in 1:lenght(v)
        println(v[i])
    end
end
```

Since each position is independent, we can calculate the sum of the odd
elements of an array

```julia

function somaImpVetor(v)
    soma = 0
    for i in 1:length(v)
        if v[i] % 2 == 1
            soma = soma + v[i]
        end
    end
    return soma
end
``` 

We also saw a few other examples in class, such as calculating the average
of the elements in an array.

```julia

function mediaV(v)
   soma = 0.0
   for i in v
      soma = soma + i
   end
   return soma / length(v)
end  
```

Return the sum of the odd elements of an array

```julia

function somaImpar(v)
    soma = 0
    for i in v
        if i % 2 == 1
            soma = soma + i
        end
    end
    return soma
end
```

Print the numbers in an array that are divisible by 5.

```julia

function imprimeDivisivelPor5(v)
    for i in v
        if i % 5 == 0
            println(i)
        end
    end
end
```

With a small variation, using the push!() command, we can see how to
return an array with the numbers divisible by 5.

```julia

function devolveDivisivelPor5(v)
    x = []  # começa com um vetor vazio
    for i in v
        if i % 5 == 0
            push!(x, i)  # adiciona um elemento ao vetor x
        end
    end
    return x
end
``` 


## Linear Algebra and Lists

Manipulating lists is a fundamental part of linear algebra, which studies vectors and matrices. Functions like the dot product of two vectors are classic examples. Below are two examples of the dot product of two vectors. Recall that it is defined
as the sum of the products of elements in matching positions.

```julia

function dotProduct(a, b)
    soma = 0
    if length(a) != length(b)
       return soma   # o produto não está definido se os tamanhos são diferentes
    end
    for i in 1:length(a)
        soma = soma + a[i] * b[i]
    end
    return soma
end
```

Above, we saw that a special case of using for consists of making the for loop vary between 
1 and a size (1:lenght(a))

Notice the difference in the version below:


```julia

function dotProduct(a, b)
    soma = 0
    if length(a) != length(b)
       return soma   # o produto não está definido se os tamanhos são diferentes
    end   
    i = 1
    for x in a
        soma = soma + x * b[i]
        i = i + 1
    end 
    return soma
end
```

## Permutation Exercise

To finish, let's write a function that, given a vector of integers of
size $n$, checks whether that vector is a permutation of the numbers from
1 to $n$. To do this, we will check whether each number from 1 to $n$ is
in the vector.

But, without forgetting the tests:

```julia

@testset "Verifica Permutação" begin
    @test permuta([1,2,3])
    @test permuta([3, 2, 1])
    @test permuta([1])
    @test permuta([2, 1])
    @test permuta([4, 2, 3, 1])
    @test !permuta([1, 1])
    @test !permuta([1, 3])
    @test permuta([])
end
```

and the code:

```julia

function permuta(v)
   tam = length(v)
   for i in 1:tam
      if  !(i in v)
         return false
      end
   end
   return true
end
```

We used Julia's in command, which checks whether an element is in the vector.
