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

Actually, the data that comes through `backpack`, it is **persistent**, and also it's **data**, it's referenced, it's linked by a **scope** property, and also JavaScript is a `lexically` scoped language => `P.L.S.R.D`

`scope`: is the rules in any programming language, for at any given line of code, what data do I have available to me?

It's called `lexical` or `static` scoping.

### Lexical Scoping => 
That is to say, where I save my function, determines for the rest of that function's life, whenever it gets run, under whatever new label it gets, what data it will have access to when that function runs. Is not where I run it, that would be called dynamic scoping, It changes depending on where I run the function => **JavaScript is a static or lexically scoped language** => `lexical means the physical positioning on the page` => I physically positioned `inner` inside the running of `outer` function.

`JavaScript is lexically scoped language` => **that means that even if I return my function out and theoretially all this data should have been deleted => `Nope`, because I have this fundamental rule of lexically scoped language** => **I'm gonna have to grab all that data behind the scenes, and put it out on the `backpack`**

`Technically we call this =>` # Closure

### Closure

is the overall concept for this.

The notion of `lexical scoping` => It's the backpack as a result of JavaScript being a lexically scoped language, one that brings the data with the function, wherever that function goes => `hidden on the scope property, hidden on the backpack`

Meaning that, if you save a function inside of another function, when it exits that should be all deleted => `nope` => but that data with us on the backpack, so when we run that function, it still has access to the persistent data from it's backpack

### What can we call this backpack?

- Closed Over Variable Environment (C.O.V.E)
- Persistent Lexical Scope Referenced Data (P.L.S.R.D)
- Backpack
- Closure

**The backpack or closure of live data is attached incrementCounter then to myNewFunction through a hidden property known as `[[scope]]` which persists when the inner function is returned out**

### Individual backpacks

if we run `outer` again and store the returned `incrementCounter` function defintion in `anotherFunction` this new incrementCounter function was created in a new execution context and therefore has a brand new independant backpack.


if you run this code

```js
const myNewFunction = outer();
myNewFunction();
myNewFunction();

const anotherFunction = outer();
anotherFunction();
anotherFunction();
```

if you console.log the `counter` you will see the `1, 2, 1, 2` => because the backpack for the `myNewFunction` is totally different from the backpack of the `anotherFunction`

Because when you run `myNewFunction` and `anotherFunction` they will create totally different `execution contexts` with totally different `backpacks`

**Closure gives our functions persistent memories and entirely new toolkit for writing professional code**

- Helper Functinos: everyday professional helper functions like `once` and `memoize`
- Iterators and Generators: which use lexical scoping and closure to achieve the most contemporary patterns for handling data in JavaScript
- Module Pattern: Preserve state for the life of an application without polluting the namespace
- Asunchronous JavaScript: Callbacks and Promises rely on Closures to persist state in an asynchronous environment.


# Asynchronous JavaScript

**Promises, Async & the Event Loop**

- Promises: the most significant ES6 feature
- Asynchronisity: the feature that makes dynamic web applications possible
- The event loop: JavaScript's triage
- Microtask queue, Callback queue and Web Browser features (APIs)

**JavaScript is a `single threaded` language it means that JavaScript runs the code `line by line`** => So JavaScript is `synchronous` language

that means we do each line, we finish it, and then we go to the next line.

in `JavaScript` if we have a line of code to do => we have to do it, it's the only line of code we can do, And we have to finish on it, before we move on to the next line.

### Asynchronisity is the backbone of modern web development in JavaScript yet...

`single-threaded`: `one command runs at a time`

`synchronous`: `the code runs line-by-line, as the way code appears`

JavaScript is:

- Single threaded (one command runs at a time)
- Synchronously executed (each line is run in order the code appears)

So what if we have a task:

- Accessing Twitter's server to get new tweets that takes a long time
- Code we want to run use those tweets

