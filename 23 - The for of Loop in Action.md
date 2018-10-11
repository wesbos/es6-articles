Let's take a deeper look into when an iterable actually is. 

```js
const cuts = ['Chuck', 'Brisket', 'Shank', 'Short Rib'];
    
for (const cut of cuts) {
    console.log(cut);
}
```

Here I've got my array, and I'm looping over it, and I'm using `console.log` to show `cut`. No big deal.
 
But what if I want to get the index of the actual cut as well to say `Chuck` is first, `Brisket` is second, and so on? There's no real good way to do that right here unless I were to say `cuts.indexOf` and find it every single time.

What we don't know about yet is that there are these things called generators - and we're going to go into a whole bunch of that later.

Over in your console, type:

```js
cuts.entries();
```

What that is going to give us is actually an `arrayIterator`. If you open it up you'd expect to see all the data, but there's actually nothing in there. All you know that there's a `next` function that we can call on it.

So if we actually store that array cuts.entries in a variable:

```js
const meat = cuts.entries();
```

This will also give us the `arrayIterator` and we can iterate through each of those ones manually. This loop was doing it for us, but if we ever wanted to do it manually - and we'll look at more examples when we hit generators - you call it by using `meat.next()`. Do that a few times, and let's see what we get in the console.

Click into it, and you'll see when we called it we got `done: false`, and that means that the loop hasn't finished -- more on that later in the generator post. 

Then `value: Array[2]`, and the first thing is going to be the index, `0`, and the next thing is going to be an actual value, `"Chuck"`.

If you click into another one, you'll find `Brisket`, `Shank`, and `Short Rib`, as well. 

It tells us what the `index` is and what the `Short Rib` is. Why would this be useful to us with `for of`? Because what you can do is iterate over not just the plain array, but we can also iterate over the `arrayIterator` which is `cuts.entries`, which will return our cuts with their index, `0`, `1`, `2`, and so on with their value as an array.
 
### A Dash of Destructuring 

Now you might be saying, "That's annoying because now I have to say `cut[0]` and `cut[1]` to get the index and the value." But hopefully you're screaming at me right now and you're going to say, "Well, Wes I can use destructuring there, right?" 

Absolutely, and what we can do is we can say:
 
```js
const cuts = ['Chuck', 'Brisket', 'Shank', 'Short Rib'];
    

for (const [i, cut] of cuts.entries()) {
    console.log(`${cut} is the ${i + 1} item`);
}
```
Go ahead and run that, and you'll see `Chuck is the 1 item`, `Brisket is the 2 item` and so on.

## Iterator Review

What are we doing here? We're using `cuts.entries()` to bring us an iterator. What's great about `for of` is that it can handle almost anything that you throw at it. 

You don't have to think, "What kind of data is this? Which syntax of mine like `for in` or different loops that I have available to me should I use?" 

In almost all cases, except for objects, you can just use your `for of`, and just throw anything at it, and the `for of` loop is going to just figure out how to handle that data for you. So here I've destructured the data as we actually go on in, and we're off and running with that.

## `for of` with `arguments`

Let's look at another example where `for of` is useful, and that is when you're trying to iterate over the `argument` objects. 

```js
const cuts = ['Chuck', 'Brisket', 'Shank', 'Short Rib'];

function addUpNumbers() {
    
}


addUpNumbers(10,20,42,62,598);

console.log(addUpNumbers)

```

Basically, you have no idea how many numbers are going to get passed. Maybe some of this is going to pass a whole bunch of them. 

When you're declaring the function, you can't really like put a whole bunch of placeholders as arguments, like `addUpNumbers(num1, num2, num3)` and so on, just in case they past that many, because you have absolutely no idea how many arguments are going to pass. Instead of putting anything in there, what we can do inside of this function is to just console.log this variable called `arguments`:

```js
const cuts = ['Chuck', 'Brisket', 'Shank', 'Short Rib'];

function addUpNumbers() {
    console.log(arguments);
}


addUpNumbers(10,20,42,62,598);

```

