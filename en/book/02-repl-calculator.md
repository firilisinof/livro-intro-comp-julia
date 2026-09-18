---
kernelspec:
  name: julia-livro-1.11
  display_name: "Julia 1.11 — livro"
---
# Using the Interpreter (REPL) as a Calculator

The goal of this chapter is to introduce the Julia interpreter as a powerful calculator and to present the first programming concepts: variables and functions. But first you need to install the Julia language on your computer. More details about the installation process can be found at this [link](https://julialang.org/downloads/).

Most likely your system is Windows (10 or 11) with a 64-bit architecture. There are a few ways to install Julia on Windows:

1. Through an executable file (`.exe`).
2. Through terminal commands (`winget`).

Either option works fine. The first option does not require any additional program, while the second requires a terminal. A terminal is an application that lets you communicate with the operating system through a command-line interface (CLI). The default terminal on Windows is Windows Terminal. It is strongly recommended that you have it installed, which can be done through the Microsoft Store.

Once you have access to a terminal, there are two possible commands to install Julia: `winget install julia -s msstore` or `winget install -e --id Julialang.Julia`. Again, either option should work.

## Exploring Julia's Interactive Session

You can open an interactive session (also known as a *read-eval-print loop*, or REPL) of Julia by typing the command `julia` on your terminal's command line. On Windows, after installing the language, you can also open an interactive session by double-clicking the Julia executable. Your window should look like this:

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

Inside the session we can enter commands that will be read, evaluated, and printed to the screen. A command is only evaluated when we press **Enter**. Let's start with integer operations. To add two numbers, we can type:

```{code-cell} julia
1 + 2
```

To multiply two other numbers:

```{code-cell} julia
40 * 4
```

As expected, we can use the basic operations of addition (`+`), subtraction (`-`), and multiplication (`*`), and the results are as predicted. However, we'll see next that division behaves in a somewhat particular way:

```{code-cell} julia
a = 84 
b = 2

# As variáveis a e b são do tipo Int64

resultado = a / b
println(resultado)
```

Notice that, in this example, a type conversion happened, because 84 and 2 are integers, while the result is a floating-point number (float). Floating-point numbers are binary representations of real numbers, a topic we'll explore in more detail soon. This conversion is evident from the result being displayed as `42.0` instead of simply `42`. If you want to obtain the result as an integer, you can use the `div` operator:

```{code-cell} julia
div(84,2)
```

Or equivalently using the `\div` operator (to see the division symbol you need to type `\div` followed by the `<tab>` key).

Besides the basic operations, it is also possible to compute exponentiation:

```{code-cell} julia
2^31
```

More complex expressions can also be evaluated:

```{code-cell} julia
23 + 2 * 2 + 3 * 4
```

Yes, the usual operator precedence also applies in Julia. However, remember the first lesson of programming: *Write for humans, not for machines*. We can use parentheses to separate the operations:

```{code-cell} julia
23 + (2 * 2) + (3 * 4)
```

Remember floating-point numbers? All the operations we've seen can also be applied to floats:

```{code-cell} julia
23.5 * 3.14
```

Or:

```{code-cell} julia
12.5 / 2.0
```


The example above shows one more piece of code written clearly for people, since by using `2.0` we make explicit that the second parameter is a floating-point number (float). Floating-point numbers have **limited** precision, so don't be surprised to find unexpected results like the ones shown below:

```{code-cell} julia
1.2 - 1.0
```

Errors like this one are quite rare, so much so that we usually place full trust in the calculations performed by computers and calculators. However, there are limitations, as the examples below show.

```{code-cell} julia
2.6 - 0.7 - 1.9
```

```{code-cell} julia
0.1 + 0.2
```

```{code-cell} julia
10e15 + 1 - 10e15
```

These precision issues are related to the limitations of how numbers are represented in a computer. In simplified terms, values in a computer are encoded in words, made up of bits. On modern computers, words have 64 bits, or 8 bytes. This leads to another limitation, related to very large integers.

```{code-cell} julia
2^63
```

However, for an introductory course, it is enough to be aware of these limitations. Dealing with these issues in depth is the subject of more advanced courses. The error mentioned above is a *silent error*: when working with integers, the value to be represented may exceed the capacity of the available number of bits, and this failure happens without any explicit notification.

Back to arithmetic. Another interesting operator is `%`, which computes the remainder of a division:

```{code-cell} julia
4 % 3
```

So far we've seen how to work with a single value, as if we were using the display of a calculator. But we can go beyond that. Instead of simple memory keys, the computer offers us **variables**. These are like names for values that we want to store and use later.

Besides the basic operations, we also have mathematical operations (functions), such as sine. To find out how a function works, we can ask the environment for help, using a `?` or the macro (special functions) `@doc`, followed by what we want to know, as in:

```julia
@doc sin
```

The output of this command indicates what the function does and even shows some examples:

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

Both `? sin` and `@doc sin` produce the same output.

Notice that not everything shown makes sense right now, but you can already get a sense of how a function like `sin` is used. Now let's look at the square root:

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

There we see that we can compute the square root as in:

```{code-cell} julia
sqrt(4)
```

```{code-cell} julia
sqrt(4.0)
```

Now, notice that the documentation for the `big()` function reads as follows:

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

The `big()` function lets us create numbers of large magnitude, represented by the `BigInt` or `BigFloat` types. This function is particularly useful when you need to work with numbers that are much larger than the limits of standard types like `Int64` or `Int32`. By using `BigInt` numbers, we eliminate overflow problems, as we can see below:

```{code-cell} julia
numero_grande = big(2) ^ 1002
(digitos = ndigits(numero_grande), excede_int64 = numero_grande > typemax(Int64))
```

## Variables and Data Types

As already introduced, Julia has the concept of variables. Variables are used to store various kinds of data, such as integers and floats. We can operate on variables the same way we operate on the data they hold (see the example below).

```{code-cell} julia
a = 7
2 + a
```

When we write `a = 7`, we are performing an operation called **assignment**. The `=` operator in Julia (and in most programming languages) does not represent mathematical equality, but rather an instruction to store the value on the right into the variable on the left. We can picture this as if we were putting the value `7` inside a box called `a`.

Variables in Julia can be assigned new values, and the type of the variable is determined by the last assignment made. The `typeof` function can be used to identify the type of a given variable.

```{code-cell} julia
a = 3
typeof(a)
```

```{code-cell} julia
a = a + 1
typeof(a)
```

Assignment always happens from right to left: first the expression on the right is evaluated, and then that value is stored in the variable on the left. In the example below, the variable `b` starts out with an integer value. However, after the multiplication operation, its value becomes a floating-point type.

```{code-cell} julia
b = 3
b = b * 0.5
typeof(b)
```

The ability to change a variable's type is known as **dynamic typing**. This feature offers several advantages, such as the flexibility to reuse variables to store different types of data over time and reduced verbosity, since it isn't necessary to specify the type of each variable, which improves code readability. In this context, we can observe that Julia has several primitive types, the main ones being:

```{code-cell} julia
typeof(1)
```

```{code-cell} julia
typeof(1.1)
```

```{code-cell} julia
typeof("Bom dia")
```

Speaking of **strings**, they are defined as sets of characters between quotation marks, like:

```{code-cell} julia
s1 = "Olha que legal"
s2 = "Outra String"
```

It is also possible to perform operations on strings, such as **concatenation**:

```{code-cell} julia
s1 = "Tenha um"
s2 = " Bom dia"
s3 = s1 * s2
```

Or repetition using the power operator:

```{code-cell} julia
s = "Não vou mais fazer coisas que possam desagradar os meus colegas "
s ^ 10
```

To avoid typing too many characters, we can sometimes use *syntactic sugar*.

```{code-cell} julia
x = 1
x = x + 1
x += 1  # '+= 1' equivale a '= x + 1', também funciona para os operadores *, - e /
```

The code above uses comments (everything after `#`). These comments are ignored by the interpreter and can be used to make the code more readable.

Still on the subject of variables, there are some rules regarding their names: they must start with a letter (or with `_`), can contain digits, and cannot be reserved words. Julia, being a modern language, also accepts Unicode characters and emojis in names, such as Δ (`\Delta`).

```{code-cell} julia
Δ = 2
```

```{code-cell} julia
🐱 = 5 # \:cat: <tab>
🐶 = 3 # \:dog: <tab>
🏠 = 20 # \:house: <tab>
```

This doesn't add anything from an algorithmic standpoint, but it does let you have some rather charming variables. The list of figures can be found [here](https://docs.julialang.org/en/v1/manual/unicode-input/).

## Output

To print information to the terminal, we use the `print()` and `println()` functions. The difference between them is that the first does not move to a new line, while the second does.

```{code-cell} julia
print("Hello ")
println("World!")
println("Ola, mundo!")
```

The `println()` command can take multiple arguments, which will be automatically converted to strings and concatenated:

```{code-cell} julia
nome = "Maria"
idade = 25
println("Olá, meu nome é ", nome, " e tenho ", idade, " anos.")
```

For more complex formatting, Julia offers **string interpolation**, where we can insert variables and expressions directly inside a string using the dollar sign `$`:

```{code-cell} julia
nome = "João"
altura = 1.75
println("$nome tem $altura metros de altura.")
```

We can also include expressions inside braces after the dollar sign:

```{code-cell} julia
preco = 9.99
quantidade = 3
println("Total da compra: R\$ $(preco * quantidade)")
```

For numeric formatting, we can use the `@sprintf` function or the `@printf` macro from the `Printf` module (more details about modules in {ref}`sec-files-modules`):

```{code-cell} julia
using Printf

valor = 123.456
@printf("Valor formatado: %.2f\n", valor)  # Exibe com 2 casas decimais
```

Or alternatively:

```{code-cell} julia
valor = 123.456
s = @sprintf("Valor formatado: %.2f", valor)
println(s)
```

(sec-files-modules)=
## External Files and Modules

In the previous example we used the syntax `using Printf`. This is the syntax for importing a module in Julia. Modules are organized collections of code that we can use in our programs. The `Printf` module is part of Julia's **standard library** and offers functions for formatting types in the style of the C language. By writing `using Printf`, we tell the interpreter that we want to access the functions of this module, such as `@printf` and `@sprintf`. To find out which functions are available in this and other modules, check Julia's official documentation. The specific documentation for the `Printf` module is available at <https://docs.julialang.org/en/v1/stdlib/Printf/>.

Julia ships with several standard modules that can be useful:

- `Statistics`: for statistical calculations (mean, median, etc.)
- `Dates`: for working with dates and times
- `Printf`: for advanced text formatting
- `LinearAlgebra`: for linear algebra operations
- `Random`: for random number generation

The Julia community also develops many modules (packages) that can be installed to expand the language's functionality. We'll learn how to do that later in the course.

Besides modules, Julia lets you load code from external files using the `include()` command. This command reads the specified file and executes its entire contents in the current context. As a result, all the functions, variables, and definitions from the file become available in the environment where `include` was called.

On Windows, file paths traditionally use backslashes (`\`). In Julia, however, we can use either forward slashes (`/`) or backslashes. There is one important detail: when we use backslashes inside strings in Julia, we need to double them. This happens because a lone backslash (`\`) is a special character in strings, used to represent characters like `\n` (newline) or `\t` (tab). To indicate that we want a literal backslash, we need to write two of them (`\\`). Because of this, paths with several folders become harder to read:

```julia
# Caminho usando barras normais (recomendado)
include("C:/Users/MeuUsuario/Documentos/arquivo.jl")

# Mesmo caminho usando barras invertidas (mais complicado)
include("C:\\Users\\MeuUsuario\\Documentos\\arquivo.jl")
```

If the file is in the same directory as your current script or REPL, you can simply use the file name:

```julia
include("funcoes.jl")
```

For files in subdirectories of the current directory:

```julia
include("utilitarios/matematica.jl")
```

For files in the parent directory:

```julia
include("../exemplos.jl")
```

Let's look at a practical example. Suppose you created a file called `funcoes.jl` in the folder `C:/Projetos/Julia/` with the following content:

```julia
function ola(nome)
    println("Olá ", nome)
end

function soma(a, b)
    return a + b
end
```

Now you can use these functions (more on functions in {ref}`sec-functions`) in the REPL or in another file:

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

This feature is especially useful for organizing your code across multiple files, letting you split larger programs into smaller pieces.

## What is a `.jl` File?

A `.jl` file is similar to a `.txt` text file, but with the `.jl` extension. Although it's possible to open it with a simple text editor like Notepad, doing so is not recommended for programming. `.jl` files are Julia source code files and are generally edited with dedicated programming editors, such as Visual Studio Code, Atom, or Sublime Text.

There isn't one text editor that's definitively better than the others, what matters is choosing the one you feel most comfortable with. Our recommendation is Visual Studio Code, which offers much more advanced features than a plain text editor and has an extension dedicated to the Julia language, making it significantly easier to write code. To get started using Visual Studio Code with Julia, the following tutorials may be helpful:

- [https://code.visualstudio.com/docs/getstarted/getting-started](https://code.visualstudio.com/docs/getstarted/getting-started)
- [https://code.visualstudio.com/docs/languages/julia](https://code.visualstudio.com/docs/languages/julia)

## Check Your Understanding

1. What is the difference between the results obtained from the `/` and `div` operators in Julia? In which situations would each be more appropriate?
2. Why doesn't the expression `2.6 - 0.7 - 1.9` result in exactly zero? What does this teach us about computational calculations?
3. Explain what "dynamic typing" means and how it affects the behavior of variables when we assign different types of values to them.
4. Use the `big()` function to compute $2^{1000}$. Compare this result with what happens when you try to compute $2^{1000}$ without using `big()`.
5. Store your first and last name in separate variables, then combine them to form your full name with a space between them. Also demonstrate string repetition.
6. Create the variables `a = 10`, `b = 3`, and `c = 4.5`. Perform the following calculations: `a + b + c`, `a * b * c`, `a % b`, and check the type of the result of each operation using `typeof()`.

## Explore on Your Own

1. Look through the documentation for two mathematical functions that were not mentioned in this chapter and test their use in the REPL.
2. What happens when you try to divide a number by zero in Julia? And when you compute `0/0`? Try it and observe the results.
3. Try the Unicode operator `≈` (type `\approx` followed by **TAB**). How does it behave when comparing `0.1 + 0.2 ≈ 0.3`?
4. Investigate the `round()` function and use it to fix some of the precision issues demonstrated in this chapter.
