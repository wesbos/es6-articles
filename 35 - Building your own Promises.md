A `promise` is built into a lot of things in the browser like, `fetch` and `getUserMedia`. We're starting to see libraries implement their own `promises`, and it's probably a good idea for you to implement `promises` into your own code base.

To create your own `promise`, you create a variable, and you store a new `promise` inside of it. A `promise` constructor takes one function here, which passes you `resolve` and `reject`...
 
```js
const p = new Promise((resolve, reject) => {
    
});
```

The idea here is that your promise is either going to `resolve`, which means it finishes and passes data back to you, or `reject`, and not pass the data.
 
For example, in the last post we used a JSON API, and the data actually came back, it resolved itself and it passed us a list of my blog posts. 

In the case of `reject`, maybe there was an error or the data was malformed, or for whatever reason you'd like to `reject` the actual `promise` and that will throw an error. 

Both `resove` and `reject` are called when you are ready to finish this `promise`.

I'm going to call one immediately, and we're going to pass a string like "Wes is cool," because that's the data for this `promise`, and try to log that to the console using `.then`.

```js
const p = new Promise((resolve, reject) => {
   resolve('Wes is cool') 
});

p
  .then(data => {
      console.log(data);
  })
```


If we load this, we'll immediately we see "Wes is cool" in the console. This is because we created a `promise`, and then immediately resolved it by passing "Wes is cool" back to us. 

It's really not that useful, but what you can probably see is that if we wanted to resolve something after some amount of time, maybe after some processing has been done. Maybe you wanted to do some processing on the background that's really intensive, like an AJAX request for data. There's a whole bunch of different use cases for when you would want to use a `promise`.

Essentially it all boils down to "I don't want to stop JavaScript from running, I just want to start this thing, and then when it comes back I'll deal with the actual result."

Let's see what happens when we put a setTimeout on here for one second. 


```js
const p = new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve('Wes is cool')
    }, 1000); 
});

p
  .then(data => {
      console.log(data);
  })
```

If we load this, you'll notice that it doesn't pop up immediately. 

Similarly, we could also call `reject` on it:

```js
const p = new Promise((resolve, reject) => {
    setTimeout(() => {
       reject('Err Wes isn\'t cool');
    }, 1000); 
});

p
  .then(data => {
      console.log(data);
  })
```

But if we run that, you'll see that we get an error: "Uncaught (in promise) "Err Wes isn't cool"...
 
Why is that uncaught in promise? Because we didn't `catch` it, right? We should `catch` the error, using `catch` and `console.error`. 

```js
const p = new Promise((resolve, reject) => {
    setTimeout(() => {
       reject('Err Wes isn\'t cool');
    }, 1000); 
});

p
  .then(data => {
      console.log(data);
  })
  .catch(err => {
      console.error(err);
  })
```

Once we run the script, it tells us a specific line where the error happens, but that's where `catch` runs and then displays the error. But didn't the error actually happen on Line 11? Why doesn't it say anything about Line 11 in here? 

Ideally what you do is you throw in an error object, not just a string, like this...


```js
const p = new Promise((resolve, reject) => {
    setTimeout(() => {
       reject(Error('Err Wes isn\'t cool'));
    }, 1000); 
});

p
  .then(data => {
      console.log(data);
  })
  .catch(err => {
      console.error(err);
  })
```
 
By doing that, we see that we have more information in the console when the error is thrown, and it tells us the line where the error is.
