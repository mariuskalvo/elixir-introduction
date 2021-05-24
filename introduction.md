Elixir is a functional, dynamically typed language that runs on the Erlang VM (BEAM). It is a language that uses the battle tested features of Erlang, while providing the comfort of a modern programming language. Elixir’s language design is similar to that of Ruby, but it is also inspired by e.g. Clojure and Erlang. 

The Erlang VM is known for its capabilities of running fault-tolerant, distributed and low-latency applications. By running on the Erlang VM, these capabilities are available when writing Elixir applications. 

# History and background

We cannot look into the history of Elixir without taking a look at Erlang, the giant whose shoulders Elixir stands on. Erlang is a functional programming language created for developing fault-tolerant, distributed and low-latency applications, specifically for the telecom industry. It was developed at Ericsson in the late 1980s and was used for their telephone switching systems. Erlang is often used as a general term of the language Erlang and Erlang/OTP. OTP, or the Open Telecom Platform consists of the Erland runtime, including the Erlang VM, libraries and middleware you can use in your applications, and a set of design principles on how to develop Erlang applications. 

Elixir was developed by Josè Valim, and first appeared in 2012. Inspired by languages such as Ruby, Clojure and Erlang, Jose created a new language that had the capabilities of Erlang and its ecosystem, with a developer friendly syntax, lowering the threshold for developers to take advantage of the Open Telecom Platform. 


# Language features

## Structuring code using functions and modules

Functions in Elixir can be either named or anonymous. Anonymous functions can be assigned to variables, in a similar fashion as Javascript. 

`product = fn (a, b) -> a * b end`

Named functions are defined using the `def` keyword, followed by the function name and the function parameters. The function scope is surrounded by a `do` and `end` block.

```
def greet(name) do
    "Hello #{name}"
end
```

Named functions must live inside a module. We use modules to group functions together, similar to a namespace in other programming languages, such as Java and C#. Wrapping our function inside a module, leaves us with the following result:

```
defmodule Greeter do
  def greet(name) do
      "Hello #{name}"
  end
end
```

To call a function from outside our defined module, we simply have to specify the module and function names. `Greeter.greet(“John Doe”)`. Structuring code in Elixir is very simple. We create contexts and boundaries using modules which contain functions. 

## Pattern matching

## Control flow

### if-else
We have different ways of working with control flow in our Elixir code. One that will be familiar to most programmers is the if-else operator, which does exactly what it says; it branches the flow into a if, and if present, an else block.

```
if true do
  "This is always true"
else
  "This is always false"
end
```

### case

Similarly, we have the case operator, which compares values against patterns until a matching pattern is found, using the pattern matching capabilities of Elixir. 

```
def case_example(input) do
  case input do
      ["1", "2", "3"] -> "Numbers"
      ["$", "£", "€"] -> "Currencies"
      ["a", "b", "c"] -> "Characters"
      _ -> "Anything else"
  end
end
```
```
case_example(["1", "2", "3"]) # "Numbers"
case_example(["$", "£", "€"]) # "Currencies"
case_example(["a", "b", "c"]) # "Characters"
case_example(["1", "b", "d"]) # "Anything else"
```

The case operator will try to match the input with a pattern, and return the expression on the right hand side of the arrow. It is common to have a "catch-all" clause at the end `_ -> `, which catches all the cases that cannot be matches. This allows us to create case clauses that can remain pure, always returning a value or executing some computation for all input passed. 

In the example above, the following output would be produced from the input passed. The three first lines are able to match the right and left hand sides respectively, with the last call evaluated using the catch-all clause. 

### cond

The cond operator will try to match a set of conditions until it finds the first one that returns true. It will do so from top to bottom. It is also common to use a catch-all clause with the cond operator, to ensure that every input is handled. 

```
def cond_example(input) do
  cond do
    rem(input, 15) == 0 -> "Fizzbuzz"
    rem(input, 3) == 0 -> "Fizz"
    rem(input, 5) == 0 -> "Buzz"
    true -> "#{input}"
  end
end
```
```
cond_example(3)   # "Fizz"
cond_example(5)   # "Buzz"
cond_example(15)  # "Fizzbuzz"
cond_example(4)   # "4"
```

In the example above, the cond block will try to evaluate each block from the top to bottom until it returns true. If no blocks return true, we have a catch-all by having a `true ->` block at the end, which will always be evaluated if none of the blocks evaluates to `true`. 

### Assignment

All the control flow operators described above can be used for assignment, even using if-else.

```
odd_even_message = if rem(x, 2) == 0 do
  "This is even"
else
  "This is odd"
end
```

The content of odd_even_message will be assigned either `"This is even"` or `"This is odd"` based on the value of x. The same pattern can be used for `case` and `cond`, assigning a value based on the evaluation of the control flow operator.

## Concurrency

To solve the challenges of concurrency, Erlang adopted an actor model pattern. The actor model is a conceptual model of computation design to help solve the challenges of computational concurrency. In the actor model, the actor is the computational entity in our software. A single actor can send messages, receive messages, create new child actors or terminate any existing child actors. At the root of our application, we have our root actor, which is responsible for creating more actors which eventually creates a process tree which represents the desired state of our application. 

The actor model is implemented in Elixir and Erlang through BEAM processes. Processes in the Erlang VM are lightweight, and must not be confused with operating system processes. They are low-cost processes managed by the virtual machine, which are fast to create, terminate and have little memory overhead. This model of computation allows applications running on the Erlang VM to scale very well. 

## Tooling

## Frameworks

# Summary