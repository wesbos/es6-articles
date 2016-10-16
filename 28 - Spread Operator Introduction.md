Let's talk about the **spread** operator. Those are those three apathetic dots, `...` , that you'll often see in front of an array or any iterable. What is the spread operator? It takes every single item from an iterable. 

Now hold on. What's an iterable?

An iterable is anything that you can loop over with our `for of` loop and that includes arrays, strings, DOM nodes, arguments object, and includes `Map` and `Set` which we are going to learn about later. 

We'll use strings and arrays, because that's the simplest and most of the uses cases where you'll actually use spread.

It's going to take every single item from an iterable, from array and apply it to the containing element or the containing array. 

Where is that useful? Let's say we've got a couple of arrays of pizza.

```js
const featured = ['Deep Dish', 'Pepperoni', 'Hawaiian'];
const specialty = ['Meatzza', 'Spicy Mama', 'Margherita'];
```



featured and the specialty. I want to make a third array called pizzas, just combine them together. You might think, OK, we're going to use something like:
 
```js
const featured = ['Deep Dish', 'Pepperoni', 'Hawaiian'];
const specialty = ['Meatzza', 'Spicy Mama', 'Margherita'];
const pizzas = featured.concat(specialty);
```
 
That will work, and you can `console.log(pizzas)` to see all six pizzas in an array. 

What if you wanted to put a veg pizza right in the middle of that? How would do that? Well, you'd have to say:
 
```js
let pizzas =  [];
pizzas = pizzas.concat(featured);
pizzas = push('veg');
pizzas = pizzas.concat(specialty);
console.log(pizzas);
```
That just becomes a big headache for having to manage that. What you can do is take every single item in the array and **spread** into a new array.

Let's take a quick example with a string first. 

If I have the string of `wes`, what is every item in a string? Every item in a string is just each character. What you can do is you pop an array around it, and that just gives you an array of one thing, `wes`.

```js
['wes']
```
What if I wanted every single item of that iterable to be its own item of the array? We can use the spread operator in front of the array.

```js
[...'wes']
```

That's going to spread each item into the actual array. I have `['w', 'e', 's']` as the entire array here. Similarly, what we can do is, we can take these existing arrays and spread the items, like `featured`, and spread the items of `specialty` into the new `pizzas` array. 

```js
const pizzas = [...featured, ...specialty];
```


Now I have the `pizzas` array, which has absolutely every single pizza inside of it. Similarly, I can just also add `vegetable` in the middle of the array:

```js
const pizzas = [...featured, 'veg', ...specialty];
```


It's not a problem because I'm essentially just saying, add every single item from `featured`, add `'veg'`, add every single item from `specialty`. If we call the `pizzas` array, we see that our `'veg'` pizza being added right in the middle. 

What's also nice about that is that, is say we wanted to take a copy of these pizzas:
 
```js
const pizzas = [...featured, 'veg', ...specialty];
const fridayPizzas = pizzas;
console.log(fridayPizzas)
```

What if I wanted to change the first `fridayPizza` from `'Deep Dish'` to something like `'Canadian'`? 

```js
fridayPizzas[0] = 'Canadian';
console.log(fridayPizzas);
```

Now our `fridayPizzas` starts with `'Canadian'`, but if we `console.log(pizzas)`, we've overwritten the original array!
 
That's a pain, because what we did there is we didn't actually copy the `pizzas` array, we just referenced it. Both `pizzas` and `fridayPizzas` are the exact same thing in this case.

What you used to have to be able to do is you just create a blank array and then you would `concat` the `pizzas` one right into it:
 
```js
let fridayPizzas = [].concat(pizzas);
``` 
 
That would just take a actual copy of the array rather then reference it. That's weird as well.

What you can do is, you just take a brand new array and then you spread every single item from `pizzas` into this hot, fresh new array:

```js
const featured = ['Deep Dish', 'Pepperoni', 'Hawaiian'];
const specialty = ['Meatzza', 'Spicy Mama', 'Margherita'];

const pizzas = [...featured, 'veg', 'specialty'];
const fridayPizzas = [...pizzas];
fridayPizzas[0] = 'Canadian';

console.log(fridayPizzas);
console.log(pizzas);
```

If we take a look in the console, we can see that `fridayPizzas` lists `'Canadian'` as its first pizza, but our `pizzas` still lists `'Deep Dish'`. We were able to change `fridayPizzas` without being destructive to the initial `pizzas` array.

Again, a spread will take every single item from an iterable and apply it into the new array.