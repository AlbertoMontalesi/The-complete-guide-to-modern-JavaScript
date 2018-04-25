# Chapter 1: Var vs Let vs Const & the temporal dead zone

With the introduction of `let` and `const` in **ES6** we can know better define our variable depending on our needs. Let's have a look at the major differences between them.

&nbsp;

## `Var`

`var` are **function scoped**, which means that if we declare them inside a `for` loop (which is a **block** scope) they will be available globally.

``` javascript 
for (var i = 0; i < 10; i++) {
  var global = "I am available globally";
}

console.log(global);
// I am available globally

function myFunc(){
  var functionScoped = "I am available inside this function";
  console.log(functionScoped);
}
myFunc();
// I am available inside this function
console.log(functionScoped);
// ReferenceError: functionScoped is not defined
```

In the first example the value of `var` global leaked out of the block-scope and could be accessed from the global scope whereas in the second example `var` was confined inside a function-scope and we could not access it from outside.

&nbsp;

## `Let`

`let` (and `const` are **block scoped** meaning that they will be available only inside of the block where they are declared and its sub-blocks.

``` javascript
// using `let`
let x = "global";

if (x === "global") {
  let x = "block-scoped";

  console.log(x);
  // expected output: block-scoped
}

console.log(x);
// expected output: global

// using `var`
var y = "global";

if (y === "global") {
  var  y= "block-scoped";

  console.log(y);
  // expected output: block-scoped
}

console.log(y);
// expected output: block-scoped
```

As you can see, when we assigned a new value to our `let` inside our block-scope it **did not** change the value in the global scope, wherease when did the same with our `var` it leaked outside of the block-scope and also change it in the global scope.

&nbsp;

## `Const`

Similarly to `let`, `const` are **block-scoped** but they differ in the fact that their value **can't change through re-assignment or can't be  redeclared**.


``` javascript
const constant = 'I am a constant';
constant = " I can't be reassigned";

// Uncaught TypeError: Assignment to constant variable
```


**Important** 
This **does not** mean that **const are immutable**.

&nbsp;

### The content of a `const` is an Object

``` javascript
const person = {
  name: 'Alberto',
  age: 25,
}

person.age = 26;

// in this case no error will be raised, we are not re-assigning the variable but just one of its properties.
``` 

---
&nbsp;

## The temporal dead zone

According to **MDN**:

> In ECMAScript 2015, let bindings are not subject to **Variable Hoisting**, which means that let declarations do not move to the top of the current execution context. Referencing the variable in the block before the initialization results in a ReferenceError (contrary to a variable declared with var, which will just have the undefined value). The variable is in a “temporal dead zone” from the start of the block until the initialization is processed.

Let's look at an example:

```javascript
console.log(i);
var i = "I am a variable";

//  expected output: undefined

console.log(j);
let j = "I am a let";

// expected output: ReferenceError: can't access lexical declaration `j' before initialization
```

`var` can be accessed **before** they are defined, but we can't access their **value**.
`let` and `const` can't be accessed **before we define them**.

This happens because `var` are subject to **hoisting** which means that they are processed before any code is executed. Declaring a `var` anywhere is equivalent to **declaring it at the top**. This is why we can still access the `var` but we can't yet see its content, hence the `undefined` result.


---
&nbsp;

## When to use `Var`, `Let` and `Const`

There is no rule stating where to use each of them and people have different opinions. Here I am going to present to you two opinions from popular developers in the JavaScript community.

The first opinion comes from [Mathias Bynes:](https://mathiasbynens.be/notes/es6-const)


- use `const` by default
- use `let` only if rebinding is needed.
- `var` should never be used in ES6.


The second opinion comes from [Kyle Simpson:]( blog.getify.com/constantly-confusing-const/)

- Use `var` for top-level variables that are shared across many (especially larger) scopes.
- Use `let` for localized variables in smaller scopes.
- Refactor `let` to `const` only after some code has to be written, and you're reasonably sure that you've got a case where there shouldn't be variable reassignment.

Which opinion to follow is entirely up to you. As always, do your own research and figure out which one you think is the best.

You may want to [read this article](https://medium.com/@sbakkila/javascript-es-6-let-and-the-dreaded-temporal-dead-zone-85b89314d168) to understand how `let` affects your performances compared to `var` before you choose to follow either [Mathias Bynes](https://mathiasbynens.be/notes/es6-const) or [Kyle Simpson]( blog.getify.com/constantly-confusing-const/).