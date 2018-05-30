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

Proxies can be very useful, for example if your object is a phone number.

You can take the value given by the user and format it to match the standard formatting of your country.