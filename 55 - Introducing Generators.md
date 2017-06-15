Let's take some time to learn all about generators in ES6. Currently with functions in JavaScript, when you run a function, it runs top to bottom and that function is done. 

However, with a **generator function**, what we are making is a function that you can start and stop, a function that you can pause for an indefinite amount of time, or a function that you can pass additional information to at a later point in time.

When we create a generator function, you add a `*` to the `function` keyword. We'll look at the syntax for doing it with a method in a bit. We'll also learn a new keyword here, `yield`. The `yield` keyword is sort of like a `return` in this case:


```js
function* listPeople() {
    yield 'Wes';
    yield 'Kait';
    yield 'Snickers';
}
```

If I were to `return` from a function, that would be it for the function. But if we `yield` from a function, what does is return one item until the function is then called again.

The function will need to be called three times in order for me to get all of the names, `Wes`, `Kait`, and `Snickers`. It's a generator function. 

The only thing that is different is that we use a `*`, and we `yield` values from it.

To use a generator function, what you need to do is first invoke this function and store that in a variable.
 
```js
function* listPeople() {
    yield 'Wes';
    yield 'Kait';
    yield 'Snickers';
}

const people = listPeople();
```
 
 
Take a look at it in your browser. 

Did that give me `Wes`, `Kait`, and `Snickers`? No. That gives me the generator, which is listed as its prototype, and the status is `"suspended"`, which means it's not running, and there's really no other information about it on there.

Now, how do we get this information out of there? 

We can call `.next` on the generator, as in `people.next()`. 

That returns to us -- this is kind of interesting -- not `Wes` like you might expect, but it returns to us an object with `Wes` as the value, which makes sense, but then we have another property called `done`, which is false. 

That means is this generator isn't done running because we've only run it once, and `Wes` is being returned.

You could probably imagine, I call `people.next()` and that gives me  `value: Kait`, and again, `done: false`. Then we call it once more, and we get `value: Snickers`, but still `done: false`. Call it once more, and you get `done: true`, and obviously, we don't get a value there.

You see how we can create these functions that offer up maybe multiple returns to us? Then when we want to use it, we simply just call `.next` on that generator. It will give us sort of the next thing in the line. I've hard-coded these values here, but those yields could be generated dynamically with you, with something like this:

```js
function* countNumbers() {
    let i = 0;
    yield i;
    i++;
    yield i;
    i++;
    yield i;
}

const numbers = countNumbers();
```

With that, every single time that I call it, I'm going to say, `countNumbers.next`, what that does is it creates a variable called `i` which is set to zero when I initialized it and the first time we yield the variable `i`.

The reference to the variable `i` is scoped to the function, and that is kept. When I call `countNumbers.next()` again, the variable `i` is still living inside of this function, because it is not yet done. If call it again and again and again. Finally, we hit `done: true`, and it gets cleaned up.

The things you need to know, as a generator function, it keeps its variables until it's finished, and you can yield multiple values from it.

Let's look at another example where we iterate through an array. Instead of just using `console.log` or showing them all at once, we're going to use `.next` to get through them:
```js
const inventors = [
     { first: 'Albert', last: 'Einstein', year: 1879 },
     { first: 'Isaac', last: 'Newton', year: 1643 },
     { first: 'Galileo', last: 'Galilei', year: 1564 },
     { first: 'Marie', last: 'Curie', year: 1867 },
     { first: 'Johannes', last: 'Kepler', year: 1571 },
     { first: 'Nicolaus', last: 'Copernicus', year: 1473 },
     { first: 'Max', last: 'Planck', year: 1858 },
     
    ];

function* loop(arr) {
    for (const item of arr) {
        yield item;
    }
}
```

I've got an array of inventors here and a new generator function called, `loop`, which is going to take in an array. 

Inside of it, we need to do our looping. We're using `for of` here, too. Then for each one, we are going to `yield` the item in the array, or in this case, our inventors.

That's kind of interesting here, because we aren't giving ourselves three `yields` like we were before. We're simply just giving ourselves one `yield`, which is going to create one, seven yields for us when we loop. The generator is smart enough to know that when there are no more to loop through, and the entire function will be done.

How do we actually call it? We have to create that generator:
 
```js
const inventorGen = loop(inventors);
```

You'll see that even though we've set `inventorGen`,  you won't actually see the inventors in the console in your browser. Even if I use `console.log(inventors)` in the `loop` function, they don't actually show up until we do the first `.next`.

If we want to call it, we call `inventorGen.next()` in the console. The first time we see that huge array of all the inventors come back as objects, but then we'll see another object, where it lists `done: false`, and then the value is Albert Einstein, the first inventor in our array.

I could call `inventorGen.next()` to give me the next value, one little trick you can do is you can use `inventorGen.next().value` onto the end if you don't care about the generator's status. As we call `inventorGen.next().value` over and over, we'll Galileo, Curie, Kepler, Copernicus, Planck, and then, eventually, once we're done, our value will return as `undefined`.

You could keep calling this until your're done, but what's really cool about that is it will just finish once it actually went through every single one. Again, we did a `yield` inside of here and a `yield` returned multiple values to us.