**Challenge**: We want to wait for the tweets, to be stored in tweets, we have to send request to the servers of `Tweeter` and surely, this will take a time to get those tweets - but no code can run in the meantime in JavaScript, because JavaScript is a `synchronous` language. So it's gonna be a really big problem:))

So what should we do?

**Slow function blocks further code running** => **So what we can do people??**

```js
const tweets = getTweets(API);

// 350ms wait while a request is sent to Tweeter HQ

displayTweets(tweets);

// more code to run
console.log("I want to runnnn!")
```

**Let's make it a little harder** => **What if we try to delay a function directly using `setTimeout`?**

`setTimeout` is a built in function - it's first argument is the function to delay followed by `ms` to delay by

```js
function printHello() {
  console.log("Hello");
};

setTimeout(printHello, 1000);

console.log("Me first!");
```

the logs will appear => first: `Me First`, then second: `Hello`

```js
function printHello() {
  console.log("Hello");
};

setTimeout(printHello, 0);

console.log("Me first!");
```

The logs will apear => first: `Me First`, then second: `Hello`

### JavaScript is not enough - We need new pieces (some of which aren't JavaScript at all)

Our Core JavaScript engine has 3 main parts:

- Thread of execution
- Memory / Varivable Environment
- Call Stack

We need to add some new components

- Web Browser APIs/Node Background APIs
- Promises
- Event Loop, Callback/Task queue and micro task queue


## Asynchronous Browser

Where is the JavaScript running? => `Browser`

`Browser` has more than just JavaScript in it.

`Web Broswer` => `JS Engine` + `Dev Tools` + `console` + `Network Requests` + `DOM` + `Timer`

**All these features we can not code directly** => **But people, we do have a programming language, that lets us use these features** => **And that programming language is: `JavaScript`**

**But we're not actually gonna find any of these features in JavaScript, So how do we interact with them?**

`in JavaScript we have some facade functions => They are functions, that they look like JavaScript, But they are actually facades for web browser features.`

And one of those features believe it or not => **JavaScript doesn't even have the feature of `Timer`, that's in the web browser too. And we get labels for each of these features**, for example label for the `Timer` in web browser is: `setTimeout`

`setTimeout` is not doing anything of interest in JavaScript, instead it's a label for the timer built to the web browser.

