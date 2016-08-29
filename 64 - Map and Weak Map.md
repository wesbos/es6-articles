# JavaScript Maps

The new ES6 Sets are to arrays as ES6 Maps are to objects. A map works very similar to a set, however it has a key and a value instead of just values. 

If you haven't already, make sure you first watch the [ES6 JavaScript Sets video](https://ES6.io) or the article. 

Right here, I've gone ahead and created a new map, which is dogs, and I've set Snickers to 3, Sunny to 2, and Hugo to 10. We'll talk about the actual values that this can take. Spoiler -- both the keys and the values can be absolutely anything. 

There is a very simple API for actually working with it. We called .set() here when you want to set something on it. Let's bring it over to here. dogs.has(Snickers), that will be true. You use .has() and you use the key of it. You can use dog.get() to actually get the item. Whereas with sets, we weren't able to reference it at all. You can use dogs.delete(Hugo) to see who's in there. Now, dogs is just a map of Snickers and Sunny.

One interesting thing about maps is that you can loop over them in two ways. First of all, we can use dot for each, and that will give us a value and a key. You can console.log() the val and the key, and that will give us the value of the key, value of key, value of key.

We can also use our trusty for-of loop that we've been using all over this series. For const dog of dogs, open that sucker up, console.log(dog). What's interesting about that is it actually gives us an array, where the first item is the key and the second item is the value.

You might be thinking, "I know what to use for that. We can use the actual destructuring here.{ Instead of saying key dogs, we can say key in val, and then we have a key and a val, separate variables here, that log out separately for us. You can use dogs.clear() to get rid of them, or you can individually delete one with .delete().

That's interesting about a map, but why is that better than actually using an object? It seems like it has a little bit of a limited API. The one benefit here is that you can use your for-of loop with a map, but that is also coming to objects sometime in the future of JavaScript.

In the next video, what I want to do is show you how you can actually use a map for what's called metadata.
