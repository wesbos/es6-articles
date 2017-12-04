If the concept of a set still hasn't set in, let's take an example with brunch, because everyone absolutely loves brunch. Let's say it's a busy Sunday morning at a restaurant and people are coming in for brunch and they want to put their name on the list.

```js
const brunch = new Set();
// as people start coming in
brunch.add('Wes');
brunch.add('Sarah');
brunch.add('Simone');
```

We've created a variable called brunch, which is a new set. Then as people start coming in, we're adding them to the list.

Then we are ready to open. What we're going to do is take the values from our `const brunch` and start to use it.

```js
const brunch = new Set();
// as people start coming in
brunch.add('Wes');
brunch.add('Sarah');
brunch.add('Simone');
// ready to open
const line = brunch.values();
```
 
Now, we're working with `line` and we could say, "All right, who's up?" 

```js
const brunch = new Set();
// as people start coming in
brunch.add('Wes');
brunch.add('Sarah');
brunch.add('Simone');
// ready to open
const line = brunch.values();
console.log(line.next().value);
```

We're going to use the console to see `line.next().value` because `line.next()` is going to give us the generator item, and `.value` is going to be the actual item from our set. 

Over in the console, you'll see that Wes is up. Then if we've got room for another person, we'll just call that same code again:
 
```js
const brunch = new Set();
// as people start coming in
brunch.add('Wes');
brunch.add('Sarah');
brunch.add('Simone');
// ready to open
const line = brunch.values();
console.log(line.next().value);
console.log(line.next().value);
```
 
And you'll see in the console that Sarah is up.

If you call `brunch` in the console, you'll see that Wes, Sarah, and Simone are still in the brunch set. 
 
If you call `line`, Just Simone is actually left.

That's really interesting because when you call `next()` against `line`, it will remove itself from the SetIterator you can see in the console when you call `line`. However, the set, `brunch`, is sort of the gold list of everyone that has currently went through it.

Then people start showing up. We've seated our first couple customers and people are showing up, so we can just starting adding them to the initial set using `brunch.add`, then using `line.next().value` to iterate over the set.

```js
const brunch = new Set();
// as people start coming in
brunch.add('Wes');
brunch.add('Sarah');
brunch.add('Simone');
// ready to open
const line = brunch.values();
console.log(line.next().value);
console.log(line.next().value);
brunch.add('Heather');
brunch.add('Snickers');
console.log(line.next().value);
console.log(line.next().value);
console.log(line.next().value);
```

Notice how we're not adding them to `line`, we're adding them to the initial set `brunch`. 

We can just keep calling next() on it. We can call it `next` a few times, and as we do that, people are being seated: Wes, Sarah, Simone, Heather, Snickers. 

Even though I added these after creating `brunch`, which is our set, and creating `line`, we can still add people to the original set, and the iterator is still going to iterate on through to them.