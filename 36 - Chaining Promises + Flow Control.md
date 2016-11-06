Another useful case for `Promise` is when you need some sort of flow control. This is an example where you would probably find out on a back end and maybe something like Node.js, when you are querying a database.

```js
const posts = [
    { title: 'I love JavaScript', author: 'Wes Bos', id: 1 },
    { title: 'CSS!', author: 'Chris Coyier', id: 2 },
    { title: 'Dev tools tricks', author: 'Addy Osmani', id: 3 },
];

const authors = [
    {name: 'Wes Bos', twitter: '@wesbos', bio: 'Canadian Developer'},
    {name: 'Chris Coyier', twitter: '@chriscoyier', bio: 'CSS Tricks and CodePen'},
    {name: 'Addy Osmani', twitter: '@addyosmani', bio: 'Googler'},
];

```
Here we have a `posts` array that contains three posts that are associated with a `title`, the `author` of the post, and the post `id`, with three different authors.

Then there is also another array that contains the authors. This array tells you who the `name`, their `twitter` and their `bio`. The author's `name` is included with the `posts` array, but if you want more information, we have to go to our second array.

We are going to use these two arrays to simulate a connection to a database that won't be accessible immediately, which is a good reason to use a `Promise`.

What we're going to do is we're going to create two separate functions that both going to return a `Promise` each, and then we are going to chain them together.

```js
function getPostById(id) {
    

}
getPostById(1)
```

In setting up our function, we want it to look up in our database for the post with an `id` of 1.

The first thing we need is a `Promise`, because we cannot access it from our database immediately. It has to do a round trip around back and forth to the database:

```js
function getPostById(id) {
   return new Promise((resolve, reject) => {
       
     const post = posts.find(post => post.id === id);
            if(post) {
                resolve(post)
                
            } else {
                reject(Error('No Post Was Found!'));
            }
   });
}

getPostById(1)
```

We are going to loop over every single post. Then, when the `id` matches what we want in our function, 1, we're going to find the `post` there for our control flow. The `if` is looking for a  `post` matching the `id`. Once it does, it will `resolve` and give the `promise` to `post`, otherwise we are going to `reject` and say `'No Post Was Found!'`.

In order to simulate this, so it takes time, what we can do is as you can just wrap this in a `SetTimeOut`, which will sort of simulate the taking 200 ms round trip. 

If you like to run it instantly, you don't have to do this, but it's totally up to you.

```js
function getPostById(id) {
    // create a new promise
   return new Promise((resolve, reject) => {
       // using a settimeout to mimic a database
       setTimeout(() => {
       //find the post we want
     const post = posts.find(post => post.id === id);
            if(post) {
                resolve(post); // send the post back
                
            } else {
                reject(Error('No Post Was Found!'));
            }
            }, 200);
   });
}

getPostById(1)
    .then(post => {
        console.log(post);
    })
```

There we go. If we run this, we'll see there is a post immediately as an Object.. We get `postById(1)`'s result, my post. 

We want to do this thing that I like to call 'hydrating'. In our `posts` array,  where the author of this post is just a string, `'Wes Bos'`, but I want to replace it with the the `author` object, which has my name as well as my twitter and bio.

I'm going to create a new function called `hydrateAuthor`, which is going to take in the post, and then return in our getPostbyId function as a `Promise`. What's great about that is that if we return a `Promise` inside of a `.then`, we're allowed to chain another `.then` on to the next line. The whole thing looks something like this: 

```js
function getPostById(id) {
    // create a new promise
   return new Promise((resolve, reject) => {
       // using a settimeout to mimic a database
       setTimeout(() => {
       //find the post we want
     const post = posts.find(post => post.id === id);
            if(post) {
                resolve(post); // send the post back
                
            } else {
                reject(Error('No Post Was Found!'));
            }
            }, 200);
   });
}

function hydrateAuthor(post) {
    // create a new promise
    return new Promise((resolve, reject) => {
        // find the author
        const authorDetails = authors.find(person => person.name === post.author);
        if(authorDetails) {
            // "hydrate" the post object with the author object
            post.author = author.Details;
            resolve(post);
        } else {
            reject(Error('Can not find the author'));
        }
    });
}

getPostById(1)
    .then(post => {
        console.log(post);
        return hydrateAuthor(post);
    })
    .then(post => {
    })
    .catch(err => {
        console.error(err);
    })
```

Let's step through all of that new code. 

We create our `hydrateAuthor` function that takes in the `post`. We create a new `Promise`, where we find the `author`. If there is an `author`, then we `hydrateAuthor` on our post object, which adds the `author` object to the `post`. Otherwise, it's rejected.

On the end of the function, I've removed the initial `console.log`, because we don't really need it anymore, We're also going to see a `catch` for error handling. If there is an error thrown in anytime, we should be able to show the error, and allow us to debug it.

If we run that in the console, you'll see that we get the is `hydrateAuthor` version of the post. Our `author` is an object that contains the `bio`, `name`, and `twitter` for whichever author wrote the post.

The `catch` allows us to find errors, too. If we run `getPostById`, we'll see an error that no post was found. Similarily, if we have a typo for an author's name, we get the error, 'Cannot find the author'.

It's a nice example of when you're chaining, because this is a little bit complex. There's a lot going on in here.
 
 What you can do is you can sort of sweep all this complexity into a nice little function, where you know how that works, you can test how that works. Then you can chain them together, whenever it is you that actually need them.