ES6 has introduced arrow functions which, in my opinion, they have three main benefits. First, they are much concise than regular functions. Secondly, they have implicit returns, which allows us to write these nifty one-liners.

Thirdly, they don't rebind the values when you use a arrow function inside of another function, which is really helpful for when you're doing things like click handlers and whatnot.
 
We're going to take a look at a whole bunch of examples as well as, we're going to be using arrow functions all over the place in this course.

I've got an array of names:

```js
const names = ['wes', 'kait', 'lux'];
```

I want to add Bos to the end of all three of these. 

Normally, you'd do something like this:
 
 
```js
const fullNames = names.map(function(name){
  return `${name} bos`;
});

console.log(fullNames);
```

We're going to use **backticks** here, which is our template strings. Don't worry exactly about what that is, if you're not sure just yet. We have a whole chapter on that coming up.


Anyway, It's going to give me Wes Bos, Kate Bos, Lux Bos in the entire array. It took this array, transformed it into whatever it was plus the name "Bos" on the end.

That makes sense to me, but this isn't an arrow function. Let's take a look at how we could rewrite that. 


The first thing you do with an arrow function is, you simply delete the word function and you put in what's called a fat arrow.


```js
const fullNames2 = names.map((name) => {
 return `${name} bos`;
});

console.log(fullNames2);
```
If you've come from other programming languages, you might have seen that before, but in JavaScript it's the first time we're seeing a fat arrow. It's an equals sign with an angle bracket beside it. 

It looks like this: `=>`

It'll do exactly the same thing as `function`. If you `console.log` it, there should be no surprises there. We get the exact same thing. 

That's the first thing that you can do, but we can go even further with it where, if you only have one or two parameters you can take out the parentheses:

```js
const fullNames3 = names.map(name => {
 return `${name} bos`;
});

console.log(fullNames3);
```

That's a bit of a stylistic choice. Some people say put the parenthesis on regardless if you have one or two. In my case, if I have one, I could do something like this. It's still working great.
 
What else could I do with this? I can use what's called an implicit return.

But what's a explicit `return`? 

That's when you explicitly write `return` for what you want to `return`. But a lot of these callback functions that we write in JavaScript are just one-liners, where we just return something immediately in one line. We don't need a whole bunch of lines. 

Why do we have to do this return? Well, what we could do is something like this:

```js
const fullNames4 = names.map(name => `${name} bos`);

console.log(fullNames4);
```


What we've done is delete the return, put it up on all of one line, and then delete your curly brackets. When you delete your curly brackets, it's going to be an implicit return, which means we do not need to specify that we are returning `${name} bos`. 

It will just assume that we're doing so, and you can `console.log` it to see the same thing again.

Then finally, if you have no arguments at all -- in our above examples obviously we need an argument -- but if no arguments at all, you need to pass some empty parenthesis there. 

Maybe we'll just say Cool Bos, and they'll all be Cool Bos at the end. 

```js
const fullNames5 = names.map(() => `cool bos`);

console.log(fullNames5);
```


Another thing we need to know about arrow functions, at least right now, they may change this in future versions of JavaScript, is that arrow functions are always anonymous functions. 

What is an anonymous function? Actually what's a named function?

A named function is something like this: 


```js
function sayMyName(name) {
  alert(`Hello ${name}`);
}
```

The benefit to using a named function is that if you have a stack trace, which means if you have an error and you want to figure out, where did this go wrong, sometimes a line number as to where it happened isn't very helpful, so you need to know actually the name of the function that it got called in. 

If you use an arrow function, **you cannot name them**. None of our arrow functions have a name. 

You can, however, put them in a variable. If I were to say something like this, and pass it a name, and create a function declaration that way.
 
```js
const sayMyName = (name) => {alert(`Hello ${name}!`)}

sayMyName('Wes');
```

The thing we need to know about that is it is an anonymous function and it will not give us very good stack traces. However, if you're not too concerned with that, then you can absolutely go ahead.
