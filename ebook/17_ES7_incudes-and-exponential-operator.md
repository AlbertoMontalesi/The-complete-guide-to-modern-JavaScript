# Chapter 17: Everything new in ES7 (ES2016)



ES7 (or ES 2016) introduced only two new features :

- `Array.prototype.includes()`
- the exponential operator

&nbsp;

## `Array.prototype.includes()`

The `includes()` method will return `true` if our array includes a certain element, or `false` if it doesn't.

```js
let array = [1,2,4,5];

array.includes(2);
// true
array.includes(3);
// false
```
&nbsp;

### Combine `includes()` with `fromIndex`

We can provide `.includes()` with an index where to begin searching for an element. Default is 0, but we can also pass a negative value.

The first value we pass is the element to search and the second one is the index:

``` js
let array = [1,3,5,7,9,11];

array.includes(3,1);
// true
array.includes(5,4);
//false
array.includes(1,-1);
// false
array.includes(11,-3);
// true
```

`array.includes(5,4);` returned `false` because, despite the array actually contains the number 5, it is at the index 2 but we started looking at position 4. That's why we couldn't find it and it returned `false`.

`array.includes(1,-1);` returned `false` because we started looking at the index -1 (which is the last element of the array) and then continued from that point onwards.

`array.includes(11,-3);` returned `true` because we went back to the index -3 and moved up, finding the value 11 on our path.

&nbsp;

## The exponential operator

Prior to ES7 we would have done this:

``` js
Math.pow(2,2);
// 4
Math.pow(2,3);
// 8
```

Now with the new exponential operator we can do this:

```js
2**2;
// 4
2**3;
// 4
```

It will get pretty useful when combining multiple operations like in this example:

``` js
2**2**2;
// 16
Math.pow(Math.pow(2,2),2);
// 16
```

Using `Math.pow()` you need to continuously concatenate them and it can get pretty long and messy. The exponential operator provides a faster and cleaner way of doing the same thing.
