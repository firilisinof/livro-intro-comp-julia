---
kernelspec:
  name: julia-livro-1.11
  display_name: "Julia 1.11 — livro"
---

# Exercícios com vetores  

Os vetores permitem fazer algoritmos bem mais complexos. Nesse capítulo, veremos alguns exercícios.

## Permutação

Dado um vetor de inteiros, queremos verificar se ele é uma permutação. Para isso, verificamos se, em um vetor de tamanho n, os números de 1 a n aparecem exatamente uma vez cada. O vetor [3, 1, 2] é uma permutação, pois tem tamanho 3 e os elementos de 1 a 3 aparecem uma vez.

Uma forma de resolver esse problema é usando um indicador de passagem: supomos, de início, que o vetor é uma permutação, e depois verificamos se todos os números entre 1 e n estão nele. Isso pode ser feito com o comando in, que verifica se um elemento pertence ao vetor.

```{code-cell} julia
function permutação(l)
    perm = true
    tamanho = length(l)
    i = 1
    while i <= tamanho
        if !(i in l)
            perm = false
        end    
        i += 1
    end
    return perm
end
```


Uma outra alternativa é verificar, para cada elemento do vetor, se ele está entre 1 e n e se é único. Ou seja, verificamos se o primeiro elemento está entre 1 e n e, depois, percorremos o vetor para ver se ele é único. Em seguida, fazemos isso para os elementos seguintes.
O código fica:

```{code-cell} julia
function permutação(l)
    perm = true
    tamanho = length(l)
    i = 1
    while i <= tamanho
        if (l[i] > tamanho || l[i] <= 0)
            perm = false
        end
        j = i +1
        while j <= tamanho
            if l[j] == l[i]
                perm = false
            end
            j += 1
        end
        i += 1
    end
    return perm
end
```

Uma outra alternativa é usar um vetor auxiliar, no qual contamos as ocorrências de cada número entre 1 e n. Ao final, todos os elementos desse vetor auxiliar têm que valer 1. Dessa vez, já aproveitamos e colocamos os testes automatizados.


```{code-cell} julia
using Test
function permutação(l)
    perm = true
    tamanho = length(l)
    aux = zeros(Int8, tamanho)
    for i in l
      if i < 1 || i > tamanho
        perm = false
      else
        aux[i] += 1
      end
    end
    for i in aux
      if i != 1
        perm = false
      end
    end  
    return perm
end

@testset "Verifica Permutação" begin
    @test permutação([1,2,3])
    @test permutação([3, 2, 1])
    @test permutação([1])
    @test permutação([2, 1])
    @test permutação([4, 2, 3, 1])
    @test !permutação([1, 1])
    @test !permutação([1, 3])
    @test !permutação([4, 2, 3, -1])
    @test !permutação([5, 2, 3, 1])
    @test permutação([])
    @test !permutação([0, 3, 3])
    @test !permutação([2, 2, 2])
end
```

## Histograma

Já que vimos, no exemplo anterior, como "contamos" o número, podemos ir um pouco além e calcular o histograma de um vetor com números entre 1 e 10.

```{code-cell} julia
using Test

function histograma(l)
    result = [0,0,0,0,0,0,0,0,0,0]
    i = 1
    while i <= length(l)
        valor_atual = l[i]
        if valor_atual >= 1 && valor_atual <= 10
           result[valor_atual] += 1
        end
        i += 1
    end
    return result
end

@testset "Verifica Histograma" begin
    @test [1,0,0,0,0,0,0,0,0,0] == histograma([1])
    @test [0,0,0,0,0,0,0,0,0,0] == histograma([-1])
    @test [0,0,1,0,0,0,0,0,0,0] == histograma([3])
    @test [0,0,0,0,0,0,0,0,0,1] == histograma([10])
    @test [0,0,0,0,0,0,0,0,0,0] == histograma([11])
    @test [1,4,0,2,5,1,0,1,0,0] == histograma([5,6,5,4,5,5,4,2,8,2,1,2,5,2])
    @test [0,0,0,0,0,0,0,0,0,0] == histograma([])
    end

```

## Modelando problemas com o computador

O computador pode ser uma ferramenta bem poderosa para modelar problemas reais. Vamos usar como exemplo o problema dos aniversários, também conhecido como paradoxo do aniversário: calcular a probabilidade de que, em uma sala com n pessoas, pelo menos duas façam aniversário na mesma data. Esse problema pode ser resolvido usando probabilidade, e o resultado mostra que, com 23 pessoas na sala, a chance de duas terem a mesma data é pouco mais de 50%.

Mas também podemos modelar esse problema computacionalmente. Para isso, o primeiro passo é simplificar as datas: em vez de mês e ano, podemos codificar os dias em um número entre 1 e 365, sendo que 1 corresponde ao primeiro de janeiro. Para resolver o problema, podemos sortear n datas e ver se há alguma repetição. Se houver, encontramos duas pessoas com a mesma data.

Isso está representado na função experimento_niver abaixo. Mas, para saber a chance real, temos que repetir o experimento várias vezes. Na função main() abaixo, pedimos a quantidade de experimentos e o número de pessoas para executar a simulação.


```julia
function experimento_niver(n)
    repetiu = false
    i = 1
    nivers = []
    while i <= n && (repetiu == false)
        niver = rand(1:365)
        if niver in nivers
            repetiu = true
        end
        push!(nivers, niver)
        i += 1
    end
    return repetiu
end

function main()
    print("Quantos experimentos? ")
    quantas = readline()
    print("Quantas pessoas? ")
    npessoas = readline()
    quantas = parse(Int64, quantas)
    npessoas = parse(Int64, npessoas)
    sucessos = 0
    i = 1
    while i <= quantas
        if experimento_niver(npessoas)
            sucessos += 1
        end
        i += 1
    end
    println("A probabilidade estimada é ", 100*sucessos/quantas, "%")
end
main()

```

A parte interessante é que, com pequenas variações, podemos ter outros experimentos, como verificar se mais de duas pessoas fazem aniversário na mesma data. Para isso, abaixo, contamos o número de repetições.


```{code-cell} julia
function experimento_niver(n)
    repetiu = 0
    i = 1
    nivers = []
    while i <= n
        niver = rand(1:365)
        if niver in nivers
            repetiu += 1
        end
        push!(nivers, niver)
        i += 1
    end
    return repetiu >= 2
end
```
