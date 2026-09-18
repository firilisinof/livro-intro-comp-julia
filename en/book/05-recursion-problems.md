---
kernelspec:
  name: julia-livro-1.11
  display_name: "Julia 1.11 — livro"
---
# More Problems Involving Recursion

In the previous chapter, we introduced the concept of recursion and saw how functions can call themselves to solve problems. In this chapter, we'll explore a few more problems that can be solved using recursion, which will help us develop our ability to "think recursively."

## Thinking Recursively

Solving problems using recursion requires a shift in how we look at problems. Instead of thinking about all the steps needed to reach the solution, we focus on:

1. How to express the problem in terms of a smaller version of the same problem
2. When to stop the recursion (base case)

Let's explore four problems that illustrate the power of recursion well: the staircase problem, exponentiation by squaring, computing a square root using Heron's method, and the binomial coefficient.

## The Staircase Problem

Imagine a staircase with $n$ steps. You can climb 1 or 2 steps at a time. In how many different ways can you reach the top of the staircase?

For example, if we have a staircase with 3 steps, there are 3 ways to climb it:

- Take three steps of 1: (1, 1, 1)
- Take a step of 1 followed by a step of 2: (1, 2)
- Take a step of 2 followed by a step of 1: (2, 1)

How can we think about this problem recursively? To reach step $n$, we must have come from step $n-1$ (by taking a step of 1) or from step $n-2$ (by taking a step of 2). Therefore, the total number of ways to reach step $n$ is the sum of the number of ways to reach step $n-1$ and step $n-2$.

Our base cases are:

- If $n = 0$ (no steps): there is 1 way (don't climb at all)
- If $n = 1$ (one step): there is 1 way (take a step of 1)

Let's implement this solution:

```{code-cell} julia
function maneiras_subir_escada(n)
    # Casos base
    if n == 0 || n == 1
        return 1
    else
        # Caso recursivo: soma das maneiras de chegar a partir de n-1 e n-2
        return maneiras_subir_escada(n - 1) + maneiras_subir_escada(n - 2)
    end
end
```

Let's test our code for different numbers of steps:

```{code-cell} julia
for i in 1:10
    println("Escada com $i degraus: $(maneiras_subir_escada(i)) maneiras diferentes")
end
```

Note that we're using a different structure here, `for`. Don't worry about the details of this structure for now, but know that we're simply repeating the execution of a block of code.

If you look closely at this sequence of results, you'll notice that it corresponds to the famous Fibonacci sequence: $(0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144, 233, 377, \dots)$. This is no coincidence. The staircase problem and the Fibonacci sequence share the same recursive structure.

## Exponentiation by Squaring

When we want to compute powers like $a^n$, the simplest approach would be to multiply $a$ by itself $n$ times. For example, to compute $2^8$, we would do $2 \times 2 \times 2 \times 2 \times 2 \times 2 \times 2 \times 2$, performing 7 multiplications. But there is a much more efficient approach using recursion.

The exponentiation-by-squaring method we're about to present can compute the same value using only about $\log_2 n$ operations. For example, to compute $2^8$, we would need only 3 multiplications. For larger numbers, this difference is even more significant: computing $2^{1000}$ would require 999 multiplications with the simple method, but only about 10 multiplications with our recursive method.

The idea is based on the following mathematical properties:

- If $n$ is negative: $a^n = 1 / a^{-n}$ (we invert the result of the positive power)
- If $n$ is even: $a^n = (a^{n/2})^2$ (we compute "half" the power and square it)
- If $n$ is odd: $a^n = a \times a^{n-1}$ (we multiply by an even power, which we know how to compute)

How does this translate into recursive thinking?

1. If we want to compute $a^n$ and $n$ is even:
   - First we compute $a^{n/2}$ (a smaller problem)
   - Then we multiply that result by itself

2. If we want to compute $a^n$ and $n$ is odd:
   - First we compute $a^{n-1}$ (which is even, and we know how to solve it using the previous case)
   - Then we multiply that result by $a$

3. If we want to compute $a^n$ and $n$ is negative:
   - We compute $a^{-n}$ (we know how to solve this using the previous cases)
   - Then we compute $1 / a^{-n}$

Our base cases (where the recursion stops) are:

- If $n = 0$, then $a^n = 1$ (any number raised to 0 is 1)
- If $n = 1$, then $a^n = a$ (any number raised to 1 is itself)

Let's implement this solution:

```{code-cell} julia
function potenciacao(base, expoente)
    # Resolvemos o expoente negativo primeiro
    if expoente < 0
        return 1 ÷ potenciacao(base, -expoente)
    end
    
    if expoente == 0
        return 1
    elseif expoente == 1
        return base
    end
    
    # Se o expoente for par
    if expoente % 2 == 0
        temp = potenciacao(base, expoente ÷ 2)
        return temp * temp
    else
        # Se o expoente for ímpar
        return base * potenciacao(base, expoente - 1)
    end
end
```

Let's test our function:

```{code-cell} julia
println(potenciacao(2, 10))  # Deve retornar 1024
println(potenciacao(3, 5))   # Deve retornar 243
```

To better understand how this approach saves operations, let's trace the execution of `potenciacao(2, 10)`:

**Step 1**: `potenciacao(2, 10)`

- `expoente = 10` is even
- We need to compute `potenciacao(2, 5)^2`

**Step 2**: `potenciacao(2, 5)`

- `expoente = 5` is odd
- We need to compute `2 * potenciacao(2, 4)`

**Step 3**: `potenciacao(2, 4)`

- `expoente = 4` is even
- We need to compute `potenciacao(2, 2)^2`

**Step 4**: `potenciacao(2, 2)`

- `expoente = 2` is even
- We need to compute `potenciacao(2, 1)^2`

**Step 5**: `potenciacao(2, 1)`

- `expoente = 1` is odd
- We need to compute `2 * potenciacao(2, 0)`

**Step 6**: `potenciacao(2, 0)`

- base case, returns `1`

Returned values:

- `potenciacao(2, 0)` returns `1`
- `potenciacao(2, 1)` returns `2 * 1 = 2`
- `potenciacao(2, 2)` returns `2^2 = 4`
- `potenciacao(2, 4)` returns `4^2 = 16`
- `potenciacao(2, 5)` returns `2 * 16 = 32`
- `potenciacao(2, 10)` returns `32^2 = 1024`

For computing enthusiasts, when we talk about **complexity** we're referring to the efficiency of an algorithm in terms of the number of operations performed. To represent the amount of operations, we use *Big-O* notation, symbolized as $O(\cdot)$.

In this context, the approach we used manages to reduce the complexity from $O(n)$ (where the number of operations grows **linearly** with the size of the input) to $O(\log n)$ (where the number of operations grows **logarithmically**, making the algorithm much more efficient for large inputs).

## Computing the Square Root (Heron's Method)

Heron's method (also known as the Babylonian method) is an ancient algorithm for computing approximations of square roots. The idea is to start with an estimate and progressively improve it.

Let's compute an approximation for $\sqrt{S}$. If $x_0 > 0$ is our initial estimate, we can improve our estimate using the following iterative formula:

$$x_{n + 1} = \frac{1}{2} \left( x_n + \frac{S}{x_n} \right).$$

Let $\varepsilon$ be the error in our estimate of $\sqrt{S}$. Then, $S = (x_0 + \varepsilon)^2$. Expanding the binomial, we get

$$S = (x_0 + \varepsilon)^2 = x_0^2 + 2x_0\varepsilon + \varepsilon^2.$$

We can solve the equation above for $\varepsilon$.

$$\varepsilon = \frac{S - x_0^2}{2x_0 + \varepsilon} \approx \frac{S - x_0^2}{2x_0}, \quad (\varepsilon \ll x_0).$$

Thus, we can compensate for the error and update our old estimate as

$$x_0 + \varepsilon \approx x_0 + \frac{S - x_0^2}{2x_0} = \frac{S + x_0^2}{2x_0} = \frac{\frac{S}{x_0} + x_0}{2} \equiv x_{1}$$

Since the computed error was not exact, this is not the final answer, but it becomes our new estimate to use in the next iteration. The update process is repeated until the desired precision is achieved.

Let's implement this method recursively:

```{code-cell} julia
function raiz_quadrada(S, xₙ = S / 2, ε = 0.0001)
    xₙ₊₁ = (xₙ + S / xₙ) / 2
    
    # Verificamos se a diferença entre as estimativas é menor que a precisão desejada
    if abs(xₙ₊₁ - xₙ) < ε
        return xₙ₊₁
    else
        return raiz_quadrada(S, xₙ₊₁, ε)
    end
end
```

The function takes three parameters:

- `S`: the number whose square root we want
- `xₙ`: our current estimate (by default, we start with half the number)
- `ε`: how close two consecutive estimates must be for us to consider that we've found the answer (by default, we're using `0.0001`)

