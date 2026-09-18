---
kernelspec:
  name: julia-livro-1.11
  display_name: "Julia 1.11 — livro"
---

# Modeling Blackjack with Programming

In this chapter, we will build a simplified terminal version of the Blackjack game and use computational simulations to evaluate different playing strategies. Blackjack (also known as "21") is a card game where the goal is to get closer to 21 points than your opponent, without going over that value.

This project will let us practice important programming concepts while exploring how simulation can help solve real-world problems.

## Interactive Version of the Game

The first step, before thinking about the implementation, is to understand what we want to model. Blackjack is played by two people: the player and the _dealer_ (the house or casino). In our game, the dealer will be the computer. In addition, we will follow these game rules:

1. Initially, the player and the dealer each receive two cards.
2. During the player's turn, the player can choose between two options: draw another card (_hit_) or stop drawing cards (_stand_).
3. During the dealer's turn, the dealer will follow this strategy: keep drawing cards while the total is below 17, and stop otherwise.

The player's goal is to get closer to 21 than the dealer, without going over that value.

The value of each card matches its face value. The Queen (Q), Jack (J), and King (K) cards are worth 10 points. The Ace can be worth 1 or 11 (depending on the player's strategy). We will treat an Ace as being worth 11, unless that would cause the hand to go over 21 (bust). In that case, the Ace is worth 1. Now that we know the rules of the game, we can implement it.

Let's start by creating a deck of 52 cards. A simple way to represent it is with a vector (we'll call it `baralho`, meaning "deck") where each element represents a card type and stores how many of that type are still available, meaning that if `baralho[i]` is 4, there are 4 cards of type `i` available in the deck.

For the numeric representation of the cards, the Ace corresponds to 1, the Two to 2, and so on up to 10. The Queen is represented by 11, the Jack by 12, and the King by 13. Since the suit doesn't matter, we consider that there are four cards of each type in a full deck.

```{code-cell} julia
function criaBaralho()
	baralho = zeros(Int8, 13)

	i = 1
	while i < 14
		baralho[i] = 4
		i = i + 1
	end

	return baralho
end
```

Besides creating the deck, we need a function to draw a card from it. In real life, the deck would be shuffled with the cards face down, and drawing a card would just mean pulling the one on top. The problem is that our model doesn't support that kind of behavior, since the vector only represents the count of cards of each type.

To simulate this behavior, we can draw a random number between 1 and 13 and check whether cards of that type still exist. If they do, we remove one card of that type from the deck and return the drawn number. Otherwise, we keep drawing numbers until we find a type that still has cards available.

```{code-cell} julia
function pegaCarta(baralho)
	i = rand(1:13)

	# Verifica se existem cartas do tipo sorteado
	while baralho[i] == 0
		i = rand(1:13)
	end

	baralho[i] = baralho[i] - 1

	return i
end
```

Now that we have a deck and can draw cards from it, we need a function to calculate the value of a hand. To represent the player's and dealer's hands, we will use a vector that stores the values of the cards received. Both hands will be defined in a function later on.

```{code-cell} julia
function calculaValorMao(mao)
	ases = 0
    valor = 0
	tamanhoMao = length(mao)

	i = 1
	while i <= tamanhoMao
		if mao[i] == 1 # Ás
			ases = ases + 1
			valor = valor + 11
		elseif mao[i] >= 11 # Dama, Valete ou Rei
			valor = valor + 10
		else
			valor = valor + mao[i]
		end
		i = i + 1
	end

	# Ajusta o valor dos Ases se necessário
	while ases > 0 && valor > 21
		valor = valor - 10
		ases = ases - 1
	end

	return valor
end
```

This function goes through every card in the hand and calculates the total value. First, we assume that every Ace is worth 11. If the total value goes over 21 and there are still Aces counted as 11, we convert one Ace at a time from 11 to 1 until the value fits within the limit or there are no more Aces left to convert.

Let's create helper functions to display the cards in a more friendly way:

```{code-cell} julia
function nomeCartaTexto(carta)
    nomes = ["Ás", "2", "3", "4", "5", "6", "7", "8", "9", "10", "Dama", "Valete", "Rei"]
    return nomes[carta]
end

function exibeMao(mao, jogador)
    print("$jogador possui as cartas: ")
	i = 1
	tamanhoMao = length(mao)
    while i <= tamanhoMao
        print(nomeCartaTexto(mao[i]), " ")
		i = i + 1
    end
	println()
    println("Valor da mão: $(calculaValorMao(mao))")
    println()
end
```

Now we can implement the main game logic following the rules we defined earlier.

