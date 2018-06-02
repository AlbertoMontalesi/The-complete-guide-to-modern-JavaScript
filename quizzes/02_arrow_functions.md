# Chapter 2: Arrow functions

## End of Chapter 2 Quiz

### 2.1 What is the correct syntax for an arrow function?

```js
let arr = [1,2,3];

//a)
let func = arr.map(n -> n+1);

//b)
let func = arr.map(n => n+1);

//c)
let func = arr.map(n ~> n+1);
```

- [ ] a
- [ ] b
- [ ] c

&nbsp;

### 2.2 What is the correct output of the following code?

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

- [ ] 10
- [ ] 11
- [ ] undefined

&nbsp;

### 2.3 Refactor the following code to use the arrow function syntax:

```js
function(arg) {
  console.log(arg);
}
```