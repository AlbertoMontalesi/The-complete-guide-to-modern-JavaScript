# Chapter 2: Arrow functions

## What is an arrow function?

ES6 introduced fat arrows (`=>`) as a way to declare functions.
This is how we would normally declare a function in ES5:

```JavaScript
const greeting = function(name) {
  return "hello " + name;
}
```

The new syntax with a fat arrow looks like this:

```JavaScript
var greeting = (name) => {
  return `hello ${name}`;
}
```

We can go further if we only have one parameter. We can drop the parentheses and write:

```JavaScript
var greeting = name => {
  return `hello ${name}`;
}
```

If we have no parameters at all, we need to write empty parenthesis like this:

```JavaScript
var greeting = () => {
  return "hello";
}
```

&nbsp;

## Implicitly return

With arrow functions, we can skip the explicit `return` and return like this:

```JavaScript
const greeting = name => `hello ${name}`;
```

Look at a side by side comparison with an old ES5 Function:

```javascript
const oldFunction = function(name){
  return `hello ${name}`
}

const arrowFunction = name => `hello ${name}`;
```

Both functions achieve the same result, but the new syntax allows you to be more concise.
Beware! Readability is more important than conciseness so you might want to write your function like this if you are working in a team and not everybody is totally up-to-date with ES6.

```javascript
const arrowFunction = (name) => {
  return `hello ${name}`;
}
```

Let’s say we want to implicitly return an object literal. We’d do it like this:

```JavaScript
const race = "100m dash";
const runners = [ "Usain Bolt", "Justin Gatlin", "Asafa Powell" ];

const results = runners.map((runner, i) =>  ({ name: runner, race, place: i + 1}));

console.log(results);
// [{name: "Usain Bolt", race: "100m dash", place: 1}
// {name: "Justin Gatlin", race: "100m dash", place: 2}
// {name: "Asafa Powell", race: "100m dash", place: 3}]
```

In this example, we're using the `map` function to iterate over the array `runners`. The first argument is the current item in the array, and the `i` is the index of it. For each item in the array we are then adding into `results` an Object containing the properties `name`, `race`, and `place`.

To tell `JavaScript` what's inside the curly braces is an **object literal** we want to implicitly return,  so we need to wrap everything inside parentheses.

Writing `race` or `race: race` is the same.

&nbsp;

## Arrow functions are anonymous

As you can see from the previous examples, arrow functions are **anonymous**.

If we want to have a name to reference them we can bind them to a variable:

```JavaScript
const greeting = name => `hello ${name}`;

greeting("Tom");
```

&nbsp;

## Arrow function and the `this` keyword

You need to be careful when using arrow functions in conjunction with the `this` keyword, as they behave differently from normal functions.

When you use an arrow function, the `this` keyword is inherited from the parent scope.

This can be useful in cases like this one:

```html
<div class="box open">
	This is a box
</div>
```

```css
.opening {
	background-color:red;
}
```

```JavaScript
// grab our div with class box
const box = document.querySelector(".box");
// listen for a click event
box.addEventListener("click", function() {
  // toggle the class opening on the div
  this.classList.toggle("opening");
  setTimeout(function(){
    // try to toggle again the class
    this.classList.toggle("opening");
    },500);
});
```

The problem in this case is that the first `this` is bound to the `const` box but the second one, inside the `setTimeout`, will be set to the `Window` object, throwing this error:

```JavaScript
Uncaught TypeError: cannot read property "toggle" of undefined
```

Since we know that **arrow functions** inherit the value of `this` from the parent scope, we can re-write our function like this:

```JavaScript
const box = document.querySelector(".box");
// listen for a click event
box.addEventListener("click", function() {
  // toggle the class opening on the div
  this.classList.toggle("opening");
  setTimeout(()=>{
    // try to toggle again the class
    this.classList.toggle("opening");
    },500);
});
```

Here, the second `this` will inherit from its parent, and will be set to the `const` box.

Running the example code, you should see our `div` turning red for just half a second.

&nbsp;

## When you should avoid arrow functions

Using what we know about the inheritance of the `this` keyword we can define some instances where you should **not** use arrow functions.

The next two examples show when to be careful using `this` inside of arrows.

#### Example 1

```JavaScript
const button = document.querySelector("btn");
button.addEventListener("click", () => {
  // error: *this* refers to the `Window` Object
  this.classList.toggle("on");
})
```

&nbsp;

#### Example 2

```JavaScript
const person1 = {
  age: 10,
  grow: function() {
    this.age++;
    console.log(this.age);
  }
}

person1.grow();
// 11

const person2 = {
  age: 10,
  grow: () => {
    // error: *this* refers to the `Window` Object
    this.age++;
    console.log(this.age);
  }
}

person2.grow();
```

Another difference between arrow functions and normal functions is the access to the `arguments object`.
The `arguments object` is an array-like object that we can access from inside functions and contains the values of the arguments passed to that function.

A quick example:

```javascript
function example(){
  console.log(arguments[0])
}

example(1,2,3);
// 1
```

As you can see we accessed the first argument using an array notation `arguments[0]`.

Similarly to what we saw with the `this` keyword, arrow functions inherit the value of the `arguments object` from their parent scope.

Let's have a look at this example with our previous list of runners:

```javascript
const showWinner = () => {
  const winner = arguments[0];
  console.log(`${winner} was the winner`)
}

showWinner( "Usain Bolt", "Justin Gatlin", "Asafa Powell" )
```

This code will return:

```JavaScript
ReferenceError: arguments is not defined
```

To access all the arguments passed to the function we can either use the old function notation or the spread syntax (which we will discuss more in Chapter 9)

Remember that `arguments` is just a keyword, it's not a variable name.

Example with **arrow function**:

```javascript
const showWinner = (...args) => {
  const winner = args[0];
  console.log(`${winner} was the winner`)
}
showWinner("Usain Bolt", "Justin Gatlin", "Asafa Powell" )
// "Usain Bolt was the winner"
```

Example with **function**:

```javascript
const showWinner = function() {
  const winner = arguments[0];
  console.log(`${winner} was the winner`)
}
showWinner("Usain Bolt", "Justin Gatlin", "Asafa Powell")
// "Usain Bolt was the winner"
```
