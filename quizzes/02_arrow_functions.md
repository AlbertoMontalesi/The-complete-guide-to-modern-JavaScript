# Chapter 2: Arrow functions

## End of chapter quiz

1) What is the correct syntax for an arrow function?

```js
let arr = [1,2,3];

a)
let func = arr.map(n -> n+1);

b)
let func = arr.map(n => n+1);

c)
let func = arr.map(n ~> n+1);
```

- [] a
- [] b
- [*] c


> The correct syntax for an arrow function is the so called fat arrow `=>`

&nbsp;


// example with this keyword

2) What is the correct output of this code?

``` js
const person = {
  age: 10,
  grow: () => {
    this.age++;
  },
}
person.grow();

console.log(person.age);
```

- [*] 10
- [] 11
- [] undefined

> Inside an arrow function, the `this` keyword inherits from its parent scope which in this case it's the window, therefore the `age` remains 10.

