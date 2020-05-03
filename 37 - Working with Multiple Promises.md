The last example we 'stepped', which is to say that we waited for the post to come back before we actually found the author because we needed to know who the author of the post was before we could hydrate the author. The first thing needed to happen before the second thing could happen. It's sort of waterfall approach. 

In some cases, you just want to fire them all off at the exact same time, because they're not dependent on each other, and you want to get two things back, or four things back, as soon as possible.

I've got an example for `weather`, where I'm going to fetch the weather, and it's going to come back after two seconds, 2000 milliseconds, with a temperature of 29, Sunny With Clouds. 

Then I also want to go get my `tweets`. I'm not searching for tweets based on weather or anything like that. The tweets have nothing to do with the weather, other than I just need to get these two pieces of data. This comes back after 500 milliseconds, half a second.


```js
const weather = new Promise((resolve) => {
    setTimeout(() => {
        resolve({temp: 29, conditions: 'Sunny with Clouds'});
    }, 2000);
});

const tweets = new Promise((resolve) => {
    setTimeout(() => {
        resolve(['I like cake', 'BBQ is good too!']);
    }, 500);
});
```

The way that we can do that, instead of chaining `.thens` together, we can say `Promise.all`, and you pass it an array of `promise`s, so in this case, `weather` and `tweets`, and you call `.then` against that. Then when that comes back, we are going to get our `responses`. 

```js
const weather = new Promise((resolve) => {
    setTimeout(() => {
        resolve({temp: 29, conditions: 'Sunny with Clouds'});
    }, 2000);
});

const tweets = new Promise((resolve) => {
    setTimeout(() => {
        resolve(['I like cake', 'BBQ is good too!']);
    }, 500);
});

Promise
    // formatting .all and .then on separate lines helps make your code readable
    .all([weather, tweets])
    .then(responses => {
        console.log(responses);
    });
```
[2:04]

In the console, you should notice that it takes two seconds to come back. This is because every `Promise` in the function has to finish before we can run the `.then` to get a result. In this case, our longest `promise` takes two seconds. So for example, if a `promise` takes 10 seconds, your `Promise.all` is going to take 10 seconds to resolve. If one takes 15 seconds, it's going to take 15 seconds. The slowest response decides how long these things should actually take.

In our case, the console should return an array of `[Object, Array[2]]`, where the first item in the array is our `weather` object, `"Sunny with Clouds"` and `temp: 29`, and our second item is our array of `tweets`.

If we wanted to clean things up a bit, we could also do something like this:

```js
Promise
    // formatting .all and .then on separate lines helps make your code readable
    .all([weather, tweets])
    .then(responses => {
        const [weather, tweets] = responses;
        console.log(weather, tweets)
    });
```

Now, we have two separate variables, one with our `weather`, one with our `tweets` in it, and we can go ahead and start populating the data on our actual home page.

However, it's probably not a good idea to name this `weather` and `tweets`. Why? Because our `promises` are named `weather` and `tweets`, so maybe call it `weatherInfo` and `tweetsInfo`. 


Let's actually do some with some real data here.

We need two APIs. If you work with an API during the day, I encourage you to go grab that API. Otherwise, you can use the ones I've got right here:

```js
const postsPromise = fetch('http://wesbos.com/wp-json/wp/v2/posts');
const streetCarsPromise = fetch('http://data.ratp.fr/api/datasets/1.0/search/?q=paris');
```

I've got `postPromise` here, which is going to go to my blog and grab all of my latest posts, which I've used before. We also have the `streetCarsPromise`, which is going to go and fetch some data from the Paris transit system.

We need to resolve each `promise`, or rather, they will resolve themselves, but we need to listen to when they are both resolved.
 
 ```js
const postsPromise = fetch('http://wesbos.com/wp-json/wp/v2/posts');
const streetCarsPromise = fetch('http://data.ratp.fr/api/datasets/1.0/search/?q=paris');

Promise.all([postsPromise, streetCarsPromise])
       .then(responses =>  {
           console.log(responses);
       })
``` 
  
Before we go to run this, you actually can't run this locally on your file system. So if we want to be working with `fetch` API, you need to be running it through some sort of server.

I actually have a `browser-sync` command I use to do this from the directory I've got this file saved in:

`browser-sync start --directory --server --files "*.js, *.html, *.css`

You can get `browser-sync` if you don't already have a way of spinning up a server, and you can get it by typing `npm install -g browser-sync` in your terminal to install it as a global package.


So once you get that all going, let's go ahead and take a look at our console for `responses`, which we'll find is an array of two things. 

We got the first response that comes back from Wesbos.com, and we got the second response that comes back from the Paris transit system, but we don't see the data in our `body`... we actually have a problem where we have to convert a readable stream into json, right? This is the problem we had on the first example. 

Here's how to do that with two things: 

 ```js
const postsPromise = fetch('http://wesbos.com/wp-json/wp/v2/posts');
const streetCarsPromise = fetch('http://data.ratp.fr/api/datasets/1.0/search/?q=paris');

Promise.all([postsPromise, streetCarsPromise])
       .then(responses =>  {
           return Promise.all(responses.map(res => res.json()))
       })
``` 


What we can do is we can return a promise.all again, and then we will take each of these things, which is the responses, and we can just map over them and call .json on each one. 

We'll say response, and we will return response.json. Why do we have to call this res.json? Why can't we just say json.parse around, res.body or something like that? 

We actually use `res` instead of `responses` here, because there are many different types of data that could come back. If you [check out the documentation on MDN](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch), you can see that the data can come back as an `.arrayBuffer()`, `.blob()`, `.json()`, `.text()`, or `.formData()`. 

Don't just assume that your APIs or your AJAX requests are always going to be json, because it could be any of those data types.

What are we doing? Our response is, in this case, in an array, and `.map` takes every item out of an array, does something to it, and then returns a new array. 

What we're doing here, is we're taking the array of `responses` and taking each one and calling `.json()` on it. This returns a second promise, which we can call `.then` on, and that should then, responses, that should give us some real data:
 
 ```js
const postsPromise = fetch('http://wesbos.com/wp-json/wp/v2/posts');
const streetCarsPromise = fetch('http://data.ratp.fr/api/datasets/1.0/search/?q=paris');

Promise.all([postsPromise, streetCarsPromise])
       .then(responses =>  {
           return Promise.all(responses.map(res => res.json()))
       })
       .then(responses => {
           console.log(responses);
       })
``` 
So if we open this up, we'll see our API data in an array. In this case, we got about 10 posts, and an object containing some information about the Paris transit system.

So what we've done overall here is using a `Promise.all` on our initial promises, `postsPromise` and `streetCarsPromise`. Then when both of those promises come back, we run `.json()` on all of them. Then, when both of those come back from being turned from just regular data into json, which is instant, then this final `promise` is called, then we can do whatever we want with that data.