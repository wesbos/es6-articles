Arrays now have a `.find` and a `.findIndex`, which are really helpful. Some of my projects I don't even need to load in a utility library like lodash. Let's take a look at what `.find` and `.findIndex` do. A very common use case is when you have some data that comes back from an API. 

Here I've got some data that comes back from an API. It's a posts array, where each item inside of the post is an object. Often we want to find an entire object inside of this array from an API:
 
```js
const posts = [
	{
		"code":"DFKJdodfi",
		"caption":"Dunno Dog'",
		"likes":42,
		"id":"328943249723",
		"display_src":"https://i.ytimg.com/vi/uFTcXaTezyA/hqdefault.jpg"
	},
	{
		"code":"BAIDfidslkaf",
		"caption":"Doggin' Around",
		"likes":40,
		"id":"2190828475324",
		"display_src":"https://i.ytimg.com/vi/uFTcXaTezyA/hqdefault.jpg"
	},
	{
		"code":"BAAJqdslkaf",
		"caption":"Strait Doggin'",
		"likes":50,
		"id":"2190823079324",
		"display_src":"https://i.ytimg.com/vi/uFTcXaTezyA/hqdefault.jpg"
	}
]
```

Now, because it's an object, we can't simply say:

```js
posts['BAAJqdslkaf']
```

Since it's an object, we don't have any keys to grab things from an array because an array is always index-based. 

What we actually need to do is use `.find`, which is a callback where you return `true` or `false` until you actually find what you're looking for. It's going to loop over every single post that we have, and `.find` is going to look for the specific one that I want.


```js
const post = posts.find(post => {
    console.log(post)
})
```
That will loop over the API's objects and return every post.

Then I want to say `post.code`, should give me all of the codes:

```js
const post = posts.find(post => {
    console.log(post.code)
})
```
 
Then we can look for our specific post:
 
```js
const post = posts.find(post => {
    console.log(post.code)
    if(post.code === 'BAAJqdslkaf') {
        return true
    }
    return false;
});
```

We should now have a variable called `post`, and that is the actual post from our object. So now we have the entire post and we can go ahead and grab the `caption`, the `source`, the `id`, or the number of likes from that post. 

This can be done a lot easier. This is just explicit, so you can see what's going on, but we can actually just use this:
 
```js
const post = posts.find(post => post.code === 'BAAJqdslkaf');
console.log(post);
```
 
One other improvement we can do is that it probably isn't hard coded like that. We probably want to put in a variable called code:
 
```js
const code = 'BAAJqdslkaf';
const post = posts.find(post => post.code === code);
console.log(post);
```

That is good for finding the actual thing inside of the array. If you want to find multiple things, how would you do that? You can use `.filter`, and that would give you an array of objects instead.


### .findIndex() 

Next up, we have `.findIndex`. That's really helpful when you actually have the item you want, but you want to know where in the array it is actually. 

Let's say we want to delete one of our posts, and all that we know is that we have the `id` or the `code` here. We need to find out where in this so we can delete it.  


```js
const code = 'BAAJqdslkaf';
const postIndex = posts.findIndex((post) => {
	if(post.code === code) {
		return true;
	}
	return false;
});
console.log(postIndex); 
```

Now we should be able to find what the `index` is of that post: It's the 3rd post in there. 

We can refactor this right down:

```js
const code = 'BAAJqdslkaf';
const postIndex = posts.findIndex(post => post.code === code);
console.log(postIndex); 
```

Why? Because we can only return if the `post.code` is equal to our `code`. 

Now that we actually have the `index`, we could go ahead and just take that out of there. We're going to look at a `.spread` example in the future where it shows a great way how to just slice out one item from an array.
