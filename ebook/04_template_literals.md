# Chapter 4: Template literals

Prior to ES6 they were called _template strings_, now we call them _template literals_. Let's have a look at what changed in the way we interpolate strings in ES6.

## Interpolating strings

In ES5 we used to write this, in order to interpolate strings:

```javascript
var name = "Alberto";
var greeting = 'Hello my name is ' + name;

console.log(greeting);
// Hello my name is Alberto
```

In ES6 we can use backticks to make our life easier.

```javascript
let name  = "Alberto";
const greeting = `Hello my name is ${name}`;

console.log(greeting);
// Hello my name is Alberto
```

## Expression interpolations

In ES5 we used to write this:

```javascript
var a = 1;
var b = 10;
console.log('1 * 10 is ' + ( a * b));
// 1 * 10 is 10
```

In ES6 we can use backticks to reduce our typing:

```javascript
var a = 1;
var b = 10;
console.log(`1 * 10 is ${a * b}`);
// 1 * 10 is 10
```

## Create HTML fragments

In ES5 we used to do this to write multi-line strings:

```javascript
// We have to include a backslash on each line
var text = "hello, \
my name is Alberto \
how are you?\ "
```

In ES6 we simply have to wrap everything inside backticks, no more backslash on each line.

```javascript
const content = `hello,
my name is Alberto
how are you?`;
```

## Nesting templates

It's very easy to nest a template inside another one, like this:

```javascript
const markup = `
<ul>
  ${people.map(person => `<li>  ${person.name}</li>`)}
</ul>
`;
```

## Add a ternary operator

We can easily add some logic inside our template string by using a ternary operator:

```javascript
// create an artist with name and age
const artist = {
  name: "Bon Jovi",
  age: 56,
};

// only if the artist object has a song property we then add it to our paragraph, otherwise we return nothing
const text = `
  <div>
    <p>  ${artist.name} is ${artist.age} years old ${artist.song ? `and wrote the song ${artist.song}` : '' }
    </p>
  </div>
`
```

## Pass a function inside a template literal

Similarly to the example above, if we want to, we can pass a function inside a template literal.

```javascript
const groceries = {
  meat: "pork chop",
  veggie: "salad",
  fruit: "apple",
  others: ['mushrooms', 'instant noodles', 'instant soup'],
}

// this function will map each individual value of our groceries
function groceryList(others) {
  return `
    <p> 
      ${others.map( other => ` <span> ${other}</span>`).join(' ')}
    </p>
  `;
}

// display all our groceries in a p tag, the last one will include all the one from the array **others**
const markup = `
  <div>
    <p> ${groceries.meat} </p>
    <p> ${groceries.veggie} </p>
    <p> ${groceries.fruit} </p>
    <p>${groceryList(groceries.others)} </p>
  <div>
`
```

## Tagged template literals

By tagging a function to a template literal we can run the template literal through the function, providing it with everything that's inside of the template.

The way it works is very simple: you just take the name of your function and put it in front of the template that you want to run it against.

```javascript
let person = "Alberto";
let age = 25;

function myTag(strings,personName,personAge){
  let str = strings[1];
  let ageStr;

  personAge > 50 ? ageStr = "grandpa" : ageStr = "youngster";

  return personName + str + ageStr;
}

let sentence = myTag`${person} is a ${age}`;
console.log(sentence);
// Alberto is a youngster
```

We captured the value of the variable age and used a ternary operator to decide what to print. `strings` will take all the strings of our `let` sentence whilst the other parameters will hold the variables.

To learn more about use cases of _template literals_ check out [this article](https://codeburst.io/javascript-es6-tagged-template-literals-a45c26e54761).

