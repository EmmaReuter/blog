---
title: "Thoughts on learning Golang"
date: 2022-02-06T14:27:11-06:00
draft: true  
---

Below are my thoughts when working through the [go tour](https://go.dev/tour/welcome/1) lessons. 

## Lesson 1

Initially, the syntax of Go feels like C without the semicolons. Having typed function signatures is nice because you can quickly see the type of a variable by going into and out of a function. You can specify the return variable names in the function signature, and then you can call return without arguments. I'm confused about how Go's packages work. I'm not sure what the go.mod and go.sum files do. It's cool that there is a built-in type for complex numbers!

## Lesson 2

Go's if statements have a similar syntax to C. However, an interesting twist is that you can calculate a variable in the if statement.
```
if v := math.Pow(x, n); v < lim {
        return v
    }
```
In this example, we calculate the power and then compare it to a limit. This syntax is nice because you can use the variable in the body of the if and else statement. The switch statement reminded me of the C switch statement but without the confusing break statements. Defer statements are interesting because it allows you to reorder function execution. They remind me of lazy evaluation in Haskell.