# Chapter 21: What's new in ES2019?

In this chapter we will look at what is included in the latest version of `ECMAScript`: ES2019.

&nbsp; 

## `Array.prototype.flat()` / `Array.prototype.flatMap()`

`Array.prototype.flat()` will flatten the array recursively up to the depth that we specify. If no depth argument is specified, 1 is the default value. We can use `Infinity` to flatten all nested arrays.

```javascript
const letters = ['a', 'b', ['c', 'd', ['e', 'f']]];
// default depth of 1
letters.flat();
// ['a', 'b', 'c', 'd', ['e', 'f']]

// depth of 2
letters.flat(2);
// ['a', 'b', 'c', 'd', 'e', 'f']

// which is the same as executing flat with depth of 1 twice
letters.flat().flat();
// ['a', 'b', 'c', 'd', 'e', 'f']

// Flattens recursively until the array contains no nested arrays
letters.flat(Infinity)
// ['a', 'b', 'c', 'd', 'e', 'f']
```

`Array.prototype.flatMap()` is identical to the previous one with regards to the way it handles the depth argument, but instead of simply flattening an array, with `flatMap()` we can also map over it and return the result in the new array.

```javascript
let greeting = ["Greetings from", " ", "Vietnam"];

// let's first try using a normal `map()` function
greeting.map(x => x.split(" "));
// ["Greetings", "from"]
// ["", ""]
// ["Vietnam"]


greeting.flatMap(x => x.split(" "))
// ["Greetings", "from", "", "", "Vietnam"]
```

As you can see, if we use `.map()` we will get a multi level array, which is a problem that we can solve by using `.flatMap()`. This will also flatten our array.

&nbsp;

## `Object.fromEntries()`

`Object.fromEntries()` transforms a list of key-value pairs into an object.

```javascript
const keyValueArray = [
  ['key1', 'value1'],
  ['key2', 'value2']
]

const obj = Object.fromEntries(keyValueArray)
// {key1: "value1", key2: "value2"}
```

We can pass any iterable as argument of `Object.fromEntries()`, whether it's an `Array`, a `Map` or other objects implementing the iterable protocol.

You can read more about the iterable protocol here: [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols#The_iterable_protocol](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols#The_iterable_protocol)

&nbsp;

## `String.prototype.trimStart()` / `.trimEnd()`

`String.prototype.trimStart()` removes white space from the beginning of a string while `String.prototype.trimEnd()` removes them from the end.

```javascript
let str = "    this string has a lot of whitespace   ";

str.length;
// 42

str = str.trimStart();
// "this string has a lot of whitespace   "
str.length;
// 38

str = str.trimEnd();
// "this string has a lot of whitespace"
str.length;
// 35
```

We can also use `.trimLeft()` as an alias of `.trimStart()` and `.trimRight()` as an alias of `.trimEnd()`.

&nbsp;

## Optional Catch Binding

Prior to ES2019, you had to include an exception variable in your `catch` clause. E2019 allows you to omit it.

```javascript
// Before
try {
   ...
} catch(error) {
   ...
}

// ES2019
try {
   ...
} catch {
   ...
}
```

This is useful when you want to ignore the error. For a more detailed list of use cases for this I highly recommend this article: [http://2ality.com/2017/08/optional-catch-binding.html](http://2ality.com/2017/08/optional-catch-binding.html)

&nbsp;

## `Function​.prototype​.toString()`

The `.toString()` method returns a string representing the source code of the function.

```javascript
function sum(a, b) {
  return a + b;
}

console.log(sum.toString());
// function sum(a, b) {
//    return a + b;
//  }
```

It also includes comments.

```javascript
function sum(a, b) {
  // perform a sum
  return a + b;
}

console.log(sum.toString());
// function sum(a, b) {
//   // perform a sum
//   return a + b;
// }
```

&nbsp;

## `Symbol.prototype.description`

`.description` returns the optional description of a `Symbol` Object.

```javascript
const me = Symbol("Alberto");
me.description;
// "Alberto"

me.toString()
//  "Symbol(Alberto)"
```