```{code-cell} julia
function jogarBlackjack()
	baralho = criaBaralho()
	maoJogador = Int[]
	maoDealer = Int[]

	push!(maoJogador, pegaCarta(baralho))
	push!(maoDealer, pegaCarta(baralho))
	push!(maoJogador, pegaCarta(baralho))
	push!(maoDealer, pegaCarta(baralho))

	println("=== BLACKJACK ===")
    println()

	println("Dealer possui a carta: $(nomeCartaTexto(maoDealer[1]))")
	println()

	exibeMao(maoJogador, "Você")

	valorJogador = calculaValorMao(maoJogador)
	while valorJogador < 21
		print("Deseja pedir mais uma carta? (s/n): ")
		resposta = readline()

		if resposta == "s" || resposta == "S"
            novaCarta = pegaCarta(baralho)
            push!(maoJogador, novaCarta)
			valorJogador = calculaValorMao(maoJogador)
            println("Você recebeu: $(nomeCartaTexto(novaCarta))")
            println()
            exibeMao(maoJogador, "Você")
        else
            break
        end
	end

	if valorJogador > 21
        println("Você estourou! Perdeu o jogo.")
        return
    end

	println("Vez do dealer...")
	println()

	exibeMao(maoDealer, "Dealer")

	valorDealer = calculaValorMao(maoDealer)
	while valorDealer < 17
		novaCarta = pegaCarta(baralho)
		push!(maoDealer, novaCarta)
		valorDealer = calculaValorMao(maoDealer)
		println("Dealer recebeu: $(nomeCartaTexto(novaCarta))")
		println()
		exibeMao(maoDealer, "Dealer")
	end

	if valorDealer > 21
		println("Dealer estourou! Você ganhou!")
    elseif valorJogador > valorDealer
        println("Você ganhou com $valorJogador pontos contra $valorDealer do dealer!")
    elseif valorDealer > valorJogador
        println("Dealer ganhou com $valorDealer pontos contra seus $valorJogador pontos!")
    else
        println("Empate! Ambos fizeram $valorJogador pontos.")
    end
end
```

The main function `jogarBlackjack()` coordinates the entire flow of the game. First, it creates a new deck and initializes empty hands. Next, it deals two cards to each player and shows the initial state, hiding one of the dealer's cards to mimic a real game.

During the player's turn, the program asks whether the player wants more cards, until the player stops or busts (goes over 21). During the dealer's turn, the rules are automatic: the dealer must keep drawing cards while the total value is below 17.

Finally, the program compares the final values and determines the winner. If both players bust, the dealer wins (the standard Blackjack rule).

To test our game, we just need to call the main function:

```julia
jogarBlackjack()
```

## Finding the Best Strategy through Simulation

Now that we have a working game, we can use simulation to answer an interesting question: what is the best strategy for Blackjack? Instead of playing thousands of games by hand (which would be impossible), we will create several automatic strategies and simulate thousands of games to find out which one works best. We will define different strategies as functions that return the final value of the hand after applying the strategy in question.

The first strategy will be more conservative. The player keeps only the first two cards and never draws another one.

```{code-cell} julia
function estrategia1(baralho)
  mao = Int[]
  push!(mao, pegaCarta(baralho))
  push!(mao, pegaCarta(baralho))
  return calculaValorMao(mao)
end
```

For the other players, we will use more aggressive strategies, meaning the player keeps drawing cards until reaching a predetermined value, for example 21, 19, 17, 15, and 13.

```{code-cell} julia
function estrategia2(baralho, valorMaximo)
  mao = Int[]
  push!(mao, pegaCarta(baralho))
  push!(mao, pegaCarta(baralho))

  while calculaValorMao(mao) < valorMaximo
    push!(mao, pegaCarta(baralho))
  end
  return calculaValorMao(mao)
end
```

Now that we have the strategies, we can define a match that applies the different strategies and returns each player's score. This will be useful for finding the winner.

```{code-cell} julia
function partida()
  baralho = criaBaralho()

  jogadores = zeros(Int8, 6)
  jogadores[1] = estrategia1(baralho)
  jogadores[2] = estrategia2(baralho, 21)
  jogadores[3] = estrategia2(baralho, 20)
  jogadores[4] = estrategia2(baralho, 19)
  jogadores[5] = estrategia2(baralho, 18)
  jogadores[6] = estrategia2(baralho, 17)

  return jogadores
end
```

To find the winner, we can compare the values of all the players, checking who busted and who got closest to 21.

```{code-cell} julia
function vencedor(jogadores)
	totalJogadores = length(jogadores)
	resultado = zeros(Int8, totalJogadores)

	maximo = 0
	i = 1

	# Descobre a maior pontuação válida
	while i <= totalJogadores
		pontuacaoAtual = jogadores[i]

		if pontuacaoAtual > 21
			pontuacaoAtual = 0
		end

		if pontuacaoAtual > maximo
			maximo = pontuacaoAtual
		end

		i = i + 1
	end

	i = 1
	
	# Marca os jogadores com a maior pontuação como vencedores
	while i <= totalJogadores
		pontuacaoAtual = jogadores[i]

		if pontuacaoAtual == maximo
			resultado[i] = 1
		end

		i = i + 1
	end

	return resultado
end
```

The `vencedor` function returns a vector with the winners. This vector has every entry equal to zero (0), except for the winner's entry, whose value is one (1). Note that in the event of a tie, we consider the tied players all to be winners.

Finally, we can simulate thousands of matches and find out which strategy is best.

```{code-cell} julia
function melhorEstrategia()
	numeroPartidas = 100000
	contagemResultados = zeros(Int64, 6)

	i = 1
	while i <= numeroPartidas
		contagemResultados = contagemResultados + vencedor(partida())
		i = i + 1
	end

	println(contagemResultados / numeroPartidas)
end

melhorEstrategia()
```

The simulation results show which strategy works best at Blackjack. The most conservative strategy (always stopping after 2 cards) wins only 13% of the time, while the optimal strategy, stopping at 19 points, wins 28% of the matches. Being too aggressive and drawing cards up to 21 doesn't work well either, winning only 18% of the time.

There is a balance point between being too cautious and too risky. Stopping at 19 points offers the best results, more than doubling the odds of winning compared to the conservative strategy.
