---
kernelspec:
  name: julia-livro-1.11
  display_name: "Julia 1.11 — livro"
---

# Exercise Class



## Revisiting Factorial: Recursive and Iterative

Now that we've also learned how to do repetitions with the while command, it's
always good to think about which command is most appropriate. Let's look at
the example below with two versions of the function for calculating factorial.


```{code-cell} julia
function fatorial_recursivo(n::Int64) # Com o ::Int64 estamos definindo que o parâmetro da função deve ser um número inteiro
    # Caso base do fatorial: 0! e 1! são iguais a 1
    if n == 0 || n == 1
        return 1
    # Chamada recursiva: n! = n * (n-1)!
    else
        return n * fatorial_recursivo(n - 1)
    end
end

function fatorial_iterativo(n::Int64)
    # Inicializa o resultado como 1 (já que o fatorial de 0 é 1)
    resultado = 1

    # No loop estamos fazendo a multiplicação: n * (n-1) * ... * 2
    while n > 1
        # Multiplica o resultado pelo valor atual de n
        resultado *= n

        # Decrementa n em 1 para continuar o cálculo do fatorial
        n -= 1
    end
    return resultado
end

println(fatorial_recursivo(3))
```

There's something new in the code above: the function parameters declare the
type explicitly. In this case, we're saying that the value `n` the function
will receive is of a specific type. That is, a 64-bit integer.

The coding style is a bit different from before, since it was written by
someone else, the teaching assistant. We can see that she has the habit of
using longer variable names, as well as using contractions like `+=` and `*=`.

## Approximating the Square Root

For the next example, let's look at the Newton-Raphson method for calculating
a square root. It's a recursive method in which the next value is based on
the previous value. The more calls that are made, the closer we get to the
final value.

More information about the method can be found [here](https://pt.wikipedia.org/wiki/M%C3%A9todo_de_Newton%E2%80%93Raphson).
But for now, let's think about the following implementation. To calculate
the root, we can use the following formula, starting from an initial guess r,
for the root of x.

$$ r_{n+1} = 0.5 * (r + x / r)$$

Since the code below is more complicated, it includes comments.


```{code-cell} julia
function aproxima_raiz(x::Float64, epsilon::Float64)::Float64
    if x < 0
        return nothing
    end

    # Chute inicial 
    aproximacao = x/2
    melhor_aproximicao = aproximacao

    while true
        # Fórmula para aproximação de raiz quadrada utilizando o método de Newthon-Raphson
        melhor_aproximicao = 0.5 * (aproximacao + x/aproximacao)

        # Se a distância absoluta entre os dois pontos é menor do que epsilon, então podemos parar o método
        if abs(aproximacao - melhor_aproximicao) <= epsilon
            break
        end

        # Se a aproximação ainda não for boa o sufuciente, então atualizamos a aproximação para a próxima iteração
        aproximacao = melhor_aproximicao
    end

    return melhor_aproximicao

end
```

Notice that a new command was introduced, `break`. This command interrupts
the execution of the while loop, forcing an exit from it.

## Checking Whether a Number Is Prime

In the next example, we'll check whether a number is prime, that is, whether
its only divisors are 1 and itself. The simplest way to do this is to try
dividing the number by others. If any of them divides it, the number is not
prime.

```{code-cell} julia
function verifica_primo(num :: Int64)
    if num <= 1
        return false
    end
    i=2
    # pode ser melhorado com i<=num/2
    # ou também com i<= sqrt(num): baseado no fato que um número composto deve ter um fator menor ou igual a raiz desse número
    while i<num
        if num % i == 0
            return false
        end
        i+=1
    end
    return true
end

```

Just as the `break` command is used to interrupt the execution of a loop, the
`return` command can be used to end the execution of a function at any
moment.

## Checking Whether a Number Is a Palindrome

A palindromic number is a number that is symmetric. That is, reading the
digits from left to right gives the same result as reading the digits in
reverse order. For example, the number 121 is a palindrome, as are 11 and
25677652. Single-digit numbers are also palindromes.

```{code-cell} julia
function e_palindromo(n::Int64)
    #=
        Guarda os dígitos de n que ainda devem ser invertidos
        A variável auxiliar é necessária para que o valor de n não seja, perdido, e possamos usar ele posteriormente.
    =#
    aux = n
    # Guarda a inversão do número n 
    n_inv = 0

    #=
        Continuamos o while enquanto ainda há números a serem invertidos,
        ou seja, enquanto aux for maior que 0.
    =#
    while aux > 0 
        # Coloca o último dígito de aux na variável que guarda a inversão
        resto = aux % 10
        n_inv= n_inv * 10 + resto

        # Retira o último dígito de aux
        aux = div(aux,10)
    end

    if n == n_inv
        println("O número $n é palíndromo")
    else
        println("O número $n não é palíndromo")
    end 
end

e_palindromo(2002)
e_palindromo(1234)

```
