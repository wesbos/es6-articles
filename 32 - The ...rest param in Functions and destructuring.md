Whenever you see three little dots, it's not just a spread. It could also be what's called a **rest param**. Even though it looks to be the exact same thing, it's actually the exact opposite thing. Let's think about that for a second.

If the **spread** param takes one thing, which is an array, and **unpacks it into multiple items**, or takes a string and unpacks it into multiple items of an array, the **rest** param does the exact opposite. It takes multiple things and **packs it into a single array**.

There are two places where you'll use a rest param. That is first in a function, and second in a destructuring situation. 

Let's say we have a function called `convertCurrency`, and it takes two things: a `rate` which is known, but it's also going to take a unknown amount of currency. Here we don't really know how many the person is going to pass. 

Now you might be saying, "That's fine, don't pass any arguments there, and use the arguments object." The problem here is that we actually want the first thing to be the `rate`, and then the rest of them to be the amounts that the person would like to convert. 
 
Let's say we're going to convert currency, and we want to convert at $1.56 per dollar. Then we want to pass a whole bunch of dollar values that we have. Before now, there wasn't really a great way to do that, but now we have the rest param, which can pack the rest of them into an array:

```js
function convertCurrecnty(rate, ...amounts){
    console.log(rate, amounts);
}

convertCurrecnty(1.56, 10, 23, 52, 1, 56);
```


In the console, the first value is just the `rate` that was passed in. But because we did a rest param for `amounts`, it captured everything else that was passed to this function. If we run it again with only one item:

```js
 function convertCurrecnty(rate, ...amounts){
     console.log(rate, amounts);
 }
 
convertCurrecnty(1.56, 10);
```
 
Here we get our `rate`, and an array of one actual item, in this case, `10`. 

That's really handy because we can do something like this:

```js
  function convertCurrecnty(rate, ...amounts){
      return amounts.map(amount => amount * rate);
  }
  
const amounts = convertCurrecnty(1.56, 10, 23, 52, 1, 56);
console.log(amounts);
```
We should be able to get an array of all of those converted currency values. 

You can use as many arguments as you need. If you had `rate`, and `tax`, and `tip`, and then amounts, what that would give us is three things. Let's take a look here:
 
```js
  function convertCurrency(rate, tax, tip, ...amounts){
     console.log(rate, tax, tip, amounts); 
     return amounts.map(amount => (amount + amount * tax + amount * tip) * rate);
  }
const amounts = convertCurrecnty(1.56, 0.13, 0.15 10, 23, 52, 1, 56);
``` 
In the console we can see that we get our `rate`, `tax`, and `tip`, and if you open it in your inspector you'll see that it's a true array, not an arguments object, or anything weird and array-ish. It's a true array.
  
So that's the first use case for rest param.

As a second use case, we can use it when we're destructuring. We took a look at this a whole bunch of posts ago, but let's take a look at a example again.

Let's say that we have an array of data that came from a jogging application, and it gives us some data:

```js
 const runner = ['Wes Bos', 123, 5.5, 5, 3, 6, 35];
```
So we have a name, a runner ID, and a few runs, including that big 35k. That's some comma separated data that we would get in. We want to destructure this data in, assuming the first item will be the name, the second item will be the runner ID, and then the rest of the items are going to be a log of all their runs. 

We could destructure that data by doing this:

```js
 const runner = ['Wes Bos', 123, 5.5, 5, 3, 6, 35];
 const [name, id, runs] = runner;
 console.log(name, id, runs);
```
 
So first, without the rest param, we get `Wes Bos`, `123`, and `5.5`, because that's just grabbing the third thing. 

What if I wanted the rest of them? We do ...runs, and that should bundle up everything that is left in the array as you are destructuring it:

```js
 const runner = ['Wes Bos', 123, 5.5, 5, 3, 6, 35];
 const [name, id, ...runs] = runner;
 console.log(name, id, runs);
``` 
 
All of our run data to the end will be bundled up into an array, and you could loop over that and use that data.


We looked at another example earlier, where we had a team array. The first person would be the captain, second person would be the assistant captain, and then the rest of the people would be on the actual team. We could do something like this:

```js
 const team = ['Wes', 'Kait', 'Lux', 'Sheena', 'Kelly'];
 const [captain, assistant, ...players] = team;
 console.log(captain, assistant, players);
```
So if we run that, we'll get `Wes`, and `Kait`, and because we used the rest param, we get an array of players, with `Lux`, `Sheena` and `Kelly`.

The rest param might not be something you're going to use all the time, but it really helps you when you don't have to do any splicing or counting on indexes. You can just say, "Just give me the rest," for either a function definition, or for when you're destructuring an array.
