# Chapter 10: Object literal upgrades

In this article we will look at the many upgrades brought by ES6 to the Object literal notation.

## Deconstructing variables into keys and values

This is our initial situation:

```javascript
const name = "Alberto";
const surname = "Montalesi";
const age = 25;
const nationality = "Italian";
```

Now if we wanted to create an object literal this is what we would usually do:

```javascript
const person = {
  name: name,
  surname: surname,
  age: age,
  nationality: nationality,
}
```

In ES6 we can simplify like this:

```javascript
const person = {
  name,
  surname,
  age,
  nationality,
}
console.log(person);
// {name: "Alberto", surname: "Montalesi", age: 25, nationality: "Italian"}
```

As our `const` is named the same way as the properties we are using we can reduce our typing by a lot.

## Add functions to our Objects

Let's looks at an example from ES5:

```javascript
const person = {
  name: "Alberto",
  greet: function(){
    console.log("Hello");
  },
}
```

If we wanted to add a function to our Object we had to use the the `function` keyword. In ES6 it got easier, look here:

```javascript
const person = {
  name: "Alberto",
  greet(){
    console.log("Hello");
  },
}

person.greet();
// Hello;
```

No more `function`, it's shorter and it does the same.

**Remember** that **arrow functions** are anonymous, look at this example:

```javascript
// arrow functions are anonymous, in this case you need to have a key
const person1 = {
  () => console.log("Hello"),
};

const person2 = {
  greet: () => console.log("Hello"),
}
```

## Dynamically define properties of an Object

This is how we would dynamically define properties of an Object in ES5:

```javascript
var name = "name";
// create empty object
var person = {}
// update the object
person[name] = "Alberto";
console.log(person.name);
// Alberto
```

First we created the Object and then we modified it.

In ES6 we can do both things at the same time, look here:

```javascript
const name = "name";
const person = {
};
console.log(person.name);
// Alberto
```

