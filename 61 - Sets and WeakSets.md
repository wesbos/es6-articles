A set in JavaScript is like a unique array, meaning that you can only ever add the same item once, with a nice API for managing the items inside of it. It's different than an array, in that you can't access the items individually and it's not index based. 

You can think of a set as just a list of items, which we can add to, remove from, and loop over. Let's take a look at how it works, how the API works, and then we'll talk about looping over it. 



```js
const people = new Set();
people.add('Wes');
people.add('Snickers');
people.add('Kait');
```

You can open this in your browser console and type `people`. You'll see that we now have a set, which looks very similar to an array. 

You can also get information about the set, you can type something like `people.size`. It's not `.length`,  because they can't be accessed by their index, so it's `.size`. 

You can list the people by typing `people` and seeing what you have in there. If you'd like to remove someone, you can use `people.delete('Wes')`. You don't have to figure out where the index of `Wes` is. Here, you just delete it.

You can also remove everyone from the set using `set.clear`, making it an empty set.

We can call other methods, which cab be `entries`, `keys` or `values`. Call `people.values()` and you can that the set's properties create a  `set iterator` which gives you all of the values. Because it's a set iterator you can loop over it, or you may remember from the generators videos, that this is a kind of generator.

If we stick `people` in a variable, we can just call `.next()` on it to view the items in hte set.
 
What that probably means to you is, "Oh, cool, I can use the generator API on it with `.next()` or I could feed it right into a `for...of` loop." 

```js
const people = new Set();
people.add('Wes');
people.add('Snickers');
people.add('Kait');

for (const person of people) {
    console.log(person);
}
```

The set also has methods of `.keys` and `.entries`. I'll show you what that is real quick. What's the difference between `keys` and `values`? Absolutely nothing, because a set is just `values`. Sets have `keys` and `values` are mapped to the same thing.

But you also have `.entries`, which if you use that, you'll see that you get, `"Wes", "Wes", "Snickers", "Snickers", "Kait", "Kait". 

You'll find that `.keys` and `.entries`, are really helpful when you start using map. 

The reason I think they're there is that we can have the same API on map and set, and we don't have to worry about if it's there or not.

Let's take a look at another example with some students. If you want to initialize the set with some values, you *can't* just pass a list of values to it like you would to an array. You have to wrap those suckers in an iterable (array, array-ish objects, another set, or any other iterable)

```js
const students = new Set(['Wes', 'Kara', 'Tony']);
```

One common mistake here is to pass a list of values instead of an iterable. So what happens when you do something like `const students = new Set(['Wes', 'Kara', 'Tony']);`? It will coerce the first argument to an array and use it to initialize the set, and the remaining arguments are ignored. This will result in `students = {"W", "e", "s"}`, which is not you want. Go ahead and try it in your console.

The simplest way to initialize a set with some values is to take an array and just pass it into a new set. 

```js
const dogs = ['Snickers', 'Sunny'];
const dogSet = new Set(dogs);
```

One other thing we can do is we can use `.has()` to see if someone is already in it. 

Remember we talked about the set as being unique? If I were to take the students set and call `.has('Tony')`, it'll say true.
 
 If we use `.has('Wessss')` spelled wrong, it'll say false. 
 
Again, sets are unique. If you add someone twice, it's still only going to show it in the set once, because the property is unique

One more thing, you cannot reference it like `students [1]`. It is not an index-based thing. It is a list, and if you need to loop over them, you have to use a `for...of` loop.

That is the entire set. It's fairly straightforward. We've got `.add`, `.clear`, `.delete`, `.size` properties, `.has` to see if it's in there, and we can use a `for...of` to loop over it, or we can manually do it with the generator by calling `.next` on it.
