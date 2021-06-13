# Introduction to Elixir

Elixir is a functional, dynamically typed language that runs on the Erlang VM (BEAM). It is a language that uses the battle tested features of Erlang, while providing the comfort of a modern programming language. Elixir’s language design is similar to that of Ruby, but it is also inspired by e.g. Clojure and Erlang. 

The Erlang VM is known for its capabilities of running fault-tolerant, distributed and low-latency applications. By running on the Erlang VM, these capabilities are available when writing Elixir applications. 

# History and background


While Elixir is a relatively new language, its foundation was created decades ago, with Erlang/OTP. Erlang is a functional programming language created for developing fault-tolerant, distributed and low-latency applications, specifically for the telecom industry. It was developed at Ericsson in the late 1980s and was used for their telephone switching systems. Erlang is often used as a general term of the language Erlang and Erlang/OTP. OTP, or the Open Telecom Platform consists of the Erland runtime, including the Erlang VM, libraries and middleware you can use in your applications, and a set of design principles on how to develop Erlang applications. 

Elixir was developed by Josè Valim, and first appeared in 2012. Inspired by languages such as Ruby, Clojure and Erlang, Jose created a new language that had the capabilities of Erlang and its ecosystem, with a developer friendly syntax, lowering the threshold for developers to take advantage of the Open Telecom Platform. Since the apperance of Elixir, many companies has taken use of the language and its underlying platform. For instance, Discord uses Elixir and OTP in its chat infrastructure, where they have used Elixir serivces to serve more than 12 million concurrent users, sending more that 26 million WebSocket events to clients every second. https://elixir-lang.org/blog/2020/10/08/real-time-communication-at-scale-with-elixir-at-discord/



# Language features
## Structuring code using functions and modules

Functions in Elixir can be either named or anonymous. Anonymous functions can be assigned to variables, in a similar fashion as Javascript. 

```
product = fn (a, b) -> a * b end
```

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

What is usually the assignment operator, `=` in most languages, is
called the match operator in Elixir. 
The match operator will attempt to match the right and left and side of the `=` sign, and throw an error if no match can me made. This is useful for assigning multiple values or destructuring a complex object, similar to Javascript destructuring. 

E.q., matching a list of unbound values in a list with a list of values on the right hand side, will assign the unbound values. 
```
[a, b, c] = [1, 2, 3]
```
In the example about, `a` will be assigned `1`, `b` assigned `2` and `c` assigned `3`. 

If we wanted to use the match operator for destructuring, we would use the following syntax. 
```
person = %{
  name: "Dave",
  age: 39
}

%{age: daves_age} = person 
```
We initially set the fields `name` and `age` in the `person` map. Afterwards, we match the left and right hand side by saying some field age should be matched to `daves_age`.

While we are not able to mutate existing data, we can rebind variables with new completely new values.

```

# This will throw an error
person.name = "John"

# This is allowed
person = %{
  name: "John",
  age: 42
}
```
If we want to disallow variables being assigned when pattern matching, we can use the pin operator. Using the pin operator, the patterns on the left and right hand side will use the variables value to pattern match instead of trying to bind a new value to the variable. 

```
a = 1
[^a, b, c] = [1, 2, 3] # This is OK
[^a, b, c] = [3, 2, 1] # This will throw an error
```

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


## Pipe operator

The pipe operator is used to chain output from one expression as the input to another function, as the first argument.

If we wanted to nest functions and use the output from one function as the input of another, we would have to write something like this in many other languages. 

``` 
third_function(second_function(first_function("input")))
```

The equivalent using the pipe operator could be something like this: 

```
"input"
|> first_function()
|> second_function()
|> third_function()
```

The pipe operator allows us to easily compose simple expressions into complex ones, while maintaining a readable code structure.  

## Concurrency

To solve the challenges of concurrency, Erlang has adopted the actor model pattern. The actor model is a conceptual model of computation design to help solve the challenges of computational concurrency. The actor is the computational entity in our software. A single actor can send messages, receive messages, create new child actors or terminate any existing child actors. At the root of our application, we have our root actor, which is responsible for creating more actors which eventually creates a process tree which represents the desired state of our application. 

The actor model is implemented in Elixir and Erlang through BEAM processes. Processes in the Erlang VM are lightweight, and must not be confused with operating system processes. They are low-cost processes managed by the virtual machine, which are fast to create, terminate and have little memory overhead. This model allows for applications running on the Erlang VM to scale very well.

## Language feature resources
It would be challenging to write about all the features of a programming language in a single blog post. If you are courious about more of Elixirs language features, [elixirschool.com](https://elixirschool.com/en/) is a great resource for learning, along with the official documentation at [elixir-lang.org](https://elixir-lang.org/docs.html).

# Tooling

Many of the tools available for Elixir development have been developed over decades with the Erlang ecosystem in mind. Since Elixir compiles to Erlang bytecode, the tooling available for Erlang is largely also available for Elixir. 

## Mix 
Mix is a build tool that is used to create your Elixir application, testing, compliation, dependency management and creating tasks. A close comparison to the Javascript world would be the node package manager, NPM. 

## IEx
IEx, or Interactive Elixir, is Elixirs interactive shell. This is a REPL tool which allows us to interactively evaluate Elixir code. We are also able to load our Mix project into the IEx scope which allows us to call any of the functions in our code for a quick feedback loop. Running an application in the context of IEx also allows us to inspect the state of the processes in our supervision tree.

# Phoenix
There are many frameworks available for different problem domains, ranging from web development and numerical computing to IoT programming. Phoenix is probably one of the more known frameworks available in the Elixir ecosystem. Phoenix is a web framework, which offers high developer productivity, performance and concurrency. It has become a go-to web framework for Elixir, similar to what Ruby on Rails is for Ruby.

Just like Ruby on Rails, Phoenix allows rapid development with a clear convention on how to architecture our applications. It ships with scaffolding tools, which allows us to quickly create the our application resources. 

Phoenix ships with a web technology called LiveView. Using LiveView, we are able to create rich, real-time web applications without writing Javascript. LiveView uses a message architecture, where messages can change the state of the users application. UI changes are reflected in the UI using pieces of static HTML which is sent over websockets and pieces into the DOM in real time. 

Phoenix is a battery included web framework, which allows developers to work very quickly in creating applications while maintaining clean code due to its clear convention and great documentation. 

# Why Elixir?

Elixir with its ecosystem provides a way to create reliable, fault-tolerant and distributed applications, with a low entry barrier for developers. It gives us a programming model with great solutions to concurrency challenges out of the box, with little overhead. 

Elixir is a great entry to functional programming. Learning functional programming and the coding style it encourages, may help developers increase their skills in other imperative and object oriented programming languages, e.g. by writing small, pure and testable functions and keeping a clear division between your business logic and side effects. 

One of the challenges with using Elixir is adoption and finding developers. While the technology may be battle tested used for decades, without existing adoption, it may often be difficult for stakeholders to take the risk of adopting new technologies. While the risk may be high, developers and stakeholders have a opportunity to define tomorrows mainstream technologies, by not only exploring established options. 

As a final note, I highly recommend to have a look at Erlang the movie, a relic from the 90s to advertise the capabilities of Erlang and OTP.

https://www.youtube.com/watch?v=BXmOlCy0oBM