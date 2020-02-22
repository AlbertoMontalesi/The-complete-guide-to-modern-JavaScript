# Chapter 5: Additional string methods

There are many methods that we can use against strings. Here's a list of a few of them:

1. `indexOf()`

Gets the position of the first occurrence of the specified value in a string.

```javascript
const str = "this is a short sentence";
str.indexOf("short");
// Output: 10
```

2. `slice()`

Pulls a specified part of a string as a new string.

```javascript
const str = "pizza, orange, cereals"
str.slice(0, 5);
// Output: "pizza"
```

3. `toUpperCase()`

Turns all characters of a string to uppercase.

```javascript
const str = "i ate an apple"
str.toUpperCase()
// Output: "I ATE AN APPLE"
```

4. `toLowerCase()`

Turns all characters of a string to lowercase.

```javascript
const str = "I ATE AN APPLE"
str.toLowerCase()
// Output: "i ate an apple"
```

There are many more methods- these were just a few as a reminder. Check the [MDN documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String#) for a more in-depth description of the above methods.

&nbsp;

## Additional string methods

ES6 introduced 4 new string methods:

- `startsWith()`
- `endsWith()`
- `includes()`
- `repeat()`

&nbsp;

### `startsWith()`

This new method will check if the string starts with the value we pass in:

```javascript
const code = "ABCDEFG";

code.startsWith("ABB");
// false
code.startsWith("abc");
// false, startsWith is case sensitive
code.startsWith("ABC");
// true
```

We can pass an additional parameter, which is the starting point where the method will begin checking.

```javascript
const code = "ABCDEFGHI"

code.startsWith("DEF",3);
// true, it will begin checking after 3 characters
```

&nbsp;

### `endsWith()`

Similarly to `startsWith()`, this new method will check if the string ends with the value we pass in:

```javascript
const code = "ABCDEF";

code.endsWith("DDD");
// false
code.endsWith("def");
// false, endsWith is case sensitive
code.endsWith("DEF");
// true

```

We can pass an additional parameter, which is the number of digits we want to consider when checking the ending.

```javascript
const code = "ABCDEFGHI"

code.endsWith("EF", 6);
// true, 6 means that we consider only the first 6 values ABCDEF, and yes this string ends with EF therefore we get *true*
```

&nbsp;

### `includes()`

This method will check if our string includes the value we pass in.

```javascript
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

As the name suggests, this new method will take an argument that specifies the number of times it needs to repeat the `string`.

```javascript
let hello = "Hi";
console.log(hello.repeat(10));
// "HiHiHiHiHiHiHiHiHiHi"
```
