---
layout: post
title:      "Hoist and Scope"
date:       2018-05-17 20:55:49 +0000
permalink:  hoist_and_scope
---

While struggling through some basic JS concepts, I found a pattern in some of my misleadings. It was a gap that I knew existed, but I couldn't quiet pin point what was going on. Enter hoist and scope. 



Functions and variables are processed first. Before the code even executes, the compiler has made a quick scan of all the functions and variables it needs to memorize, in order to process the code correctly. 

Local variables get hoisted onto their local scope, and global variables hoisted up onto the global scope. Functions are hoisted to the very top, while variables are underneath. 

Because functions get compiled the very top, our last function is the one that get's called first. Variables don't come until after all of the functions, and those get compiled the same as the function compiler. That's why this code returns the last var:

```
var a = 1;
var a = 2;
var a = 3;

a => 3
```

and this code returns the last function:

```
function a(){
  console.log(1);
}
function a(){
  console.log(2);
}
function a(){
  console.log(3);
}

a(); => 3
```

However, things start to get tricky when you add both variables and functions.

```
foo();  =>  3

function foo() {
	console.log( 1 );
}

var foo = function() {
	console.log( 2 );
};

function foo() {
	console.log( 3 );
}
```


Since all of our variables are now available to call into action, our functions will seek variables within the local function block first. If that variable is not found in the local scope, it moves on to find a global source. If that global variable is not found it will return undefined. Take a look at the example:

```
var salary = '$100,000'

var bonus = function(){
  console.log('My current salary is', salary)
  
  salary = '$100,000,000'
  
  console.log('My bonus salary is', salary)
}

bonus()
```

Because the first console log of "salary" cannot yet be found locally, the function goes out of it's way to find a global variable of "salary", which happens to be $100,000. Therefore the resulting code is:

```
My current salary is $100,000
My bonus salary is $100,000,000
```

A word on variables. 

How you declare a variable can determine it's scope. For example:

var = available globally at any time. Wether declared in a nested function or not. 

let = available only in whatever local scope it was created in. 

const = similar to var, available globally, can only be declared once. 
