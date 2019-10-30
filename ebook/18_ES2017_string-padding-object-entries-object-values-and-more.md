# Chapter 18: ES2017 string padding, `Object.entries()`, `Object.values()` and more

ES2017 introduced many cool new features, which we are going to see here.

## String padding (`.padStart()` and `.padEnd()`)

We can now add some padding to our strings, either at the end (`.padEnd()`) or at the beginning (`.padStart()`) of them.

```javascript
"hello".padStart(6);
// " hello"
"hello".padEnd(6);
// "hello "
```

We specified that we want 6 as our padding, so then why in both cases did we only get 1 space?
It happened because `padStart` and `padEnd` will go and fill the **empty spaces**. In our example "hello" is 5 letters, and our padding is 6, which leaves only 1 empty space.

Look at this example:

```javascript
"hi".padStart(10);
// 10 - 2 = 8 empty spaces
// "        hi"
"welcome".padStart(10);
// 10 - 6 = 4 empty spaces
// "   welcome"
```

&nbsp;

### Right align with `padStart`

We can use `padStart` if we want to right align something.

```javascript
const strings = ["short", "medium length", "very long string"];

const longestString = strings.sort(str => str.length).map(str => str.length)[0];

strings.forEach(str => console.log(str.padStart(longestString)));

// very long string
//    medium length
//            short
```

First we grabbed the longest of our strings and measured its length. We then applied a `padStart` to all the strings based on the length of the longest so that we now have all of them perfectly aligned to the right.

&nbsp;

### Add a custom value to the padding

We are not bound to just add a white space as a padding, we can pass both strings and numbers.

```javascript
"hello".padEnd(13," Alberto");
// "hello Alberto"
"1".padStart(3,0);
// "001"
"99".padStart(3,0);
// "099"
```

&nbsp;

## `Object.entries()` and `Object.values()`

Let's first create an Object.

```javascript
const family = {
  father: "Jonathan Kent",
  mother: "Martha Kent",
  son: "Clark Kent",
}
```

In previous versions of JavaScript we would have accessed the values inside the object like this:

```javascript
Object.keys(family);
// ["father", "mother", "son"]
family.father;
"Jonathan Kent"
```

`Object.keys()` returned only the keys of the object that we then had to use to access the values.

We now have two more ways of accessing our objects:

```javascript
Object.values(family);
// ["Jonathan Kent", "Martha Kent", "Clark Kent"]

Object.entries(family);
// ["father", "Jonathan Kent"]
// ["mother", "Martha Kent"]
// ["son", "Clark Kent"]
```

`Object.values()` returns an array of all the values whilst `Object.entries()` returns an array of arrays containing both keys and values.

&nbsp;

## `Object.getOwnPropertyDescriptors()`

This method will return all the own property descriptors of an object.
The attributes it can return are `value`, `writable`, `get`, `set`, `configurable` and `enumerable`.

```javascript
const myObj = {
  name: "Alberto",
  age: 25,
  greet() {
    console.log("hello");
  },
}
Object.getOwnPropertyDescriptors(myObj);
// age:{value: 25, writable: true, enumerable: true, configurable: true}

// greet:{value: ƒ, writable: true, enumerable: true, configurable: true}

// name:{value: "Alberto", writable: true, enumerable: true, configurable: true}
```

&nbsp;

## Trailing commas in function parameter lists and calls

This is just a minor change to a syntax. Now, when writing objects we can leave a trailing comma after each parameter, whether or not it is the last one.

```javascript
// from this
const object = {
  prop1: "prop",
  prop2: "propop"
}

// to this
const object = {
  prop1: "prop",
  prop2: "propop",
}
```

Notice how I wrote a comma at the end of the second property.
It will not throw any error if you don't put it, but it's a better practice to follow as it will make your colleague or team members life easier.

```javascript
// I write
const object = {
  prop1: "prop",
  prop2: "propop"
}

// my colleague updates the code, adding a new property
const object = {
  prop1: "prop",
  prop2: "propop"
  prop3: "propopop"
}
// suddenly, he gets an error because he did not notice that I forgot to leave a comma at the end of the last parameter.
```

&nbsp;

## Shared memory and `Atomics`

From [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Atomics):

> When memory is shared, multiple threads can read and write the same data in memory. Atomic operations make sure that predictable values are written and read, that operations are finished before the next operation starts and that operations are not interrupted.

`Atomics` is not a constructor, all of its properties and methods are static (just like `Math`) therefore we cannot use it with a new operator or invoke the `Atomics` object as a function.

Examples of its methods are:

- add / sub
- and / or / xor
- load / store