Let's test our function:

```{code-cell} julia
println(raiz_quadrada(25))  # Deve ser próximo de 5
println(raiz_quadrada(2))   # Deve ser próximo de 1.4142...
```

Notice what happens to the estimate's value when computing the square root of 25:

1. First call: `x₀ = 25/2 = 12.5`
2. Second call: `x₁ = (12.5 + 25/12.5)/2 = 7.25`
3. Third call: `x₂ = (7.25 + 25/7.25)/2 = 5.35`
4. Fourth call: `x₃ = (5.35 + 25/5.35)/2 = 5.01`
5. Fifth call: `x₄ = (5.01 + 25/5.01)/2 = 5.0`

## Binomial Coefficient

The binomial coefficient $\binom{n}{k}$ (read as "n choose k") represents the number of ways to choose $k$ elements from a set of $n$ elements, without regard to order. For example, $\binom{5}{2}$ is the number of ways to choose 2 elements from a set of 5 elements.

The binomial coefficient has the following recursive definition (for $n > k$):

$$\binom{n}{k} = \binom{n-1}{k-1} + \binom{n-1}{k}$$

This formula can be derived by splitting the problem into two complementary cases. Consider a specific element $X$ among the $n$ available options. We can classify all possible subsets of $k$ elements into two categories:

1. Subsets that include $X$: In this case, since $X$ is already selected, we only need to choose $k-1$ additional elements from the remaining $n-1$ elements. This corresponds to $\binom{n-1}{k-1}$ possibilities.

2. Subsets that do not include $X$: In this case, we must select all $k$ elements from the remaining $n-1$ elements (excluding $X$). This corresponds to $\binom{n-1}{k}$ possibilities.

The total number of possible subsets is the sum of these two cases, which justifies the recursive formula presented above.

We can determine the base cases from the following properties:

1. $\binom{n}{0} = 1$ for any $n \geq 0$ (there is only one way to choose 0 elements)
2. $\binom{n}{n} = 1$ for any $n \geq 0$ (there is only one way to choose all the elements)

Let's implement this solution:

```{code-cell} julia
function coeficiente_binomial(n, k)
    if k == 0 || k == n
        return 1
    else
        return coeficiente_binomial(n - 1, k - 1) + coeficiente_binomial(n - 1, k)
    end
end
```

Let's test our function:

```{code-cell} julia
println(coeficiente_binomial(5, 2))  # Deve retornar 10
println(coeficiente_binomial(10, 4)) # Deve retornar 210
```
