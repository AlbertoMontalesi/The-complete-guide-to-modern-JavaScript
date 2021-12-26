# Chapter 24: What's new in ES2022

Let's get started with the first of the new ES2022 features:

## Class Fields

### Class public Instance Fields & private Instance Fields

Before ES2022 we would define properties of a `class` in its `constructor` like this:

```js
class ButtonToggle extends HTMLElement {
    constructor(){
        super();
        // public field
        this.color = 'green'
        // private field
        this._value = true;
    }

    toggle(){
        this.value = !this.value
    }
}

const button = new ButtonToggle();
console.log(button.color);
// green - public fields are accessible from outside classes

button._value = false;
console.log(button._value);
// false - no error thrown, we can access it from outside the class
```

Inside of the `constructor`, we defined two fields. As you can see one of them is marked with an `_` in front of the name which is just a `JavaScript` naming convention to declare the field as `private` meaning that it can only be accessed from inside of a `class` method. Of course, that's just a naming convention and not something that the language itself enforces and that's why when we tried to access it, it didn't raise any error.

In ES2022 we have an easier way to declare both `public` and `private` fields. Let's have a look at this updated example:

```js
class ButtonToggle extends HTMLElement {
   
    color = 'green';
    #value = true;

    toggle(){
        this.#value = !this.#value;
    }
}
const button = new ButtonToggle();
console.log(button.color);
// green - public fields are accessible from outside classes

// SyntaxError - cannot be accessed or modified from outside the class
console.log(button.#value); 
button.#value = false;
```

The first thing to notice is that don't have to define them inside of the `constructor`. Secondly, we can also define `private` fields by pre-pending `#` to their names.

The main difference with the previous example is that this time an actual error will be thrown if we try to access or modify the field outside of the class.

&nbsp;

### Private methods and getter/setters for JavaScript classes

Similar to how we did in the previous example, we can also define `private` methods and getter/setters for our classes.

```js
class ButtonToggle extends HTMLElement {
   
    color = 'green'
    #value = true;

    #toggle(){
        this.#value = !this.#value
    }

    set #setFalseValue(){
        this.#value = false;
    }
}
const button = new ButtonToggle();
// SyntaxError - cannot be accessed or modified from outside the class
button.#toggle();
// SyntaxError - cannot be accessed or modified from outside the class
button.#setFalseValue;
```

In the example above we replaced `toggle()` with `#toggle()` thus making the `toggle` method `private` and only accessible from inside of the `class`.

### Static class fields and private static methods

A `static` field or method is only accessible in the prototype and not in every instance of a `class` and ES2022 provides us with the means to define `static` fields and `static` public/private methods by using the `static` keyword.

Previously we would have to define them outside of the `class` body such as:

```js
class ButtonToggle extends HTMLElement {
    // ... class body
}
ButtonToggle.toggle(){
    // static method define outside of the class body
}
```

Now, instead, we can define them directly inside of the `class` body with the use of the `static` keyword:

```js
class ButtonToggle extends HTMLElement {
   
    static #value = true;

    static toggle(){
        this.#value = !this.#value
    }
}
// this will work
ButtonToggle.toggle();

// SyntaxError - private static field
const button = new ButtonToggle();
button.toggle();
```

As you can see in the example above, we can access `toggle()` directly on our `ButtonToggle` but we cannot do the same on a new instance of it.

We can use the `static` keyword in front of fields and methods (both private and public) and by combining it with the `#` (`private`) we can create a `private static` method only accessible from inside of our prototype `class`.

```js
class ButtonToggle extends HTMLElement {
   
    static #value = true;

    static #toggle(){
        this.#value = !this.#value
    }
}
// this will error, it's a private static method
ButtonToggle.#toggle();
```

&nbsp;

## Ergonomic brand checks for private Fields

As we saw in the examples above, if we try to access a `private` field outside of a `class` it will throw an exception and will not return `undefined` like it does with `public` fields.

We could try using a simple `try/catch` inside of the `class` to check if the field exists:

```js
class ButtonToggle extends HTMLElement {
   
   // initialised as null
    #value = null;

    get #getValue(){
        if(!this.#value){
            throw new Error('no value');
        } 
        return this.#value
    }

    static isButtonToggle(obj){
        try {
            obj.#getValue;
            return true;
        } catch {
            // could be an error internal to the getter
            return false; 
        }
    }

}
```

In the example above we added a `private` `getter` that will throw an error if there is no value yet. We then created a `static` method to access that `getter` and tried to determine if it exists by checking with a `try/catch`. The problem lies in the fact that we don't know if the code in the `catch` is executed because the `getter` is not present or simply because it threw an error.

ES2022 provides us with an easy way to check if said field belongs to a `class` by using the operator `in`. Let's rework our example code:

```js
class ButtonToggle extends HTMLElement {
   
   // initialised as null
    value = null;

    get #getValue(){
        if(!this.#value){
            throw new Error('no value');
        } 
        return this.#value;
    }

    static isButtonToggle(obj){
       return #value in obj && #getValue in obj
    }

}
```

