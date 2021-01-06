# Chapter 23: What's new in ES2021

Let's get started with the first of the new ES2021 features:

## String.prototype.replaceAll

`String.replace` is a useful method that allows us to replace a pattern in a string with something else. The problem is that if we want to use a `string` as a pattern and not a RegEx, only the **first** occurrence will get replaced.

```js
const str = "I like my dog, my dog loves me";
const newStr = str.replace("dog", "cat");
newStr;
// "I like my cat, my dog loves me"
```

As the name implies, `String.replaceAll` will do exactly what we need in this scenario, replace all the matching pattern, allowing us to easily replace all mentions of a substring, without the use of RegEx:

```js
const str = "I like my dog, my dog loves me";
const newStr = str.replaceAll("dog", "cat");
newStr;
// "I like my cat, my cat loves me"
```

[Read More](https://github.com/tc39/proposal-string-replaceall)

&nbsp;

## Promise.any

During the past years we've seen new methods such as `Promise.all` and `Promise.race` with ES6, `Promise.allSettled` last year with ES2020 and ES2021 will introduce a new one: `Promise.any`.

I bet you can already tell what it does from the name of the method.

`Promise.any` shorts-circuits once a given promise is fulfilled but will continue until all promises are rejected.

Don't get confused with `Promise.race` because with `race`, the promise short-circuits once one of the given promises resolves **or rejects**.

They have similar behavior for what concerns fulfillment but very different for rejection.

If all the promises inside of `Promise.any` fails, it will throw an `AggregateError` (a subclass of `Error`) containing the rejection reasons of all the promises.

We can use it like this:

```javascript
// example taken from: https://github.com/tc39/proposal-promise-any
Promise.any(promises).then(
  (first) => {
    // Any of the promises was fulfilled.
  },
  (error) => {
    // All of the promises were rejected.
  }
);
```

[Read More](https://github.com/tc39/proposal-promise-any)

&nbsp;

## Logical Operators and Assignment Expressions

With `ES2021` we will be able to combine Logical Operators (`&&`, `||` and `??`) with Assignment Expression (`=`) similarly to how it's already possible to do in Ruby.

If you skipped on `ES2020` you may not be aware of `??` which is the **nullish coalescing** operator. Let's look at an example:

```js
const a = null ?? "test";
// 'test'
const b = 0 ?? "test";
// 0
```

The **nullish coalescing** operator returns the _right_ hand-side when the left-hand-side is `null` or `undefined`, otherwise it returns the _left_ hand-side. In the first example the left-hand-side was `null` thus it returned the right-hand-side while on the second example it returned the left-hand-side because it was neither `null` nor `undefined`.

Moving back to ES2021 stuff, in `JavaScript` we already have many [assignment opeartors](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators#Assignment_operators) like the following basic example:

```js
let a = 0;
a += 2;
// 2
```

But with this new proposal we will be able to do the following:

```js
a ||= b;
// equivalent to a = a || b

c &&= d;
// equivalent to c = c && d

e ??= f;
// equivalent to e = e ?? f
```

Let's go over each one by one:

- `a ||= b` will return `a` if `a` is a truthy value, or `b` if `a` is falsy
- `c &&= d` will return `d` if both `c` and `d` are truthy, or `c` otherwise
- `e ??= f` will return `f` if `e` is `null` or `undefined` otherwise it will return `e`

[Read More](https://github.com/tc39/proposal-logical-assignment)

&nbsp;

## Numeric Separators

The introduction of Numeric Separators will make it easier to read numeric values by using the `_` (underscore) character to provide a separation between groups of digits.

Let's look at more examples:

```js
x = 100_000;
// 100 thousand

dollar = 55_90;
// 55 dollar 90 cents

fraction = 0.000_1;
// 1 thousandth
```

[Read more](https://github.com/tc39/proposal-numeric-separator)

&nbsp;

## WeakRefs

From [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WeakRef#:~:text=A%20weak%20reference%20to%20an%20object%20is%20a%20reference%20that,reclaimed%20by%20the%20garbage%20collector.&text=When%20an%20object%20no%20longer,object%20and%20reclaim%20its%20memory.): A weak reference to an object is a reference that does not prevent the object from being reclaimed by the garbage collector.

With this new proposal for ES2021, we will be able to create weak references to objects with the `WeakRef` class. Please follow the link below to read a more in-depth explanation.

[Read More](https://github.com/tc39/proposal-weakrefs)

&nbsp;

## Intl.ListFormat

The `Intl.ListFormat` object is a constructor for objects that enable language-sensitive list formatting.

Looking at an example is easier than explaining it:

```js
const list = ["Apple", "Orange", "Banana"];

new Intl.ListFormat("en-GB", { style: "long", type: "conjunction" }).format(
  list
);
// Apple, Orange and Banana

new Intl.ListFormat("en-GB", { style: "short", type: "disjunction" }).format(
  list
);
// Apple, Orange or Banana
```

You are not limited to English, let's try with a few different languages:

```js
const list = ["Apple", "Orange", "Banana"];

// Italian
console.log(
  new Intl.ListFormat("it", { style: "long", type: "conjunction" }).format(list)
);
// Apple, Orange e Banana

// Spanish
console.log(
  new Intl.ListFormat("es", { style: "long", type: "conjunction" }).format(list)
);
// Apple, Orange y Banana

// German
console.log(
  new Intl.ListFormat("de", { style: "long", type: "conjunction" }).format(list)
);
// Apple, Orange und Banana
```

Pretty neat uh? For a more detailed look at this specification check out the link below.

[Read More](https://github.com/tc39/proposal-intl-list-format)

&nbsp;

## dateStyle and timeStyle options for Intl.DateTimeFormat

We can use `dateStyle` and `timeStyle` to request a locale-specific date and time of a given length.

```js
// short
new Intl.DateTimeFormat("en", {
  timeStyle: "short",
}).format(Date.now());
// "2:45 PM"

// medium
new Intl.DateTimeFormat("en", {
  timeStyle: "medium",
}).format(Date.now());
// "2:45:53 PM"

// long
new Intl.DateTimeFormat("en", {
  timeStyle: "long",
}).format(Date.now());
// "2:46:05 PM GMT+7"
```

Now let's try with `dateStyle`:

```js
// short
new Intl.DateTimeFormat("en", {
  dateStyle: "short",
}).format(Date.now());
// "7/25/20"

// medium
new Intl.DateTimeFormat("en", {
  dateStyle: "medium",
}).format(Date.now());
// "Jul 25, 2020"

// long
new Intl.DateTimeFormat("en", {
  dateStyle: "long",
}).format(Date.now());
// "July 25, 2020"
```

You can pass whatever locale you want and you can also pass both `dateStyle` and `timeStyle` options at the same time, choosing between the three options of 'short', 'medium', and 'long' that best suit your needs.

[Read More](https://github.com/tc39/proposal-intl-datetime-style)