`document` is a label for HTML DOM web browser feature. (So, whenever you see, `document` that aint do anything in `JavaScript`, it's a command to go and use the feature of web browser)

`xhr / fetch` is the lable for making network request

`console` for console

All of these features, are not JavaScript at all. All of thsese are web browser features.


## Web API Example

**ES5 solution: introducing callback functions and web browser APIs**

```js
function printHello() { console.log('Hello') };

setTimeout(printHello, 1000);

console.log('Me First');
```

**We're interacting with a world outside of JavaSctipt now - So we need rules**

`setTimeout` do not nothing in JavaScript

```js
function printHello() {
  console.log("Hello");
}

function blockFor1Sec() {...}

setTimeout(printHello, 0);

blockFor1Sec();

console.log("Me First");
```

in here:

- first we're defining a function with a label `printHello` in  global memory
- second we're defining a function with a label `blockFor1Sec` in global memory
- now things get interesting, the `setTimeout` a facade function, is going to send a message to web browser, to set a `Timer` with duration of `0ms` and a `function definition` to run `on completion` in the timer of the web browser

now as we see `setTimeout` is `0ms` and normally, `printHello` should run

But there's a point a here  => `queue`.

What, do we call little functions passed to the other functions => `callback` => `setTimeout(printHello, 0ms)` => `printHello` is callback function here.

So, it's a `Callback Queue`.

Do not by the way confuse it, or do not confuse these callbacks, with the ones, we saw yesterday where they run inside of the higher order functions, no no no, this one is grabbed and thrown right out of it. `setTimeout` is just a command, at no point, `printHello` runs inside of `setTimeout`, they just grab this function, and throw it into the web browser, and it's stored here.

At this point in `0ms`, `printHello` **ain't** go on the **CallStack** directly, it's gonna have to queue it self up here into the `Callback Queue` and ready to run.

But as we `completed` `setTimeout` JavaScript continues to run the code, at `1ms`, now in `1ms` we're going to run `blockFor1Sec` function, so `blockFor1Sec` is going to the `CallStack`, and blockFor1Sec is going to block for `1000ms`

And in `1001ms` the function of `printHello` is going to say, okkkk, finally, you finished your function and `blockFor1Sec` is going to popped off from the `CallStack`, and `printHello` it's been waiting there quite eagerly, very excited, say I wanna come out, I wanna be out of queue into the `Call Stack`

And finally at `1001ms`, what do we think happens? is it `printHello` allowed out from `CallBack Queue` to the `Call Stack`?

Nooooo, it's still not allowed out. And `printHello` is gonna sit there and wait, and instead what's going to run first? at `1001ms` => `console.log("Me First")`

Now, at the `1002ms` => `printHello` is going to grabbed out of queue and put on to the callStack and going to executed.


So, People can we try and ascertain, what was our rule for when a function in the `queue` that's being thrown out by using these facade functions, out into the web browser, `setTimeout` ain't doing anything in JavaScript, instead it's just grabbing that function, throw it out here (web browser features)

### So when the functions inside queue will run? => after all the functions inside global execution context runned. you could have a million console.logs, your code inside the queue will run after a million console.logs :)))

## All the reqular code will run first, until I ever touch anything from the queue, until I ever put anything out of the queue

## So, how does JavaScript implement that?

Well, it has an amazing feature, which says I'm gonna check before every single line of code => is the `CallStack` empty? => is the CallStack empty? is there something in the `queue`?

If the CallStack is not empty, if there's still further global code to run, **Then, I will not even go to look at the `queue`**

But if the `CallStack` is empty, or if I head down to the `queue`, I grab the function, I put it out to the `CallStack`

### And What is that know as that little feature? (That little feature that does that very very fast checking, every single line, before it runs any line of code, It checks is there anything on the call stack, if there is, just do it, is there anything left running global? => just do it => if it's all finished, head down to the queue)

## And that feature is known as the `Event Loop`

## And `Event Loop's job` is simply to very quickly be checking very constantly => `is The Call Stack Empty?` `is there anything in the queue?` `is there any global code to run?` `If no, finally I get down to the queue`, `Finally, when all the global code is finished running` => `what happens?` => `It heads down to the queue` => `It says yes, I'm ready to head down to the queue` => `It grabs printHello, and sits on to the Call Stack` => `It puts the parenthises for us, no problem and prints hello`

## And it was the entire model of `Asynchronous JavaScript`


# ES5 Web Browser APIs with CallBack Funcrions

**Problems:**

- Our response data is only available in the callback fuction - Callback hell
- Maybe it feels a little odd to think of passing a function into another function only for it to run much later

**Benefits:**

- Super explicit once you understand how it works under-the-hood


A lot of times these background features, the function that's then automatically called, the background feature gets some data, so for example if the background feature, speaking over the network, to get some new data in, well then where that data gonna show up, that data shows up as the input, automatically inserted into the running of that `printHello` function, that's pretty amazing, But problem with that, is that means, is the data is only available inside the `execution context` of that `printHello` function? that's pretty rough, and that's what created something known as `callback hell`


# Promises

**ES6+ Solution (Promises)**

Using two-pronged facade functions that both:

- Initiate background web browser work and
- Return a placeholder object (Promise) immediately in JavaScript

`Promise would be this`, when I have this model here, when I use `setTimeout`, did `setTimeout` do anything in JavaScript? it's consequence was in the `web browser`, but once I set a timer in the `web browser`, keeping track of the fact that I've started it, or other background features, I didn't really get any way of tracking that back in JavaScript, in JavaScript I kind of just threw them up, there's no consolel visual for web browser features, but have it have some sort of consequence in JavaScript memory as well. So that I have a consistency between things going on the background is gonna have some consequnce back in JavaScript at some point. 

And one of those facade functions in ES6 is `fecth` => `fetch` is a label for a web browser feature of `Network Request` => `Speaking to the internet` => it does setup a `newtwork request` in the web browser features, but rather than just doing that, and then nothing know in JavaScript back on, it's gonna have a consequnce immediately in JavaScript as well. This `fetch` label is gonna trigger speaking to the internet down here (Web Browser) => sending a network request, speaking to the internet, asking for data from `Twitter`, And `Simultinously`, it's going to also in JavaScript return out a special kind of `Object` => Called **`Promise` Object** that's going to sit in `memory`, and when the background work is done here, that's going to fill in and update that object's data with the data from the background, and now when we finish our background work, we're not surprised that something gonna have a result back in JavaScript, because we had a two pronged facade functions, background feature, when it completes, it's going to have consequence for that immediately returned down JavaScript object

```js
function printHello() {
  console.log("Hello");
}

function blockFor1Sec() { ... }

setTimeout(printHello, 0);

blockFor1Sec();
console.log("Me First");
```

ES6+

```js
function display(data) {
  console.log(data)
}

const futureData = fetch(API).then(display);

console.log("Me First");
```

when you call `fetch` => it's actually a lebel in JavaScript to setup a `Network Request` to speak to the internet, Not in JavaScript, JavaScript can't do it, but `Web Browser can`

But it also, `simultaniously` in `JavaScript` returns out a `little` object, that's gonna be called `Promise`


When you run this line of code:

```js
const futureData = fetch(API).then(displayData)
```

`fecth` is a `facade` function => it's gonna do both JavaScript for us, and `simultaniously` it's going to do some work in the background in the `Web Browser`, But let's do the JavaScript side first: => when you run this code, it is going to `immediately` => create an `object` => a `Promise object` for us. it's just an object automatically created in JavaScript by `fetch` => it has `two` properties => `value`, which is `undefined` nothing stored in it yet, and another property called `onfullfilled`, it's hidden but super important, which is an `empty array []` and this `Promise` object is going to be stored in the `futureData` label:

So `Promise:`

```js
Promise = {
 value: undefined,
 onfullfilled: [],
 onRejection: []
}
```

And `simultaniously` I'm about to go and setup some background web browser stuff in the `Web Browser` a `Network Request` and we're doing this, at roughly I guess in `0ms`.

and in the web browser you set an address to make network request => and here, exactly just like `timer` => it has `complete?` and `on completion`, in `0ms` is it complete? `No`, let's get started, it's gonna send a `message` over the internet to `Twitter's Servers` => that started on `0ms` => But it's not completed at this point => 

`on completion` => what is gonna happen?

Well, if you remember when I said `setTimeout` => when you setup a background feature => we had a function here, that would auto run on the `Call Stack` on completion of the background task => But anyone see a function that passed into the function of `fecth` here? `no` => here instead of having a function passed in, you've got this pretty object `Promise` that's kind of keeping track of the fact that we set something up in the background, And in fact when that data comes back, where could that go? 

in JavaScript the data comes back is going to sit in the `value` property of `Promise` object => 

```js
futureData.value = the data comes back from the server
```

So, let us speak what those five words did => `fetch(API)`

They setup in the background, 

- they speak to the internet, with all the information it needs to go and get the right data back 
- but they also setup in JavaScript a little placeholder object => known as a `Promise` object
- And that was stored into futureData

But, we don't know when the data comes back, => but as we said the functions inside of `onfullfilled` empty array automatically, trigger to run when the data comes back from the server

We can put any function inside this empty array. and any function we stick in this array will be automatically triggered to run, when the `value` property gets updated, and more importantly, that `data of value` property will be auto inserted as the input as the `argument`, to fill the `parameter` of any functoin that's stored in here

But how would we get display into that array?

JavaScript gives us a method built in that allows us, whatever we pass to it as input, will be grabbed in stuck in that array. Where is that method come from?

```js
futureData.then(pass in the entire display function definition)
```

futureData is this object `Promise` => it has a `onfullfilled` property which is an empty array, and whatever function we pass to the `.then` is gonna be taken and pushed to the `onfullfilled` empty array.

So, when the data comes back from the server, and the `value` property of `Promise` object is updated with the value property, this value or data will be passed as an argument to the function that we passed into the `onfullfilled` array.

Now we're back to our global memory => in `1ms` the `console.log("Me First")` will be run.

And then for example in `270ms` our data comes back from `Twitter` and web browser `Network Request` is complete now => So, on completion, we will get response from Twitter, but it's still in the `Web Browser` => How does it get back into the `JavaScript`? => `futureData value property`

so in `270ms` we are going to call, we're going to call `display` function with the argument of `futureData value property`


## But we need to know how our promise-deferred functionality gets back into JavaScript to be run

when does that function, that was delayed, deferred, by being attached to the `Promise` object `onfullfillment` array?

you know what, get back into the `Call Stack` it's in JavaScript => `get back into the Call Stack in order to be executed`

**then method and functionality to call on completion**

Any code we want to run on the returned data must also be saved on the `Promise` object

Added using `.then` method to the hidden property `onFulfilment`

Promise objects will automatically trigger the attached function to run (with its input being the retuned data)

```js
function display(data) { console.log(data) };
function printHello() { console.log('Hello') };
function blockFor1Sec() { ... // blocks code for 1 second };

setTimeout(printHello, 0);

const futureData = fecth(API);
futureData.then(display);

blockFor1Sec();
console.log("Me First");
```

`Asynchronous meanin`: `It means doing code out of order when you saw it from when it was said to be done`.

So, when we see normal JavaScript code, if a line says do the code, I do it right then, `Asynchronous say no no no` => `JavaScript is gonna handle when that functionality comes back in` And `it's gonna be out of order of when you trigger the Browser feature` => Now we can actually `have code that's gonna be run after all of our regular code is run`

when we run this code:

- store function definitons of diplay and printHello and blockFor1Sec
- at 0ms `setTimeout(printHello, 0)` => it's going to set the `Timer feature of Web Browser` with the duration of  `0ms` and on completion of duration, function definition `printHello` and send the function definition of `printHello` to the `Callback Queue` on completion. And the function `printHello` is waiting to be sent to the `Call Stack` and to be run after that. But this is the place, where `Event Loop` comes to play, `Event Loop` constantly checking if there is any funciton or code to be run inside our `Call Stack` or if there is any code to be run inside of our `global memory` => If there is, then the `printHello` function inside of our `Call Stack` will have to wait, but if there's not any code inside of out `Call Stack` and `global memory` so printHello function can be put on to the `Call Stack` and then can be run.
- So, in `1ms` we're going to declare a const `futureData`, that `fetch` facade function is a two pronged facade function, when you call `fecth` function it has `two` consequnces, => It's JavaScript consequence => It's going to retunes out immediately a special object called `Promise` { value: undefined, onFullfilled: [] } and that's gonna be stored into `futureData`, and as soon as `value` property updates, it's going to trigger all the functions inside of the `onfullfilled` array with the argument of `value`, And now the fetch funciton `Web Browser Side` consequence, it's going to make a `Network Request` with the `address` which includes `Domain Name` and `path` and it's going to start sending request to the server, And is it complete at that point when we send request? do we have data comes back? `no`, So what happens on completion of the background work, is that it's going send the data comes back from request to the `value` property of the `futureData` object and immediately the value property updates, it's going to trigger all the functions inside of the `onfulfiiled` empty array with the input of the `value` property.
- Now imagine if we had to wait for it in the foreground in JavaScript

### But the question is how do get a function into onfullfilled array? => `.then`

now we're here at `1ms`

we're going to pass in all the entire definition of the function (entire display function) that we're going to be called after the fetch function finishes.

```js
futureData.then(display)
```

- now we're gonna grab the `display` function and put it into the `onfullfilled` array in that `Promise` object
- Now, good we have setup `2 pieces` of background work one of them's already complete and landed in the `queue`
- in `2ms` we're going to call `blockFor1Sec` function => It's going to run on to the `Call Stack`, And amazing news is that, when we are running the function `blockFor1Sec` in the background the `Twitter` send the data comes back to us, for example it takes `270ms` to send data back to us, So I've got tweets in `270ms` => there it is, it's complete, now for example the data that has come back to us is a string `hi` => **OK => now stuff gets interesting => when `hi` returns or gets responded back from speaking to the internet, on completion of these stuff the value property of the `futureData` is gonna be updated with the retured response from network request, which is gonna trigger the `diaplay` function to call `where?` on to the call stack?** => `It does not go on to the Call Stack, No way` => `there's no way it can, because we've got blockFor1Sec right there` instead where it goes? => `It goes on to the Callback Queue` and when we come back to our code, our function `blockFor1Sec` is complete in `1002ms` => then get the `blockFor1Sec` function off the `Call Stack`, then `Event Loop` looks at the `Call Stack` and our `global memory` and sees oh, there is one another command left to be run, so `Event Loop` is not allowing the functions inside our `Callback Queue` to be run so the `display and printHello` functions have to wait still, to `console.log('Me First')` to be run first, so at `1002ms` the `log me first` will be print to the console, and after that
- And finally in `1003ms` `Event Loop` sees that both `Call Stack` and `global execution context` are empty so Now, `Event Loop` allows `Callback Queue` functions to put on to the `Call Stack`
- Now in the `callback queue` we have both `printHello` and `display` functions waiting to be sent to the `Call Stack`, So which goes first to the `Call Stack`?
- Interesting is that `display` function will go the `Call Stack` first, it is very very interesting!! WHYYYY?
- **The Answer is there is an additional queue => It's called the `microtask queue`**
- So the `display` function will sent to `microtask queue` **Not** in the `callback queue`
- So, once the `Event Loop` at `1003ms` goes, we're clear synchronous global code, where do you think it `(Event Loop)` heads to first? => **Answer: `Microtask queue`**
- It grabs `display`, it `dequeues` it and push it on to the `Call Stack` with the inserted argument automatically. And it executes it. and popped off the call stack `display`
- And finally `printHello` function can go to the `Call Stack` and executes the function
- So we have `2 queues`

Tips:

**behind the scenes the `value property` of `Promise` object is never filled in actually, Until all global code finished running, So we can't get even access to it at that point**

**Any functions that is attached to a `Promise` object by one of these two pronged facade functions those functions are going into the `microtask queue`, And any function that's passed directly to a facade function that triggers a web browser feature, those functions when the timer completes => these functions will be passed to the `callback queue`**

**You have to look at your function and see, does our particular facade function that trigger stuff in the background, does it take in a function? => That ones going to go into the callback queue. Or does it return out `Two pronged, a Promise object and they task in the background` => they will go to the `Microtask queue`**

**When your request fails, the `Promise` object gives us an amazing feature, if we get an error back, not the actual `response` object we want, It's not gonna run the functions inside `onfullfilled` array, It's gonna trigger any functions you've stored in `onRejection`, how do we pass functions into `onRejection`? there is two ways, First one is `.catch` any functions that's written in there, it's gonna passed to the `onRejection` and the other way, is to pass to `.then` as the second argument**

**Promises, Web APIs, Callback & Microtask Queues and Event Loop enable: `Non-blocking applications: this means we don't have to wait in the single thread and don't block further code from running`, However Long it takes: `We cannot predict when our browser features work will finish, so we let JS handle automatically running the function on it's completion`, Web applications: `Asynchronous JavaScript is the backbone of the modern web letting us build fast non-blocking applications`**

### hufffff => That was the entire model of the `Asynchronous JavaScript`

# Class & OOP

- An enormously popular paradigm for structuring our complex code
- Prototype chain - the feature behind the scenes that enables emulation of OOP but is a compelling tool in itself.
- Understanding the difference between __proto__ and prototype
- The new and class keywords as tools to automate our object & method creation

Once of the most important questions which asked in interviews is: => `what's new keyword doing under the hood?`

**I want my code to be:**

1. Easy to reason about
2. Easy to add features (new functionality)
3. Nevertheless efficient and performant

The Object-oriented paradigm aims is to let us achieve these three goals

**So if I'm storing each user in my app with their respective data**

user1:
 - name: 'Tim'         
 - score: 3

user2:
 - name: 'Arghun'
 - score: 5

And the functionality I need to have for each user (again simplifying!)

 - increment functionality (there'd be a ton of functions in practice)

How could I store my data and functionality together in one place? => `in an object`

```js
const user1 = {
  name: "Will",
  score: 3,
  increment: function() { user1.score++; }
}

user1.increment(); // user1.score -> 4
```

```js
const user2 = Object.create(null); // it's going to create an empty object {}

user3.name = 'Sahand';
user3.score = 5;
user3.increment = function() {
  user3.score++;
}
```

Now as you can see we have object oriented programming => But what rule we're breaking => `DRY` => `Don't Repeat Your Self`

**Solution1:** Generate objects using a function

```js
function userCreator(name, score) {
  const newUser = {};
  newUser.name = name;
  newUser.score = score;
  newUser.increment = function() {
    newUser.score++;
  }
  return newUser;
}

const user1 = newUser('Will', 3);
const user2 = newUser('Tim', 5);
user1.increment();
```

So, what is wrong with this style of writing code, that I could never use this: `We're storing the same function twice`
And also in here we have another problem: `If you wanna add a new feature to this, you hava to add it to each user`

**Can anybody propose a better way?**

It could be better if we store our functions in another object and each user can access to the function store object and grab that function and use it.

So

**Solution2: Using the prototype chain**

Store the increment function in just one object and have the interpreter, if it doesn't find the function on user1, look up to that object to check if it's there

Link user1 and functionStore so the interpreter, on not finding `.increment`, makes sure to check up in functionStore where it would find it

Make the link with `Object.create()` technique

```js
function userCreator(name, score) {
  const newUser = Object.create(userFunctionStore);
  newUser.name = name;
  newUser.score = score;
  return newUser;
}

const userFunctionStore = {
  increment: function() { this.score++; },
  login: function() { console.log("Logged in") }
}

const user1 = userCreator("Will", 3);
const user2 = userCreator("Arghun", 6);
```

**All Objects in JavaScript have a hidden property called `__proto__`. If you create an object using `object.create(someObject)` => It's going to gives us access inside that object a hidden property called `__proto__` which is a link to the object we passed to that `object.create(someObject)` `(someObject)` => this is because of nature of `prototypal chain` of `JavaScript` feature, it's a JavaScript feature**

**Point is that, `object.create(userFunctionStore)` => does create an empty object, but inside that object, there is a hidden property called `__proto__` which is link our our object to the `userFunctionStore` object => `__proto__` => this proto link, this chain connection from `user1` up to `userFunctionStore` => this is because JavaScript `prototypal feature` => `That means when it does not find on the object A given property, method or data, it does not panic, instead it goes straight to the __proto__ property, and it looks at what is linking to up the prototype chain`**

So, now I'm going to run this function, `this.score++` => this is actually when I run => `user1.increment();` => `user1.score++;`

Actually, when you are running the code:

```js
user1.increment();
```

increment is => `this.score++`

it's going to create an execution context and inside of that execution context => in the `localMemory` of that execution context => it's going to save an implicit parameter `this` and assign it to `user1` actually.

### hasOwnProperty

It turns out people, that there is a big old headline object in JavaScript called => `Object.prototype` And it has a bunch of useful functions that are gonna be available to all of our objects. But how? Because =>

**All objects in JavaScript have a `__proto__` property** that defaults to linking to the `Object.prototype` object. so our `userFunctionStore` is gonna link down to `Object.prototype` object.

## Declaring and calling a new function inside our method increment

```js
const userFunctionStore = {
  increment: function() {
    const add1 = () => { this.score++ }
    add1();
  }
}
```

nowadays, we declare and save our functions in arrow function style, because when we save functions using arrow function style, it's going to automatially, is lexicallt scoped. That means, when we save the function and execute it, this is set to is determined by where the function was saved, so if it was saved where this is user1, when we end up running it `this` inside will be the value from where the function was saved, which is `user1`,

when we are running `add1` in it's local memory, because this is an arrow function => is `this` assignment in our local memory, will it be global?

Remember our one simple rule, is any function, that's being run to the right hand side of the `.` whatever the left hand side that's gonna be `this` assignment. But when there's no `.` in here => `add1()`? => it defaults to global `window`, unless that function was defined as an arrow function. in this case our `this` assignment will be `user1` exacylt what was this assignment around the definition of that one, is lexically scoped.

## Solution 2 (Using the prototype chain)

**Problems: ** No Problem, It's beautiful, maybe a little long-winded

write this every single time - but it's 6 words!

```js
const newUser = Object.create(userFunctionStore);
...
return newUser;
```

**Benefits: ** super sophisticated but not standard.


## What's the `new` keyword is actually doing behind the scenes? (interview question)

## Solution3 (Introducing the `new` keyword that automates the hard work)

When we call the function that returns an object with `new` in front we automate 2 things

```js
const user1 = new userCreator('Arghun', 20);
```

1. Create a `newUser` object
2. Return the `newUser` object

But now, we need to adjust how we write the body of `userCreator` - how can we?

**functions are both objects and functions in javascript**

loo at this example:

```js
function multiplyBy2(num) {
  return num*2;
}

multiplyBy2.stored = 5;
multiplyBy2(3) // 6

multiplyBy2.stored // 5
multiplyBy2.prototype // {}
```

We could use the fact that all functions have a default property `prototype` on their object version, (itself an object) - to replace our `functionStore` object

when we use `()` => actually we're running this function, and also we have our object using the `.` notation.

```js
multiplyBy2 => function + object
```

```js
object = {
  stored: 5
}
```

**And all functions also have by default, they have a property, `prototype`**

**All functions in their `object` format automatically have a property called `prototype`, it's not a hidden property => and it's default equals to `empty object`**


### new keyword

the `new` keyword is gonna automate so much stuff for us:

1. It's going to create automatically inside of here an object for us
2. It's going to return out that object for us automatically
3. It's going to make the link to some objects full of functions out here automatically for us, going to set the `__proto__` property automatically for us as well

When we are calling this line of code:

```js
function userCreator(name, score) {
  this.name = name;
  this.score = score;
}

const user1 = new userCreator("Arghun", 5);
```

It's just going to call the `userCreator()` nothing magic. But it's going to do some stuff inside of function call `userCreator`

when we run the code above:

1. in it's local memory, it's going to save `name: "Arghun"` and `score: 5` and it's going to create an empty object with the label of `this` => `this: {}`

**And more interestingly it's going to link the `this object` through the `__proto__` link to the `prototype` object which was created when we we're defining the `userCreator` function. which is an object with full of the functions we put in there. Fantasticc!!!!**

2. __proto__ : userCreator.prototype => this is automatically done for us. and `this` would be

```js
this: {
  name: "Eva",
  score: 9,
  __proto__ => // which is a link to the prototype object
}
```

3. Return `this` is the last this that `new` keyword automatically does for us.

So finally the `user1` would be

```js
user1: {
  name: "Eva",
  score: 9,
  __proto__
}
```

```js
userCreator = function + { prototype: {} }
```


```js
function userCreator(name, score) {
  this.name = name;
  this.score = score;
}

userCreator.prototype.increment = function() { this.score++ };
userCreator.prototype.login = function() { console.log("login") };

const user1 = new userCreator("Eva", 9);
user1.increment();
```
