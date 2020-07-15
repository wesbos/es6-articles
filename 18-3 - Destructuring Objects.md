There's one last thing we need to know about destructuring objects, and that is the ability to set defaults. This one's a little bit confusing, so bear with me here and we're going to circle back for another example later on in a couple of videos over at [ES6.io](https://ES6.io).

When you destructure an object, what happens if that value isn't there? 

```js
const settings = {
	speed: 150
}
const { speed, width } = settings; 
console.log(width);
```

What is width? It's `undefined` because we create the variable, but it's not able to be set to anything. 

With destructuring we can set defaults, or _fallback values_ so that if an item is not in the object (or Array, Map, or Set) it will fall back to what you have set at the default.

This syntax is a little hard to read:

```js
const settings = {
	speed: 150
}
const { speed = 750, width = 500 } = settings;
console.log(speed); // 150 - comes from settings object
console.log(width); // 500 - fallback to default
```

Now if the `speed` or `width` properties don't exist on our `settings` object, they fallback to `750` and `500` respectively.

## Careful: null and undefined

One thing to note here is that this isn't 100% the same as this old trick used to fallback when `settings.speed` is not set:

```js
const mySpeed = 0;
const speed = mySpeed || 760; 
console.log(speed); // 760!
```

Why? Because ES6 destructuring default values only kick in if the value is undefined. null, false and 0 are all still values!

```js
const { dogName = 'snickers' } = { dogName: undefined }
console.log(dogName) // what will it be? 'snickers'!

const { dogName = 'snickers' } = { dogName: null }
console.log(dogName) // what will it be? null!

const { dogName = 'snickers' } = { dogName: false }
console.log(dogName) // what will it be? false!

const { dogName = 'snickers' } = { dogName: 0 }
console.log(dogName) // what will it be? 0!
```


### Combining with Destructuring Renaming

[In my last post](http://wesbos.com/destructuring-renaming/) we learned that we can destrucutre and rename varaibles at the same time with something like this:

```js
const person = {
  first: 'Wes',
  last: 'Bos',
};

const { first: firstName } = person;
console.log(firstName); // Wes
```

We can also set defaults in the same go. Hold onto your head because this syntax is going to get funky!

```js
const person = { first: 'Wes', last: 'Bos' };
const { middle: middleName = 'Super Rad' } = person;
console.log(middleName); // 'Super Rad'
```

Woah - let's step through that one! 

1. First we create a new const var called `middleName`.
2. Next we look for `person.middle`. If there was a `person.middle` property, it would be put into the `middleName` variable.
3. There isn't a `middle` property on our `person` object, so we fall back to the default of `Super Rad`. 

Cool! Make sure to check out [ES6.io](https://ES6.io) for more like this!
