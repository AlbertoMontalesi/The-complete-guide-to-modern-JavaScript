# Chapter 7: Iterables and looping

## The `for of` loop

ES6 introduced a new type of loop, the `for of` loop.
Let’s take a look at how it's used

&nbsp;

### Iterating over an array

Usually, we would iterate using the following method:

```javascript
var fruits = ['apple','banana','orange'];
for (var i = 0; i < fruits.length; i++){
  console.log(fruits[i]);
}
// apple
// banana
// orange
```

This is a normal loop where at each iteration we increase the value of `i` by 1 as long as it's less than `fruits.length`. At that point the loop will stop.

Look at how we can achieve the same with a `for of` loop:

```javascript
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

Objects are **non iterable**, so how do we iterate over them?
We have to first grab all the values of the object using something like `Object.keys()` or the new ES6 function: `Object.entries()`.

```javascript
const car = {
  maker: "BMW",
  color: "red",
  year : "2010",
}

for (const prop of Object.keys(car)){
  const value = car[prop];
  console.log(prop,value);
}
// maker BMW
// color red
// year 2010
```

&nbsp;

## The `for in` loop

Even though it's not a new ES6 loop, let's look at the `for in` loop to understand what differentiates it to the `for of` loop.

The `for in` loop is a bit different because it will iterate over all the [enumerable properties](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Enumerability_and_ownership_of_properties) of an object in no particular order.

It's therefore suggested not to add, modify or delete properties of the object during the iteration. There’s no guarantee that they will be visited, or if they will be visited before or after being modified.

```javascript
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

```javascript
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
