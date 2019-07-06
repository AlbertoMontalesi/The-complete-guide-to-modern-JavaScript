# Chapter 15: Proxies

## What is a Proxy?

From MDN:

> the Proxy object is used to define custom behavior for fundamental operations (e.g. property lookup, assignment, enumeration, function invocation, etc).

&nbsp;

## How to use a `Proxy` ?

This is how we create a Proxy:

``` js
var x = new Proxy(target,handler)
```

- our `target` can be anything, from an object, to a function, to another `Proxy`
- a `handler` is an object which will define the behavior of our `Proxy` when an operation is performed on it

``` js
// our object
const dog = { breed: "German Shephard", age: 5}

// our Proxy
const dogProxy = new Proxy(dog, {
  get(target,breed){
    return target[breed].toUpperCase();
  },
  set(target, breed, value){
    console.log("changing breed to...");
    target[breed] = value;
  }
});

dogProxy.breed;
// "GERMAN SHEPHARD"
dogProxy.breed = "Labrador";
// changing breed to... 
// "Labrador"
dogProxy.breed;
// "LABRADOR"
```

When we call the `get` method we step inside the normal flow and change the value of the breed to uppercase.

When setting a new value we step in again and log a short message before setting the value.

Proxies can be very useful for example to validate data. Look at this example.

```js
const validateAge = {
  set: function(object,property,value){
    if(property === 'age'){
      if(value < 18){
        throw new Error('you are too young!')
      }
    } else {
      // default behaviour
      object.property = value;
      return true
    }
  }
};

const user =  new Proxy({},validateAge)

user.age = 17
// Uncaught Error: you are too young!

user.age = 21
// 21
```

When we set the `age` property of the `user Object` we pass it through our `validateAge` function which checks if it is more or less than 18 and throws an error if it's less than 18.
