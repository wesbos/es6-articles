Another feature that comes with template literals, or template strings, is the ability to **tag** them. That means is that we can essentially run a template string through a function, and rather than have the browser put in the variables for us, we can have control over how this actual string is made.

I'm going to show you some examples of when that would actually be useful but first, let's look at the mechanics of it and how it all comes together.

The way the tag template works is you simply make a function, and you take the name of the function that you want to run against the string, and you just put the name right in front of the template:

```js
function highlight() {
    
}

const name = 'Snickers';
const age = '100';
const sentence = highlight`My dog's name is ${name} and he is ${age} years old`;
console.log(sentence)
```

This tags it with a function name. What's going to happen is that the browser is going to run this function, it's going to provide it with all the information about the string, all the pieces that the person typed, as well as the variables that get passed in. Whatever we return from this function is what `sentence` actually is going to be.

It acts like a step in between. Before this function actually gets put into `sentence`, we have the ability to modify it in any given way. Let's take a look at `highlight` here:

```js
function highlight() {
    return 'cool';
}
```


If I just use `return 'cool'` that is going to take in this entire string as an argument, but then just return the word "cool." Now we see `cool` in console.

Obviously, that's not what we want to do,but this function will take in strings, and it will take in the actual values of those strings. In this case, we're passing `name` and `age`.

```js
function highlight(strings, name, age) {
    return 'cool';
}
```

Now, we could do this. It passes a name and passes an age, but if you don't know exactly how many values someone is being pulled in, there's no way for you to really scale it. You could say `Arg1`, `Arg2`, go through all the way to `Arg100`, just in case there are a hundred arguments, but that's no way to live your life.

We can use ES6 rest operator, which is three dots. We're going to go into that a lot more in a future video, but know for now, if we do `... values`, what it's going to do is take the rest of the values that are passed to that argument, and put them into an array for us.

```js
function highlight(strings, ...values) {
    return 'cool';
}
```

I'm going to pop a `debugger` in here, and what that will do is it's going to stop my function from running. 

In Google Chrome, it will automatically open your inspector and show you a few things.

First of all, the local scope of the function has `this: Window` which isn't important here. What is important is that it has  **strings** and **values**.

What's `strings`? `Strings` is an array of three things:

- `"My dog's name is "`
- `"and he is "` 
- `"years old "` 

As you can see, it breaks up the string into the biggest pieces it can until a variable interrupts it. 


We've got `"My dog's name is"` then it pops in a variable, `"and he is "`, that's the second item in the strings array, and then, `"years old "`, which are the three strings that don't get changed. 

The other things we get is an array of `values`, which we passed in two values here. We get `"Snickers"` and `"100"` which are the values that I passed in.

The `strings` array  is always going to be one bigger than the `values` array. Even if I were to put in another variable and include it at the end of our `sentence` string:

```js
const name = 'Snickers';
const age = '100';
const gender = 'male';
const sentence = highlight`My dog's name is ${name} and he is ${age} years old ${gender}`;
console.log(sentence)
```

Even though there is nothing after it, the debugger will report four values for `strings`, but there will be a new empty string in our `strings` array, and `gender` will be included in our `values`.

Again, `strings` array is **always** going to be one longer than the `values` array. The idea behind this is that, we can use this function to build the string ourselves rather than relying on the browser to do it for us

So let's remove our `debugger` and `gender`, and move on to creating a string using `let`: 

```js
function highlight() {
   let str = ''; 
}

const name = 'Snickers';
const age = '100';
const sentence = highlight`My dog's name is ${name} and he is ${age} years old`;
console.log(sentence)
```


I'm using `let` here, why because we're going to be tacking things on to this actual string. If I used `const`, I wouldn't be able to tack onto it.

Then, we want to loop over this first strings array here, tack on `My dog's name is`, and then the first value, `and he is` and the second value, and then `years old` and then third value, but in this case, we don't have a third value.

You can use whatever loop you prefer to do. You could even use a `map` and then `join` it together. I'm going to use `forEach` in my example:

```js
function highlight() {
   let str = '';
   strings.forEach((string, i) => {
       str += string + values[i];
   });
   return str;
}

const name = 'Snickers';
const age = '100';
const sentence = highlight`My dog's name is ${name} and he is ${age} years old`;
console.log(sentence)
```

That's going to say, "Give me the first value from the strings and the first value from our values array and tack them on."
 
We're going to hit a little bit of an arrow, but if we `return str`, the string that we've been tacking things onto, we should now see "My dog's name is Snickers and he is 100 years oldundefined."

First of all, remember, that's because the `strings` array is always going to be one larger than the `values` array. When we hit that last one, it's only going to be a string and there's going to be no value to tack on the end. You could check if values of `[i]` is `undefined` and, if it is, then don't put it on.

A little trick you can do here, is you can use your values of I or just a blank string:

```js
function highlight() {
   let str = '';
   strings.forEach((string, i) => {
       str += string + (values[i] || '');
   });
   return str;
}
```

That will adjust if there is no value, it will just return a blank string there.

Now, what can we actually do with this? Well, we could change this so any value that gets passed in gets wrapped in a `<span>` and we can use CSS to highlight it. 

You maybe want to show your user which pieces of the string were variables or which pieces of the string that they passed in, maybe it's their user name or a total for an order. In our case, we just want to highlight the name and the actual age.

```js
function highlight() {
   let str = '';
   strings.forEach((string, i) => {
       str += `${string} <span class='hl'>${values[i] || ''}</span>`;
   });
   return str;
}
```

Now with our `<span>`, let's see what our sentence looks like here. There we go. We've got a span with a class of `hl` wrapped around every single possible one.

We've got a little bit of a weirdness going on at the end of the sentence, because that's the end value, it's not really an issue in this case. Let's add our styling in the document `<head>`:

```html
<style>
.hl {
background: #ffc600
}
</style>
```

Then, we can add it to our document instead of using the console:

```js
document.body.innerHTML = sentence;
```
Once you do that, you'll see that the values being highlighted. You can go ahead and maybe add a `contenteditable` to our `<span>` so that if the user didn't like it, they can change the value they actually wanted. Maybe they didn't like that Snickers is a hundred years old, so they could change that value as well. It's kind of a fun way to do it.

There are much more complicated use cases for that and there are many more in-depth use cases for that, but that is the main idea behind these template tag templates. If you have a string, you pass in the regular string that you want, and then you get all the pieces. 

It's up to you to craft it together and to return some sort of modified data. In our case, we're wrapping it in a `<span>`, but you may want to format it or do some `if` statement and conditional work on all the values.