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
