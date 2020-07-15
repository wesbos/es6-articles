Last post we took a look at [an intro to destructuring](http://wesbos.com/destructuring-objects/). Let's take a look at another use case which would be renaming your variables. Sometimes data comes back in some odd names, and you might not necessarily want to use a property key as the end variable name. Maybe you don't like that variable name or it's already taken in your scope.


```js
const twitter = 'twitter.com';
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

For example here, I already used twitter as a variable. I can't use it again, but I'm stuck, because this object gives me twitter as a key. What you can do is you can rename them as you destructure them.

So - I want the `twitter` property, but I want to call it `tweet`. I want the `facebook` property, but I want to call it `fb`.

```js
const { twitter: tweet, facebook: fb } = wes.links.social;
```

The above code will pull the `wes.links.social.twitter` into a variable called `tweet` and similarly for `facebook`. 

