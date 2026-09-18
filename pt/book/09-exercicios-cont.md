---
kernelspec:
  name: julia-livro-1.11
  display_name: "Julia 1.11 — livro"
---

# Revisitando a aula passada

Nessa aula, além de revisarmos o que vimos na aula passada, vimos uma nova solução para o problema de verificar se um número é palíndromo.

Para isso, usamos uma técnica um pouco diferente: em vez de inverter o número e compará-lo com o original, verificamos se os seus extremos são iguais.

Observe o número 234432: o primeiro passo é verificar que, nos extremos mais significativo e menos significativo, temos o número 2. Em seguida, continuamos a verificação com o número 3443. Se em algum momento a verificação falhar, o número não é palíndromo.

Seguem os testes e o código abaixo.

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

## Aleatoreidade

Em Julia, temos a função rand(), que devolve um número em ponto flutuante entre 0 e 1. Dependendo dos parâmetros, podemos obter outros tipos de número, como:

```{code-cell} julia
rand(Int)  # devolve um inteiro
rand(1:10) # devolve um número entre 1 e 10
rand(Bool) # devolve verdadeiro ou falso
```

Mas antes de ver um código com rand(), vamos pensar em um problema da vida real. Imagine que precisamos fazer um sorteio justo, e o único instrumento que temos é uma moeda viciada, que sai cara com muito mais frequência do que coroa. Dá para usar essa moeda em um sorteio justo?

A ideia para resolver o problema é olhar para pares de sorteios: vamos ignorar os pares em que saem duas caras ou duas coroas. Nos outros pares, teremos uma cara e uma coroa, em alguma ordem, e as chances de cada ordem são de 50%. Assim, conseguimos corrigir a moeda viciada.

Para simplificar o exercício, a moeda pode devolver 0 ou 1, correspondentes a cara ou coroa. Observe a seguinte função que simula uma moeda viciada.

```{code-cell} julia
function sorteio()
  if rand() > 0.90
    return 1
  else 
    return 0
  end
end
```

Podemos observar que a função devolve 0 na maior parte das vezes. Para confirmar isso, vamos fazer mil sorteios:

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

Mas podemos corrigir o sorteio da seguinte forma:

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

Podemos usar o verificaSorteio para ver a diferença.

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

Podemos ainda aproximar o número de Euler (𝑒), a constante matemática que é a base dos logaritmos naturais, usando uma simulação probabilística. A ideia é que o número médio de tentativas necessárias para que a soma de números aleatórios entre 0 e 1 ultrapasse 1 se aproxima do valor de 𝑒.

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


Para terminar a aula, vamos aplicar o método de Monte Carlo para o cálculo de Pi. Imagine o primeiro quadrante, onde temos um semicírculo de raio 1 dentro de um quadrado de lado 1. Podemos sortear pontos e contar os que caem dentro do círculo para estimar essa área. Mais informações podem ser vistas aqui (https://pt.wikipedia.org/wiki/M%C3%A9todo_de_Monte_Carlo)

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