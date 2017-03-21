Another neat thing about ES6 is that you can extend the built-ins. I don't mean monkeying with the prototype of something like an array or a number, but I mean that we can create our own classes that are modeled after an array. Let's say we wanted to create a movie collection that will look something like this:


```js
const moview = new MovieCollection('Wes\'s Fav Movies',
    {name: 'Bee Movie', stars: 10},
    {name: 'Star Wars Trek', stars: 1},
    {name: 'Virgin Suicides', stars: 7},
    {name: 'King of the Road', stars: 8}
);
```

Now the movie collection is going to take in `Wes's Fav Movies`, that's the name of our collection, and then argument two, three, four, and so on, are going to have a movie, where you have the `name` of the movie and the number of `stars` that we're giving it. This is kind of like an array, except it's going to have a the property of  `Wes's Fav Movies`, and then we're also going to add methods to this. 

What we do is we create a `MovieCollection` class that extends the built-in array:

```js
class MovieCollection extends Array {
 
}

const moview = new MovieCollection('Wes\'s Fav Movies',
    {name: 'Bee Movie', stars: 10},
    {name: 'Star Wars Trek', stars: 1},
    {name: 'Virgin Suicides', stars: 7},
    {name: 'King of the Road', stars: 8}
);
```

We need a constructor here, but what arguments are getting passed in when we create a new movie collection? The first thing getting passed in is the `name` of the movie, that's easy. But then, how do we capture items two, three, four, and so on, for the rest of the arguments you need?

I just said it! We can use a rest, which in this case is `...items`:
 
 
```js
class MovieCollection extends Array {
 constructor(name, ...items) {
     
 }
}

const moview = new MovieCollection('Wes\'s Fav Movies',
    {name: 'Bee Movie', stars: 10},
    {name: 'Star Wars Trek', stars: 1},
    {name: 'Virgin Suicides', stars: 7},
    {name: 'King of the Road', stars: 8}
);
```

What that's going to do is the first thing first argument is a `name` and the rest of them that get passed in are going to be `items`. So let's try something:

```js
class MovieCollection extends Array {
 constructor(name, ...items) {
    this.name = name;     
 }
}

const moview = new MovieCollection('Wes\'s Fav Movies',
    {name: 'Bee Movie', stars: 10},
    {name: 'Star Wars Trek', stars: 1},
    {name: 'Virgin Suicides', stars: 7},
    {name: 'King of the Road', stars: 8}
);
```

If we try to run this, we'll get a prety familiar error, telling us that `this is not defined`. Why can't we set `this` on movie collection? Because we have not yet given ourselves that initial array that we are extending. 

Remember, whenever you extend something, you need to create the thing that you're extending first, before you create the thing that you want.

How do we do that? 

```js
class MovieCollection extends Array {
 constructor(name, ...items) {
     super(items);
    this.name = name;     
 }
}

const moview = new MovieCollection('Wes\'s Fav Movies',
    {name: 'Bee Movie', stars: 10},
    {name: 'Star Wars Trek', stars: 1},
    {name: 'Virgin Suicides', stars: 7},
    {name: 'King of the Road', stars: 8}
);
```

What we've done is called `super();` and passed it `items`. We did that because it's kind of like creating a new array that will take item one, two, three and so on, kind of like `new Array(1, 2, 3)`. Because we don't know how many items we're going to have, we can pass it `items` to capture everything.

If we run this code, we'll see our `movies` in the console, but our `movies` will be an array of an array. 

That's not exactly what we want. We want to be able to put each item in one array, not an array in an array, so we use a spread, in this case `...items` into the function:
 

```js
class MovieCollection extends Array {
 constructor(name, ...items) {
     super(...items);
    this.name = name;     
 }
}

