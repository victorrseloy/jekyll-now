---
layout: post
title: "What Is Javascript Hoisting"
date: 2016-09-18 11:41:40
image: '/assets/img/wat_hoisting.jpg'
description: "Have you ever wondered what this fancy word means? Or why sometimes your variables behave in an unexpected way? Today I will explain."
main-class: 'dev'
color: '#637a91'
tags:
- javascript
- fundations
categories:
twitter_text: "Have you ever wondered what this fancy word means? Or why sometimes your variables behave in an unexpected way? Today I will explain."
introduction: "Have you ever wondered what this fancy word means? Or why sometimes your variables behave in an unexpected way? Today I will explain."
---

## 9 Thousand feet view about hoisting

If you are a front-end developer probably the first time you've heard the word
'hoisting' you might have thought "what in the seven hells does this mean???"
(or if this is the first time you are seeing this word maybe you are thinking this now).

![what is hoisting?](/assets/img/wat_hoisting.jpg)

Hoisting is a fancy word to a simple concept (that if not properly understood can lead
you to hours of code blaming). Hoisting basically is the act of the javascript interpreter
moving  declarations(both variables and functions) to the top of the current function.

## OK Let's Understand How It Works

In javascript when we are dealing with variables normally we do this 3 things:

{% highlight js%}

//quick example

var age; // Variable Declaration

age = 25; //Variable Initialization

console.log(age); //Variable Usage

//or in shorthand

var name = 'my name'; //Variable Declaration and Initialization

console.log(name);

{% endhighlight%}

or when we are dealing with functions we would do something like this:

{% highlight js%}

//quick example

//function declaration
function foo(){

  return 'bar';

}

{% endhighlight%}

this is such a natural thing that we never stop to think about what really happens
under the hood. So lets do it now.

The javascript engine does multiple passes on the source code before actually running it,
one of the very first things that the engine will do is to go through all your code
searching for variable and functions declarations. Once it finds these declarations it will move them
to the top of the current scope.

let's see this example:

{% highlight js %}

var foo = "bar";

foo = "other value";

var age = 5;

function() myAmazingFunction(){
  // do stuff
}

{% endhighlight %}

under the hood it will look like this after the engine pass

{% highlight js %}

var foo, age;

function() myAmazingFunction(){
  // do stuff
};

foo = "bar";

foo = "other value";

age = "5";


{% endhighlight %}


![why should i care about hoisting](/assets/img/hoisting_why_should_i_care.jpg)

## Pratical Hoisting Implications

The first implication of hoisting is that you can use a variable or function before
actually declaring it, this comes from the fact that javascript engine will move our declarations
to the top of our scope. This is why code like this do not throw errors.

{% highlight js %}

myAmazingFunction();

//do lots of other stuff

function myAmazingFunction(){
  //my magic logic
}

{% endhighlight %}


Other interesting hoisting implication is this common gotcha

{% highlight js %}

var foo = "outside scope value";

function bar(){

  console.log(foo); //undefined

  var foo = "inside scope value";

  console.log(foo); //inside scope value

}

bar();

{% endhighlight %}


some beginner programmers may wonder the reason why this *console.log(foo);* will print undefined.
If you don't know how hoisting works this behavior may look weird, but undefined will be printed
because under the hood the real executed code looks like this;

{% highlight js %}

var foo;

function bar(){

  var foo;

  console.log(foo); //undefined

  foo = "inside scope value";

  console.log(foo); //inside scope value

}

foo = "outside scope value";

bar();

{% endhighlight %}

## function declaration x function expression

One important thing to note is that **hoisting will move only declarations**. So for example
if you have a function expression and try to invoke it before its assignment, you will get
an error

{% highlight js %}

myAmazingFunction(); //myAmazingFunction is not a function

var myAmazingFunction = function(){
  //logic here
}

{% endhighlight %}

Remember this happens because even if we have the declaration of myAmazingFunction being moved to the top,
the Initialization of it will happen only after we tried to invoke it, this happens because we are not dealing with  a function  definition, we are dealing with a function expression instead.

## Final Thoughts

For completeness sake, I need to mention that behind the scenes things are actually implemented a little bit different. The ECMAScript standard does not define hoisting as “the declaration gets moved to the top of the function”. Handling code happens in two steps: the first one is parsing and entering the context and the second one is runtime code execution. In the first step variables, function declarations and formal parameters are created. In the second stage function expressions and unqualified identifiers (undeclared variables) are created. However for practical purposes we can adopt the concept of hoisting.

I hope you enjoyed the reading, my focus on this post was to demystify some of the strange behaviors you sometimes may experience when dealing with hoisting. Hope you enjoyed the reading. See you in the next post.
