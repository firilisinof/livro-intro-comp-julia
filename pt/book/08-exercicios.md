---
kernelspec:
  name: julia-livro-1.11
  display_name: "Julia 1.11 — livro"
---

# Aula de exercícios



## Revisitando o cálculo do fatorial, recursivo e interativo

Agora que também aprendemos a fazer repetições com o comando while, é sempre bom pensar em qual comando é mais adequado. Vejamos o exemplo abaixo com duas versões da função que calcula o fatorial.


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

No código acima temos uma novidade: nos parâmetros da função, o tipo está sendo declarado expicitamente. Ou seja, estamos dizendo que o valor n que a função vai receber precisa ser de um tipo específico, um inteiro de 64 bits.

O estilo de código está um pouco diferente do que antes, pois foi escrito por outra pessoa: a monitora. Ela tem o hábito de usar nomes de variáveis mais longos e também usa contrações como += e *=.

## Aproximação da raiz quadrada

Para o próximo exemplo, vamos ver o método de Newthon-Raphson para calcular a raiz quadrada. É um método recursivo no qual o próximo valor é baseado no valor anterior: quanto mais chamadas forem feitas, mais perto do valor final chegamos.

Mais informações sobre o método podem ser encontradas em [aqui](https://pt.wikipedia.org/wiki/M%C3%A9todo_de_Newton%E2%80%93Raphson). Por enquanto, vamos pensar na implementação a seguir. Para calcular a raiz, podemos usar a seguinte fórmula, partindo de um palpite inicial r para o valor da raiz de x.

$$ r_{n+1} = 0.5 * (r + x / r)$$

Como o código abaixo é mais complicado, foram usados comentários.


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

Notem que foi introduzido um comando novo, o break. Esse comando interrompe a execução do while, ou seja, força a saída do laço.

## Verificar se um número é primo

No próximo exemplo, vamos verificar se um número é primo, ou seja, se os seus únicos divisores são 1 e ele mesmo. A forma mais simples de fazer isso é tentar dividir o número por outros números: se algum dividir, o número não é primo.

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

Assim como o comando break interrompe a execução de um laço, o comando return pode ser usado para terminar a execução de uma função a qualquer momento.

## Verificar se um número é palíndromo

Um número palíndromo é um número simétrico, ou seja, a leitura dos dígitos da esquerda para a direita é igual à leitura dos dígitos na ordem inversa. Por exemplo, o número 121 é palíndromo, assim como 11 e 25677652. Números de um dígito também são.

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