const moview = new MovieCollection('Wes\'s Fav Movies',
    {name: 'Bee Movie', stars: 10},
    {name: 'Star Wars Trek', stars: 1},
    {name: 'Virgin Suicides', stars: 7},
    {name: 'King of the Road', stars: 8}
);
``` 

We do this because `items` is an array, but we want to pass `super` each item as an argument. This is cool because we've captured all of them with a rest, and then we then turn around and spread them into our `super` function, which is the array.

If call `movies` in our console, it returns these objects, which if we open them up, you'll see all of the different movies that are inside our array.

If we ask for `movies.name` in the console, we'll see it's called "Wes's Fav Movies". 


We can have an array but we can also have properties. That's because, at the end of the day, arrays still are objects in JavaScript. 

Now what we can do is add methods to this movie collection. Let's add a movie using `this.push(movie)`:

```js
class MovieCollection extends Array {
 constructor(name, ...items) {
     super(...items);
    this.name = name;     
 }
 add(movie) {
     this.push(movie);
 }
}

const moview = new MovieCollection('Wes\'s Fav Movies',
    {name: 'Bee Movie', stars: 10},
    {name: 'Star Wars Trek', stars: 1},
    {name: 'Virgin Suicides', stars: 7},
    {name: 'King of the Road', stars: 8}
);
``` 

Even though we have our `MovieCollection` class, because we extended Array, it's inherited all of the array methods. 

So we can add a new movie by adding `movies.add({name: "Titanic", stars: 5});` which adds Titanic as a five star movie. If we run this code in our console, we'll see that Titanic is there, it's our last movie is Titanic.

The next thing I want to show you is, using the `for...in` loop.

In the console we can run:

```js
for (const movie in movies) {console.log(movie)}
```

What we'll see is we get `0`, `1`, `2`, `3`, and `4`. That makes sense because it's the index of each object in our class, but then we get `name`. 

Why do we get name? Because `movie` has five items in it, but then we also gave it a property of `name`, and `for...in` will loop over that.

However, we also learned about `for...of`, which only iterates over the iteratible properties of an object. We can use:

```js
for (const movie of movies) {console.log(movie)} 
```
 
 If we run that, you'll see we don't get `name`, and we also get the actual `object` for each movie, not just the index like we did before. That's a really neat thing where you can still add properties to an array, but we're able to skip over those properties with a `for...of` loop.

Let's add one more method to this, where I want to be able to sort this array of movies and then get the top 2, 3, or 10, or however many we want. 

```js
class MovieCollection extends Array {
 constructor(name, ...items) {
     super(...items);
    this.name = name;     
 }
 add(movie) {
     this.push(movie);
 }
 topRated(limit = 10){
     return this.sort((a, b) => (a.stars > b.stars ? -1 : 1)).slice(0, limit);
 }
}

const moview = new MovieCollection('Wes\'s Fav Movies',
    {name: 'Bee Movie', stars: 10},
    {name: 'Star Wars Trek', stars: 1},
    {name: 'Virgin Suicides', stars: 7},
    {name: 'King of the Road', stars: 8}
);
``` 

So we've called our method `topRated`, which is taking in a `limit` as its argument. If you have thousands move movies, you only want to bring a few back, but here we've set our default function argument to 10, which will bring back our top 10 movies.

On the next line we can make a quick and dirty sort, and `this` in this case is `MovieCollection`, which is an array, which we can use `sort()`, which is a method available to arrays. With that, we can sort through each object in our array, and  see if `a.stars` is greater than `b.stars` then we'll have a -1 and 1. Then we can call `.slice` on it, and slice from index 0 to the limit, which is 10 by default.

Now we should be able to call `movies.top_rated()` and it will bring back our movies, but they'll just show as `Object, Object, Object, Object, Object`.

That's not very good, but let's try using `console.table(movies.topRated())` instead.

If you run that in your console, that will give us all of the movies, sorted by the number of stars, in a nice table. You can also pass in `console.table(movies.topRated(2)), the top two rated movies, and that will bring me a table of the top two rated.

That's really handy if you're able to have an array, but also have methods on front of it. Extending arrays, you can extend any of the native stuff that are built into JavaScript to create your own collections.