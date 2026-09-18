---
kernelspec:
  name: julia-livro-1.11
  display_name: "Julia 1.11 — livro"
---
(sec-testes)=
# Automated Testing

We have developed several recursive algorithms, and to verify that they worked correctly, we ran manual tests by executing the functions with different inputs and checking their results. However, as our programs become more complex, this manual approach becomes inefficient and error-prone. In this chapter, we will introduce the concept of automated tests, which will let us check, systematically and reliably, whether our functions are behaving as expected.

## Why Test?

When we write code, we want to be sure it is working correctly. Automated tests help us:

1. Check whether the code produces the expected results for different inputs
2. Catch bugs and errors before the program is used
3. Ensure that changes to the code do not break existing functionality
4. Document the expected behavior of our functions

In software development, a common practice is TDD (_Test-Driven Development_), where we first write the tests and then implement the code that satisfies them. This approach encourages a clearer design and a better understanding of the requirements even before we start programming.

## Writing Tests With Conditionals

Let's start with a simpler approach to creating automated tests using conditional structures. We will consider the "Staircase Problem" that we saw in the previous chapter.

As a reminder, the problem consisted of determining how many different ways we can climb a staircase with $n$ steps, if we can take steps of 1 or 2 steps at a time. Our solution was:

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

Now, let's create a test function to check whether our implementation is correct:

```{code-cell} julia
function testa_maneiras_subir_escada()
    # Geralmente, verificamos alguns casos conhecidos ou que sabemos a resposta
    if maneiras_subir_escada(1) != 1
        println("Erro para n = 1")
        return false
    end
    
    if maneiras_subir_escada(2) != 2
        println("Erro para n = 2")
        return false
    end
    
    if maneiras_subir_escada(3) != 3
        println("Erro para n = 3")
        return false
    end
    
    if maneiras_subir_escada(4) != 5
        println("Erro para n = 4")
        return false
    end
    
    println("Todos os testes para a função maneiras_subir_escada passaram!")
    return true
end

# Executamos os testes
testa_maneiras_subir_escada()
```

In this test function, we check whether our implementation returns the correct values for different inputs. If a test fails, we display a message indicating which case failed. If all tests pass, we display a success message.

This is an important principle for automated tests: **if the test passes, it should only indicate that it succeeded!** This means that, ideally, tests should not print many messages when everything is working correctly, only when something goes wrong.

Let's do the same for the "Binomial Coefficient" calculation, which we also saw in the previous chapter:

```{code-cell} julia
function coeficiente_binomial(n, k)
    if k == 0 || k == n
        return 1
    else
        return coeficiente_binomial(n - 1, k - 1) + coeficiente_binomial(n - 1, k)
    end
end

function testa_coeficiente_binomial()
    if coeficiente_binomial(5, 2) != 10
        println("Erro para (5, 2)")
        return false
    end
    
    if coeficiente_binomial(10, 4) != 210
        println("Erro para (10, 4)")
        return false
    end
    
    if coeficiente_binomial(7, 3) != 35
        println("Erro para (7, 3)")
        return false
    end
    
    println("Todos os testes para a função coeficiente_binomial passaram!")
    return true
end

# Executamos os testes
testa_coeficiente_binomial()
```

## Testing With the Test Module

So far, we have created test functions manually using conditional structures. However, Julia provides a built-in testing module called `Test`, which offers more advanced functionality for automated tests.

Let's rewrite our tests using the `Test` module:

```{code-cell} julia
using Test

@testset "Testes para maneiras_subir_escada" begin
    @test maneiras_subir_escada(1) == 1
    @test maneiras_subir_escada(2) == 2
    @test maneiras_subir_escada(3) == 3
    @test maneiras_subir_escada(4) == 5
end

@testset "Testes para coeficiente_binomial" begin
    @test coeficiente_binomial(5, 2) == 10
    @test coeficiente_binomial(10, 4) == 210
    @test coeficiente_binomial(7, 3) == 35
end
```

With the `Test` module, we use the `@testset` macro to group related tests and the `@test` macro to check specific conditions. If a test fails, the module automatically displays useful information about the failure, such as the expression that failed and the expected versus obtained values.

In addition, the `Test` module offers other useful macros:

- `@test_throws`: checks whether an expression throws a specific exception
- `@test_approx_eq`: checks whether two floating-point values are approximately equal (accounting for rounding errors)
- `@test_broken`: marks a test that is expected to fail (useful for documenting known bugs)

## More Examples

Let's implement two new functions and their respective tests: one function to calculate the sum of the digits of a number, and another to check whether a number is prime.

## Sum of Digits

First, let's create a function that calculates the sum of the digits of an integer. For example, for the number 123, the sum of the digits would be 1 + 2 + 3 = 6.

Before implementing the function, let's think about the test cases:

- If the function receives a single-digit integer, it should return that digit
- If the function receives 100, it should return 1 + 0 + 0 = 1
- If the function receives 123, it should return 1 + 2 + 3 = 6
- If the function receives 99, it should return 9 + 9 = 18

We can implement the function using recursion. The idea is to "peel" the number, extracting one digit at a time:

```{code-cell} julia
function soma_digitos(n)
    if n <= 0
        return 0
    else
        # Obtemos o último dígito com o resto da divisão por 10
        ultimo_digito = n % 10
        # Removemos o último dígito com a divisão inteira por 10
        resto_numero = n ÷ 10
        # Somamos o último dígito com a soma dos dígitos do resto do número
        return ultimo_digito + soma_digitos(resto_numero)
    end
end
```

The test cases discussed above can be implemented using the `Test` module:

```{code-cell} julia
@testset "Testes para soma_digitos" begin
    @test soma_digitos(0) == 0
    @test soma_digitos(1) == 1
    @test soma_digitos(100) == 1
    @test soma_digitos(123) == 6
    @test soma_digitos(99) == 18
end
```

## Checking for Prime Numbers

Let's create a function to check whether a number is prime. A prime number is one that is divisible only by 1 and by itself. Before writing the function, let's think about the tests:

- By definition, any number less than or equal to 1 is not prime
- The number 2 is prime (easy to check)
- The number 3 is prime (also easy to check)
- The number 4 is not prime, since 2 also divides 4
- The number 17 is prime
- The number 25 is not prime, since 5 also divides 25

We can implement the function using a recursive approach that tries to divide the number by each integer from 2 up to the square root of the number:

```{code-cell} julia
function verifica_divisor(n, divisor)
    # Se encontramos um divisor, o número não é primo
    if n % divisor == 0
        return false
    # Se já testamos até a raiz quadrada, o número é primo
    elseif divisor * divisor > n
        return true
    else
        # Continua verificando com o próximo divisor
        return verifica_divisor(n, divisor + 1)
    end
end

function e_primo(n)
    if n <= 1 # Por definição
        return false
    elseif n == 2 # Primeiro primo
        return true
    else
        # Verifica se n tem algum divisor começando com 2
        return verifica_divisor(n, 2)
    end
end
```

The tests can be written as:

```{code-cell} julia
@testset "Testes para e_primo" begin
    @test e_primo(2) == true
    @test e_primo(3) == true
    @test e_primo(4) == false
    @test e_primo(17) == true
    @test e_primo(25) == false
end
```

## Check Your Understanding

1. What is the difference between using conditional structures and the Test module for automated testing?
2. Why are automated tests important in software development?

## Explore on Your Own

1. Explore other macros available in Julia's Test module and try using them in your own tests.
