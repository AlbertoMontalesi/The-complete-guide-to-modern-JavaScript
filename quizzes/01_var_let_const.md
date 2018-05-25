# Chapter 1: Var vs Let vs Const & the temporal dead zone

## End of chapter quiz

1) What is the correct output of this code?

```js
var greeting = "Hello";

greeting = "Farewell";

for (var i = 0; i < 2; i++) {
  var greeting = "Goodmorning";
}

console.log(greeting);
```

- [] Hello
- [*] Goodmorning
- [] Farewell;

&nbsp;

2) What is the correct output of this code?


```js
let value = 1;

if(true) {
  let value = 2;
  console.log(value);
}

value = 3;
```

- [] 1
- [*] 2
- [] 3


3) What is the correct output of this code?

```js
console.log(constant);

const constant = 1;
```

- [] undefined
- [*] ReferenceError
- 1

