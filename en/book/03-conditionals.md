---
kernelspec:
  name: julia-livro-1.11
  display_name: "Julia 1.11 — livro"
---

# Control Structures and Decision Making

The goal of this chapter is to understand how a program can make decisions and change its flow of execution. We will explore comparison operators, the Boolean type, and the `if`, `else`, and `elseif` conditional structures in Julia.

## Comparison Operators and the Boolean Type

Before studying conditional structures, we need to understand comparison operators and the data type they produce: the Boolean type (`Bool`). A Boolean variable can only have two possible values: `true` or `false`.
Let's look at the main comparison operators in Julia:

```{code-cell} julia
# Igualdade: retorna true se os valores forem iguais
2 + 2 == 4
```

```{code-cell} julia
# Diferença: retorna true se os valores forem diferentes
3 != 8
```

```{code-cell} julia
# Menor que: retorna true se o primeiro valor for menor que o segundo
23 < 24
```

```{code-cell} julia
# Menor ou igual: retorna true se o primeiro valor for menor ou igual ao segundo
42 <= 44
```

```{code-cell} julia
# Maior que: retorna true se o primeiro valor for maior que o segundo
42 > 2
```

```{code-cell} julia
# Maior ou igual: retorna true se o primeiro valor for maior ou igual ao segundo
42 >= 42
```

In programming languages, including Julia, the equal sign (`=`) is used to assign values to variables, while the equality operator (`==`) is used for comparisons.

We can check the type of a comparison expression:

```{code-cell} julia
typeof(2 == 3)
```

As expected, the type is `Bool`, indicating a Boolean value.


## Logical Operators

In addition to comparison operators, Julia also provides logical operators that let us combine or modify Boolean values:

```{code-cell} julia
# Operador NOT (negação): inverte o valor booleano
!true
```

```{code-cell} julia
!false
```

```{code-cell} julia
# Operador AND: retorna true apenas se ambos os valores forem true
true && true
```

```{code-cell} julia
true && false
```

```{code-cell} julia
# Operador OR: retorna true se pelo menos um dos valores for true
true || false
```

```{code-cell} julia
false || false
```

These operators are essential for building more complex conditions in our conditional structures.

## Changing the Flow of Execution with if-else

So far, our programs have followed a linear flow of execution, with instructions being executed in the order they were written. Consider the example:

```{code-cell} julia
println("Oi")
println("um")
println("dois")
```

The order of printing will be "Oi", "um", and "dois", exactly in the sequence in which the commands were written.

However, we often need our program to make decisions and execute different blocks of code depending on certain conditions. This is where the `if` conditional structure comes in.

## The if Structure

The **if** structure lets us execute a block of code only if a condition is true:

```{code-cell} julia
pandemia = true
println("Vou sair de casa?")
if pandemia == true
   println("Só vou sair de casa se for essencial")
end
```

In this example, the message "Só vou sair de casa se for essencial" will only be printed if the variable `pandemia` is equal to `true`.

Here is another example:

```{code-cell} julia
denominador = 1
if denominador != 0
   println("Sei fazer a divisão se não for por zero")
   println("O resultado da divisão de 30 por ", denominador, " é igual a ", 30/denominador)
end
```

The code inside the `if` block will only run if the denominator is different from zero, thus avoiding a division-by-zero error.

## Adding Alternatives with else

We often want to execute one block of code if a condition is true and another block if the condition is false. For this, we use the **if-else** structure:

```{code-cell} julia
pandemia = true
println("Vou sair de casa?")
if pandemia == true
   println("Só vou sair de casa se for essencial")
else
   println("Balada liberada!!")
end
```

If the variable `pandemia` is `true`, the message "Só vou sair de casa se for essencial" will be printed. Otherwise, the message "Balada liberada!!" will be printed.

## Multiple Conditions with elseif

What if we have more than two possible situations? In that case, we can use the **if-elseif-else** structure:

```{code-cell} julia
pandemia = true
tenhoqueestudar = true
println("Vou sair de casa?")
if pandemia == true
   println("Só vou sair de casa se for essencial")
elseif tenhoqueestudar == true
   println("Melhor ficar em casa")
else
   println("Balada liberada")
end
```

In this example, we have three possible paths:

1. If there is a pandemic, only go out if it is essential
2. If there is no pandemic but I have to study, stay home
3. If there is no pandemic and I don't have to study, go to the party

The **if-elseif-else** structure evaluates conditions in the order they appear. As soon as a true condition is found, the corresponding block is executed and the remaining conditions are ignored.

## Check Your Understanding

1. What is the difference between the `=` operator and the `==` operator in Julia? Why does this distinction matter?
2. Explain the difference between **if-else** and **if-elseif-else**. In which situations would you use each one?
3. Consider the following Boolean expression: `(a > b) && !(c == d)`. Explain in words what it means.

## Explore on Your Own

1. Research short-circuit evaluation of the `&&` and `||` logical operators in Julia. How can this behavior be useful in programming?
2. In Julia, besides the values `true` and `false`, what other values are considered "truthy" or "falsy" in a Boolean context?
3. Investigate the ternary operator (`?:`) in Julia and how it can be used as a more concise alternative to certain **if-else** structures.
4. Explore how conditional structures can be combined with functions to create more modular and reusable code.
