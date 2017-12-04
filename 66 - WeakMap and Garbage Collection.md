A couple of videos back, we talked about a WeakSet where it gets garbage collected. A WeakMap is pretty much exactly the same thing. 

A WeakMap doesn't have a size, and you can never tell how many elements are in it. A WeakMap is not enumerable, which means you cannot loop over it. The items inside of a WeakMap, if they no longer exist anywhere else in your application, they will get garbage collected and they'll be removed from your WeakMap.

Let's go through a quick example of where we have a regular map and a regular WeakMap, and we'll show exactly the difference here. We'll create two objects here, then we'll create two maps.

```js
let dog1 = {name: 'Snickers'};
let dog2 = {name: 'Sunny'};

const strong = new Map();
const weak = new WeakMap();


strong.set(dog1, 'Snickers is the best!');
strong.set(dog2, 'Sunny is the 2nd best!');
```

Notice how we're using the keys here, which are objects.

Let's take a look at what we've got here. If we call up `strong`, we'll see that it's our Map, and `weak`, we get our WeakMap.

They each have one item in it. If I were to say `weak.size`, we get nothing. 

But if I say `strong.size`, we get 1. 

Why? Because a WeakMap doesn't have a size. Even though DevTools knows what it is here, you cannot get the size of a WeakMap at all.

Now, let's go and delete some of them. 

```js
let dog1 = {name: 'Snickers'};
let dog2 = {name: 'Sunny'};

const strong = new Map();
const weak = new WeakMap();


strong.set(dog1, 'Snickers is the best!');
strong.set(dog2, 'Sunny is the 2nd best!');

dog1 = null;
dog2 = null;
```

We've created these, we've added them to the map, and then we've deleted them. Let's take a look at our maps here. 

We have our `strong`, which still has an item in it. However `weak` has nothing in it, because it's been garbage collected. Remember, it can take a couple seconds.

We no longer have a `dog1`, and we no longer have a `dog2`, but the `strong` one is still holding onto this object that no longer exists. That's a memory leak there, because there's no way for us to reference it, whereas the WeakMap has totally cleaned it up. Garbage collection has happened and we no longer reference it there.

If you need something like that, where it gets garbage collected, where you don't have to babysit what is and isn't inside of the map, then that's the case where you reach for a WeakMap over a Map.