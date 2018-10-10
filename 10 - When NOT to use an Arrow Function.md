Before you start going absolutely bananas on using arrow functions everywhere, we need to chat. **Arrow functions don't replace regular functions**. Just like Flexbox and floats, pixels and rems, and anything else new that comes along, the older thing still retains lots of utility because it works differently than the new thing. 

We talked about the benefits of ES6 Arrow Functions in earlier videos and blog posts but let's go through a couple examples of when you probably _don't want an arrow function_. All of these are just going to boil down to not having the keyword `this`, but there are also different use cases that you'd run into. 

### #1: Click Handlers

First of all, I've got this big button that says 'Push me':

```html
<style>
button {font-size: 100px; }
.on {background: #ffc600;}
</style>

<button type="button" id="pushy">Push me</button>
```

When someone pushes or clicks that button, I want to toggle the class of `on`, which should turn it yellow. When someone clicks that button, I'm going to run the following function:

```js
const button = document.querySelector('#pushy');
button.addEventListener('click', () => {
    this.classList.toggle('on');
});
```

But if we click it, we get an error in the console: `TypeError, cannot read property 'toggle' of undefined`

What does that mean? Well, if we remember from earlier, `this` is referencing the browser's `window` attribute, right? We can use `console.log` to confirm it:

```js
const button = document.querySelector('#pushy');
button.addEventListener('click', () => {
    console.log(this); // Window!
    this.classList.toggle('on');
});
```

Remember: as we discussed with arrow functions, the keyword `this` is not bound to that element. If we use a regular function, the keyword `this` will be bound to the element we clicked!

```js
const button = document.querySelector('#pushy');
button.addEventListener('click', function() {
    console.log(this);
    this.classList.toggle('on');
});
```

In the console, `this` is now referring to our button, and our big yellow button is actually working. The sames rules apply with jQuery, Google Maps or any other DOM library you are using.

### #2: Object Methods

Now, let's take a look at this next one, when we need a method to bind to an object. 

```js
const person = {
    points: 23,
    score: () => {
        this.points++;
    }
}
```

We have our method called `score`, and whenever we call `person.score()`, it should add one to our `points`, which is currently 23.

If we run `person.score();` a few times, we should be at 26 or something similar. 

But if I call `person`, `points` is still at 23. Why?

Because it's trying to add points to the window! Remember, when using an arrow function, `this` is not bound to anything and it just inherits it from the parent scope, which in this case is the window.

So, let's do the same thing with a regular function:

```js
const person = {
    points: 23,
    score: function()  {
        this.points++;
    }
}
```

There we go. That will actually work, because that's a full on function, and not an arrow function.

### 3: Prototype Methods

As our third example, we'll talk about when we need to add a prototype method. 

```js
class Car {
    constructor(make, colour) {
        this.make = make;
        this.colour = colour;
    }
}
```

Here, I've got a `class`. We haven't learned about classes yet, but just know that this is a way for us to make new cars.

I have a `class` constructor where, when we call `new Car`, we pass it the type of `Car`, as well as the `colour` of the `Car`. 

I can say `beemer` is a `BMW` that is `blue`, and the `subie` is a `Subaru` that is `white`:

```js
const beemer = new Car('BMW', 'blue');
const subie = new Car('Subaru', 'white');
```

Let's go ahead and look at them by calling them in the console. You'll see that `subie` comes back as `Car {make: "Subaru", colour: "white"}`, and `beemer` will come back as `Car {make: "BMW", colour: "blue"}`, which is what we'd expect.



Now, after the fact, I added on this prototype method:

```js
Car.prototype.summarize = () => {
    return `This car is a ${this.make} in the colour ${this.colour}`;  
};
```

What that allows us to do is that, even after these cars have been created, we can add methods onto all of them. With our `Car.prototype.summarize` method set, let's type into the console: `subie.summarize`.

If you're using Chrome's console, you'll see that it auto-completes the method, because it's available to you. Even though we added it after we created the `Car`, because I added it to the `prototype`, it's available in every object that has been created from there.

What this `prototype` does is it returns `this.make`, which is the make that we passed in, and `this.color` in a sentence.

However, with our example, `this.car` is `undefined` and the `colour` is `undefined`. Why is that? 

It's because we tried to be cool. We tried to be a bit of a hot shot here by using an arrow function. Again, why don't we use an arrow function here? Because we explicitly need the keyword `this`, we have to use a regular function:

```js
Car.prototype.summarize = function() {
    return `This car is a ${this.make} in the colour ${this.colour}`;  
};
```

Now, if we call `subie.summarize`, it says it's a white Subaru, and by calling `beemer.summarize`, we get BMW in blue. 

Again, we must use a regular function for that. 

### 4: When we need an arguments Object

For our last example, this is a little bit different:

```js
const orderChildren = () => {
    console.log(arguments);
    const children = Array.from(arguments);
    return children.map((child, i) => {
        return `${child} was child #${i + 1}`;
    })
}
```

This example doesn't have to do with the keyword `this`, but instead shows that we don't have access to the `arguments` object when we use an arrow function.

This is helpful for when we want to run a function like `orderChildren` here, which can take unlimited arguments.

It might take one, it might take 100. It's going to just say "This child was born #1", or whichever.

As an example, let's type into the console `orderChildren('jill', 'wes', 'jenna')`, which passes in `jill` as our first argument, `wes`, as our second, and `jenna` as our third. When we run it, we'll get an error: `ReferenceError, arguments is not defined`. 

This is because `arguments` is a keyword that we have in our `orderChildren`. That's going to give us an `Array` or array-ish value of everything that was passed in. 

However, we do not get the `arguments` object if we use an arrow function. However, when we use a regular function, it is going to give us the actual content that we need.

```js
const orderChildren = function() {
    console.log(arguments);
    const children = Array.from(arguments);
    return children.map((child, i) => {
        return `${child} was child #${i + 1}`;
    })
}
```

**Note:** Another fix for this is to use a `...rest` param to collect all the arguments into an array. We will learn all about that in the rest videos and blog posts!

Again, to go through all those really quickly. Make sure that you aren't just using arrow functions willy-nilly. In general, if you do not need the `arguments` object or you do not need `this`, or you know that you will not need it in the future, then you can feel free to go ahead and use an arrow function on everything else.
