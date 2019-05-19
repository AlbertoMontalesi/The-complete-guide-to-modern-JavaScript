# Chapter 2: Arrow functions

## What is an arrow function?

ES6 introduced fat arrows (`=>`) as a way to declare functions.
This is how we would normally declare a function in ES5:

``` javascript
var greeting = function(name) {
  return "hello " + name;
}
```

The new syntax with a fat arrow looks like this:

``` javascript
const greeting = (name) => {
  return `hello ${name}`;
}
```

We can go further, if we only have one parameter we can drop the parenthesis and write:

``` javascript
const greeting = name => {
  return `hello ${name}`;
}
```

If we have no parameter at all we need to write empty parenthesis like this:

``` javascript
const greeting = () => {
  return "hello";
}
```

&nbsp;

## Implicitly return

With arrow functions we can skip the explicit `return` and return like this:

``` javascript
const greeting = name => `hello ${name}`;
```

Look at a side by side comparison with an old ES5 Function:

```js
const oldFunction = function(name){
  return `hello ${name}`
}

const arrowFunction = name => `hello ${name}`;
```

Both functions achieve the same result, but the new syntax allows you to be more concise.
Beware! Readability is more important than conciceness so you might want to write your funciton like this if you are working in a team and not everybody is totally up-to-date with ES6.

```js
const arrowFunction = (name) => {
  return `hello ${name}`;
}
```

Let's say we want to implicitly return an **object literal**, we would do like this:

``` javascript
const race = "100m dash";
const runners = [ "Usain Bolt", "Justin Gatlin", "Asafa Powell" ];

const results = runners.map((runner, i) =>  ({ name: runner, race, place: i + 1}));

console.log(results);
// [{name: "Usain Bolt", race: "100m dash", place: 1}
// {name: "Justin Gatlin", race: "100m dash", place: 2}
// {name: "Asafa Powell", race: "100m dash", place: 3}]
```

To tell JavaScript that what's inside the curly braces is an **object literal** that we want to implicitly return, we need to wrap everything inside parenthesis.

Writing `race` or `race: race` is the same.

&nbsp;

## Arrow functions are anonymous

As you can see from the previous examples, arrow functions are **anonymous**.

If we want to have a name to reference them we can bind them to a variable:

``` javascript
const greeting = name => `hello ${name}`;

greeting("Tom");
```

&nbsp;

## Arrow function and the `this` keyword

You need to be careful when using arrow functions in conjunction with the `this` keyword, as they behave differently from normal functions.

When you use an arrow function, the `this` keyword is inherited from the parent scope.

This can be useful in cases like this one:

``` javascript 
// grab our div with class box
const box = document.querySelector(".box");
// listen for a click event 
box.addEventListener("click", function() {
  // toggle the class opening on the div
  this.classList.toggle("opening");
  setTimeout(function(){
    // try to toggle again the class
    this.classList.toggle("open");
    });
});
```


The problem in this case is that the first `this` is bound to the `const` box but the second one, inside the `setTimeout`, will be set to the `Window` object, trowing this error:

``` javascript
Uncaught TypeError: cannot read property "toggle" of undefined 
```

Since we know that **arrow functions** inherit the value of `this` from the parent scope, we can re-write our function like this:

``` javascript
// grab our div with class box
const box = document.querySelector(".box");
// listen for a click event
box.addEventListener("click", function () {
  // toggle the class opening on the div
  this.classList.toggle("opening");
  setTimeout(() => {
    // try to toggle again the class
    this.classList.toggle("open");
   });
});
```

Here, the second `this` will inherit from its parent, and will be set to the `const` box.

&nbsp;

## When you should avoid arrow functions

Using what we know about the inheritance of the `this` keyword we can define some instances where you should **not** use arrow functions.

The next 2 examples show when to be careful using `this` inside of arrows.

``` javascript
const button = document.querySelector("btn");
button.addEventListener("click", () => {
  // error: *this* refers to the `Window` Object
  this.classList.toggle("on");
})
```

``` javascript
const person = {
  age: 10,
  grow: () => {
    // error: *this* refers to the `Window` Object
    this.age++;
  }
}
```

Another difference between Arrow functions and normal functions is the access to the `arguments object`.
The `arguments object` is an array-like object that we can access from inside functions and contains the values of the arguments passed to that function.

A quick example:

```js
function example(){
  console.log(arguments[0])
}

example(1,2,3);
// 1
```

As you can see we accessed the first argument using an array notation `arguments[0]`.

Similarly as what we saw with the `this` keyword, Arrow functions inherit the value of the `arguments object` from their parent scope.

Let's have a look at this example with our previous list of runners:

```javascript
const showWinner = () => {
  const winner = arguments[0];
  return `${winner} was the winner`
}

showWinner( "Usain Bolt", "Justin Gatlin", "Asafa Powell" )
```

This code will return:

``` javascript
ReferenceError: arguments is not defined
```

To access all the arguments passed to the function we can either use the old function notation or the spread syntax(which we will discuss more in Chapter 9)

Remember that `arguments` it's just a keyword, it's not a variable name.

Example with **arrow function**:

```javascript
const showWinner = (...args) => {
  const winner = args[0];
  return `${winner} was the winner`
}
showWinner("Usain Bolt", "Justin Gatlin", "Asafa Powell" )
// "Usain Bolt was the winner"
```

Example with **function**:

```js
const showWinner = function() {
  const winner = arguments[0];
  return `${winner} was the winner`
}
showWinner("Usain Bolt", "Justin Gatlin", "Asafa Powell")
// "Usain Bolt was the winner"
```