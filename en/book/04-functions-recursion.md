---
kernelspec:
  name: julia-livro-1.11
  display_name: "Julia 1.11 — livro"
---
(sec-functions)=
# Introduction to Functions

The goal of this chapter is to understand the concept of functions in programming and how they are implemented in Julia. We will explore how to create our own functions, how they can receive parameters and return values, and we will also introduce the concept of recursion.

## Functions as Natural Abstractions

In the previous class, we already used some predefined functions in Julia. Functions are blocks of code that perform specific tasks and can be reused whenever needed. They let us abstract complex operations into simple commands, making code more readable and modular.

Let's recall some of the functions we've already used:

- `typeof()` - Takes a value as a parameter and returns its type.
- `div()` - Takes two numbers and returns the integer division of the first by the second.
- `print()` and `println()` - Print values to the console, with the second one adding a line break after printing.

So far, we've seen examples of **function calls** like `sin(0.5)` or `sqrt(4)`. When we write the name of a function followed by parentheses containing arguments, we are "calling" or "invoking" that function.

But how are these functions created? In Julia, we can declare our own functions using the `function` keyword:

```{code-cell} julia
function dobro(x)
    return x * 2
end
```

Here, `dobro` is the name of the function, `x` is a parameter (a value that the function receives), and `return x * 2` specifies what the function should compute and return when called. Providing parameters to a function is optional.

After declaring the function, we can call it multiple times with different arguments:

```{code-cell} julia
resultado1 = dobro(5)    # Chama a função com o argumento 5
println(resultado1)      # Imprime 10

resultado2 = dobro(3.5)  # Chama a função com o argumento 3.5
println(resultado2)      # Imprime 7.0
```

Declaring a function and calling it are two different actions:

- **Declaration**: Defines how the function should behave (happens once)
- **Call**: Executes the function with specific values (can happen multiple times)

Functions like `sin()`, `sqrt()`, and `big()` already come declared in Julia, which is why we can use them directly.

## Functions Calling Other Functions

A function can call another function, allowing for the composition of more complex operations:

```{code-cell} julia
function imprime(a)
   println("Vou imprimir ", a)
end

function imprimeduasvezes(a)
   imprime(a)
   imprime(a)
end
```

Let's test our new function:

```{code-cell} julia
imprimeduasvezes(13)
```

## Accessing Function Documentation

We can ask the interpreter for help to better understand how these functions work. To do this, we use the question mark `?` or the `@doc` macro before the function name:

```julia
# Exemplos de como acessar a documentação
@doc typeof
```

```julia
@doc div
```

```julia
@doc println
```

By checking the documentation, we discover that some functions such as `div()` can be used with an alternative syntax, such as `\div`. This kind of notation is particularly useful for mathematical operations.

## Conversion Functions

An important category of functions in Julia are conversion functions, which transform values from one type into another. Let's look at some examples:

```{code-cell} julia
# Converte uma string para um número em ponto flutuante
parse(Float64, "32")
```

```{code-cell} julia
# Converte um número em ponto flutuante para um inteiro (removendo a parte decimal)
trunc(Int64, 2.25)
```

```{code-cell} julia
# Converte um inteiro para um número em ponto flutuante
float(2)
```

```{code-cell} julia
# Converte um número para uma string
string(3)
```

```{code-cell} julia
# Converte um número em ponto flutuante para uma string
string(3.57)
```

## Mathematical Functions

Julia has a large library of ready-to-use mathematical functions. Here are some of the most common ones:

| Function          | Description                                        |
|:------------------|:--------------------------------------------------|
| `sin(x)`          | Computes the sine of \( x \) in radians            |
| `cos(x)`          | Computes the cosine of \( x \) in radians          |
| `tan(x)`          | Computes the tangent of \( x \) in radians         |
| `deg2rad(x)`      | Converts \( x \) from degrees to radians           |
| `rad2deg(x)`      | Converts \( x \) from radians to degrees           |
| `log(x)`          | Computes the natural logarithm of \( x \)          |
| `log(b, x)`       | Computes the logarithm of \( x \) in base \( b \)  |
| `log2(x)`         | Computes the base-2 logarithm of \( x \)           |
| `log10(x)`        | Computes the base-10 logarithm of \( x \)          |
| `exp(x)`          | Computes the natural exponential of \( x \)        |
| `abs(x)`          | Computes the absolute value of \( x \)             |
| `sqrt(x)`         | Computes the square root of \( x \)                |
| `cbrt(x)`         | Computes the cube root of \( x \)                  |
| `factorial(x)`    | Computes the factorial of \( x \)                  |

A good way to get familiar with these functions is to experiment with different values and check the results. For more complex functions, there may already be ready-made implementations in Julia. A useful tip is to search the internet using keywords like "julia lang hyperbolic sin" to find the function you need. In general, searching in English tends to produce better results.

## Function Overloading

In Julia, we can have functions with the same name but with different numbers or types of parameters. This is called "function overloading":

```{code-cell} julia
function recebe(a)
  println("Recebi um parâmetro: ", a)
end

function recebe(a, b)
  println("Recebi dois parâmetros: ", a, " e ", b)
end
```

The interpreter decides which version of the function to call based on the arguments provided:

```{code-cell} julia
recebe(1)
```