Now that is a special word and it's going to be in kind of an array-ish format where it will give us all of the actual arguments. 

That arguments is now what looks to be an array, it's not exactly an array of everything that got passed in. The reason I'm so bent up on saying it's array-ish is because if you open it up in your console, you'll see a couple of things right here. First of all, the `prototype` is not `array`.

If you `console.log` a regular array and open it up in the console, you'll see that the `prototype` is `array`, and all the crazy array methods that we're used to, like `map` and `push` and everything we could possibly want, right? Because that has built up a proper array.

Now, `arguments` is simply just a list with only one thing on it: `length`. However, you can iterate over it with the `for of` loop because it has a `Symbol.iterator`. 

Normally, in a case like this, I would probably just convert `arguments` to a real array and use `reduce` on it and add them all up. But for whatever reason, if you're ever in the use-case where you cannot convert your `arguments` to an object for some performance reasons, or whatever you have, then you can go ahead and loop over arguments without first converting to an array. There's no need to convert it to an array if you just need to loop over them.

So let's loop over each of them in a `for of` loop:

```js
const cuts = ['Chuck', 'Brisket', 'Shank', 'Short Rib'];

function addUpNumbers() {
   let total = 0;
   for (const num of arguments){
       total += num;
   }
   console.log(total);
   return total; 
    
}


addUpNumbers(10,20,42,62,598);

```

What I'm going to do is I'm going to say `let total = 0`, start with zero and then I'm going to loop over each of them. So I'm going to say for every `num` in `arguments` `total += num`, and then we should be able to return the total here. That should add them all up, let's see. I'm going to console.log total as well just so we can see it.

And we get our total, `267`, because I created a variable and it looped through it and updated every single one of them. Now, if I ever want to call add up numbers, we can just pass in any numbers we like by calling `addUpNumbers(10,10)` or `addUpNumbers(20,20)` or whatever we want. That is one example of what you could loop over it.

You do not need to convert `arguments` to a true array, you can just use the `for of` to iterate over it. 

### `for of` with Strings

What else can we use to the `for of` to iterate over? How about a string?

```js
const name = 'Wes Bos';
for (const char of name){
    console.log(char);
}
```

As a note, you have make sure you put a `const`, `let`, or `var` in front of your variable in the `for of` loop, otherwise you're going to be overwriting the actual variable every single time instead of creating a `let` or a `const` that is scoped to that actual block.

### `for of` with NodeLists and HTMLCollections

Finally, you can also loop over DOM collections without having to convert them to a true array. DOM collections, or `NodeList` collections, or HTML collections, whatever you're going to call them, they are being changed so that you will have all of your `.forEach`, and `.map`, and all of those array methods that you're used to. However, in most browsers they are not a true array, so we could use the `for of`, no problem there.

```html
 <p>Hi I'm p 01</p>
 <p>Hi I'm p 02</p>
 <p>Hi I'm p 03</p>
 <p>Hi I'm p 04</p>
 <p>Hi I'm p 05</p>
```

We've now got a whole bunch of paragraphs on the page here, and we want to select them:

```js
const ps = document.querySelectorAll('p');
console.log(ps);
```

If you open pen it up in the console, you can see that it's not a true array, it is a `NodeList`, but if you open that up you will see that we have `.forEach` as well as a couple other of the array methods that we're used to.

Let's loop over them:

```js
const ps = document.querySelectorAll('p');
for (const paragraph of ps) {
    console.log(paragraph);
}
```

You see that you'll get each one here, and that's useful if you want to do something like an `addEventListener` to show the contents of your `<p>` tag:

```js
const ps = document.querySelectorAll('p');
for (const paragraph of ps) {
    paragraph.addEventListener('click', function() {
        console.log(this.textContent);
    })    
}
```

Again, you totally can convert things to an array. I'm not sure of a huge performance difference between the two. You can do your own testing on that if you want, but it's helpful to know that you can use this `for of` with things that aren't arrays, as long as they're an iterable, like a DOM collection, argument, a string, an array, a map, or a set.
