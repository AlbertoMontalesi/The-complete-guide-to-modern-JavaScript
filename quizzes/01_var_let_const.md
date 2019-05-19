## End of Chapter 1 Quiz

### 1.1 What is the correct output of the following code?

```js
var greeting = "Hello";

greeting = "Farewell";

for (var i = 0; i < 2; i++) {
  var greeting = "Good morning";
}

console.log(greeting);
```

- [ ] Hello
- [ ] Good morning
- [ ] Farewell;

&nbsp;

### 1.2 What is the correct output of the following code?

```js
let value = 1;

if(true) {
  let value = 2;
  console.log(value);
}

value = 3;
```

- [ ] 1
- [ ] 2
- [ ] 3

&nbsp;

### 1.3 What is the correct output of the following code?

```js
let x = 100;

if (x > 50) {
  let x = 10;
}

console.log(x);
```

- [ ] 10
- [ ] 100
- [ ] 50

&nbsp;

### 1.4 What is the correct output of the following code?

```js
console.log(constant);

const constant = 1;
```

- [ ] undefined
- [ ] ReferenceError
- [ ] 1