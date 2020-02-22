# Chapter 6: Destructuring

MDN defines **destructuring** like this:

> The **destructuring** assignment syntax is a `JavaScript` expression that makes it possible to unpack values from arrays, or properties from objects, into distinct variables.

Let's start with **destructuring objects** first.

&nbsp;

## Destructuring Objects

To create variables from an object, we used to do the following:

```javascript
var person  = {
  first: "Alberto",
  last: "Montalesi"
}

var first = person.first;
var last = person.last;
```

In ES6 we can instead do it as following:

```javascript
const person = {
  first: "Alberto",
  last: "Montalesi"
}

const { first, last } = person;
```

Since our `const` variable- `person`-  has the same name as the properties we want to grab, we don't have to specify `person.first` and `person.last` anymore.

The same applies even when we have nested data, such as what we could get from an API.

```javascript
const person = {
  name: "Alberto",
  last: "Montalesi",
  links:{
    social: {
      facebook: "https://www.facebook.com/alberto.montalesi",
    },
    website: "http://albertomontalesi.github.io/"
  }
}

const { facebook } = person.links.social;
```

We're not limited to name our variables the same as the property of the object. We can also rename as the following:

```javascript
const { facebook:fb } = person.links.social;
// it will look for the property person.links.social.facebook and name the variable fb
console.log(fb); // https://www.facebook.com/alberto.montalesi
console.log(facebook); //ReferenceError: facebook is not defined
```

We're using the syntax `const { facebook:fb }` to specify that we want to target the property `facebook` of the object `person.links.social`, and that we want the `const` variable to be called `fb`.
That is why when we try to log `facebook` we get an error.

We can also pass in **default values** like this:

```javascript
const { facebook:fb = "https://www.facebook.com"} = person.links.social;
// we renamed the variable to *fb* and we also set a default value to it
```

&nbsp;

## Destructuring Arrays

The first difference we notice when **destructuring arrays** is that we're going to use `[]` and not `{}`.

```javascript
const person = ["Alberto","Montalesi",25];
const [name,surname,age] = person;
```

What if the number of variables that we create is less than the elements in the array?

```javascript
const person = ["Alberto","Montalesi",25];
// we leave out age, we don't want it
const [name,surname] = person;
//the value of age will not be bound to any variable.
console.log(name,surname);
// Alberto Montalesi
```

Let's say we want to grab all the other values remaining. We can use the **rest operator** for that:

```javascript
const person = ["Alberto", "Montalesi", "pizza", "ice cream", "cheese cake"];
// we use the **rest operator** to grab all the remaining values
const [name,surname,...food] = person ;
console.log(food);
// Array [ "pizza", "ice cream", "cheese cake" ]
```

In the example above, the first two values of the array were bound to `name` and `surname`, while the rest (that's why it's called the **rest operator**) get assigned to `food`

The `...` is the syntax for the **rest operator**.

&nbsp;

## Swapping variables with destructuring

The destructuring assignment makes it **extremely easy** to swap variables, just look at this example:

```javascript
let hungry = "yes";
let full = "no";
// after we eat we don't feel hungry anymore, we feel full, let's swap the values

[hungry, full] = [full, hungry];
console.log(hungry,full);
// no yes
```

It can't get easier than this to swap values.

&nbsp;