```{code-cell} julia
recebe(1, 2)
```

We can also call functions using variables and expressions as arguments:

```{code-cell} julia
a = 10
recebe(a)
recebe(a, a + 1)
```

## Functions That Return Values

So far, we've seen functions that only print messages but don't return any value. The return type of these functions is `Nothing`, indicating that they don't produce a value that can be assigned to a variable.

However, we often want our functions to compute and return values. For this, we use the `return` keyword:

```{code-cell} julia
function soma1(a)
  return a + 1
end
```

Now we can use this function in expressions and assignments:

```{code-cell} julia
resultado = soma1(5)
println("O resultado é: ", resultado)
```

```{code-cell} julia
# Também podemos usar o resultado em outras expressões
println("Resultado multiplicado por 2: ", soma1(5) * 2)
```

We can create functions for more complex calculations:

```{code-cell} julia
function hipotenusa(a, b)
  hip = sqrt(a^2 + b^2)
  return hip
end
```

Let's test our function:

```{code-cell} julia
# Calculando a hipotenusa de um triângulo 3-4-5
hipotenusa(3, 4)
```

## Introduction to Recursion

Now let's explore a fundamental concept in programming: recursion. A recursive function is one that calls itself as part of its execution. This may seem strange at first, but it's a powerful technique for solving certain kinds of problems.

Let's start with a simple example: computing the factorial of a number. The factorial of $n$ (written $n!$) is the product of all positive integers less than or equal to $n$. For example, $5! = 5 \times 4 \times 3 \times 2 \times 1 = 120$.

The factorial can be defined recursively as:

- Base case: $0! = 1$
- Recursive case: $n! = n \times (n-1)!$

Let's implement this in Julia:

```{code-cell} julia
function fatorial(n)
  if n == 0
    return 1  # Caso base
  else
    return n * fatorial(n - 1)  # Chamada recursiva
  end
end
```

Let's test our function:

```{code-cell} julia
fatorial(5)
```

To understand how recursion works, let's trace through the computation of `fatorial(3)` step by step:

1. We call `fatorial(3)`
   - Since $3$ is not equal to $0$, we execute `return 3 * fatorial(2)`
2. Now we need to compute `fatorial(2)`
   - Since $2$ is not equal to $0$, we execute `return 2 * fatorial(1)`
3. Now we need to compute `fatorial(1)`
   - Since $1$ is not equal to $0$, we execute `return 1 * fatorial(0)`
4. Now we need to compute `fatorial(0)`
   - Since $0$ equals $0$, we return $1$
5. Now we can complete the computation of `fatorial(1)` = $1 \times 1 = 1$
6. Now we can complete the computation of `fatorial(2)` = $2 \times 1 = 2$
7. Finally, we complete the computation of `fatorial(3)` = $3 \times 2 = 6$

Recursion has two fundamental parts:

1. A **base case** that ends the recursion (in our example, when $n = 0$)
2. A **recursive case** that moves the problem closer to the base case (in our example, by reducing $n$ by $1$)

The recursion must always reach the base case, otherwise the function will keep calling itself indefinitely, causing a stack overflow error.

## More Recursion Examples

Let's implement a recursive function for a countdown:

```{code-cell} julia
function contagem(n)
    if n < 0
        println("Fim!")
    else
        print(n, " ")
        contagem(n - 1)
    end
end
```

Let's test our function:

```{code-cell} julia
contagem(5)
```

We can also use recursion to compute the sum of the first $n$ integers:

```{code-cell} julia
function soma(n)
  if n == 0
    return 0  # Caso base
  else
    return n + soma(n - 1)  # Caso recursivo
  end
end
```

Let's test our function:

```{code-cell} julia
soma(10)
```

Another interesting example is computing the sum of the terms of the harmonic series:

```{code-cell} julia
function somaharmonica(atual, n)
  # Caso base: quando chegamos ao último termo
  if atual > n
    return 0.0
  else
    # Caso recursivo: somamos o termo atual e chamamos a função para o próximo termo
    return 1.0 / atual + somaharmonica(atual + 1, n)
  end
end
```

Let's compute the sum of the first 10 terms of the harmonic series:

```{code-cell} julia
somaharmonica(1, 10)
```

## Check Your Understanding

1. What is the difference between a function that prints a value and a function that returns a value? Why does this distinction matter?
2. Explain the concept of recursion in your own words. What are the essential components of a recursive function?
3. Write a function that takes a positive integer and returns the sum of its digits. For example, for the number $123$, the function should return $1+2+3 = 6$.
4. Implement a function that computes the $n$-th number of the **Fibonacci sequence** using recursion. Recall that
    - $Fib(0) = 0$
    - $Fib(1) = 1$
    - $Fib(n) = Fib(n-1) + Fib(n-2)$, if $n > 1$.
5. Write a function that takes two numbers as parameters and returns their **greatest common divisor (GCD)** using Euclid's algorithm recursively.

## Explore on Your Own

1. Research the concept of the "call stack" and how it relates to recursion. What are the practical limitations of recursion due to the call stack?
2. Think about how you could optimize the recursive Fibonacci function to avoid repeated computations.
    - Hint: look up "memoization".
3. Explore functions with a variable number of arguments in Julia using "splat" syntax (`...`).
4. Find out how you can define default values for function parameters in Julia.
