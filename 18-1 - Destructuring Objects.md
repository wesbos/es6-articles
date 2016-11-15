## How to use Destructuring with Objects in JavaScript

Destructuring is a huge part of ES6. If you want to learn all about destructuring check out my [ES6.io](https://ES6.io) video tutorials.

Along with arrow functions, let, and const, **destructuring** is probably something you're going to be using every single day. I find it to be extremely useful whether I'm writing client side or Node. 

What does destructuring mean? It's a JavaScript expression that allows us to extract data from arrays, objects, maps and sets — which we're going to learn more about in a future [ES6.io](https://ES6.io) video —into their own variable. It allows us to extract properties from an object or items from an array, multiple at a time.

Let's take a look at what problem JavaScipt destructuring really solves. Sometimes you need top level variables like so:


```js
const person = {
  first: 'Wes',
  last: 'Bos',
  country: 'Canada',
  city: 'Hamilton',
  twitter: '@wesbos'
};
const first = person.first;
const last = person.last;
```

You get the point. You've got this pretty much repetitive code over and over again, where you need to make a variable from something that is inside of an object or inside of an array. What we could do instead of creating multiple variables, we destrucure them in a single like like so:

```js
const { first, last } = person;
```

Curly bracket on the left of the equals sign?! That is not a block. That is not an object. It's the new destructuring syntax.

The above code says, give me a variable called **first**, a variable called **last**, and take it from the `person` object. We're taking the `first` property and the `last` property and putting them into two new variables that will be scoped to the parent block (or window!). 

```js
console.log(first); // Wes
console.log(last); // Bos
```

Similarly, if I also wanted twitter, I would just add twitter into that, and I would get a third top level variable inside of my actual scope `const { first, last, twitter } = person;`

That's really handy in many use cases. This is just one nested level, but for example, in React.js often you want to use destructuring because the data is so deeply nested in props or state. 

Let's say we have some deeply nested data like we might get back from a JSON api:

```js
const wes = {
  first: 'Wes',
  last: 'Bos',
  links: {
    social: {
      twitter: 'https://twitter.com/wesbos',
      facebook: 'https://facebook.com/wesbos.developer',
    },
    web: {
      blog: 'https://wesbos.com'
    }
  }
};
```

I want to be able to pull out **Twitter** and **Facebook** URLs here. I could do this like it's 1994 and we're all running around with walkmans:

```js
const twitter = wes.links.social.twitter;
const facebook = wes.links.social.facebook;
// Annoying!
```

We can use destructuring to do it one better! 

```js
const { twitter, facebook } = wes.links.social;
console.log(twitter, facebook); // logs the 2 variables 
```

Notice how we destrucure `wes.links.social` and not just `wes`? That is important because we are destructuring a few levels deep. 
