A WeakSet is just like a set, except there are a number of limitations, or I guess you could say benefits to using a WeakSet in some situations.


```js
let dog1 = {name: 'Snickers', age: 3};
let dog2 = {name: 'Sunny', age: 1};

const weakSauce = new WeakSet([dog1, dog2]);
```

We create it just like we would before, and you can pass it an iterable of items, `dog1` and `dog2`. I could also have done `weakSauce.add(dog2)` just like a regular set. 

If you check it out in your console by calling up `weakSauce.has(dog1)` and `weakSauce.has(dog2)` which will both return true.

The important thing that we need to know here about a WeakSet is that a WeakSet can only ever contain objects.

It cannot contain strings, or numbers, or arrays, or anything else. It can only ever contain an object. Now, we'll see why that's important in just a second. That's one thing that we need to know about it.

The other thing we need to know about it is you cannot enumerate, or loop over it, so we can't do something like:
 
```js
let dog1 = {name: 'Snickers', age: 3};
let dog2 = {name: 'Sunny', age: 1};

const weakSauce = new WeakSet([dog1, dog2]);

for (const dog of weakSauce) {
    console.log(dog)
}
```
 
That will throw an Uncaught TypeError telling us that symbol.iterator is not a function for `weakSauce`. There is no iterator on a WeakSet like there is on an array, or on a set, or any of the other things that we can use a `for...of` loop on, so that's out the window.

You might be thinking, "Well, that's kind of weak. Why would I ever want to use that?" Really, the last thing that we need to know about a WeakSet is that there is no `.clear` method. If I have my `weakSauce` here, there is no `.clear` method that I can call against it, and I'll tell you why. 

WeakSets sort of clean themselves up. This has everything to do with garbage collection and memory. What that means is that when the reference to one of our `dogs` no longer exists, for example, if we were to delete `dog2`, then it would automatically be garbage collected, which means it would automatically removed from our WeakSet.

We have the set that is weakly held, which means that things can just automatically go away because if they're deleted in the code that you're running, then they are going to be also deleted from the set, so it's something like this:

```js
let dog1 = {name: 'Snickers', age: 3};
let dog2 = {name: 'Sunny', age: 1};

const weakSauce = new WeakSet([dog1, dog2]);

console.log(weakSauce);
dog1 = null;
console.log(weakSauce);

```

Notice how we have `console.log` before and after we set `dog1` to null. 

When does garbage collection happen? It really depends on your browser and a whole bunch of other things. There's no way to force it. But generally what you can do is you can wait for a couple seconds and you'll see that `dog1` has been removed from the set. 


Here we deleted `dog1` by setting it to null, but it might still be in the in the set for a second or two depending on your browser. 

In a few seconds, call up `weakSauce` and you'll see it's gone. You'll see how Snickers is no longer in there because we deleted it in our JavaScript, and the WeakSet ran a garbage collection that says, "OK, who of my items no longer exists? Because get out of here, we don't need you any longer. You've been garbage collected."

Not a whole bunch of use cases for that, but if you're storing like a DOM node or any type of other object, then you don't have to make sure that even though you delete the actual reference to the object, you don't have to worry about it also being in an array or something somewhere and causing a memory leak. You'll know that the WeakSet is going to take care of it for you.