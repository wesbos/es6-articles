A really great use case for spreads is when you spread into a function. Now, let's take a look at this example. I've got my inventors array right here, and I want to take the items from newInventors and put them into Inventors. I don't want a third array. I don't want to overwrite the entire array. I just want to tack them onto the end.

```js
const inventors = ['Einstein', 'Newton', 'Galileo'];
const newInventors = ['Musk', 'Jobs'];
inventors.push(newInventors);
```

You might think, "OK, we'll just take the `inventors` and we'll call `push()` on it, and we'll pass it the new inventors." Let's see what that gives us. 

If we do that, we'll get our first three `inventors`, and then we have the fourth item as an array of two things, our `inventors` array and our our `newInventors` array, `'Musk'` and `'Jobs'`. That's not what we want. 

We essentially just did this. We took the array and added it as a fourth item:
```js
const inventors = ['Einstein', 'Newton', 'Galileo', ['Musk', 'Jobs']];
```
Which is an array in an array, which we don't want. 

A way you would have solved this in the past is by using `apply`, like this:

```js
const inventors = ['Einstein', 'Newton', 'Galileo'];
const newInventors = ['Musk', 'Jobs'];
inventors.push.apply(inventors, newInventors);
```

That will work, and I'll tell you why. 

What's happening here is when you call `apply`, it's going to run the function that you called `apply` against, in this case `push`, but every single item of the array is used as an argument.

It's essentially doing this: `inventors.push('Musk', 'Jobs)`, because you can't pass push an array. You can pass push as many arguments as you wish. 

That's a little bit funky, and I've seen a lot of people scratch their heads looking at something like that, where you have Inventors twice, and then you call this weird apply thing on it? It's a little bit hard to actually understand.

Rather than that, what we can do is you just simply call `push`, and we only pass it the `newInventors`, we'll get the problem we have before, with the fourth item in our `inventors` array being an array.

However, if you were to spread into it, what will that do? Spread is going to take `'Musk`' and `'Jobs'` to pass it as individual argument into `push`.
```js
const inventors = ['Einstein', 'Newton', 'Galileo'];
const newInventors = ['Musk', 'Jobs'];
inventors.push(...newInventors);
```
 
Now we don't have to worry about any of that apply, or this, or any of these things. 

We simply just spread right into the functions. We've been spreading into arrays, but you can also spread into a function where every single item of the array is going to be used as an argument. 

Let's build another example ourselves where we have a function that says in an alert "Hey there first last." 

```js
const name = ['Wes', 'Bos'];
function sayHi(first, last) {
    alert (`Hey there ${first} ${last}`)
}
```
For whatever reason, we've got an array, and we need to pass argument one, argument two. You could do this: 

```js
sayHi(name[0], name[1]);
```

Which will work, but that doesn't really work that well, especially if you had a whole bunch of arguments that you need to pass in. 

What we could do instead of passing the two individual arguments is you just spread the array:
 
```js
sayHi(...name);
```
 
And that will pass `'Wes'` as the first argument and `'Bos'` as the second argument. So it will say, "Hey there, Wes Bos," without having to do any of that weird square bracket stuff. 

Spreading into a function is very, very handy.