Our method `isButtonToggle` will check if the `class` contains the `private` fields 'value' and 'getValue'.

&nbsp;

## Class Static Block

This is yet another upgrade to the `static` fields in ES2022 that allows us to have `static` blocks inside of classes. The issue this is trying to solve arises from the fact that we cannot evaluate statements such as a `try/catch` during initialization meaning that we would have to put that code **outside** of the `class` body:

```js
class ButtonToggle{
    value = false;

    get getValue(){
        if(!this.#value){
            throw new Error('no value');
        } 
        return this.#value
    }
}

// this has to sit outside of the class body
try {
    const val = ButtonToggle.getValue;
    ButtonToggle.value = val
} catch {
    ButtonToggle.value = false
}
```

As you can see, our `try/catch` had to be put outside of the `class` body. Thankfully we can replace that with a `static` block like the following:

```js
// method defined outside of the class body
let initVal;

class ButtonToggle{
    #value = false;

    get getValue(){
        if(!this.#value){
            throw new Error('no value');
        } 
        return this.#value
    }

    static {
        initVal = () => {
            this.#value = this.getValue;
        }
    }
}

initVal();
```

We created a `static` block inside of our `class` that defines a function that we declared outside of the context of that `class`. As you can see, the method will have access to '#value' which is a `private` field or our class. They will have access to `private` methods and fields, being them `instance-private` (meaning non `static`, `private` fields) or `static-private`.

## RegExp Match Indices

This upgrade will allow us to use the `d` character to specify that we want to get the indices (starting and ending) of the matches of our RegExp.

We can use `Regexp.exec` or `String.matchAll` to find a list of matches, with the main difference between them being that `Regexp.exec` returns its results one by one whereas `String.matchAll` returns an iterator. Let's see them in practice:

```js
const fruits = 'Fruits: mango, mangosteen, orange'
const regex = /(mango)/g;

// .exec
RegExp(regex).exec(fruits);
// [
//   'mango',
//   index: 8,
//   input: 'Fruits: mango, mangosteen, orange',
//   groups: undefined
// ]

// matchAll
const matches = [...fruits.matchAll(regex)];
matches[0];
// [
//   'mango',
//   'mango',
//   index: 8,
//   input: 'Fruits: mango, mangosteen, orange',
//   groups: undefined
// ]
```

Both return the index of the match, the match itself, and the initial input. What we don't know are the indices at which the string ends, something that we will now be able to do like this:

```js
const fruits = 'Fruits: mango, mangosteen, orange'
// /gd instead of the previous /g
const regex = /(mango)/gd;

const matches = [...fruits.matchAll(regex)];
matches[0];

// [
// "mango",
// "mango",
// groups: undefined
// index: 8
// indices:[]
//  [8, 13],
//  [8, 13]
// ]
// groups: undefined
```

As you can see it returned [8,13] as the indices of the first occurrence of 'mango' in our string.]

&nbsp;
## Top-level await

"`await` operator can only be used within an `async` method" is probably an error you have encountered frequently. In ES2022 we will be able to use it outside of the context of an `async` method in our modules. For example, we could defer the execution of a module and its parent until something else is imported.

This can be useful in many scenarios, for example when we have a **dynamic path** for a dependency that depends on a runtime value:

```js
// we need to get the appropriate translation keys based on the language
const translationKeys = await import(`/i18n/${navigator.language}`);
```

Another use could be to provide a fallback for a dependency:

```js
let jQuery;
try {
  jQuery = await import('https://cdn-a.com/jQuery');
} catch {
  jQuery = await import('https://cdn-b.com/jQuery');
}
```

## .at()

In `JavaScript` you can do `arr[1]` to access the value at index 1 of an `Array` but you cannot do `arr[-1]` to count backward from the ending of the `Array`. The reason is that the brackets syntax is used not only for arrays but also for Objects, where `obj[-1]` would simply refer to the property '-1' of that `Object`.

With the .`at()` method we now have an easy way to access any index, positive or negative of arrays and strings:

```js
const arr = [10,20,30,40];

// same -> 10
arr[1];
arr.at(1);

// same -> 40
arr[arr.length -1];
arr.at(-1);
```

Note that a negative value simply means: 'Start counting backward from the end of the array'.
&nbsp;

## Accessible Object.prototype.hasOwnProperty

In `JavaScript` we already have an `Object.prototype.hasOwnProperty` but, as the MDN documentation also suggests, it's best to not use `hasOwnProperty` outside the prototype itself as it is not a protected property, meaning that an `object` could have its property called `hasOwnProperty` that has nothing to do with `Object.prototype.hasOwnProperty`.

For example:

```js
const obj = {
    hasOwnProperty:()=> {
        return false
    }
}

obj.hasOwnProperty('prop'); // false
```

As you can see, we defined our own method `hasOwnProperty` that has overridden the one on the prototype, an issue that is not present with `Object.hasOwn()`.

`Object.hasOwn()` takes our `Object` as the first argument and the property we want to check as the second:

```js
const student = {
    name: 'Mark',
    age: 18
}

Object.hasOwn(student,'age'); // true
Object.hasOwn(student,'grade'); // false
```
