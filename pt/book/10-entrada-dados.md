---
kernelspec:
  name: julia-livro-1.11
  display_name: "Julia 1.11 — livro"
---

# Entrada de dados e o começo de listas
Nessa aula, temos dois tópicos principais: como fazer a entrada de dados, por meio de comandos de entrada e de argumentos na linha de comando, e como tratar um tipo especial de variável, que permite guardar mais de um valor.

## O comando input
Quando estamos usando Julia interativamente, basta digitar os dados diretamente. Mas, em um programa comum, como fazemos para receber dados de entrada?

Para isso, temos o comando readline(), que interrompe a execução do programa e espera a entrada de uma string, que é registrada quando a tecla "enter" é pressionada.

```julia

println("Digite o seu nome")
resposta = readline()
println("O seu nome é: ", resposta)
```

Se, ao rodar o programa, você digitar `Maria` e pressionar a tecla enter, a resposta final do programa será `O seu nome é: Maria`.

Como o readline() lê strings, se quisermos ler números, é necessário usar o comando parse. De forma simples, o parse tem dois parâmetros: o primeiro é o tipo para o qual queremos converter, e o segundo é o valor original.

```julia

println("Digite um inteiro")
valor = parse(Int64, readline())
println("O numero digitado foi ", valor)
```


Agora que sabemos ler números do teclado, vamos a um exercício simples: ler uma sequência de números inteiros terminada por zero e devolver a soma deles.

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

Observe o seguinte exemplo que calcula os quadrados dos números de uma
lista terminada por zero.

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

Notem que o readline também pode receber uma variável de arquivo, para que os dados sejam lidos diretamente dele. Mas, nesse caso, temos que tomar cuidado para abrir (open()) e fechar (close()) o arquivo, como abaixo:

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

## Lendo através da linha de comando

A outra forma de ler comandos é por meio da constante ARGS, que é preparada na chamada de um programa. Para entender melhor isso, vamos ver o seguinte programa.

```julia

println(ARGS)
```

Se a linha acima estiver no arquivo args.jl, ao chamarmos julia args.jl com parâmetros diferentes, teremos resultados diferentes.

Por exemplo, ao chamar:

julia args.jl 1 2 3 abc

Teremos como resposta

```julia

["1", "2", "3", "abc"]
```

Vamos analisar um pouco melhor essa resposta observando que cada
parâmetro está em uma posição.

```julia

tam = length(ARGS)
println("O tamanho dos argumentos é: ", tam)
for i in 1:tam
    println(ARGS[i])
end
```

Olhando o código acima, podemos ver que a função length() devolve o número de argumentos, ou seja, o tamanho da lista ARGS. Além disso, com os colchetes é possível acessar cada posição da lista individualmente.


O exemplo abaixo soma os parâmetros inteiros dados como argumentos. Ele também ilustra uma boa prática, que é sempre colocar o código em módulos, no caso abaixo em funções:

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

A flexibilidade que temos ao usar listas é enorme, por isso listas, ou vetores, merecem um tópico próprio.

## Listas

Vamos primeiro brincar um pouco no console.

```{code-cell} julia
vetor = [1, 2, 3]
println(vetor[1])
println(length(vetor))
vetor[2] = vetor[2] + 1
vetor[1] = 2 * vetor[3]
println(vetor)
``` 

Como disse antes, o for foi feito para manipular vetores. Vamos ver algumas funções, a primeira delas imprime os elementos de um vetor, um por linha.

```julia

function imprimeVetor(v)
    for el in v
        println(el)
    end
end
```

Isso também pode ser feito por meio dos índices do vetor:

```julia

function imprimeVetor(v)
    for i in 1:lenght(v)
        println(v[i])
    end
end
```

Como cada posição é independente, podemos calcular a soma dos elementos ímpares de um vetor.

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

Também vimos em aula alguns outros exemplos, como calcular a média dos
elementos em um vetor.

```julia

function mediaV(v)
   soma = 0.0
   for i in v
      soma = soma + i
   end
   return soma / length(v)
end  
```

Devolver a soma dos elementos ímpares de um vetor

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

Imprimir os números divisíveis por 5 de um vetor.

```julia

function imprimeDivisivelPor5(v)
    for i in v
        if i % 5 == 0
            println(i)
        end
    end
end
```

Com uma pequena variação e usando o comando push!(), podemos ver como devolver um vetor com os números divisíveis por 5.

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


## Álgebra linear e Listas

A manipulação de listas é uma parte fundamental da álgebra linear, que estuda vetores e matrizes. Funções como o produto escalar de dois vetores são exemplos clássicos. Abaixo temos dois exemplos de produto escalar de dois vetores, lembrando que ele é definido como a soma dos produtos de elementos em posições iguais.

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

Acima vimos que um caso especial do uso do for consiste em fazer o for variar entre 1 e um tamanho (1:lenght(a))

Observem a diferença na versão abaixo:


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

## Exercício de permutação

Para terminar, vamos fazer uma função onde dado um vetor de inteiros
de tamanho $n$, verifica se esse vetor é uma permutação dos números de
1 a $n$. Para isso, veremos se cada número de 1 a $n$ está no vetor.

Mas sem esquecer dos testes:

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

e o código:

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

Foi usado o comando in de Julia, que verifica se um elemento está no vetor.
