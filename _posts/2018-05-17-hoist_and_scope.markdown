---
layout: post
title:      "Hoist and Scope"
date:       2018-05-17 20:55:49 +0000
permalink:  hoist_and_scope
---

While struggling through some basic JS concepts, I found a pattern in some of my misleadings. It was a gap that I knew existed, but I couldn't quiet pin point what was going on. Enter hoist and scope.


When Javascript gets rendered in the browser, webpack sends it all together in one giant file. Instead of being rendered once from top to bottom like most programming languages, Javascript renders the one large file twice. Certain functions and variables get rendered first (or hoisted). When a function declaration gets rendered the first time it's values are available to be used on the second render. However when a variable gets called that same first time, it's values/initialization does NOT get rendered. That happens on the second and final render of our code in the browser.

Where exactly these values get hoisted to depends on their scope. The scope of a function or variable can depend on a lot of things. It's important to know how scope works for multiple reasons, but I know we've all been at the point of knowing something in our program exists, but you don't quiet know how to access it.

Variable Scope.

var: once you declare something with var it's declaration is available on the top of that scope, but it's value initialization isn't determined until the second pass. For example:

```
function do_something() {
  console.log(bar); // undefined
  var bar = 111;
  console.log(bar); // 111
}
```

The computer reads this as:

```
// is understood as:
function do_something() {
  var bar;
  console.log(bar); // undefined
  bar = 111;
  console.log(bar); // 111
}
```

However, if a var is defined without a decleration (i.e. i = 10), then Javascript will seek out wherever it was initially declared as a var, and make the appropriate value accessible within that scope. For example:

```
(function () {
  for (i = 0; i<10; i++){
  console.log(i)
  }
})()
console.log("after loop", i);
```

i is given a value in the local scope of this immediately-invoked function expression (search IIFE for more information on this type of function), so one would naturally think the value of i is only available inside of that scope. Instead JS looks for a variable of i in the local scope and can't find it. If this function were to be nestled in even more functions, JS will continue to look for the var of i until it hits the global scope. Once it hits the global scope and no i variable is found, it simply creates one for us. Which is typically not a good thing... The result of the above code is:
```
0
1
2
3
4
5
6
7
8
9
after loop 10
```

As you can see, there is good reason why let and const got introduced to ES6. Let's take the following function and try assigning i a variable declared with let.

```
(function () {
  for (let i = 0; i<10; i++){
  console.log(i)
  }
})()
console.log("after loop", i);
```

puts out the following:

```
0
1
2
3
4
5
6
7
8
9
ReferenceError: i is not defined
    at eval:6:27
    at eval
    at new Promise
```

As you can see, let is only available in the local scope, and therefore cannot be called from outside of it's scope.

const is also a "new" feature with ES6. Const is similar to let in that it stays inside whatever block you call it in, however const cannot be reassigned unlike let. For example:

```
let i = 20;
i = 100;

console.log(i) // 100
```

let is being reassigned from 20, to 100. However if we try this using const we will get an error:

```
const i = 20;
i = 100;

console.log(i) // SyntaxError: Assignment to constant variable: i at 2:0
```

If you were to pick one that you use all the time, make it const. It will lead to fewer errors, especially when it comes to accidentally defining duplicate variables. It's easy enough to reserve let for variables we know will have to change.

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
