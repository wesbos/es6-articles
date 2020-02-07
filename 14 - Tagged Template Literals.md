One feature that comes along with template literals, or template strings, is the ability to **tag** them.

That means we can run a template string through a function, and rather than have the browser immediately assign that value to a variable, we can have control over how the string is actually made. 

I'm going to show you some examples of when that would be useful, but first, let's look at how it all comes together.

The way the tag template works is you make a function, you take the name of the function that you want to run against the string, and you put that name right in front of the template:

```js
function highlight() {
    // do something
}

const name = 'Snickers';
const age = '100';
const sentence = highlight`My dog's name is ${name} and he is ${age} years old`;
console.log(sentence)
```

This tags the string with a function name. What will happen is the browser will run this function, it will provide it with all the information about the string, the pieces that the person typed, as well as the variables that get passed in. Whatever we return from this function is what `sentence` is actually going to be.

It acts like a step in between. Before the string actually gets put into `sentence`, we have the ability to modify it in any given way. Let's take a look at `highlight` here:

```js
function highlight() {
    return 'cool';
}
```

If I just use `return 'cool'`, that is going to take in the entire string as an argument, but then just return the word "cool." Now we see `cool` in console.

```js
const sentence = highlight`My dog's name is ${name} and he is ${age} years old`;
console.log(sentence); // 'cool'
```

Obviously, that's not what we want to do, but this function will take in a few arguments so we can create the string ourselves:

First, we get an array of all the string pieces:

1. My dog's name is
2. and he is
3. years old

After this array, the subsequent arguments will be the values that are being interpolated.

In this case, we're passing `name` and `age`.

```js
function highlight(strings, name, age) {
    return 'cool';
}
```

Now, we could do this. It passes a name and an age value. However, if you don't know exactly how many values someone is passing, there's no way for you to really scale it. You could say `Arg1`, `Arg2`, go all the way to `Arg100`, just in case there are a hundred arguments, but that's no way to live your life, right? 

For this, We can use ES6 rest operator, which is three dots. We're going to go into that a lot more in a future video, but know for now, if we do `... values`, what it's going to do is take the _rest_ of the values that are passed to that argument, and put them into an array for us.

```js
function highlight(strings, ...values) {
    return 'cool';
}
```

What's `strings`? `Strings` is an array of three things:

- `"My dog's name is "`
- `" and he is "` 
- `" years old"` 

As you can see, it breaks up the string into the biggest pieces it can until a variable interrupts it. 

We've got `"My dog's name is "` then it pops in a variable, `" and he is "`, that's the second item in the strings array, and then, `" years old"`, which are the three strings that don't get changed. 

The other things we get is an array of `values`, which we passed in two values here. We get `"Snickers"` and `"100"` which are the values that I passed in.

So - the whole point of the tagged template is that we can create the string ourselves. Let's move along to creating it!

```js
function highlight(strings, ...values) {
   let str = ''; 
}

const name = 'Snickers';
const age = '100';
const sentence = highlight`My dog's name is ${name} and he is ${age} years old`;
console.log(sentence)
```

I'm using `let` here. Why? Because we're going to be appending more strings on to this actual string. If I used `const`, I wouldn't be able to tack onto it.

Then, we want to loop over this first `strings` array, tack on `My dog's name is `, and then the first value, ` and he is `, and the second value, ` years old` and then third value, but in this case, we don't have a third value.

You can use whatever loop or logic you prefer inside of this function. You could even use a `map` and then `join` it together. I'm going to use `forEach` in my example:

```js
function highlight(strings, ...values) {
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
 
We're going to hit a little bit of an error here. If we `return str`, the string that we've been tacking things onto, we should now see `"My dog's name is Snickers and he is 100 years oldundefined."`

First of all, remember, that's because the `strings` array is always going to be one larger than the `values` array. When we hit that last one, it's only going to be a string and there's going to be no value to tack on the end. You could check if values of `[i]` is `undefined` and, if it is, then don't put it on.

A little trick you can do here is using the values of `i` or just a blank string:

```js
function highlight(strings, ...values) {
   let str = '';
   strings.forEach((string, i) => {
       str += string + (values[i] || '');
   });
   return str;
}
```

That will adjust if there is no value, it will just return a blank string there.

### Tagged Template Usecases

What can we actually do with this? Well, we could change this so any value that gets passed in gets wrapped in a `<span>` and we can use CSS to highlight it. 

You maybe want to show your user which pieces of the string were variables or which pieces of the string that they passed in, maybe it's their user name or a total for an order. In our case, we just want to highlight the name and the actual age.

```js
function highlight(strings, ...values) {
   let str = '';
   strings.forEach((string, i) => {
       str += `${string} <span class='hl'>${values[i] || ''}</span>`;
   });
   return str;
}
```

Now with our `<span>`, let's see what our sentence looks like here. There we go. We've got a span with a class of `hl` wrapped around every single possible one.

```html
<style>
.hl {
background: #ffc600;
}
</style>
```

Then, we can add it to our document instead of using the console:

```js
document.body.innerHTML = sentence;
```
Once you do that, you'll see that the values are being highlighted. You can go ahead and add a `contenteditable` to our `<span>` so that if the user didn't like it, they can change the value they actually wanted. Maybe they didn't like that Snickers is a hundred years old, so they could change that value as well. It's kind of a fun way to do it.

### Bring it in, Team

There are much more complicated and in-depth use cases, but that is the main idea behind tagged template literals. If you have a string, you pass in the regular string that you want, and then you get all the pieces. 

It's up to you to craft it together and to return some sort of modified data. In our case, we're wrapping it in a `<span>`, but you may want to format it or do some `if` statement and conditional work on all the values.
