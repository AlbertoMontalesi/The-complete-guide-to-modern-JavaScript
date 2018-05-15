# Chapter 12: Classes

Quoting MDN:
> classes are primarily syntactical sugar over js's existing prototype-based inheritance. The class syntax **does not** introduce a new object-oriented inheritance model to JavaScript.


That being said, let's review prototypal inheritance before we jump into classes.

``` js
function Person(name,age) {
  this.name = name;
  this.age = age;
}

Person.prototype.greet = function(){
  console.log("Hello, my name is " + this.name);
}

const alberto = new Person("Alberto", 25);
const caroline = new Person("Caroline",25);

alberto.greet();
// Hello, my name is Alberto
caroline.greet();
// Hello, my name is Caroline
```

We added a new method to the prototype in order to make it accessible to all the new instances of Person that we created.

Ok, now that I refreshed your knowledge of prototypal inheritance, let's have a look at classes.

&nbsp;

## Create a `Class`

There are two way of creating a class:

- class declaration
- class expression


``` js
// class declaration
class Person {

}

// class expression

const person = class Person {

}
```

>Remember: class declaration (and expression) are **not hoisted** which means that unless you want to get a **ReferenceError** you need to declare your class before you access it.

Let's start creating our first `Class`.

We only need a method called `constructor` (remember to add only one constructor, a `SyntaxError` will be thrown if the class contains more than one constructor methods).

``` js
class Person {
  constructor(name,age){
    this.name = name;
    this.age = age;
  }
  greet(){
    console.log(`Hello, my name is ${this.name} and I am ${this.age} years old` );
  } // no commas in between methods
  farewell(){
    console.log("goodbye friend");
  }
}

const alberto = new Person("Alberto",25);

alberto.greet();
// Hello, my name is Alberto and I am 25 years old
alberto.farewell();
// goodbye friend
```

As you can see everything works just like before. As we mentioned at the beginning, Classes are just a syntactical sugar, a nicer way of doing inheritance.

&nbsp;

## Static methods

Right now the two new methods that we created, `greet()` and `farewell()` can be accessed by every new instance of `Person`, but what if we want a method that can only be accessed by the class itself, similarily to `Array.of()` for arrays?

```js
static info(){
  console.log("I am a Person class, nice to meet you");
}

alberto.info();
// TypeError: alberto.info is not a function

Person.info();
// I am a Person class, nice to meet you
```

&nbsp;

## `set` and `get`

We use setter and getter methods to set and get values inside our `Class`.

```js
class Person {
  constructor(name,surname) {
    this.name = name;
    this.surname = surname;
    this.nickname = "";
  }
  set nicknames(value){
    this.nickname = value;
  }
  get nicknames(){
    return `Your nickname is ${this.nickname}`;
  }
}

const alberto = new Person("Alberto","Montalesi");

// first we call the setter
alberto.nicknames = "Albi";
// "Albi"

// then we call the getter
alberto.nicknames;
// "Your nickname is Albi"
```

&nbsp;

## Extending our `Class`

What if we want to have a new `Class` that inherits from our previous one? We use `extends`:


``` js
// our initial class
class Person {
  constructor(name,age){
    this.name = name;
    this.age = age;
  }
  greet(){
    console.log(`Hello, my name is ${this.name} and I am ${this.age} years old` );
  }
}


// our new class
class Adult extends Person {
  constructor(name,age,work){
    this.name = name;
    this.age = age;
    this.work = work;
  }
}

const alberto = new Adult("Alberto",25,"teacher");
```

We created a new `Class Adult` that inherits from `Person` but if you try to run this code you will get an error:

```js
ReferenceError: must call super constructor before using |this| in Adult class constructor
```

The error message tells us to call `super()` before using `this` in our new `Class`.
What it means is that we basically have to create a new Person before we create a new Adult and the `super()` constructor will do exactly that.

``` js
class Adult extends Person {
  constructor(name,age,work){
    super(name,age);
    this.work = work;
  }
}
```

Why did we set `super(name,age)` ? Because our `Adult` class inherits name and age from the `Person` therefore we don't need to redeclare them. 
Super will simply create a new Person for us.

If we now run the code again we will get this:

``` js
alberto.age
// 25
alberto.work
// "teacher"
alberto.greet();
// Hello, my name is Alberto and I am 25 years old
```

As you can see our `Adult` inherited all the properties and methods from the `Person` class.

&nbsp;

## Extending Arrays

We want to create something like this, something similar to an array where the first value is a property to define our Classroom and the rest are our students and their marks.

``` js
// we create a new Classroom
const myClass = new Classroom('1A', 
  {name: "Tim", mark: 6},
  {name: "Tom", mark: 3},
  {name: "Jim", mark: 8},
  {name: "Jon", mark: 10},
);
```

What we can do is create a new `Class` that extends the array.

``` js
class Classroom extends Array {
  // we use rest operator to grab all the students
  constructor(name, ...students){
    // we use spread to place all the students in the array individually otherwise we would push an array into an array
    super(...students);
    this.name = name;
    // we create a new method to add students
  } add(student){
    this.push(student);
  }
}
const myClass = new Classroom('1A', 
  {name: "Tim", mark: 6},
  {name: "Tom", mark: 3},
  {name: "Jim", mark: 8},
  {name: "Jon", mark: 10},
);

// now we can call
myClass.add({name: "Timmy", mark:7});
myClass[4];
// Object { name: "Timmy", mark: 7 }

// we can also loop over with for of
for(const student of myClass) {
  console.log(student);
  }
// Object { name: "Tim", grade: 6 }
// Object { name: "Tom", grade: 3 }
// Object { name: "Jim", grade: 8 }
// Object { name: "Jon", grade: 10 }
// Object { name: "Timmy", grade: 7 }
```

