There are a couple of new ways to declare variables in ES6 that help us out with scoping. We can declare variables with `var`, which we've always used, but now we can use `let` and `const` to declare variables too.

These two have some attributes about them which are going to be helpful for us in creating variables but let's do a quick review to show you  how `var`, `let`, and `const` are different.

Firstly, `var` variables can be **updated** or **redefined**. Let's use `console.log` to show how a variable `width` can be set to `100` and then updated to `200`.

```js
var width = 100;
console.log(width); // 100
width = 200;
console.log(width); // 200
```

If you run this in your browser, you'll see we get `100` and then `200`, which isn't a big deal. We're able to update them. You can also put a `var` in front of line 3, if you were to accidentally redeclare the variable. 

```js
var width = 100;
console.log(width); // 100
var width = 200;
console.log(width); // 200
```

If you do, it won't do anything. It will work as you'd expect, but it won't yell at you for creating the same variable name twice in the same scope because `var` variables can be **updated** or **redefined**.

We also need to remember how `var` variables are scoped. **Scoping** essentially means, "Where are these variables available to me?". In the case of `var` variables, they're **function scoped**, which means that they are only available inside the function in which they are created. If they are not declared in a function, then they are **globally scoped**, and they're available in the whole window.

What if I were to put my variable `width` inside of a function, and `console.log` its value? Is that going to work?

```js
function setWidth() {
  var width = 100;
  console.log(width);
}
setWidth(); // 100
```

Of course, it's going to work because this `width` is available inside of this function.

But what if I also tried to `console.log` the variable after I've set the width like this?

```js
function setWidth() {
  var width = 100;
  console.log(width);
}
setWidth();
console.log(width); // error, width is not defined
```

It won't work. Why won't it work? Because **`width` is only scoped to the `setWidth()` function**. It is a local variable to our `setWidth` function. It is not available outside of the function's confines. Think of the curly brackets as gates, or function jail, and our `var` variables are in there.

That's important to know. If we do want to globally scope `width`, we need to declare it outside the function like so, and update its value inside the function:

```js
var width;
function setWidth() {
  width = 100;
  console.log(width);
}
setWidth();
console.log(width);
```

Generally, it's probably not what you want to do. You want to keep your variables inside of your functions. If you need something outside of a function, you want to return it and store that in a variable. That's something that we need to know about function scoping. But I'm going to show you a use case where function scoping sort of comes back and bites us.

Let's say we have an `age` variable and we have an `if` statement. We want to create the number of dog years for that variable. If `age` is greater than 12, let's calculate the age in dog years and show "You are (however many) dog years old" in the console.

```js
var age = 100;
if(age > 12) {
  var dogYears = age * 7;
  console.log(`You are ${dogYears} dog years old!`);
}
```

Just as an aside, you can see I'm using backticks in this example, but I'm going to tell you all about this in the "Template Strings" section.

The one thing that is a little bit strange here is that `var dogYears` is just a temporary variable. I need this variable real quick in order to calculate something and then stick it into a `console.log` or stick it into a string or whatever. If you go to your browser console and call `dogYears` however, you'll see that it's leaked outside of the `if` statement and it is now a global variable that lives on `window`.

```js
var age = 100;
if(age > 12) {
  var dogYears = age * 7;
  console.log(`You are ${dogYears} dog years old!`);
}
console.log(dogYears); // 700
```

Even though this was a temporary variable that I only needed inside of one if statement, because `var` variables are **function scoped** and remember, there's no function here, it's going to be **globally scoped**. It's scoped to the entire window, which is a little bit of a pain here. That is one of the benefits to using `let` and `const`. Instead of being scoped to the function, they are **block scoped**, which is something new.

What is a block? Here is a great example:

```js
// ...
if(age > 12) {
  var dogYears = age * 7;
  console.log(`You are ${dogYears} dog years old!`);
}

// ...
```

Any time that you see **{ curly brackets }**, that's a block. Functions are also blocks, which means `let` and `const` are still going to be scoped to a function. If inside of a function or if inside another block, **they will be scoped to the closest set of curly brackets**.

If I now take this dog years here and change it to `let`...

```js
var age = 100;
if(age > 12) {
  let dogYears = age * 7;
  console.log(`You are ${dogYears} dog years old!`);
}
console.log(dogYears); // error because it's scoped only to the above block
```

...and I refresh, everything works as we would want. However, if you call `dogYears` in the browser console, it says, "Dog years is not defined." Why? Because I declared it as a **`let` variable**. It is only declared inside of a block scope, and not a global scope like `var`. This means that the temporary variable has not leaked out of the block.

You can also use `const` and get the same result. The difference between the two is what we will talk about in the [ES6.io](https://es6.io) video.
