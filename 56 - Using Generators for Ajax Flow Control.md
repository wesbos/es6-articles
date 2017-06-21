One use case for generators is the ability to do waterfall-like Ajax requests. You've probably run into a situation before where you first need to hit an Ajax request where you search for something, and then when that search comes back, it maybe gives you a user ID. Given that ID, you have to hit a second Ajax request based on that ID. When that comes back, maybe you get a list of photos for that user, and you need specific information about the first photo, or something that came back based on that photo ID. 

It's kind of like a waterfall. We need the information from the previous one in order to do the next one. You know that that can lead to what's called callback hell, where you have nested code inside of each other. There's libraries like async that are really good for dealing with that, however the browser is getting much better at being able to do it natively. We're going to look at an example of how you can do it with generators.

What I want to do is make a function called steps. This is going to be a generator itself, so we have to put a `*` after the word function. You can also put an asterisk in front of the function name. Either will work, it's just a personal preference.

```js
function* steps() {
    const beers = yield ajax('http://api.react.beer/v2/search?q=hops&type=beer');
    
    const wes = yield ajax('https://api.github.com/users/wesbos');
    
    const fatJoe = yield ajax('https://api.discogs.com/artists/51988');
}
```

What I want to do here is that I've got three Ajax requests that I'm going to be making here. 

I've got one that's going to go fetch me a list of beers, one that's going to go fetch my information from GitHub and one that's going to go fetch information about the artist Fat Joe.

Assume that these relied on each other -- they don't really, but I just want to make one Ajax request, and when that comes back I want the second one, then I want the third one. 

Would you believe me if I were to say that we could write all this code in one line just like this? 

```js
function* steps() {
    const beers = yield ajax('http://api.react.beer/v2/search?q=hops&type=beer');
    const wes = yield ajax('https://api.github.com/users/wesbos');
    const fatJoe = yield ajax('https://api.discogs.com/artists/51988');
}
```

This would execute, stop, execute, stop, execute and stop. 

What I've done here is I've used `yield`. I've created a variable called `beers`, and I've yielded the result of `ajax`. We're going to create a function called `ajax`, and only once that Ajax function returns a value, will the next one run. Only once that Ajax request returns my GitHub info, it's going to come back and run the next one.

We're going to be able to write code that's very nice and clean, top-to-bottom here, without having to worry about callbacks or promises or anything like that.

So let's make that Ajax function.
 
```js
function ajax(url) {
    fetch(url).then(data => data.json()).then(data => )
}
```
 
It's going to take in a URL. Inside of that function we'll say `fetch` the URL. When that comes back, we are going to get the data and return the `data.json`. You'll know from our previous ones that that also returns a promise, which we call `.then` on, which we have the data that gives it back.

What do we then do with this data once we have it? Once we have the data, essentially what we want to do is call `.next` on our generator that we're going to use by assigning a `const`. 

```js
const dataGen = steps();
```

Now we can go back to the `ajax` function and add in our `dataGen` and say `datagen.next(data)`, to pass it the data.


This is a little bit confusing, so bear with me a second.

```js

function* steps() {
    const beers = yield ajax('http://api.react.beer/v2/search?q=hops&type=beer');
    const wes = yield ajax('https://api.github.com/users/wesbos');
    const fatJoe = yield ajax('https://api.discogs.com/artists/51988');
}

function ajax(url) {
    fetch(url).then(data => data.json()).then(data => dataGen.next(data))
}

const dataGen = steps();
dataGen.next(); // kick it off
```
 

What's happening here is that, on page load, we are going to create a brand new generator and call it `dataGen`, which is based on the generator we've created, `steps`. 

Then it's going to call the first one, just to kick it off, otherwise it won't run its first one, and we do that with `dataGen.next()`. We've put a comment there, "kick it off."

What will happen is when you kick it off, the first one is going to run, which is `beers`. It's going to request the Ajax function, which means it's going to `fetch` the URL at `api.react.beer`. When the data comes back to us, we're going to call `next`, which is on our generator that we created, `dataGen`.

The cool thing about this here is that whatever we pass from `next` will go back to the generator and get stored in the initial variable, `beers`.

It's really cool, because you create our `beers` variable, and then even if this takes 10 minutes or 10 hours, the result of this yield is going to get put back into `beers`. 

Then next is going to run, so the next yield will run here, where we'll run another Ajax request, this time to get my github data. That might take five seconds, five minutes, who knows? When that comes back, we're going to get the data for Fat Joe.

Let's take a look at how it works, and to see it working, let's I'm going to put some comments in here. 

```js

function* steps() {
    console.log('fetching beers');
    const beers = yield ajax('http://api.react.beer/v2/search?q=hops&type=beer');
    console.log(beers);
    
    console.log('fetching wes');
    const wes = yield ajax('https://api.github.com/users/wesbos');
    console.log(wes);
    
    console.log('fetching fat joe');
    const fatJoe = yield ajax('https://api.discogs.com/artists/51988');
    console.log(fatJoe);
}

function ajax(url) {
    fetch(url).then(data => data.json()).then(data => dataGen.next(data))
}

const dataGen = steps();
dataGen.next(); // kick it off
```

Give this code a refresh, and we'll see where we're at, fetching `beers` comes back, then `wes`, and `fatJoe`. It said "Fetching beers", took a second, and then when `beers` came back it stored it in a variable called `beers`, then logged the beer information in the console, then it moved along to say "fetching Wes Bos", made that second request to go fetch my information from GitHub. When that one came back, "fetching Fat Joe", run the Ajax request, and came back with Fat Joe's information. 

That's really nice top-to-bottom, very readable code. The only thing that you need to know is that we must call the `.next(data)` on our actual generator that gets called, and pass it the actual data, and that will put it inside of there.

That's really nice because if I wanted to maybe see if there's any `beers` like this:

```js
  console.log('fetching wes');
    const wes = yield ajax('https://api.github.com/users/wesbos' + beers[0]);
    console.log(wes);
```

Obviously there's not a GitHub name with that beer, but if I needed the data from the first request in the second request, I could have easily done that because this code will not run until this one comes back.

Hopefully that helped you understand a little bit more about generators.