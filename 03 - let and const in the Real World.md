You can probably see where `let` and `const` are going to be useful: **if you need to scope something to a block**, or **if you want to make a variable that cannot be changed** by anyone by accident or on purpose.

Let's take a look at a couple of more examples of when it might be useful.

The first one is replacing the **Immediately-Involved Function Expression**, or **IIFE**. I'm not sure if you've ever heard of this before, but it was coined by [Ben Allman back in 2010]((http://benalman.com/news/2010/11/immediately-invoked-function-expression/)).

An IIFE function runs itself immediately, and it creates a scope where nothing is going to leak into the parent scope. In our case, nothing is going to leak into the global scope of the window.

If I have a `var` variable: `var name = 'wes'`

You can call that in the console, and that's fine here. However, the window already has a `name` attribute, which is needed when you have a window opening up a another window.

That could something that some third-party JavaScript relies on in order for it to run, or maybe another script is using a variable called `name` and you accidentally overwrite that. It can get a little bit messy.

The way the IFFE fixes that is that the function runs immediately and you put your variables inside of that:

```js
(function() {
  var name = 'wes';
})();
```

These variables are now scoped to the IIFE function, and because `var` variables are function-scoped, they are not available in the global scope.

If you try to call `name` in the console now, it's not undefined, it's blank because, like I mentioned, it's just blank because that is a property that lives on the window natively in JavaScript.

If I needed to access our function's `name`, obviously, I'd have to do a `console.log` inside of the IIFE function, but the important thing is that it's no longer leaking into the global scope.

With `let` and `const` variables, we don't need a function for our variables to be scoped to that.

Why? Because `let` and `const` use **block scope**.

Let's start over with a `const` instead of a `var`

```js
const name = 'wes';
```

If we call this in the console, we'll see `'wes'`, but if we wrap it in curly brackets:

```js
{
const name = 'wes';
}

```

Our `const` is going to be scoped to that block. If you try to call `name` in the console, we'll get the window's `name`, which is blank. But if we add a `console.log` to our block:

```js
{
const name = 'wes';
console.log(name);
}

```
...we'll get `wes` in the console. You don't the IFFE stuff anymore. You're using `let` and `const` because they are going to be scoped to that block.

The other problem using `let` and `const` will fix is with our `for` loop.

This is something that you probably have all run into with your regular `for` loop, like one that will count from zero to nine:

```js
for(var i = 0; i < 10; i++) {
    console.log(i);
    }
```


Problems that arise here are two things:

First of all, if I type `i` into the console, it returns `10`. We have this global variable that has leaked into the window, or into a parent scope, which is not something we necessarily want. It's just a placeholder value that we need to work inside of this loop.

Second of all, maybe you had something that's going to run after some bit of time, an AJAX request, or, for this case, I going to mark it up with a `setTimeout()` and that function is going to run after one second:

```js
for(var i = 0; i < 10; i++) {
  console.log(i);
  setTimeout(function() {
    console.log('The number is ' + i);
  },1000);
}
```

If we run this, all of them are 10. The reason that we have that is because, `console.log(i)`  will run immediately and reference whatever `i` is. That runs immediately at `console.log` itself.

However, after one second, this entire loop has already gone through every iteration that it needs to and the variable `i` here is being overwritten every single time.

The problem with that is that by the time the first `setTimeout()` runs, `i` is already at 10, and we don't have any way to reference it.

If you had an AJAX request where you are looping through a few of them, there isn't any way without an IFFE to reference what the `i` variable was when it ran, not what it currently is after the loop.

One quick way we can fix that is if we just change `var` to `let`:

```js
for(let i = 0; i < 10; i++) {
 console.log(i);
 setTimeout(function() {
   console.log('The number is ' + i);
 },1000);
}
```

What do we know about `let`? It's block-scoped. We have curly brackets in the `for` loop. If you run it now, after a second we'll log zero through nine. We're not getting 10, 10 times. We getting it as it was declared each and every time.

As a note, you couldn't use a `const` for this because it needs to overwrite itself, and you can't assign the same variable twice. When we use `let`, it's going to scope `i` to our curly brackets.
