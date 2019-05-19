# Chapter 13: Promises

## What is a Promise?

From MDN:
> A Promise is an object representing the eventual completion or failure of an asynchronous operation.

JavaScript works almost entirely asynchronously which means that when we are retrieving something from an API, for example, our code won't stop executing. Look at this example to understand what is going to happen:

```js
const data = fetch('your-api-url-goes-here');
console.log('Finished');
console.log(data);
```

The code won't stop once it hits the fetch, therefore our next `console.log` will be executed before we actually get some value in return, meaning that the `console.log(data)` will be empty.

To avoid this we would use **callbacks** or **promises**.

&nbsp;

### Callback hell

You may have heard of something called **callback hell** which looks roughly like this:

``` js
fs.readdir(source, function (err, files) {
  if (err) {
    console.log('Error finding files: ' + err)
  } else {
    files.forEach(function (filename, fileIndex) {
      console.log(filename)
      gm(source + filename).size(function (err, values) {
        if (err) {
          console.log('Error identifying file size: ' + err)
        } else {
          console.log(filename + ' : ' + values)
          aspect = (values.width / values.height)
          widths.forEach(function (width, widthIndex) {
            height = Math.round(width / aspect)
            console.log('resizing ' + filename + 'to ' + height + 'x' + height)
            this.resize(width, height).write(dest + 'w' + width + '_' + filename, function(err) {
              if (err) console.log('Error writing file: ' + err)
            })
          }.bind(this))
        }
      })
    })
  }
})
```

We try to write our code in a way where executions happens visually from top to bottom, causing excessive nesting on functions and result in what you can see above.

To improve your callbacks you can check out http://callbackhell.com/

Here we will focus on how to write promises.

&nbsp;

## Create your own promise

```js
const myPromise = new Promise((resolve, reject) => {
  // your code goes here
});
```

This is how you create your own promise, `resolve` and `reject` will be called once the promise is finished.

We can immediately return it to see what we would get:

```js
const myPromise = new Promise((resolve, reject) => {
  resolve("The value we get from the promise");
});

myPromise.then(
  data => {
    console.log(data);
  });
// The value we get from the promise
```

We immediately resolved our promise and can see the result in the console.

We can combine a `setTimeout()` to wait a certain amount of time before resolving.

```js
const myPromise = new Promise((resolve, reject) => {
  setTimeout(() => {
      resolve("The value we get from the promise");
    }, 2000);
});

myPromise.then(
  data => {
    console.log(data);
  });
// after 2 seconds ...
// The value we get from the promise
```

These two examples are very simple but **promises** are very useful when dealing with big requests of data.

In the example above we kept it simple and only resolved our promise but in reality you will also encounter errors so let's see how to deal with them:

``` js
const myPromise = new Promise((resolve, reject) => {
  setTimeout(() => {
      reject(Error("this is our error"));
    }, 2000);
});

myPromise
  .then(data => {
    console.log(data);
  })
  .catch(err => {
    console.error(err);
  })
  // Error: this is our error
  // Stack trace:
  // myPromise</<@debugger eval code:3:14
```

We use `.then()` to grab the value when the promise resolves and `.catch()` when the promise rejects.

If you see our error log you can see that it tells us where the error occured, that is because we wrote `reject(Error("this is our error"));` and not simply `reject("this is our error");`.

&nbsp;

### Chaining promises

We can chain promises one after the other, using what was returned from the previous one as the base for the subsequent one, whether the promise resolved or got rejected.

``` js
const myPromise = new Promise((resolve, reject) => {
  resolve();
});

myPromise
  .then(data => {
    // take the data returned and call a function on it
    return doSomething(data);
  })
  .then(data => {
    // log the data that we got from the previous promise
    console.log(data);
  })
  .catch(err => {
    console.error(err);
  })
```

We called a function (it can do whatever you want, in this case it does nothing) and we passed the value down to the next step where we logged it.

You can chain as many promises as you want and the code will still be more readable and shorter than what we have seen above in the **callback hell**.

We are not limited to chaining in case of success, we can also chain when we get a `reject`.

```js
const myPromise = new Promise((resolve, reject) => {
  resolve();
});

myPromise
  .then(data => {
    throw new Error("ooops");

    console.log("first value");
  })
  .catch(() => {
    console.log("catch an error");
  })
  .then(data => {
  console.log("second value");
  });
  // catch an error
  // second value
```

We did not get "first value" because we threw an error therefore we only got the first `.catch()` and the last `.then()`.

&nbsp;

### `Promise.resolve()` & `Promise.reject()`

`Promise.resolve(`) and `Promise.reject()` will create promises that automatically resolve or reject.

```js
//Promise.resolve()
Promise.resolve('Success').then(function(value) {
  console.log(value); 
  // "Success"
}, function(value) {
  // not called
});

// Promise.reject()
Promise.reject(new Error('fail')).then(function() {
  // not called
}, function(error) {
  console.log(error);
  // Error: fail
});
```

&nbsp;

### `Promise.all()` & `Promise.race()`

`Promise.all()` returns a single Promise that resolves when all promises have resolved.

Let's look at this example where we have two promises.

```js
const promise1 =  new Promise((resolve,reject) => {
  setTimeout(resolve, 500, 'first value');
});
const promise2 =  new Promise((resolve,reject) => {
  setTimeout(resolve, 1000, 'second value');
});

promise1.then(data => {
  console.log(data);
});
// after 500 ms
// first value
promise2.then(data => {
  console.log(data);
});
// after 1000 ms
// second value
```

They will resolve independently from one another but look at what happens when we use `Promise.all().`

```js
Promise
  .all([promise1, promise2])
  .then(data => {
    const[promise1data, promise2data] = data;
    console.log(promise1data, promise2data);
  });
// after 1000 ms
// first value second value
```

Our values returned together, after 1000ms (the timeout of the *second* promise) meaning that the first one had to wait the completion of the second one.

If we were to pass an empty iterable then it will return an already resolved promise.

If one of the promise was rejected, all of them would asynchronously reject with the value of that rejection, no matter if they resolved.

```js
const promise1 =  new Promise((resolve,reject) => {
  resolve("my first value");
});
const promise2 =  new Promise((resolve,reject) => {
  reject(Error("oooops error"));
});

// one of the two promise will fail, but `.all` will return only a rejection.
Promise
  .all([promise1, promise2])
  .then(data => {
    const[promise1data, promise2data] = data;
    console.log(promise1data, promise2data);
  })
  .catch(err => {
    console.log(err);
  });
  // Error: oooops error
```

`Promise.race()` on the other hand returns a promises that resolves or rejects as soon as one of the promises in the iterable resolves or reject, with the value from that promise.

``` js
const promise1 =  new Promise((resolve,reject) => {
  setTimeout(resolve, 500, 'first value');
});
const promise2 =  new Promise((resolve,reject) => {
  setTimeout(resolve, 100, 'second value');
});

Promise.race([promise1, promise2]).then(function(value) {
  console.log(value);
  // Both resolve, but promise2 is faster
});
// expected output: "second value"
```

If we passed an empty iterable, the race would be pending forever!.