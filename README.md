# The-Hard-Parts-of-Functional-JavaScript

## JavaScript Principles:

1. When JavaScript Code runs it, => Goes through the code line by line and runs (executes) each line known as `thread of execution`
2. Saves data like strings and array so we can use that data later - in it's memory (we can even save code => functions)


## Functions

Code we save ('define') functions and can use (call / invoke / execute / run) later with the function's name & ()


## Execution Context

Created to run the code of a function - has 2 Parts (We've already seen them)

- Thread of execution => because we're gonna be going the the code of a function line by line
- Memory


## Call Stack

- JavaScript keeps track of what function is currently running (where's the thread of execution)
- Run a function adds to the call stack
- Finish running the function - JS removes it from the call stack
- Whatever is top of the CallStack - that's the function we're currently running

**What keyword tells us actually to end this function and move on** => `return`

as soon as we `return` the function will be popped off from the call stack.

so what remains on top of the call stack? is there anything yet on top of the call stack?

**Yes, always on the bottom of the call stack, is our global execution context, think of all of our code, inside a function with the label `global`, and as soon as we turn on JavaScript, start running the code, run that global function, run the overall code, so the global() is always on the bottom of the call stack**

**If I was running another function inside that function, the inner function would be added to the top of the outer function insdie the call stack**


## Higher Order Functions

imagine we have 3 functions called `copyArrayAndMultiplyBy2`, `copyArrayAndDivideBy2`, `copyArrayAndAdd3` => as you can see all of these functions are same, but in just functionality they are different. `*`, `/`, `+` => in JavaScript you can not pass functionality like a string as a paramater => you
