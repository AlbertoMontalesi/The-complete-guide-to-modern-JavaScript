# Chapter 9: Spread operator and rest parameters

## The Spread operator

According to MDN:

> Spread syntax allows an iterable such as an array expression or string to be expanded in places where zero or more arguments \(for function calls\) or elements \(for array literals\) are expected, or an object expression to be expanded in places where zero or more key-value pairs \(for object literals\) are expected.

### Combine arrays

```javascript
const veggie = ["tomato","cucumber","beans"];
const meat = ["pork","beef","chicken"];

const menu = [...veggie, "pasta", ...meat];
console.log(menu);
// Array [ "tomato", "cucumber", "beans", "pasta", "pork", "beef", "chicken" ]
```

The `...` is the spread syntax, and it allowed us to grab all the individual values of the arrays veggie and meat and put them inside the array menu and at the same time add a new item in between them.

### Copy arrays

The spread syntax is very helpful if we want to create a copy of an array.

```javascript
const veggie = ["tomato","cucumber","beans"];
const newVeggie = veggie;

// this may seem like we created a copy of the veggie array but look now

veggie.push("peas");
console.log(veggie);
// Array [ "tomato", "cucumber", "beans", "peas" ]

console.log(newVeggie);
// Array [ "tomato", "cucumber", "beans", "peas" ]
```

Our new array changed aswell, but why? Because we did not actually create a copy but we just referenced our old array in the new one. This is how we would usually make a copy of an array in ES5 and earlier.

```javascript
const veggie = ["tomato","cucumber","beans"];
const newVeggie = [].concat(veggie);
// we created an empty array and put the values of the old array inside of it
```

And this is how we would do the same using the spread syntax:

```javascript
const veggie = ["tomato","cucumber","beans"];
const newVeggie = [...veggie];
```

### Spread into a function

```javascript
// OLD WAY
function doStuff (x, y, z) {
  console.log(x + y + z);
 }
var args = [0, 1, 2];

// Call the function, passing args
doStuff.apply(null, args);

// USE THE SPREAD SYNTAX

doStuff(...args);
// 3 (0+1+2);
console.log(args);
// Array [ 0, 1, 2 ]
```

We can replace the `.apply()` syntax and just use the spread operator.

Let's look at another example:

```javascript
const name = ["Alberto", "Montalesi"];

function greet(first, last) {
  console.log(`Hello ${first} ${last}`);
}

greet(...name);
// Hello Alberto Montalesi
```

The two values of the array are automatically assigned to the two arguments of our function.

What if we provide more values than arguments?

```javascript
const name = ["Jon", "Paul", "Jones"];

function greet(first, last) {
  console.log(`Hello ${first} ${last}`);
}
greet(...name);
// Hello Jon Paul
```

We provided 3 values inside our array but only have 2 arguments in our function therefore the last one is left out.

### Spread in Object Literals \(ES 2018/ ES9\)

This feature is not part of ES6 but as we already discussing this topic, it is worth mentioning that ES9 will introduce the Spread operator for Objects. Let's look at an example:

```javascript
let person = {
  name : "Alberto",
  surname: "Montalesi",
  age: 25,
}

let clone = {...person};
console.log(clone);
// Object { name: "Alberto", surname: "Montalesi", age: 25 }
```

## The Rest parameter

The rest syntax looks exactly the same as the spread, 3 dots `...` but it is quite the opposite of it. Spread expands an array, while rest condenses multiple elements into a single one.

```javascript
const runners = ["Tom", "Paul", "Mark", "Luke"];
const [first,second,...losers] = runners;

console.log(...losers);
// Mark Luke
```

We stored the first two values inside the `const` first and second and whatever was left we put it inside losers using the rest parameter.

