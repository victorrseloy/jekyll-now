---
layout: post
title: "What The Hell Is The javascript Hoisting"
date: 2016-09-18 11:41:40
image: '/assets/img/wat_hoisting.jpg'
description: "have you ever wondered what this fancy word means, or why sometimes your variables behave in an unexpected way. Today I will explain."
main-class: 'dev'
color: '#637a91'
tags:
- javascript
- fundations
categories:
twitter_text: "have you ever wondered what this fancy word means, or why sometimes your variables behave in an unexpected way. Today I will explain."
introduction: "have you ever wondered what this fancy word means, or why sometimes your variables behave in an unexpected way. Today I will explain."
---



## 9 Thousand feet view about hoisting

If you are a front-end developer probably the first time you've heard the word
'hoisting' you might have thought "what in the seven hells does this mean???"
(or if this is the first time you are seeing this word maybe you are thinking this now).

![what is hoisting?](/assets/img/wat_hoisting.jpg)

Actually hoisting is a fancy word to a simple concept, that if not properly undesrstood can lead
you to hours of code blaming. Hoisting basically is the act of the javascript interpreter
moving  declarations(both variables and functions) to the top of the current functions.

## OK Lets Actually Understand How It Works

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

this is such a natural thing that we never really stop to think about what really happens
under the hood, so lets do it now.

the javascript engine does multiple passes on the source code before actually running it,
so one of the very first things that the engine will do is to go through all you code
searching for variable and functions declarations, once it find these declarations it will move them
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

actually will look like this under the hood after this pass

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

speak about

Declaration - create a new variable. E.g. var myValue
Initialization - initialize the variable with a value. E.g. myValue = 150
Usage - access and use the variable value. E.g. alert(myValue)

## how it works

resolution order

function declaration
function definition

variable leakage

However, function definition hoisting only occurs for function declarations, not function expressions. For example:

// ReferenceError: funcName is not defined
funcName();

// TypeError: undefined is not a function
varName();

var varName = function funcName() {
    console.log("Definition not hoisted!");
};



##Gotcha moment
// Outputs: undefined
console.log(declaredLater);

var declaredLater = "Now it's defined!";

// Outputs: "Now it's defined!"
console.log(declaredLater);

speak about the problem with hoiting redefining var inside block

##Final thoughts
