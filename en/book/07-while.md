---
kernelspec:
  name: julia-livro-1.11
  display_name: "Julia 1.11 — livro"
---

(sec-while)=
# Iterative Repetition Structures

So far, we've seen that computers are very good at performing calculations and repetitions. We carried out these repetitions using recursive functions, where a function calls itself to solve smaller problems. Now we'll look at an alternative way of performing repetitions: the iterative approach.

## Introduction to `while`

The `while` structure is one of the most fundamental ways of creating loops in programming. It allows a block of code to be executed repeatedly while a specific condition is true, in an iterative fashion. The basic syntax of `while` in Julia is:

```julia
while condição
    # Bloco de código a ser repetido
end
```

The `while` structure works through these steps:

1. The condition is evaluated
2. If the condition is true, the block of code is executed
3. After the block runs, the condition is evaluated again
4. This cycle continues until the condition becomes false

An important thing to understand about `while` is that, to avoid an infinite loop (a loop that never ends), something related to the condition must be changed inside the block of code.

Let's start with a simple example: a countdown.

```{code-cell} julia
function contagem_regressiva(n)
    while n > 0
        println(n)
        n = n - 1  # Esta linha é essencial para evitar um loop infinito
    end
    println("Fim!")
end

contagem_regressiva(5)
```

In this example, the condition `n > 0` is initially true (assuming `n` starts with a positive value). The block of code prints the current value of `n` and then decrements `n` by 1. Eventually, `n` will reach zero, making the condition false and ending the loop.

## Comparing Recursion and Iteration

To better understand the difference between recursion and iteration, let's rewrite some functions we previously implemented using recursion.

## Countdown

First, let's recall the recursive version of the countdown:

```{code-cell} julia
function contagem_recursiva(n)
    if n <= 0
        println("Fim!")
    else
        println(n)
        contagem_recursiva(n - 1)
    end
end

contagem_recursiva(5)
```

Comparing the two implementations, we can observe that:
- In the recursive version, the base case (`n <= 0`) corresponds to the `while` loop's stopping condition
- The recursive call with `n - 1` corresponds to the update of `n` in the `while` loop

Both versions produce the same result, but with different approaches.

## Sum of the First N Numbers

Let's implement a function that calculates the sum of the first `n` positive integers (1 + 2 + ... + n), using both recursion and `while`.

Recursive version:

```{code-cell} julia
function soma_recursiva(n)
    if n <= 0
        return 0
    else
        return n + soma_recursiva(n - 1)
    end
end

println("Soma dos primeiros 5 números (recursiva): ", soma_recursiva(5))
```

Version with `while`:

```{code-cell} julia
function soma_while(n)
    soma = 0
    i = 1
    
    while i <= n
        soma = soma + i
        i = i + 1
    end
    
    return soma
end

println("Soma dos primeiros 5 números (while): ", soma_while(5))
```

In the recursive version, we have an explicit base case (`n <= 0`) and a recursive call that reduces the problem. In the `while` version, we use a control variable `i` that is incremented on each iteration, and an accumulator variable `soma` that stores the partial result.

## Calculating Mathematical Series

Repetition structures are especially useful for calculating sums of mathematical series. Let's implement a function to calculate the approximation of sine using the Taylor series:

$$\sin(x) = x - \frac{x^3}{3!} + \frac{x^5}{5!} - \frac{x^7}{7!} + \ldots$$

This series can be represented as:

$$\sin(x) = \sum_{n=0}^{\infty} \frac{(-1)^n \cdot x^{2n+1}}{(2n+1)!}$$

Implementation using `while`:

```{code-cell} julia
function sin_taylor(x, termos = 10)
    resultado = 0.0
    termo = x
    i = 0
    
    while i < termos
        # Adicionamos o termo atual à soma
        resultado = resultado + termo
        
        # Calculamos o próximo termo
        i = i + 1
        termo = -termo * x * x / ((2 * i) * (2 * i + 1))
    end
    
    return resultado
end

# Teste com π/6 (30 graus), cujo seno é 0.5
println("sin(π/6) ≈ ", sin_taylor(π/6))
println("sin(π/6) exato: ", sin(π/6))
```

Notice how `while` allows precise control over the number of terms of the series we want to calculate.

Let's compare this with a recursive implementation:

```{code-cell} julia
function sin_taylor_recursivo(x, i = 0, termos = 10, termo = x, resultado = 0.0)
    if i >= termos
        return resultado
    else
        # Adicionamos o termo atual à soma
        novo_resultado = resultado + termo
        
        # Calculamos o próximo termo
        novo_i = i + 1
        novo_termo = -termo * x * x / ((2 * novo_i) * (2 * novo_i + 1))
        
        return sin_taylor_recursivo(x, novo_i, termos, novo_termo, novo_resultado)
    end
end

println("sin(π/6) recursivo ≈ ", sin_taylor_recursivo(π/6))
```

The recursive version is more complex here, since it needs several additional parameters to maintain state between recursive calls. The `while` version is clearer and more straightforward in this case.

## When to Use Recursion and Iteration?

Both recursion and iteration can be used to solve repetition problems, each with its own strengths:

**Advantages of iteration**

- Generally more memory efficient
- Avoids the risk of stack overflow for large inputs
- Can be more intuitive for simple repetition operations
- Allows more detailed control over the iteration process

**Advantages of recursion**

- Often more elegant for problems that decompose naturally
- Can make code more concise and readable for certain algorithms
- Directly reflects recursive mathematical definitions
- Particularly useful for hierarchical data structures

A practical rule of thumb is:

- Use iteration when you need to repeat an operation a fixed or indeterminate number of times
- Use recursion when the problem can naturally be divided into smaller subproblems of the same type

## Check Your Understanding

1. What is the difference between using recursion and iteration for repetition?
2. Implement a function that counts the number of digits in an integer using the `while` structure.
3. Given an integer, write a function that reverses its digits (for example, 123 would become 321).

## Explore on Your Own

1. Research loop optimization (loop unrolling) and try applying this concept to a function using `while`.
