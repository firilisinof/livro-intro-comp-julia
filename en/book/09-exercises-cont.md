---
kernelspec:
  name: julia-livro-1.11
  display_name: "Julia 1.11 — livro"
---

# Revisiting the Previous Class

In this class, in addition to reviewing what we saw last time, we looked at
a new solution to the problem of checking whether a number is a palindrome.

For this, we used a slightly different technique: instead of reversing the
number and comparing it to the original, we check whether its outer digits
are equal.

Consider the number 234432. The first step is to check that at the
extremes, the most significant and least significant digits, we have the
number 2. Next, we can continue the check with the number 3443. If the
check ever fails, the number is not a palindrome.

The tests and code follow below.

```{code-cell} julia
using Test

function testaPal()
  @test testaPal(1)
  @test testaPal(131)
  @test testaPal(22)
  @test testaPal(53877835)
  @test !testaPal(123)
  @test !testaPal(23452)
  println("Final dos testes")
end

function testaPal(n::Int64)
# o primeiro passo é encontrar um número com o mesmo número de dígitos de n
  pot10 = 1
  while pot10 < n
    pot10 = pot10 * 10
  end
  pot10 = div(pot10, 10)


  while n > 9
    d1 = n % 10
    d2 = div(n, pot10)
    if d1 != d2
      return false
    end
    n = div(n % pot10, 10)
    pot10 = div(pot10, 100)
  end
  return true
end 
``` 

## Randomness

In Julia we have the rand() function, which returns a floating-point
number between 0 and 1. Depending on the parameters, we can get other
types of numbers, such as:

```{code-cell} julia
rand(Int)  # devolve um inteiro
rand(1:10) # devolve um número entre 1 e 10
rand(Bool) # devolve verdadeiro ou falso
```

But before looking at code that uses rand(), let's think about a
real-world problem. Imagine we need to run a fair drawing, and the only
instrument we have for it is a biased coin, one that lands on heads far
more often than tails. Can we use this coin for a fair drawing?

The idea for solving this problem is to look at pairs of flips. That is,
we ignore pairs where we get two heads or two tails. In the remaining
pairs, we'll have one heads and one tails, or vice versa. The chances of
either outcome will be 50%. This way, we can correct for the biased coin.

To simplify the exercise, the coin can return 0 or 1, corresponding to
heads or tails. Look at the following function that simulates a biased
coin.

```{code-cell} julia
function sorteio()
  if rand() > 0.90
    return 1
  else 
    return 0
  end
end
```

Notice that the function returns 0 most of the time. We can even verify
this by running a thousand flips:

```{code-cell} julia
function verificaSorteio()
   cara = 0
   coroa = 0
   i = 0
   while i < 1000
     if sorteio() == 0
        cara = cara + 1
     else
        coroa = coroa + 1
     end
     i = i + 1
   end
   println("O número de caras foi: ", cara," e de coroas foi :", coroa)
end
```

But we can correct the flip as follows:

```{code-cell} julia
function sorteioBom()
   sorteio1 = sorteio()
   sorteio2 = sorteio()
   while sorteio1 == sorteio2 # se forem iguais, tente novamente
     sorteio1 = sorteio()
     sorteio2 = sorteio()
   end
   return sorteio1   # ao termos um diferente, podemos devolver o primeiro sorteio
end
```

We can use verificaSorteio to see the difference.

```{code-cell} julia
function verificaSorteio()
   cara = 0
   coroa = 0
   i = 0
   while i < 1000
     if sorteioBom() == 0
        cara = cara + 1
     else
        coroa = coroa + 1
     end
     i = i + 1
   end
   println("O número de caras foi: ", cara," e de coroas foi :", coroa)
end
```

We can also approximate Euler's number (𝑒), the mathematical constant
that is the base of the natural logarithm, using a probabilistic
simulation. The idea behind this code is that the average number of
attempts needed for the sum of random numbers between 0 and 1 to exceed
1 approaches the value of 𝑒.

```{code-cell} julia
function calculaEuler(total)
    soma_tentativas = 0
    for i in 1:total
        soma = 0.0
        tentativas = 0      
        while soma <= 1   # Continue gerando números até a soma ultrapassar 1
            soma += rand()     # Gera número aleatório entre 0 e 1
            tentativas += 1
        end        
        soma_tentativas += tentativas     # Somar o número de tentativas necessárias
    end 
    return soma_tentativas / total     # A média do número de tentativas será uma estimativa de e
end

println("Estimativa de e (1000 iterações): ", calculaEuler(1000))
println("Estimativa de e (100000 iterações): ", calculaEuler(100000))
println("Estimativa de e (100000000 iterações): ", calculaEuler(100000000))
```


To wrap up the class, let's apply the Monte Carlo method to calculate
Pi. Picture the first quadrant, where we have a quarter-circle of radius
1 inside a square with side 1. We can draw random points, and those that
fall inside the circle count toward its area. More information can be
found here (https://pt.wikipedia.org/wiki/M%C3%A9todo_de_Monte_Carlo)

```{code-cell} julia
function calculaPi(total)
   noAlvo = 0
   i = 0
   while i < total
     x = rand() / 2.0 # gera um número entre 0 e 0.5
     y = rand() / 2.0
     if sqrt(x * x + y * y) <= 0.5
       noAlvo = noAlvo + 1
     end
     i = i + 1
   end
   return 4 * (noAlvo / total)  # precisamos multiplicar para ter a área de 4 quadrantes
end 

println(calculaPi(100))
println(calculaPi(1000000))
println(calculaPi(1000000000))
```
