Let's look at a couple more JavaScript arrow function examples.

Oftentimes, I find that I'm building an application and I don't necessarily have the data in the format that I need.

I'm building a website for a bunch of racers, where the winners have been supplied to me in an array of strings in the order in which they placed, and then they also gave me a variable, which includes the race that each racer won.

```js
const race = '100m Dash';
const winners = [ 'Hunter Gath', 'Singa Song', 'Imda Bos' ];
```

So Hunter Gath came in first, Singa Song came in second, and Imda Bos came in third.

Ideally, I would have an object with the key `name` and a value of the name of the racer, the key `place` and a value of the place in which he/she came, and the key `race`, the value of which is the `race` variable, used to indicate which race the racer won. The object would look something like this:

```js
{
  name: 'Wes Bos',
  place: 1,
  race: race
}
```

How would we do that? Well, we can actually use `.map()` again for that. By the way, `.map()` is not the only function with which arrow functions work. Arrow functions work with anything, and they're particularly useful in these callback situations.

```js
const win = winners.map((winner, i) => { name: winner, race: race, place: i });
```

We're going to loop over each winner and return an object for each of the winners here. Our function passes two arguments, the `winner` string and `i` (for our index), and we're going to return our `name`, `race`, and the `place` in which the person came.

### Arrow Function Implicit Return of an Object Literal

That should work; however, we have this object here, but it doesn't work. Instead, it throws a syntax error: `Unexpected token :`.

What's wrong here? Well, earlier we told you that if you take the curly brackets off, it's an implicit `return`. How do you implicitly `return` curly brackets when you mean for it to be an **object literal**, not the actual function block?

What we can do is put parentheses around the object literal so that the arrow function knows it's an object and _not_ a function block.

```js
const win = winners.map((winner, i) => ({ name: winner, race: race, place: i + 1 }));
```

These parentheses show that you're actually going to return an object literal, and the curly brackets aren't for the actual function block. So `name` is `winner`, `race` is the `race` variable we have, and `place` is going to be set to `i + 1`, our index plus one, so that we aren't zero-based.

### Viewing Our Data

Now we'll take a look at our data.

![](https://wesbos.com/wp-content/uploads/2016/09/ss-2016-09-07-at-10.50.42-AM.png)

A little hot tip: You refresh. You can type `win` in the console to see all of your objects, but then you have to do all this work on opening them up. What you can actually do is use `console.table(win)` and you get a sweet-looking table that will show you all of the information.

Then the other thing: We haven't learned about this yet, but `race: race` in our object looks a little bit redundant. A new feature of ES6 is that you can just use `race`,  which is the same thing as saying `race: race`. Just say that the variable `race` and the property name `race` is all named `race`. It's going to do it just for us.

```js
const win = winners.map((winner, i) => ({ name: winner, race, place: i + 1 }));
```

There we go. So that's an example of doing an implicit return with an object literal.

### Arrow Function Filtering Example

There's another example here in which we poll the people in the room for ages.

```js
const ages = [ 23, 62, 45, 234, 2, 62, 234, 62, 34 ];
```

We want to filter this list for people who are older than 60 years old. Normally, you'd do something like this:

```js
const old = ages.filter(age => if (age > 60))...
```

Since we can pass it a condition that evaluates to `true` or `false`, and `.filter()` will return if it's `true` and won't return if it's `false`, we can just say, "Age is greater than or equal to 60."

```js
const old = ages.filter(age => age >= 60);
```

That looks a bit funky because you have an arrow and a greater than, but what that will do is the following: If the `age` is greater than or equal to `60`, it will return `true` and, thus, be put into the `old` array.

That `age` is going to be put back into this new `old` array, and we can use `console.log(old);` to return our filtered array.