---
kernelspec:
  name: julia-livro-1.11
  display_name: "Julia 1.11 — livro"
---

# String Exercises

In this lesson, we'll explore functions that manipulate strings and write tests to check their correctness. For some functions, we'll notice that there are several ways to get the same result.

## Concatenating letters

The first function, `concatena`, concatenates the first two and the last two letters of a string.

```{code-cell} julia
function concatena(s::String)::String
    if length(s) < 2
        return "Erro: tamanho da string menor do que 2"
    end
    resposta = s[1:2]*s[end-1:end]
    return resposta
end
```

Here we use `s[1:2]` to get the first two letters of s, which is a more concise way to access more than one index of an object. Alternatively, we could access these two indices separately with the command `s[1]*s[2]`.

To check whether the function is working correctly, we can use the following test:

```{code-cell} julia
using Test

function testeConcatena()
  @test concatena("Ola Bom Dia") == "Olia"
  @test concatena("oi") == "oioi"
  @test concatena("tre") == "trre"
  @test concatena("a") == "Erro: tamanho da string menor do que 2"
  @test concatena("a123") == "a123"
end
```

## String reversal

We need to create a function that reverses a string, returning the characters in reverse order.

```{code-cell} julia
function inverte(s::String)::String
    # Inicializamos uma string vazia
    inversa=""

    # Intervalo de lenght(s) até 1, a passos de -1
    for i in length(s):-1:1
        # Concatena cada caractere na ordem inversa
        inversa*=s[i]
    end

    return inversa
end
```

To get the result we want, we build a `for` loop that iterates from the last index of the string, `length(s)`, down to the first, using a step of -1. At each iteration, we concatenate the current character, `s[i]`, onto the string `inversa`, so the characters end up in reverse order.

Now we can write a test function to check that our `inverte` function works correctly.

```{code-cell} julia
using Test

function testeInverte()
  @test inverte("123") == "321"
  @test inverte("x") == "x"
  @test inverte("SOS") == "SOS"
  @test inverte("tres") == "sert"
end
```

## The reverse function


Julia already provides a function called `reverse`, which can be used to reverse both vectors and strings. For example:

```{code-cell} julia
reversa = reverse("exemplo")
```

In this example, the `reverse` function receives only the object to be reversed as a parameter, but in the case of vectors, we can also specify exactly which range we want reversed.

```{code-cell} julia
vetor = [1, 2, 3, 4, 5]
reversa = reverse(vetor, 2, 4)
```

##  Modifying a string

The third function, `modifica`, alters a string that ends with "ing" by adding "ly", or otherwise adds "ing".

```{code-cell} julia
function modifica(s::String)::String
    if length(s) < 3
        return "Erro: tamanho da string menor do que 3"
    end
    
    if s[end-2:end] == "ing"
        s = s*"ly"
    else    
        s = s*"ing"                
    end


    return s
end
```

In this example, we manually check the last three characters of the string s. However, Julia offers a more practical and readable function called `endswith`, which we can use to simplify this check.

```{code-cell} julia
function modifica(s::String)::String
    if length(s) < 3
        return "Erro: tamanho da string menor do que 3"
    end
    
    if endswith(s, "ing")
        s = s*"ly"
    else    
        s = s*"ing"                
    end

    return s
end
```

Let's now write the test that checks that the functions above work correctly.

```{code-cell} julia
using Test
function testaModifica()
  @test modifica("doing") == "doingly"
  @test modifica("sing") == "singly"
  @test modifica("run") == "runing"
  @test modifica("talk") == "talking"
end
```

## Rearranging letters

The second function, `rearranja`, receives a string and returns a string that contains the lowercase letters first, followed by the uppercase letters.

We can check whether a letter is uppercase or lowercase using the ASCII table, which encodes characters as integers. In the table, uppercase letters fall in the range 65 to 90, and lowercase letters in the range 97 to 122.

