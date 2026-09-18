---
kernelspec:
  name: julia-livro-1.11
  display_name: "Julia 1.11 — livro"
---
# Usando o Interpretador (REPL) como Calculadora

O objetivo deste capítulo é apresentar o interpretador de Julia como uma calculadora poderosa e introduzir os primeiros conceitos de programação: variáveis e funções. Mas primeiro é preciso instalar a linguagem Julia em seu computador. Mais detalhes sobre o processo de instalação podem ser encontrados neste [link](https://julialang.org/downloads/).

Muito provavelmente seu sistema é Windows (10 ou 11) e sua arquitetura é de 64-bits. Há algumas formas de instalar Julia no Windows:

1. Através de um arquivo executável (`.exe`).
2. Através de comandos pelo terminal (`winget`).

A princípio qualquer uma das opções é adequada. A primeira opção não requer nenhum programa adicional, enquanto que a segunda requer um terminal. Um terminal é um aplicativo que permite a comunicação com o sistema operacional por meio de uma interface de linha de comando (CLI). O terminal padrão do Windows é o Windows Terminal. É fortemente recomendado que você o tenha instalado e isso pode ser feito através da Microsoft Store.

Uma vez que você tenha acesso a um terminal há dois comandos possíveis para instalar Julia: `winget install julia -s msstore` ou `winget install -e --id Julialang.Julia`. Mais uma vez, qualquer uma das opções deve funcionar.

## Explorando a Sessão Interativa de Julia

Você pode abrir uma sessão interativa (também conhecido como um *read-eval-print loop* ou REPL) de Julia digitando o comando `julia` na linha de comando do seu terminal. No Windows, após instalação da linguagem, é possível abrir uma sessão interativa clicando duas vezes no executável Julia. A sua janela deve ser parecida com

```
               _
   _       _ _(_)_     |  Documentation: https://docs.julialang.org
  (_)     | (_) (_)    |
   _ _   _| |_  __ _   |  Type "?" for help, "]?" for Pkg help.
  | | | | | | |/ _` |  |
  | | |_| | | | (_| |  |  Version 1.11.3 (2025-01-21)
 _/ |\__'_|_|_|\__'_|  |  Official https://julialang.org/ release
|__/                   |


julia>
```

Dentro da sessão podemos inserir comandos que serão lidos, avaliados e impressos na tela. Um comando só é avaliado quando teclamos **Enter**. Vamos começar com operações com números inteiros. Para somar dois números podemos digitar:

```{code-cell} julia
1 + 2
```

Para multiplicar outros dois número:

```{code-cell} julia
40 * 4
```

Como esperado, podemos utilizar as operações básicas de soma (`+`), subtração (`-`) e multiplicação (`*`), e os resultados ocorrem como previsto. No entanto, observaremos a seguir que o comportamento da divisão apresenta algumas particularidades:

```{code-cell} julia
a = 84 
b = 2

# As variáveis a e b são do tipo Int64

resultado = a / b
println(resultado)
```

Notem que, neste exemplo, ocorreu uma conversão de tipo, pois 84 e 2 são números inteiros, enquanto o resultado é um número em ponto flutuante (float). Os pontos flutuantes são representações binárias de números reais, tema que exploraremos com mais detalhes em breve. Esta conversão fica evidente pela representação do resultado como `42.0`, em vez de simplesmente `42`. Caso deseje obter o resultado como um número inteiro, é possível utilizar o operador `div`:

```{code-cell} julia
div(84,2)
```

Ou de forma equivalente usando o operador `\div` (para conseguir ver o símbolo da divisão é necessário digitar `\div` seguido da tecla `<tab>`).

Além das operações básicas, é possível fazer exponenciação:

```{code-cell} julia
2^31
```

Expressões mais complexas também podem ser calculadas:

```{code-cell} julia
23 + 2 * 2 + 3 * 4
```

Sim, a precedência de operadores usual também é válida em Julia. Entretanto, lembre-se da primeira lição de programação: *Escreva para humanos, não para máquinas*. Podemos usar parênteses para separar as operações:

```{code-cell} julia
23 + (2 * 2) + (3 * 4)
```

Lembra dos pontos flutuantes? Todas as operações vistas podem ser aplicadas em pontos flutuantes:

```{code-cell} julia
23.5 * 3.14
```

Ou:

```{code-cell} julia
12.5 / 2.0
```


O exemplo acima demonstra mais um código escrito de forma clara para pessoas, onde ao utilizarmos `2.0` deixamos explícito que o segundo parâmetro é um número de ponto flutuante (float). É fundamental compreender que números de ponto flutuante possuem precisão **limitada**, portanto não se surpreenda ao encontrar resultados inesperados como os demonstrados abaixo:

```{code-cell} julia
1.2 - 1.0
```

Erros como esse são bastante raros, tanto que normalmente depositamos total confiança nas contas realizadas por computadores e calculadoras. No entanto, é importante reconhecer que existem limitações (veja os exemplos abaixo).

```{code-cell} julia
2.6 - 0.7 - 1.9
```

```{code-cell} julia
0.1 + 0.2
```

```{code-cell} julia
10e15 + 1 - 10e15
```

Esses problemas de precisão estão ligados à limitação de como os números são representados no computador. De maneira simplificada, os valores no computador são codificados em palavras, formadas por bits. Nos computadores modernos, as palavras têm 64 bits, ou 8 bytes. Logo, uma outra limitação está relacionada aos números inteiros muito grandes.

```{code-cell} julia
2^63
```

No entanto, para um curso introdutório, é suficiente estar ciente dessas limitações. O tratamento dessas questões faz parte de disciplinas mais avançadas. Vale ressaltar que o erro mencionado anteriormente é um *erro silencioso*, ou seja, ao trabalharmos com números inteiros, pode acontecer que o valor a ser representado exceda a capacidade do número de bits disponível, resultando em uma falha que ocorre sem notificação explícita.

Voltando às contas. Um outro operador interessante é o `%` que calcula o resto da divisão

```{code-cell} julia
4 % 3
```

Até agora vimos como trabalhar com um único valor, como se estivéssemos usando o visor de uma calculadora. Mas podemos ir além disso. Em vez de simples teclas de memória, o computador nos oferece **variáveis**. Essas são como nomes para valores que queremos armazenar e utilizar posteriormente.

Além das operações básicas também temos as operações matemáticas (funções), como por exemplo o seno, *sine* em inglês. Para saber como uma função funciona podemos pedir ajuda ao ambiente, usando uma `?` ou o macro (funções especiais) `@doc`, e em seguida digitando o que queremos saber, como por exemplo em:

```julia
@doc sin
```

A saída desse comando indica a operação que a função realiza e ainda apresenta alguns exemplos: 

```
sin(x)

Compute sine of x, where x is in radians.

See also sind, sinpi, sincos, cis, asin.

Examples
≡≡≡≡≡≡≡≡

julia> round.(sin.(range(0, 2pi, length=9)'), digits=3)
1×9 Matrix{Float64}:
0.0  0.707  1.0  0.707  0.0  -0.707  -1.0  -0.707  -0.0
```

Ambos os comandos `? sin` e `@doc sin` possuem a mesma saída.

Notem que nem tudo que foi apresentado faz sentido no momento, mas já dá para entender o uso de uma função como `sin`. Vejamos agora a raiz quadrada:

```julia
@doc sqrt
```

```
sqrt(x)

Return \sqrt{x}.

Throws DomainError for negative Real arguments. Use complex negative arguments instead. Note that sqrt has a branch cut
along the negative real axis.

The prefix operator √ is equivalent to sqrt.

See also: hypot
...
```

Nela vemos que é possível calcular a raiz como em:

```{code-cell} julia
sqrt(4)
```

```{code-cell} julia
sqrt(4.0)
```

Agora, observe que a documentação da função `big()` tem a seguinte ajuda:

```
big(T::Type)

Compute the type that represents the numeric type T with arbitrary precision. Equivalent to typeof(big(zero(T))).

Examples
≡≡≡≡≡≡≡≡

julia> big(Rational)
Rational{BigInt}

julia> big(Float64)
BigFloat

julia> big(Complex{Int})
Complex{BigInt}

big(x)

Convert a number to a maximum precision representation (typically BigInt or BigFloat). See BigFloat for information about
some pitfalls with floating-point numbers.
```

A função `big()` permite criar números de grande magnitude, representados pelos tipos `BigInt` ou `BigFloat`. Essa função é particularmente útil quando você precisa trabalhar com números muito grandes que ultrapassam os limites dos tipos padrão, como `Int64` ou `Int32`. Ao utilizar números do tipo `BigInt`, eliminamos problemas de estouro (overflow), conforme podemos observar abaixo:

```{code-cell} julia
numero_grande = big(2) ^ 1002
(digitos = ndigits(numero_grande), excede_int64 = numero_grande > typemax(Int64))
```

## Variáveis e Tipos de Dados

Como já introduzido, em Julia, temos o conceito de variáveis. Variáveis servem para armazenar dados diversos, como inteiros e floats. Podemos operar nas variáveis da mesma forma que operamos nos dados que elas guardam (veja o exemplo abaixo).

```{code-cell} julia
a = 7
2 + a
```

Quando escrevemos `a = 7`, estamos realizando uma operação chamada **atribuição**. O operador `=` em Julia (e na maioria das linguagens de programação) não representa igualdade matemática, mas sim uma instrução para armazenar o valor à direita na variável à esquerda. Podemos visualizar isso como se estivéssemos colocando o valor `7` dentro de uma caixa chamada `a`. 

É importante destacar que as variáveis em Julia podem receber novos valores, e o tipo da variável é determinado pela última atribuição realizada. A função `typeof` pode ser usada para identificar o tipo da variável especificada.

```{code-cell} julia
a = 3
typeof(a)
```

```{code-cell} julia
a = a + 1
typeof(a)
```

A atribuição sempre acontece da direita para a esquerda: primeiro calcula-se o valor da expressão à direita, e depois esse valor é armazenado na variável à esquerda. No exemplo a seguir, a variável `b` começa com um valor de tipo inteiro. No entanto, após a operação de multiplicação, seu valor passa a ser do tipo ponto flutuante.

```{code-cell} julia
b = 3
b = b * 0.5
typeof(b)
```

A capacidade de alterar o tipo da variável é conhecida como **tipagem dinâmica**. Esta característica apresenta diversas vantagens, como a flexibilidade de reutilizar variáveis para armazenar diferentes tipos de dados ao longo do tempo e a menor verbosidade, pois não é necessário especificar o tipo de cada variável, o que melhora a legibilidade do código. Neste contexto, podemos observar que Julia possui vários tipos primitivos, sendo os principais:

```{code-cell} julia
typeof(1)
```

```{code-cell} julia
typeof(1.1)
```

```{code-cell} julia
typeof("Bom dia")
```

Falando em **strings**, eles são definidos por conjuntos de caracteres entre aspas como:

```{code-cell} julia
s1 = "Olha que legal"
s2 = "Outra String"
```

Também é possível realizar operações com strings, como **concatenação**:

```{code-cell} julia
s1 = "Tenha um"
s2 = " Bom dia"
s3 = s1 * s2
```

Ou repetição usando o operador de potência:

```{code-cell} julia
s = "Não vou mais fazer coisas que possam desagradar os meus colegas "
s ^ 10
```

Para evitar que se digitem muitos caracteres, por vezes podemos usar *açucares sintáticos*.

```{code-cell} julia
x = 1
x = x + 1
x += 1  # '+= 1' equivale a '= x + 1', também funciona para os operadores *, - e /
```

O código acima utiliza comentários (tudo depois do `#`). Esses comentários são ignorados pelo interpretador e podem ser usados para tornar o código mais legível.

Ainda sobre variáveis, há algumas regras referentes aos seus nomes: devem começar com uma letra (ou com `_`), podem conter dígitos e não podem ser palavras reservadas. Vale ressaltar que Julia, por ser uma linguagem moderna, aceita caracteres unicode e emojis nos nomes, como por exemplo o Δ (`\Delta`).

```{code-cell} julia
Δ = 2
```

```{code-cell} julia
🐱 = 5 # \:cat: <tab>
🐶 = 3 # \:dog: <tab>
🏠 = 20 # \:house: <tab>
```

Isso não adiciona nada do lado de algoritmos, mas é possível ter variáveis bem bonitinhas. A lista de figuras pode ser encontrada [aqui](https://docs.julialang.org/en/v1/manual/unicode-input/).

## Saída de Dados

Para imprimir informações no terminal, usamos as funções `print()` e o `println()`. A diferença entre elas é que a primeira não pula linha, enquanto que a segunda pula.

```{code-cell} julia
print("Hello ")
println("World!")
println("Ola, mundo!")
```

O comando `println()` pode receber múltiplos argumentos, que serão convertidos em strings e concatenados automaticamente:

```{code-cell} julia
nome = "Maria"
idade = 25
println("Olá, meu nome é ", nome, " e tenho ", idade, " anos.")
```

Para formatações mais complexas, Julia oferece **interpolação de strings**, onde podemos inserir variáveis e expressões diretamente dentro de uma string usando o cifrão `$`:

```{code-cell} julia
nome = "João"
altura = 1.75
println("$nome tem $altura metros de altura.")
```

Também podemos incluir expressões dentro de chaves após o cifrão:

```{code-cell} julia
preco = 9.99
quantidade = 3
println("Total da compra: R\$ $(preco * quantidade)")
```

Para formatação numérica, podemos usar a função `@sprintf` ou a macro `@printf` do módulo `Printf` (detalhes sobre módulos na {ref}`sec-files-modules`):

```{code-cell} julia
using Printf

valor = 123.456
@printf("Valor formatado: %.2f\n", valor)  # Exibe com 2 casas decimais
```

Ou alternativamente:

```{code-cell} julia
valor = 123.456
s = @sprintf("Valor formatado: %.2f", valor)
println(s)
```

(sec-files-modules)=
## Arquivos Externos e Módulos

No exemplo anterior usamos a sintaxe `using Printf`. Esta é a sintaxe para importar um módulo em Julia. Os módulos são coleções organizadas de código que podemos utilizar em nossos programas. O módulo `Printf` faz parte da **biblioteca padrão** de Julia e oferece funções para formatação de tipos no estilo da linguagem C. Ao escrever `using Printf`, informamos o interpretador que queremos acessar as funções deste módulo, como `@printf` e `@sprintf`. Para descobrir quais funções estão disponíveis neste e em outros módulos, consulte a documentação oficial de Julia. A documentação específica do módulo `Printf` está disponível em <https://docs.julialang.org/en/v1/stdlib/Printf/>.

Julia vem com vários módulos padrão que podem ser úteis:

- `Statistics`: para cálculos estatísticos (média, mediana, etc.)
- `Dates`: para trabalhar com datas e horas
- `Printf`: para formatação avançada de texto
- `LinearAlgebra`: para operações de álgebra linear
- `Random`: para geração de números aleatórios

A comunidade de Julia também desenvolve diversos módulos (pacotes) que podem ser instalados para expandir as funcionalidades da linguagem. Vamos aprender a fazer isso mais adiante no curso.

Além de módulos, Julia permite carregar código de arquivos externos usando o comando `include()`. Este comando lê o arquivo especificado e executa todo seu conteúdo no contexto atual. Como resultado, todas as funções, variáveis e definições do arquivo tornam-se disponíveis no ambiente onde `include` foi chamado.

No Windows, os caminhos de arquivo tradicionalmente usam barras invertidas (`\`). Porém, em Julia, podemos usar tanto barras normais (`/`) quanto barras invertidas. Há um detalhe importante: quando usamos barras invertidas dentro de strings em Julia, precisamos duplicá-las. Isso ocorre porque a barra invertida sozinha (`\`) é um caractere especial em strings, usado para representar caracteres como `\n` (nova linha) ou `\t` (tabulação). Para indicar que queremos uma barra invertida literal, precisamos escrever duas (`\\`). Por esse motivo, caminhos com várias pastas tornam-se mais difíceis de ler:

```julia
# Caminho usando barras normais (recomendado)
include("C:/Users/MeuUsuario/Documentos/arquivo.jl")

# Mesmo caminho usando barras invertidas (mais complicado)
include("C:\\Users\\MeuUsuario\\Documentos\\arquivo.jl")
```

Se o arquivo estiver no mesmo diretório que seu script ou REPL atual, basta usar o nome do arquivo:

```julia
include("funcoes.jl")
```

Para arquivos em subdiretórios do diretório atual:

```julia
include("utilitarios/matematica.jl")
```

Para arquivos no diretório pai:

```julia
include("../exemplos.jl")
```

Vamos ver um exemplo prático. Suponha que você tenha criado um arquivo chamado `funcoes.jl` na pasta `C:/Projetos/Julia/` com o seguinte conteúdo:

```julia
function ola(nome)
    println("Olá ", nome)
end

function soma(a, b)
    return a + b
end
```

Agora você pode usar essas funções (mais sobre funções no {ref}`sec-functions`) no REPL ou em outro arquivo:

```julia
# No REPL ou em um arquivo na mesma pasta:
include("funcoes.jl")

# Ou com caminho completo:
include("C:/Projetos/Julia/funcoes.jl")

# Agora podemos usar as funções definidas em funcoes.jl
ola("Alfredo")         # Imprime: Olá Alfredo
resultado = soma(5, 3) # resultado = 8
println(resultado)     # Imprime: 8
```

Esta funcionalidade é especialmente útil para organizar seu código em múltiplos arquivos, permitindo que você divida programas maiores em partes menores.

## O que é um arquivo `.jl`?

Um arquivo `.jl` é semelhante a um arquivo de texto `.txt`, porém com a extensão `.jl`. Embora seja possível abri-lo com um editor de texto simples como o Bloco de Notas, não é recomendado utilizá-lo para programação. Os arquivos `.jl` são arquivos de código-fonte da linguagem Julia e são geralmente editados com editores específicos para programação, como Visual Studio Code, Atom ou Sublime Text.

Não existe um editor de texto definitivamente superior aos demais, o importante é escolher aquele com o qual você se sinta mais confortável. Nossa recomendação é o Visual Studio Code, que oferece recursos muito mais avançados que um editor de texto comum e possui uma extensão dedicada à linguagem Julia, facilitando significativamente a escrita de código. Para começar a usar o Visual Studio Code com Julia, os tutoriais a seguir podem ser úteis:

- [https://code.visualstudio.com/docs/getstarted/getting-started](https://code.visualstudio.com/docs/getstarted/getting-started)
- [https://code.visualstudio.com/docs/languages/julia](https://code.visualstudio.com/docs/languages/julia)

## Verifique seu Aprendizado

1. Qual a diferença entre os resultados obtidos pelos operadores `/` e `div` em Julia? Em quais situações cada um seria mais apropriado?
2. Por que a expressão `2.6 - 0.7 - 1.9` não resulta exatamente em zero? O que isso nos ensina sobre cálculos computacionais?
3. Explique o que significa 'tipagem dinâmica' e como isso afeta o comportamento das variáveis quando atribuímos diferentes tipos de valores a elas.
4. Use a função `big()` para calcular $2^{1000}$. Compare este resultado com o que acontece ao tentar calcular $2^{1000}$ sem usar `big()`.
5. Armazene seu nome e sobrenome em variáveis separadas e depois combine-as para formar seu nome completo com um espaço entre elas. Demonstre também a operação de repetição de strings.
6. Crie as variáveis `a = 10`, `b = 3` e `c = 4.5`. Realize os seguintes cálculos: `a + b + c`, `a * b * c`, `a % b` e verifique o tipo do resultado de cada operação usando `typeof()`.

## Explore por Conta Própria

1. Procure na documentação duas funções matemáticas que não foram mencionadas no capítulo e teste seu uso no REPL.
2. O que acontece quando você tenta dividir um número por zero em Julia? E quando calcula `0/0`? Teste e observe os resultados.
3. Experimente o operador Unicode `≈` (digite `\approx` seguido de **TAB**). Como ele se comporta ao comparar `0.1 + 0.2 ≈ 0.3`?
4. Investigue a função `round()` e utilize-a para corrigir alguns dos problemas de precisão demonstrados no capítulo.
