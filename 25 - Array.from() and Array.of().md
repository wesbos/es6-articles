We have a whole new bunch of new array methods added in ES6. I'm going to talk about `.from` and `.of` first. The reason I'm going to talk about these two together is because they are not on the prototype. 

If we look at an array like `const ages=[23,34,234];`, you can't call `ages.from()` or `ages.of()`. Why? Because they are not on the prototype, they are static methods on the array itself, so you can call `Array.of()` and `Array.from()`.

What do these do? `Array.from` will take something that is array-ish and turn it into a true array. There's couple situations where we often need that, and that is when we are working with DOM elements.

```html
<div class="people">
    <p>Wes</p>
    <p>Kait</p>
    <p>Snickers</p>
</div>
```

Let's go ahead and select and `console.log` all the people:

```js
const people = document.querySelectorAll('.people p');
console.log(people);
```

But what if I wanted just an array of the people's names? I could use `.map`:

```js
const people = document.querySelectorAll('.people p');
const names = people.map(person => person.textContent);
```

Using that, we get an error: `people.map is not a function`. 

Why is that? Let's do a `console.log` of `people`, open it up in the inspector, and you can see we have a length, but the prototype is not an `Array`, it's a `NodeList`.

That's what I mean. I've been saying a couple times in this series, where something is array-ish or array-like, and that means that it has a length, which means it's array-ish, it has some of the methods, but doesn't have all of the methods that a regular array would have. We looked at that a couple posts ago.

What do we need to do in order to make this map work? We simply would have to make another variable:


```js
const people = document.querySelectorAll('.people p');
const peopleArray = Array.from(people);
console.log(peopleArray);
```

If we open `peopleArray` in the inspector, its prototype is now `Array`, and you open that sucker up, and you see absolutely all the methods that you're probably used to. 

You can call `Array.from` onto it:

```js
const people = document.querySelectorAll('.people p');
const peopleArray = Array.from(people);
console.log(peopleArray);
const names = peopleArray.map(person => person.textContent);
```
We'll get our `names` array, which is actually everyone's name.

Similarly, we can have also wrapped our querySelector right into it, and then we don't need that second variable, we can just map over the people:

```js
const people = Array.from(document.querySelectorAll('.people p'));
const names = people.map(person => person.textContent);
```

I could have also done this in one single go, rather than having to do it in two lines. `Array.from()` takes a second argument, which is a `.map` function, which allows us to modify data as we are creating an array. Let's look at it on two lines so it's nice and visible:

```js
const people = document.querySelectorAll('.people p');
const peopleArray = Array.from(people, person => person.textContent);
console.log(peopleArray);
```

### Converting the `arguments` Object to an Array

Another use case is that if we want to convert the `arguments` object into an actual array.

```js
function sumAll() {
    console.log(arguments)
}

sumAll(2,34,23,234,234,234234,234234,2342);
```

We just did an example like this when we looped over it. However, if you want to use `reduce`, here's how to actually do that. In the inspector, `arguments` looks like an array, and that would be a perfect use case for `reduce`:
 
```js
function sumAll() {
    console.log(arguments)
    return arguments.reduce((prev,next) => prev + next, 0);
}

sumAll(2,34,23,234,234,234234,234234,2342);
```

That should return `arguments.reduce`, right? But it doesn't, with the error: `arguments.reduce is not a function`. 

Why not? Looking into our `console.log(arguments)`, do we see `arguments.reduce`? Looking into its prototype, you don't see `.reduce`. Why not? Because this is `arguments`. It is not an array, it's array-ish.

To turn this into an array:

```js
function sumAll() {
    const nums = Array.from(arguments);
    return nums.reduce((prev,next) => prev + next, 0);
}

sumAll(2,34,23,234,234,234234,234234,2342);
```

If you look at it in the console, you see that we got our numbers added on up for us, too.

That's pretty common use case where you actually do want to convert your `arguments` object onto an array. Again, it is array-ish, because it has a `length`, but it doesn't have any other of methods on it.

### Array.of()

Next up, we have `Array.of`. It's pretty straight forward. Pretty much how it works is you pass it as many arguments as you wish:

```js
const ages =  Array.of(12,4,23,62,34);
```

That's going to create an array from every single that argument you pass it. 

That's it. There is nothing else much more to `Array.of`. That's one of those you put in your back pocket, because you might need it at some point.
