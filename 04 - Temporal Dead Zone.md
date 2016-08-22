Let's talk real quick about the **Temporal Dead Zone**. This is a bit of a boring topic, so I'm going to try and make it really fun for you. 

This is something that you probably won't come across too often, but it's helpful to know in case someone ever slings it out in an interview. You'd be able to talk them about it. First of all, I call it the temporal dad zone, which is kind of fun.

Let's get started.

```js
    var pizza = 'Deep Dish 🍕🍕🍕’
    console.log(pizza);
````

What happens when you try and log `pizza` to the console after it's been created? You'll see that we actually get to see `'Deep Dish 🍕🍕🍕’`. 

No big issue there.

What if I try to run `console.log` before creating it? 

```js
    console.log(pizza);
    var pizza = 'Deep Dish 🍕🍕🍕'
````

Are we going to get: 

-  `Undefined` 
-  An error saying `pizza` is not defined yet
-  Are we actually going to see `Deep Dish :pizza:` 

We get undefined. 

Why? 

Because with `var` variables, you can only access them as they are defined. Before they are defined, you cannot access the actual value of them, but you can access the fact that the variable has been created before.

However, if I change that to `const` or `let`, you'll now see that `pizza` is not defined at all. That's an actual error, and that will **break your code**. 

That is called the **temporal dead zone**, where you cannot access a variable before it is defined. 

They mean that this is the temporal dead zone between `console.log` and a `let` or `const` variable, because you cannot access those kinds of variables before they're created.

That's all you need to know. Put that in your back pocket, because you'll need it someday.