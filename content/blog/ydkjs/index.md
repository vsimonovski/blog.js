---
title: YDKJS
date: '2016-12-13'
description: Where it takes a pretty in-depth knowledge of a language like C or C++ to write a full-scale program, full-scale production JavaScript can, and often does, barely scratch the surface of what the language can do.
---

**Y**ou **D**on't **K**now **J**avaScript, is a [great book series](https://github.com/getify/You-Dont-Know-JS) by Kyle Simpson about JavaScript core mechanisms. Unfortunately it's harsh truth for most of Web Developers.

> While JavaScript is perhaps one of the easiest languages to get up and running with, its eccentricities make solid mastery of the language a vastly less common occurrence than in many other languages. Where it takes a pretty in-depth knowledge of a language like C or C++ to write a full-scale program, full-scale production JavaScript can, and often does, barely scratch the surface of what the language can do.

I completely agree. Also, one big problem of newcomers to JavaScript is expecting from JavaScript to behave like some other programming language, however, JavaScript has its own set of ideas and ways of solving stuff, some of them are bad some of them are good and most of them are awesome.

Initial idea was to cover all books from YDKJS series, I realized it would be "reinventing the wheel" most of the time so I decided to cover only [Up & Going](https://github.com/getify/You-Dont-Know-JS/tree/master/up%20%26%20going) with some extra examples. Following text represents brief summary of topics covered deeply in other YDKJS books, **article is not substitute for reading all YDKJS books**, main purpose is to make you more interested in YDKJS book series and javascript itself.

---

## Types

JavaScript is loosely typed, this doesn't mean JavaScript has no types, you just don't need to write them.

The following built-in types are available:

- `string`
- `number`
- `boolean`
- `null` and `undefined`
- `object`
  - `function`
  - `array`
  - `date`
  - `regExp`
- `symbol`

You can check type of variable with `typeof` operator. You can call it as `typeof(variable)` too.

```js{17-19}
let a

console.log(typeof a) // "undefined"

a = 'Some text'

console.log(typeof a) // "string"

a = 42

console.log(typeof a) // "number"

a = true

console.log(typeof a) // "boolean"

a = null

console.log(typeof a) // "object" <- Caution!

a = undefined

console.log(typeof a) // "undefined"

a = {
  b: 'c',
}

console.log(typeof a) // "object"
```

This probably work as you expect, but again, be carefull `typeof(null)` returns an object, so for an example if you want to check if some variable is an object you can do it like this:

```js
let object = {}
let falseObject = null

function isObj(arg) {
  return typeof of === 'object' && arg !== null
}

console.log(isObj(object)) // true
console.log(isObj(falseObject)) // false
```

---

## Objects

In JavaScript, an object is a standalone entity, with properties and type. Compare it with a car, for example. A car is an object, with properties. A car has a color, a design, a type, number of doors etc. The same way, JavaScript objects can have properties, which define their characteristics.

You can access object properties in two way, . ( dot notation ), [ ] ( brackets notation )

```js
let ShibaInu = {
  legs: 4,
  race: 'Dog',
  sex: 'Male',
}

ShibaInu.legs // 4
ShibaInu['race'] // "Dog"
```

**Objects are passed by reference, not by value.**

```js
let objA = {
    prop: "Some property value";
}

let objB = objA; // objB now "points" to objA object

objA.prop2 = "Another property value";

objB.prop2; // "Another property value"
```

![alt text](https://blog.penjee.com/wp-content/uploads/2015/02/pass-by-reference-vs-pass-by-value-animation.gif)

More informations about JavaScript objects later on.

## Arrays

An array is an object that holds values (of any type) not particularly in named properties/keys, but rather in numerically indexed positions. For example:

```js
let arr = [
    "1",
    23,
    { a : "b", c : "d" },
    function() { console.log("Hi!")
];

arr[0]; // "1"
arr[3](); // "Hi!"
```

Because arrays are objects, they can also have properties, including the automatic updated length property.

```js
// arr from previous example
console.log(arr.length) // 4

arr.returnFirst = function() {
  return this[0]
}

arr.returnLast = function() {
  let len = this.length
  return this[len - 1]
}

arr.returnFirst() // "1"
arr.returnLast() // function () { ... }

// returnLast() returns function since last element of arr is a function
// we can invoke it with another set of ()
arr.returnLast()() // "Hi!"
```

## Functions

JavaScript implements a first-class functions. This basically means that you can treat functions as any other type. You can pass them around, you can declare them inline, you can return them from other functions etc..

Functions, as many other things in JS, are objects. So like in case with Array they can have properties too. More details about functions later on, for now here is small example:

```js
function ultimateQuestionOfLife() {
  return 42
}

ultimateQuestionOfLife() // 42
```

## Comparing values

You can compare values with one of following operators:

- `==`
- `!=`
- `===`
- `!===`
- `Object.is()` (ES6)

Result of any comparison is boolean value, true or false. Main difference between `==` and `===` is coercion. `==` allow coercion and `===` don't.

It's handy to know what evaulates to true and false before comparing values.

Falsy values:

- `""` - empty string
- `0`, `-0`, `NaN`
- `null`, `undefined`
- `false`

Truthy values:

- `"hello"`
- `42`
- `true`
- `[]`
- `{}`
- `function bar() { ... }`

## Variables

Valid name: must start with `a-z`, `A-Z`, `$` or `_`, It can contain any of those characters plus the numerals `0-9`.

## Hoisting

> Wherever a var appears inside a scope, that declaration is taken to belong to the entire scope and accessible everywhere throughout.
> Metaphorically, this behavior is called hoisting, when a var declaration is conceptually "moved" to the top of its enclosing scope. Technically, this process is more accurately explained by how code is compiled, but we can skip over those details for now.

Consider this example:

```js
var a = 2

foo() // works because foo() declaration
// is hoisted

function foo() {
  a = 3
  console.log(a) // 3
  var a // declaration is hoisted
  // on top of foo()
}

console.log(a)
```

Or:

```js
var a = 42

function bar() {
  console.log(typeof a) // "function"
  a = 23
  function a() {}
  return a
}

bar()
console.log(a) // 42
```

So `function a()` is hoisted on top of function `function bar()`, then we give it a value of 23 so it becomes a number, and finally we return it. Because of this behaviour the global `a` variable stays unchanged.

When you declare a variable it's availible anywhere in that scope. JavaScript used to have only function scope, meaning creating function creates new scope. ES6 changed that introducing the `let` keyword, using `let` one can declare block scope.

```js
function bScope() {
  var a = 10
  if (a >= 10) {
    let a = 5
  }
  console.log(a)
}

function fScope() {
  var a = 10
  if (a >= 10) {
    var a = 5
  }
  console.log(a)
}

bScope() // 10
fScope() // 5
```

## Strict mode

ES5 added "strict mode" to the language. Strict mode tightens the rules for certain behaviours. In general using strict mode, your code will become "safer place", this doesn't mean your code will become error proof or perfect, but it will be one step closer to that.

```js
function foo() {
    "use strict";
    // this code is in strict mode
    function bar() {
        // this code is in strict mode
    }
```

Or:

```js
'use strict'
// this code is in strict mode
function foo() {
  // this code is in strict mode
  function bar() {}
}
```

Strict mode is disallowing the implicit auto-global variable declaration from omitting the `var` keyword.

```js
function foo() {
  'use strict'
  a = 42 // var missing, ReferenceError
}

foo()
```

## Immediately Invoked Function Expressions (IIFEs)

IIFEs can be very usefull, let's see some examples:

<!-- prettier-ignore -->
```js
(function IIFE() {
  console.log('Hi from IIFE!')
})()

// "Hi from IIFE!"
```

The outer `( .. )` that surrounds the function is just a mechanism needed to prevent it from being treated as a normal function declaration. The final `()` on the end of expression is what executes the function.

Creating IIFE you also create new variable scope, so you can use IIFE to do something like this:

<!-- prettier-ignore-start -->
```js
var a = 42
(function IIFE() {
  var a = 10
  console.log(a) // 10
})()

console.log(a) // 42
```
<!-- prettier-ignore-end -->

IIFEs can also have return values:

```js
var x = (function IIFE() {
  return 42
})()

console.log(x) // 42
```

## Closure

Closures are functions that refer to independent (free) variables (variables that are used locally, but defined in an enclosing scope). In other words, these functions 'remember' the environment in which they were created.

Little motivation to learn and understand closures, recently I took couple of interviews and in 90% of them I had task to write function that sums two number, function must be called in this way: `sum( arg1 )( arg2 )`

Solution:

```js
let sumES6 = x => {
  return y => {
    return x + y
  }
}

console.log(sumES6(2)(3)) // 5
```

If you are not familiar with ES6 arrow functions, here is equialent ES5 example:

```js
let sumES5 = function(x) {
  return function(y) {
    return x + y
  }
}

console.log(sumES5(2), 3) // 5
```

Because of this mechanism, we can set one argument, and pass other argument later on. In our example we double invoke over `sumES6` function. On first invoke we return reference to inner function, on second invoke we return `x + y` value. Closure give us acess to `x` value we passed in first invoke.

### Module pattern

> The most common usage of closure in JavaScript is the module pattern. Modules let you define private implementation details (variables, functions) that are hidden from the outside world, as well as a public API that is accessible from the outside.

Consider the example:

```js
function Employee() {
  let name, surname

  function returnSallary(nm, srnm) {
    name = nm
    surname = srnm

    // connect to a database
    // return sallary or smth. simillar here ...
  }

  let publicAPI = {
    sallary: returnSallary,
  }

  return publicAPI
}

// create a `Employee` module instance
let john = Employee()

john.sallary('John', 'Doe')
```

So, the `publicAPI` object is returned after Employee is invoked, `john` will have acess to sallary property of that object, that will again return inner returnSallary function.

### this keyword

`this` is very specific mechanism in javascript and it's value mainly depends on context of execution.

> If a function has a this reference inside it, that this reference usually points to an object. But which object it points to depends on how the function was called.
> It's important to realize that this does not refer to the function itself, as is the most common misconception.

```javascript
function foo() {
  console.log(this.bar)
}

let bar = 'global'

let obj1 = {
  bar: 'obj1',
  foo: foo,
}

let obj2 = {
  bar: 'obj2',
}

// --------

foo() // "global"
obj1.foo() // "obj1"
foo.call(obj2) // "obj2"
new foo() // undefined
```

There are four rules for how this gets set, and they're shown in those last four lines of that snippet.

- `foo()` ends up setting this to the global object in non-strict mode -- in strict mode, this would be undefined and you'd get an error in accessing the bar property -- so "global" is the value found for this.bar.
- `obj1.foo()` sets this to the obj1 object.
- `foo.call(obj2)` sets this to the obj2 object.
- `new foo()` sets this to a brand new empty object.

---

I will stop here. For further informations consider looking [this YDKJS books overview.](https://github.com/getify/You-Dont-Know-JS/blob/master/up%20%26%20going/ch3.md)

I hope you enjoyed this article and that you're more interested in learning javascript than you were before reading this article. Remember:

![knowledge is power](https://media.giphy.com/media/ijxKTF6iE4K4M/giphy.gif)
