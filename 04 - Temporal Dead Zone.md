Let's talk real quick about the **Temporal Dead Zone**. This is a bit of a boring topic, so I'm going to try and make it at least a little fun for you. 

This is something that you probably won't come across too often, but it's helpful to know in case someone ever slings it out in an interview. You'd be able to talk to them about it. First of all, I call it the temporal dead zone, which is kind of fun.

```js
var pizza = 'Deep Dish 🍕🍕🍕';
console.log(pizza);
```

What happens when we try and log `pizza` to the console after it's been created? You'll see that we get `'Deep Dish 🍕🍕🍕'`. 

No big issue there.

What if I try to run `console.log(pizza)` before creating it? 

```js
console.log(pizza);
var pizza = 'Deep Dish 🍕🍕🍕';
```

Are we going to get: 

-  `undefined`

-  An error saying `pizza` is not defined yet

-  The actual value `'Deep Dish 🍕🍕🍕'` 

We get `undefined`.  Why? 

With `var` variables, you can only access them after they are defined. Before they are defined, you cannot access their actual value, but you can access the fact that the variable has been created. This **before its eventual declaration**.

However, if I change that to `const` or `let`, you'll now see that `pizza` is not defined at all. That's an actual error, and that will **break your code**. 

```js
console.log(pizza);
let pizza = 'Deep Dish 🍕🍕🍕'; // Cannot access 'pizza' before initialization
```

That is called the **temporal dead zone**, where you cannot access a variable before it is defined. 

The temporal dead zone refers to the space between `console.log` and a `let` or `const` variable, because you cannot access those kinds of variables before they're created.

That's all you need to know. Put that in your back pocket, because you'll need it someday!
