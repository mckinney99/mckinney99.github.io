---
layout: post
title:      "Hoist and Scope"
date:       2018-05-17 20:55:49 +0000
permalink:  hoist_and_scope
---

While struggling through some basic JS concepts, I found a pattern in some of my misleadings. It was a gap that I knew existed, but I couldn't quiet pin point what was going on. Enter hoist and scope. 


When Javascript gets rendered in the browser, webpack sends it all together in one giant file. Instead of being rendered once from top to bottom like most programming languages, Javascript renders the one large file twice. Certain functions and the declaration of variables get rendered first (or hoisted). Notice the *declaration of variables* part. When a function declaration gets rendered the first time it's values are available to be used on the second render. However when a variable gets called that same first time, it's values/initialization does NOT get rendered. That happens on the second and final render of our code in the browser. Let's see what this looks like starting with examples of var. Any guess what console.log(y) might return?


```
var x = function() {
  console.log(y);
  var y = 1;
}
x();
```

One might think this returns "1" when in fact you will get "undefined". Just like global variables and functions, local ones within a code block also get hoisted to the top of the their block on the first render. Again however, the variable does not get initiated until the second run. Meaning any value that var might have won't get rendered until the second pass, or in the natural top to bottom notion we are used to. Our function simply sees a y variable with no value. What about this one? Any guess?

```
y = 2

var x = function() {
  console.log(y);
  var y = 1;
}
x();
```

If you guessed 1 or 2 you are not alone, however you are incorrect. Again, the var y inside our function gets hoisted to the top, but not it's value of 1. Because of this, the function doesn't look for a global variable of y. It already sees a local one that again, is not defined. The return value is "undefined". Our browser essiantelly does this: 

```
var y; //our browser hoists the variable to the top just like this on it's first render.

var x = function() {
  console.log(y); //we are now calling on an empty var y on the second render. 
  y = 1; //the one doesn't get assigned because var y is already taken locally. 
}
```

This again will return "undefined". However if we make one small change to our code, we can get this thing to work.

```
var x = function() {
  var y = 1; 
  console.log(y); // 
}
x();
```

The browser reads this as:

```
var y; //on first render

var x = function() {
  y = 1; //on second render
  console.log(y); // 
}
x();
```
To save time and stress, I always declare my variables right up front in the begginning of my function. This way it looks how the computer renders it. It's an easy little cheat that can be taken for granted.

```
var x = function() {
  var y; 
  y = 1; 
  console.log(y); // output = 1
}
x();
```

The same rule of variables and their values when hoisted is the same for var, let and const. However the scope of these keywords is what sets them apart.

var = available globally at any time. Wether declared in a nested function or not. 

let = available only in whatever local scope it was created in. 

const = similar to var, available globally, but can only be declared once.

These changes don't occur in block level functions. For example:

```
let y = 2

var x = function() {
  console.log(y); // still outputs 2
}
x();
```

They do however occur inside of JS type block. Or block within with a function. For example:

```
function a(c) {
// a doesn't exist here

// console.log(a); => ReferenceError: a is not defined
if (a) {
        let a = "x";
        // some other code
        return a;
    } else {
        // a doesn't exist here as well
        return false;
    }
}
a();
```
This returns "x" because our let variable is not only true, but because it's returning only our local scope. Unlike the const keywork, the let variable is unavailable anywhere else in our code. 

```
function a(c) {
// a does exist here
return a;

if (a) {
        const a = "x";
        // some other code
        return a;
    } else {
        // a exists here as well
        return false;
    }
}
a();
```

If you were to pick one that you use all the time, make it const. It will lead to fewer errors, especially when it comes to accidentally defining duplicate variables. It's easy enough to reserve let for our code blocks {}.

Ok so enough on this variable stuff. What about functions?

Functions can be hoisted with their values UNLESS it is declared using an expression. Only function declarations can pass values and initialize on the first render. For examples:

```
willThisWork();

function willThisWork() {
  console.log("yes")
}
//outputs => "yes"
```

However when we do the same exact thing but instead use a function expression we get:

```
willThisWork();

var willThisWork = function () {
  console.log("yes")
}
//outputs => TypeError
```

So for clarification:

What gets hoisted?
-All variables without their values/initiation. 
-Function expressions without their values/initiation. 
-Function declarations with their values/initiation. 

Everything else gets rendered on the second pass. I hope this clears things up in what can be a confusing topic. Happy coding! 




