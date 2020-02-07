# Destructuring Arrays with ES6 JavaScript

Did you know destructuring also works with arrays? 

Sometimes we have data where it's based on its index - or it's place in the array. Let's say you want to get the first, the second and the third thing out of that array.

Traditionally we might be able to do something like this:

```js
const names = ['Wes', 'Kait', 'Lux'];
const dad = names[0];
const mom = names[1];
const daughter = names[2];
```

But this is no way to live your life!

What we can do is use **array destructuring**. Instead of using curly brackets like we did with object destructuring we use `[square brackets]`. When you pull from an array, you use square brackets to destructure. 

Similarly, we're going to learn about maps and sets in future [ES6.io](https://ES6.io) videos. When you pull from a map, you'll use curly brackets. When you pull from a set, you'll use square brackets. 

Let's say we have some data: `const details = ['Wes Bos', 123, 'wesbos.com'];`

We can use destructuring to pull each item into it's own variable. 

```js
const [name, ID, website] = details;
console.log(name, ID, website); // Wes Bos, 123, wesbos.com
```

### Destructuring Dirty Data

Cool — That is very helpful when you have index-based data. It could also be helpful when you have a comma-separated list or string. Let's say we're given some data in a string that looks like this:

```js
const data = 'Basketball,Sports,90210,23';
```

That's the item's name, category, SKU, and inventory left on hand. The problem is that it's just a string. That's not really helping us out. We do know that one thing that we can do if the data is perfectly separated by some sort of separator is we could use `data.split()` to split it up based on the comma. 

```js
const [itemName, category, sku, inventory] = data.split(',');
```

Why? `data.split(',')` is going to return in array, and then immediately we're going to destructure that array into its own four variables.

If I console log those variable, we should see **"Basketball Sports 90210 23"**. Good. I'm really happy with that. One question, though, is what happens if the data is not great, and you have some extra stuff on the end of the string?

```js
const data = 'Basketball,Sports,90210,23,wes,bos,cool';
```

What happens to them when you destructure something that is not the same length as the actual array? Well, nothing happens, because it will just throw those extra ones out! So `[wes, bos, cool]` are not stored in any variables and are garbage collected.

### A Look Ahead to ES6 Destructuring with `...rest`

We're going to learn a more about the JavaScript `...rest` in future videos and blog posts, but there is a helpful use case in using rest with destructuring syntax. Let's say I have a team, and I want to know who's the **captain**, who's the **assisting captain**, and who is **the rest of the actual team**? I'm going to make myself a quick array here:

```js
const team = ['Wes', 'Harry', 'Sarah', 'Keegan', 'Riker'];
```

I've got a list of names here. 'Wes' is the 'captain', 'Harry' is the 'assistant captain', and then the rest of is just going to be part of the team. How would you destructure that into captain, assistant and then the rest?

We use what's called the 'rest operator', and it does exactly what it says. It just gives you the rest of them. Let's take a look now. What's 'players'? 'players' is an array of whatever's left. 

```js
const [captain, assistant, ...players] = team;
```

Sometimes you want to grab the first, the second, and the third, and then you just want to bundle whatever else is on the end of that array into the rest of the array. That is where you can use the 'rest operator'. 

We're going to talk much more about this. You probably may also have heard of this 'spread operator' which looks exactly the same. We'll dive into that in future videos! See ya there :) 
