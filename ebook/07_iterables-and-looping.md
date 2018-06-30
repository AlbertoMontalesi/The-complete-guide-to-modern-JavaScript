# Chapter 7: Iterables and looping

ES6 introduced a new type of loop, the `for of` loop.

&nbsp;

## The `for of` loop

### Iterating over an array

Usually we would iterate using something like this:

``` js
var fruits = ['apple','banana','orange'];
for (var i = 0; i < fruits.length; i++){
  console.log(fruits[i]);
}
// apple
// banana
// orange
```

Look at how we can achieve the same with a `for of` loop:

``` js
const fruits = ['apple','banana','orange'];
for(const fruit of fruits){
  console.log(fruit);
}
// apple
// banana
// orange
```

&nbsp;

### Iterating over an object

Objects are **non iterable** so how do we iterate over them?
We have to first grab all the values of the object using something like `Object.keys()` or the new ES6 `Object.entries()`.


```js
const car = {
  maker: "BMW",
  color: "red",
  year : "2010",
}

for (const prop of Object.keys(car)){
  const value = car[prop];
  console.log(value,prop);
}
// BMW maker
// red color
// 2010 year
```

&nbsp;

## The `for in` loop

Even though it is not a new ES6 loop, let's look at the `for in` loop to understand what differentiate it compared to the `for of`.

The `for in` loop is a bit different because it will iterate over all the [enumerable properties](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Enumerability_and_ownership_of_properties) of an object in no particular order.

It is therefore suggested not to add, modify or delete properties of the object during the iteration as there is no guarantee that they will be visited, or if they will be visited before or after being modified.

```js
const car = {
  maker: "BMW",
  color: "red",
  year : "2010",
}
for (const prop in car){
  console.log(prop, car[prop]);
}
// maker BMW
// color red
// year 2010
```

&nbsp;

## Difference between `for of` and `for in`

The first difference we can see is by looking at this example:

```js
let list = [4, 5, 6];

// for...in returns a list of keys
for (let i in list) {
   console.log(i); // "0", "1", "2",
}


// for ...of returns the values 
for (let i of list) {
   console.log(i); // "4", "5", "6"
}
```

`for in` will return a list of keys whereas the `for of` will return a list of values of the numeric properties of the object being iterated.


Another differences is that we **can** stop a `for of` loop but we can't do the same with a `for in` loop.

```js
const digits = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];

for (const digit of digits) {
  if (digit % 2 === 0) {
    continue;
  }
  console.log(digit);
}
// 1 3 5 7 9
```

The last important difference I want to talk is that the `for in` loop will iterate over new properties added to the object.

```js
const fruit = ["apple","banana", "orange"];

fruit.eat = "gnam gnam";

for (const prop of fruit){
  console.log(prop);
}
// apple
// banana
// orange

for (const prop in fruit){
  console.log(fruit[prop]);
}

// apple
// banana
// orange
// gnam gnam
```
