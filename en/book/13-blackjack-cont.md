---
kernelspec:
  name: julia-livro-1.11
  display_name: "Julia 1.11 — livro"
---

# Continuing the Modeling

```{code-cell} julia
function jogador1(cards)
  carta1 = pegarCarta(cards)
  carta2 = pegarCarta(cards)
  if carta1 == 1 || carta2 == 1
    return carta1 + carta2 + 10
  else
    return carta1 + carta2
  end  
end  
```

Notice that above, we used the strategy of using the Ace in the most advantageous way.

For the other players, we'll use more elaborate strategies, that is, the
player keeps taking cards while they haven't reached a predetermined value, such
as 21, 19, 17, 15, and 13.

Since each player can have several cards, and the sum needs to account for an
Ace in the most advantageous way, we'll use a function that takes a vector
of cards and calculates the sum.

```{code-cell} julia
function somaCartas(c)
  soma = 0
  temAz = false
  for i in c
    soma += i
    if c == 1 
      temAz = true
    end
  end
  if soma <= 11 && temAz
      return soma + 10
  else 
      return soma
  end
end
```

With the card-sum function in hand, we can model the players.

```{code-cell} julia
function jogador2(cards)
  cartas = []
  push!(cartas, pegarCarta(cards))
  push!(cartas, pegarCarta(cards))
  while somaCartas(cartas) < 21
    push!(cartas, pegarCarta(cards))
  end
  return somaCartas(cartas)
end

function jogador3(cards)
  cartas = []
  push!(cartas, pegarCarta(cards))
  push!(cartas, pegarCarta(cards))
  while somaCartas(cartas) < 19
    push!(cartas,pegarCarta(cards))
  end
  return somaCartas(cartas)
end

function jogador4(cards)
  cartas = []
  push!(cartas, pegarCarta(cards))
  push!(cartas, pegarCarta(cards))
  while somaCartas(cartas) < 17
    push!(cartas,pegarCarta(cards))
  end
  return somaCartas(cartas)
end

  function jogador5(cards)
    cartas = []
    push!(cartas, pegarCarta(cards))
    push!(cartas, pegarCarta(cards))
    while somaCartas(cartas) < 15
      push!(cartas,pegarCarta(cards))
    end
    return somaCartas(cartas)
  end

function jogador6(cards)
  cartas = []
  push!(cartas, pegarCarta(cards))
  push!(cartas, pegarCarta(cards))
  while somaCartas(cartas) < 13
    push!(cartas,pegarCarta(cards))
  end
  return somaCartas(cartas)
end
```

Now that we have all the players, we can model a game.
To do this we create a deck and have each player follow
their strategy.

```{code-cell} julia
function partida()
  cards = criaBaralho()
  jogadores = zeros(Int8, 6)
  jogadores[1] = jogador1(cards)
  jogadores[2] = jogador2(cards)
  jogadores[3] = jogador3(cards)
  jogadores[4] = jogador4(cards)
  jogadores[5] = jogador5(cards)
  jogadores[6] = jogador6(cards)
end  
```

There wasn't time to continue. It was left for the next class.

In the previous chapter, we ended up with a game, but without checking the winner,
that is, the player with the highest value, less than or equal to 21. A design decision
is to say that in the case of a tie, the players with the highest values win and
split the prize.


```{code-cell} julia
function partida()
  cards = criaBaralho()
  jogadores = zeros(Int8, 6)
  jogadores[1] = jogador1(cards)
  jogadores[2] = jogador2(cards)
  jogadores[3] = jogador3(cards)
  jogadores[4] = jogador4(cards)
  jogadores[5] = jogador5(cards)
  jogadores[6] = jogador6(cards)
  return jogadores
end
```

The game returns the score of each player, so we can check in the
winner routine who won.

```{code-cell} julia
function ganhador(v)
    i = 1
    maximo = 0
    while i <= length(v)
        if v[i] > 21  # se estourou é como se tivesse o menor valor
            v[i] = 0
        end
        if v[i] > maximo
            maximo = v[i]  # encontra o vencedor
        end
        i = i + 1
    end
    result = zeros(Int64, length(v))
    i = 1
        while i <= length(v)
            if v[i] == maximo
                result[i] = 1
            end
            i = i + 1
        end
    return result
end

```

The winner routine returns a vector with the winners, with 1 in the position of whoever won
and zero in the position of the losers. 

One of the advantages of using a computer is that we can run thousands of games of 21
to find out what would be the best strategy.

```{code-cell} julia
function porcentagem()
    i = 1
    porc = zeros(Int64, 6)
    while i < 100000
        porc = porc + ganhador(partida())
        i = i + 1
    end
    println(porc)
end
```

By simulating the game 10000 times, we can find out which is the best strategy
among the ones presented.

The code above ended up relatively large, and there is a lot of duplication
in the code of the Players from the second one onward. One of the biggest
problems with code is duplication. In the case above, we can avoid it by adding
a parameter to the Player function, so that it becomes the limit to be considered in the
loop. The jogador2 function then looks like this:

```{code-cell} julia
function jogador2(cards, valor)
    cartas = []
    push!(cartas, pegarCarta(cards))
    push!(cartas, pegarCarta(cards))
    while somaCartas(cartas) < valor
       push!(cartas, pegarCarta(cards))
    end
    return somaCartas(cartas)
end
```

Since the function has a new parameter, we have to fix the game. But now
we can use all the values.


```{code-cell} julia
function partida()
   cards = criaBaralho()
   jogadores = zeros(Int8, 6)
   jogadores[1] = jogador1(cards)
   jogadores[2] = jogador2(cards, 21)
   jogadores[3] = jogador2(cards, 20)
   jogadores[4] = jogador2(cards, 19)
   jogadores[5] = jogador2(cards, 18)
   jogadores[6] = jogador2(cards, 17)
   return jogadores
end
```

Notice that there's no change to the winner function, which keeps working.

To finish, we can now have an interactive version that allows
a human player to play against the computer.

```{code-cell} julia
function partidaComHumano()
    cards = criaBaralho()
    humano = []
    computador = jogador2(cards, 19)
    push!(humano, pegarCarta(cards))
    push!(humano, pegarCarta(cards))
    println("O humano tem ", humano, " e soma ", somaCartas(humano))
    println("O humano quer mais cartas (S/N)?")
    resp = readline()
    while resp == "S" || resp == "s"
         push!(humano, pegarCarta(cards))
         println("O computador tem ", computador, " e soma ", somaCartas(computador))
         println("O humano tem ", humano, " e soma ", somaCartas(humano))
         println("O humano quer mais cartas (S/N)?")
         resp = readline()
    end
    println("O computador tem ", computador, " e soma ", somaCartas(computador))
    if somaCartas(computador) <= 21 && somaCartas(humano) <= 21
         if somaCartas(computador) > somaCartas(humano)
             println("Humano Perdeu")
         elseif somaCartas(computador) == somaCartas(humano)
             println("Empate")
         else
             println("Humano ganhou")
         end
    elseif somaCartas(computador) > 21 && somaCartas(humano) > 21
         println("os dois perderam")
    elseif somaCartas(computador) > 21
         println("Humano ganhou")
    else
         println("Computador ganhou")
    end
end
```  
