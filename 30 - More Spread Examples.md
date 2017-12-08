Let's take a look at some more spread examples. One use case for a spread is an alternative to `array.From`, which we learned about a couple of videos ago. 

Let's say we want to select all of these people right here:

```html
<div class="people">
<p>Wes</p>
<p>Kait</p>
<p>Randy</p>
</div>
<script>
const people = document.querySelectorAll('.people p');
console.log(people)
</script>
```

 
In the console, we get all of our three people, and we know that it's a NodeList. 

But if I wanted to call `.map` against `people`:

```js
const names = people.map();
```

We aren't able to do that because `people.map()` is not an actual function. If we `console.log(people)` we can see in the inspector that it's a NodeList, and the prototype doesn't have our `.map` or `.reducer` or any of our arrays. 

You could use `Array.from` to convert the NodeList to a true array:

```js
const people = Array.from(document.querySelectorAll('.people p'));
console.log(people)
const names = people.map();
```

Quite honestly, I think that I prefer `Array.from`, just because it reads a little bit nicer. 

However, another way you could do it is you could immediately take every single item that it selects, and spread it right onto an array.
 
```js
const people = [...document.querySelectorAll('.people p')];
console.log(people)
const names = people.map((person) => person.textContent);
```

Using spread, we can take everything that it selects in our `document.querySelectorAll`. Why? Because a NodeList is an iterable, and every single item from that iterable can then be spread onto this new array. Then you see that we have a proper array here too.

It's really up to you what you like to use. If you're probably on a team, `Array.from` probably reads a little bit nicer. But I have seen people do it the other way, and that would give us our `.map` function as we like it. 

As a note here, to run our `.map` function, we have to run person and get `person.textContent` to form our array of names.

A couple of other examples are when you want to create a new array off of a property on an object. 

This is similar where we merged two arrays together.

```js
const deepDish = {
    pizzaName: 'Deep Dish',
    size: 'Medium',
    ingredients: ['Marinara', 'Italian Sausage', 'Dough', 'Cheese']
};
```

We need to make a pizza tonight, and milk to make the pizza, we know we have to get flour. But we also know that since tonight is deep dish pizza night, we have to get all of the ingredients from it.
 
```js
const shoppingList = ['Milk', 'Flour', ...deepDish.ingredients];
console.log(shoppingList);
```

What we can do is just immediately spread all of the ingredients from deep dish.ingredients. Since the `ingredients` property is an array, it's going to take every single item from that and put it into our `shoppingList`. In the console, you'll see that we now have every single item in it. That is a true copy, not a reference to the `deepDish.ingredients` array.

This is an example where I've run into a couple times. Specifically I do it a lot in React when using Redux. It's when you have an array of objects and you need to remove one of those objects from the actual array:

```js
const comments = [
 { id: 123456, text: 'I love your dog!' },
 { id: 789101, text: 'Cuuuuuute!' },
 { id: 234567, text: 'You are so dumb' },
 { id: 891011, text: 'Nice work on this Wes!' },
];
``` 
Because it's a mean comment, I want to delete, "You are so dumb." 

All you're given is an `id` variable with that post id inside of it. How do you actually take it away? First of all, we need to figure out where in this array the comment is, because `id` doesn't tell us anything about its location inside the array.

How would we do that? We would use the `.findIndex` method of the array.

```js
const id = 234567;
const commentIndex = comments.findIndex(comment => comment.id === id);
```

When the comment ID is equal to the ID variable we have set here, then return. 

That's going to tell us where the index is. Let's `console.log` the comment index, see where it tells us.

```js
console.log(commentIndex);
```

Good, it tells me the index is 2, so in our array, we find it at index 2, which is the third position, remember.

We've found the one we want to delete, but how do I actually delete it from there? Previously, what we had to do is `slice` everything before, `slice` everything after. 

What we can do here is we will make a new comment variable, and make an array.

```js
const newComments = [];
```
 
We want everything up until the mean comment, and everything after the mean comment. What we can do is `slice` from the start to where we want, and then `slice` from one after the one that we want, to the end.

```js
const newComments = [comments.slice(0, commentIndex), comments.slice(commentIndex + 1)];
```

Because we're using `commentIndex + 1`, it will automatically pick up and keep listing the remaining comments. Even if I had 10 more comments, it would go to the end of it.

But this method isn't going to totally work and I'll show you why. 

If we `console.log` our `newComments`, we'll wee that we have an array of arrays, which has arrays that contains the first part and the second part. That's not very useful because you don't really want an array of arrays. You want an array of comment objects. 

What we can do is just spread these two and spread this one, and that will take every item out of the array and put it into this new comment array right here:

```js
const newComments = [...comments.slice(0, commentIndex), ...comments.slice(commentIndex + 1)];
console.log(newComments);
```

Now when I refresh it, you'll see this is our new comments array, that you have the new comments, and we've excluded that mean comment that we have sliced on out. 

If you're doing something like react, you can pass the new comments:

```js
const newComments = [...comments.slice(0, commentIndex), ...comments.slice(commentIndex + 1)];
this.setState({comments: newComments })
```

Then, it's going to update your new state without mutating the past state.
