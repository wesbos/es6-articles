In the next couple of posts, we're going to learn all about `promise`. In order to get the most out of these lessons, you need to know two things. 

First of all, `promise`s are often used when you're fetching a JSON API and doing some AJAX work. As an example, take a look at my [blog API](http://wesbos.com/wp-json/wp/v2/posts). You can see that it lists my blog posts as JSON.
 
Secondly, I'm going to be using a thing called `fetch` to be able to `fetch` in this JSON API. It's not a library, it's actually something built right into the browser. It returns a `promise`.

If you haven't heard of `fetch` before, it's very similar to `$.getJSON', if you've used jquery before, or '$.ajax', or any of these other AJAX libraries. Except, rather than having to load an external library, it's going to be built right into the browser.

Knowing that, let's jump into some examples of what a `promise` is. 

A `promise` is something that will happen between now and the end of time. A really nice way to think of that is something that will happen in the future, but probably not immediately.

In order to understand this, we need to be reminded that JavaScript is almost entirely asynchronous, which means that if we take some code here...

```js
console.log('Going to fetch the latest posts from Wes\' Blog');
const posts = fetch('http://wesbos.com/wp-json/wp/v2/posts');
console.log('Done!');
console.log(posts);
```

Using `console.log`, we're logging that we're fetching the latest posts from my blog, and attempt to store them in a variable called `posts` using `fetch` on the API URL, then in the console, we'll say that we're done, and then see the posts.

What happens when we refresh this? We see going to `fetch`, done immediately. How can we see both of these immediately?

If we call `posts` in the console, we don't see the data inside of it.

That's really important because `fetch` queues up the search. It goes over to wesbos.com and tries to `fetch` the posts immediately, but it does not just store them in your `post` variable.

That might be different if you're coming from another language like PHP, where it would stop everything from running. This will just queue it up and put inside of `posts`, not the post, but it puts what's called a `promise`. Which means like, "Hey, I don't have the data from it just yet, but here's a little IOU the posts from Wes's blog. When they do come back, I'll be sure to let you know about that."

Let's take a look at a different example, but first a quick refresher about callbacks. Let's say we're going to create a callback function for a link...

```js
$('a').on('click',function() {
    alert('hey');
    
})
```

In that example, the alert saying 'hey' is waiting for the user to click the link, which is called a callback. The callback will run only when someone clicks it.

So if we look at a different example here...

```js
const postPromise = fetch('http://wesbos.com/wp-json/wp/v2/posts');

postPromise.then()
```

It's the same thing here, where we have a `then` function that will only run when the data comes back. The `then` function will give us the data. 


If we expand this a little further with an arrow function to log our `data`...

```js
const postPromise = fetch('http://wesbos.com/wp-json/wp/v2/posts');

postPromise.then(data => {
    console.log(data);
})
```

If we look at the console, it looks like we have a response here. After two seconds the data actually comes back. The console shows some good data, some headers and the body. That's probably what we want. If we open up our body, though, we don't get the actual posts. What we have here is the actual raw data, but it doesn't yet know that we're expecting JSON, because you could `fetch` any type of data, not just JSON. 

So let's fix that. We could probably just do an implicit return like the first posts:

```js
const postPromise = fetch('http://wesbos.com/wp-json/wp/v2/posts');

postPromise.then(data => data.json()).then(data => {console.log(data)}
    
)
```

If we open it up in the console, we have the actual data that comes back from our blog. You see there's the content, excerpt, id, Wordpress slug, and all that stuff. 

We have some good data here. Now, a couple of things we can do here is, probably you want to format it a little bit:

```js
const postPromise = fetch('http://wesbos.com/wp-json/wp/v2/posts');

postPromise
    .then(data => data.json())
    .then(data => {console.log(data)})
    .catch((err) => {
        console.error(err);
    })
```

Often people will put a `then` on its own line, just for readability's sake, but I've also added a `catch` function. 

Remember, our `then` will only fire when the promise successfully comes back. If there is an error with the data that comes back, `catch` runs. Generally in your function, you'll have all of the `thens` that we need, it might be one, it might be four, but if we have a `catch` on the end, that will just catch any errors that happen anywhere along the way. 

So let's pass some broken code, like a typo:

```js
const postPromise = fetch('http://wessboss.com/wp-jsan/wp/v2/posts');

postPromise
    .then(data => data.json())
    .then(data => {console.log(data)})
    .catch((err) => {
        console.error(err);
    })
```

In the console, we see our error here, along with some other errors that happened. But the `TypeError` gets thrown by this catch block here, which reports that it failed to `fetch` anything.

That's `fetch` at a very high level.