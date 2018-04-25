# Chapter 3: Default function arguments

## Default function arguments

ES6 makes it very easy to set default function arguments. Let's look at an example:

``` javascript
function calculatePrice(total, tax = 0.1, tip = 0.05){
// When no value is given for tax or tip, the default 0.1 and 0.05 will be used 
return total + (total * tax) + (total * tip);,
}
```

What if we don't want to pass the parameter at all, like this:

``` javascript
// The 0.15 will be bound to the second argument, tax even if in our intention it was to set 0.15 as the tip
calculatePrice(100, 0.15)
```

We can solve by doing this:

``` javascript
// In this case 0.15 will be bound to the tip
calculatePrice(100, undefined, 0.15)
```

It works, but it's not very nice, how to improve it?

With **destructuring** we can write this:

``` javascript
const Bill = calculatePrice({ tip: 0.15, total:150});
```

We don't even have to pass the parameters in the same order as when we declared our function, since we are calling them the same way as the arguments JavaScript will know how to match them.

Don't worry about destructuring, we will talk about it in a later chapter.