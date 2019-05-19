# Chapter 5: Additional string methods

There are many methods that we can use againt strings. Here's a list of a few of them:

```js
// .length()
const str = 'hello'
str.length() // 5
// .indexOf()
const str2 = 'this is a short sentence'
str2.indexOf('short'); // 10
// slice()
const str3 = 'pizza, orange, cereals'
str3.slice(0,5) // 'pizza'
// toUppercase() / toLowerCase()
const str4 = 'lowercase'
str4.toUpperCase(); // 'LOWERCASE'
const str5 = 'UPPERCASE'
str5.toLowerCase() // 'uppercase'
```

There are many more methods, these were just a few as a reminder.

&nbsp;

## Additional string methods

ES6 intoduced 4 new string methods:

- `startsWith()`
- `endsWith()`
- `includes()`
- `repeat()`

&nbsp;

### `startsWith()`

This new method will check if the string starts with the value we pass in:

```js
const code = "ABCDEFG";

code.startsWith("ABB");
// false
code.startsWith("abc");
// false, startsWith is case sensitive
code.startsWith("ABC");
// true
```

We can pass an additional parameter, which is the starting point where the method will begin checking.

``` js
const code = "ABCDEFGHI"

code.startsWith("DEF",3);
// true, it will begin checking after 3 characters
```

&nbsp;

### `endsWith()`

Similarly to `startsWith()` this new method will check if the string ends with the value we pass in:

```js
const code = "ABCDEF";

code.endsWith("DDD");
// false
code.endsWith("def");
// false, endsWith is case sensitive
code.endsWith("DEF");
// true

```

We can pass an additional parameter, which is the number of digits we want to consider when checking the ending.

``` js
const code = "ABCDEFGHI"

code.endsWith("EF", 6);
// true, 6 means that we consider only the first 6 values ABCDEF, and yes this string ends with EF therefore we get *true*
```

&nbsp;

### `includes()`

This method will check if our string includes the value we pass in.

```js
const code = "ABCDEF"

code.includes("ABB");
// false
code.includes("abc");
// false, includes is case sensitive
code.includes("CDE");
// true
```

&nbsp;

### `repeat()`

As the name suggests, this new method will repeat what we pass in.

``` js
let hello = "Hi";
console.log(hello.repeat(10));
// "HiHiHiHiHiHiHiHiHiHi"
```