To learn more about the ASCII table you can visit [this page](https://www.ime.usp.br/~kellyrb/mac2166_2015/tabela_ascii.html).

```{code-cell} julia
function rearranja(s::String)::String
    maiusculos=""
    minusculos=""

    for i in 1:length(s)
        if Int(s[i]) >= 65 && Int(s[i]) <= 90 
            maiusculos = maiusculos*s[i]
        elseif Int(s[i]) >= 97 && Int(s[i]) <= 122
            minusculos = minusculos*s[i]
        end 
    end

    return minusculos*maiusculos
     
end
```

A more readable approach is to use the `islowercase` and `isuppercase` functions, which check whether a letter is lowercase or uppercase, respectively.

```{code-cell} julia
function rearranja(s::String)::String
    maiusculos=""
    minusculos=""

    for i in 1:length(s)
        if isuppercase(s[i]) 
            maiusculos = maiusculos*s[i]
        elseif islowercase(s[i])
            minusculos = minusculos*s[i]
        end 
    end

    return minusculos*maiusculos
     
end
```

We can then write the test for our functions.

```{code-cell} julia
using Test

function testaRearranja()
  @test rearranja1("PaRaLelO") == "aaelPRLO"
  @test rearranja1("ELEfantE") == "fantELEE"
  @test rearranja1("Olá") == "lO"
  @test rearranja1("13La2") == "aL"
  @test rearranja2("PaRaLelO") == "aaelPRLO"
  @test rearranja2("ELEfantE") == "fantELEE"
  @test rearranja2("Olá") == "láO"
  @test rearranja2("13La2") == "aL"
end
```

## Finding the longest word

Our last function should receive a list of words and return the longest one, along with its length.

```{code-cell} julia
function maior_palavra(vetor::Vector{String})
    # Inicialmente, a maoior palavra que encontramos é uma string vazia
    maior_palavra = ""
    maior_tamanho = 0

    for palavra in vetor
        #  Verifica se a palavra atual é maior que a maior encontrada até agora
        if length(palavra) > maior_tamanho 
            maior_palavra = palavra
            maior_tamanho = length(palavra)
        end
    end

    return maior_palavra, maior_tamanho

end
```

Although this code looks correct, it doesn't handle the case where there is more than one word with the greatest length. For example:

```{code-cell} julia
vetor = ["boa", "bem", "oi"]
maior_palavra(vetor)
```

In this case, only the word "boa" is returned, even though "bem" has the same length. To fix this, we need to change the variable that stores the longest word so it can hold more than one word. We'll use a vector of strings for that.

```{code-cell} julia
function maiores_palavras(vetor::Vector{String})
    maiores_palavras = String[]
    maior_tamanho = 0

    for palavra in vetor
        # Se a palavra é maior do que o maior tamanho salvo, 
        # então todas as palavras que estão no vetor maior_palavra são menores do que a palavra atual
        if length(palavra) > maior_tamanho 
            # Limpa o vetor e salva a palavra atual
            maiores_palavras = String[]
            push!(maiores_palavras, palavra)
            maior_tamanho = length(palavra)

        # Se é igual ao tamanho salvo, então é do mesmo tamanho que as palavras já salvas no vetor maiores_palavras,
        # apenas damos push na palavra atual
        elseif length(palavra) == maior_tamanho
            push!(maiores_palavras, palavra) 
        end
    end

    return maiores_palavras, maior_tamanho

end
```

Now we can write tests for this last function.

```{code-cell} julia
using Test

function testeMaioresPalavras()
    vetor1 = ["gato", "elefante", "cachorro"]
    @test maiores_palavras(vetor1) == (["elefante", "cachorro"], 8)  
    
    vetor2 = ["a", "ab", "abc"]
    @test maiores_palavras(vetor2) == (["abc"], 3)        
    
    vetor3 = ["bem", "boa", "bom", "oi"]
    @test maiores_palavras(vetor3) == (["bem", "boa", "bom"], 3)       

    vetor4 = ["", " ", "teste"]
    @test maiores_palavras(vetor4) == (["teste"], 5)      

    vetor5 = String[]
    @test maiores_palavras(vetor5) == ([], 0)              

    vetor6 = ["a", "ab", "abc", "xyz", "xy"]
    @test maiores_palavras(vetor6) == (["abc", "xyz"], 3)  
end
```

## Returning multiple values

As we saw in the previous exercise, Julia allows a function to return multiple values. This lets you send back more than one result from a function call, making the code more concise and easier to understand. This is especially useful for things like mathematical operations, decompositions, or data processing, which often need more than one result.

To return multiple values in Julia, you can simply separate them with commas. Here's a simple example:

```{code-cell} julia
function troca(a,b)
    aux = a
    a = b
    b = aux

    return a, b
end
```

When calling this function, you can capture the multiple returned values in separate variables:

```{code-cell} julia
a, b = troca(1, 10)
```
