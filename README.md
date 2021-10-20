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

any function that takes in a function or passes out a function is called `higher order function`

imagine we have 3 functions called `copyArrayAndMultiplyBy2`, `copyArrayAndDivideBy2`, `copyArrayAndAdd3` => as you can see all of these functions are same, but in just functionality they are different. `*`, `/`, `+` => in JavaScript you can not pass functionality like a string as a paramater.

So, what should we do => we have wrap this, in another function => called `higher order function` => How?

example:

```js
function copyArrayAndManipulate(array, instructions) {
  const output = [];
  for(let i = 0; i < array.length; i++) {
     output.push(instructions(array[i]))
  }
  
  return output;
}

function multiplyBy2 = (input) => input * 2;
const result = copyArrayAndManipulate([1,2,3], multiplyBy2);
```

**Behind the scenes `functions` are just `objects`**


## Closure

- Closures is the most esoteric of JavaScript concepts
- Enables powerful pro-level functions like `once` and `memoize`
- Many JavaScript design pattrns including the module pattern use closure
- Build iterators, handle partial application and maintain state in asynchronous world

`once function`: this is a function that can turn other functions into functions that are gonna run once. If they run them again, they don't work, and already you might be thinking, hold on, that doesn't make sense, because I know how functions work. They don't remember anything from their previous running, they're brand new every time.

And yet, hold on, I'm telling you they're gonna somehow remember they've been run before, and not run again. We can achieve `memoization`, a core performance optimizer.


**Functions get a new memory every run / invocation**

we do know that every time a function gets executed, run, invoke as an invocation, it creates a brand new local memory. => a brand new execution context as a little temporary store memory => when we finish running that function, that's all deleted. when we run the function again, it doesn't remember the data stored in the previous running of the function, and actually we want that, every time a funciton called and finished the local memory of that function should be freshed.


And yet, what if we could have that function also have a permanent memory attached to it. that could change everything about how we write code.


### Functions with Memories

- When our functions get called, we create a live store of data (local memory / variable environment / state) for that function's execution context.
- When the function finishes executing, it's local memory is deleted (except the returned value)
- But what if our functions could hold on to live data between executions?
- This would let our function definitions have an associated cache / persistent memory
- But it all starts with us **returning a function from another function**

This would be really, really special that allow us to do things like say, => oh, you've previously run this function I know that, because in my rememberence of the previous running, I've stalled that you previously run me, so you can't run me again, So I can limit a function to only run one time and a thousand more things we're gonna see.


**All of these will start with us returning a function from the invocation, from the running of another function**

So we have to understand `returning functions from other functions` =>  Functions can be returned from other functions in JavaScript

```js
function createFunction() {
  function multiplyBy2() {
    return num * 2;
  }
  
  return multiplyBy2;
}

const generatedFunc = createFunction();
const result = generatedFunc(3); // 6
```

So the question is: **Why did I save a nicely semantically that means kind of meaningfully named function inside another function only to then return it out, giving it a really bad name out and use it globally?**

`Well, saving a function, declaring storing a function inside the execution context of running another functions of saving its local memory inside another function, when that function gets returned out, it gets the most powerful peoperty bonus feature of JavaScript that we can ask for`

### Nested Function Scope

So, we saw there that a function can be returned from the running with other function, stored in the global label, used that in the function by its new global label.

**Calling a function in the same function call as it was defined**

```js
function outer() {
  let counter = 0;
  function incrementCounter() {
    counter++;
  }
  incrementCounter();
}

outer();
```

this is gonna have extraordinary consequences. And also ourselves we thinking what determines the fact that when I run `incrementCounter` inside `outer` and I don't find counter inside of `incrementCounter`, I'm somehow going to have access to counter stored in `outer`

`elegant example`

```js
funtion outer() {
  let counter = 0;
  function incrementCounter() {
    counter++;
  }
  return incrementCounter;
}

const myNewFunction = outer();
myNewFunction();
myNewFunction();
```

**Always when I return the `incrementCounter` function out from the `outer` function, I got something else with that function => `it took with it all the surrounding data from where that function was saved, where it was borned, where it was stored` => `it grabbed its surrounding data, and brought it out on the` `backpack` of the function**


### So what do we call this? of getting surrounding data of the function definition inside a `packpack`

How does the function get to grab onto what it surrounding data and return it out with the function definition?

As soon as, we declare `incrementCounter`, that is literally saving in the computer's memory, in the computer store of function definition, `under the hood if you console.log the function you will see that`, `that behind the scenes, in JavaScript immediately gets a hidden property => in JavaScript you will see the hidden properties inside` => `[[scope]]`

`[[scope]]`: it is a hidden property that links to where all this surrounding data is stored => it gives a little link to all that surrounding data.

Meaning when I return `incrementCounter` function out of `outer` into `muNewFunction` it will get all the surrounding data with it, through that hidden property `([[scope]])`

And also, it aint going anywhere, it's not like the execution context temporary local memory => It's `permamnent` as long as its function definitions they're not overwritten.

We can't get access to it though in any other way besides running this function.

And hoping the function was written in such a way when it was defined, when was born, was initially saved, that it looks for something in `local memory`, it aint there, then it goes out and looks on the scope property into the backpack.

### Alright let's talk about what this backpack is actually called?

Actually, the data that comes through `backpack`, it is **persistent**, and also it's **data**, it's referenced, it's linked by a **scope** property => `P.S.R.D`

`scope`: is the rules in any programming language, for at any given line of code, what data do I have available to me?

It's called `lexical` or `static` scoping.
