These last couple examples we've been creating HTML and inserting it right into the DOM. If you have any sort of security background and you're probably screaming, "Wes, you must sanitize that data before you put it into the DOM."

If you don't know what that means, essentially when you get data from a user, or whenever you're displaying data from a user, you must make sure that the user isn't doing any Sneaky Pete stuff on you, and trying to maybe insert an iFrame, or an image, or to do an XSS attack. 

Here I've got an example:

```js
const first = 'Wes';
const aboutMe = `I love to do evil <img src="http://unsplash.it/100/100?random" onload="alert('you got hacked');" />`;

const html = `
    <h3>${first}</h3>
    <p>${aboutMe}</p>
    `;
    
const bio = document.querySelector('.bio');
bio.innerHTML = html;
```

Let's assume I took this first name in the about me. I got that from a database, or an API or something like that, and the user had this saved in their database. 

They said, "I love to do evil," and they inserted an image from Unsplash, which is allowed. You can insert an image into your bio, no problem, but they do the Sneaky Pete thing here where they inserted an `onload=alert('you got hacked');`, so when this image loads, run some JavaScript. 

That is a huge problem. You cannot let your users run JavaScript on your page because then they could drain your bank account, or delete your app, or post, or really anything. Imagine if you let someone run JavaScript on Facebook. You could have people unfriending everyone, or you could look at all of their messages, or send nasty messages on their behalf. 

Here I've taken the about me and I've just popped it into HTML and then I've set it into our bio:

```js
const first = 'Wes';
const aboutMe = `I love to do evil <img src="http://unsplash.it/100/100?random" onload="alert('you got hacked');" />`;

const html = `
    <h3>${first}</h3>
    <p>${aboutMe}</p>
    `;
    
const bio = document.querySelector('.bio');
bio.innerHTML = html;
```

I'm taking it, carte blanche, putting it in, I refresh, and I get this message, "You got hacked." That is what's called an XSS, cross site scripting, where we let someone else run JavaScript on our page. That's really dangerous. 

The solution to this is to sanitize your HTML. That's another place where you could use the tag template. In creating all this HTML here, I have all kinds of inputs from a user. But before I put it into my HTML, it's probably a good idea to run it through a sanitizer, so our HTML will look something like this: 


```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/dompurify.0.8.2/purify.min.js"></script>
<script>
const first = 'Wes';
const aboutMe = `I love to do evil <img src="http://unsplash.it/100/100?random" onload="alert('you got hacked');" />`;

const html = `
    <h3>${first}</h3>
    <p>${aboutMe}</p>
    `;
    
const bio = document.querySelector('.bio');
bio.innerHTML = html;
</script>
```

I've loaded it in this library called [DOMPurify](https://github.com/cure53/DOMPurify). It's a pretty good library that we use to sanitize. You can have all kinds of options in here in terms of what tags are allowed and aren't allowed. There are all kinds of sanitize libraries here.

You could make a sanitized tag template and use this library inside of it. I'm going to show you how to do that now:

```js
function sanitize(strings, ...values) {
    return strings.reduce((prev, next, i) => `${prev}${next}${values[i]} || ''}`, '');
}
const first = 'Wes';
const aboutMe = sanitize`I love to do evil <img src="http://unsplash.it/100/100?random" onload="alert('you got hacked');" />`;

const html = `
    <h3>${first}</h3>
    <p>${aboutMe}</p>
    `;
    
const bio = document.querySelector('.bio');
bio.innerHTML = html;
```

So we have a `reduce` function, which takes in a function and what we start with, an arrow function, which we'll use the first iteration, `prev`, our current iteration, `next`, and `i` for index. So we're just going to return a string that tacks all of those things together. We've got `prev`. We've got the `next`. We've got `values`, which is `i` or nothing.


We still have, "you got hacked" because we're not finished yet. 

```js
function sanitize(strings, ...values) {
    const dirty = strings.reduce((prev, next, i) => `${prev}${next}${values[i]} || ''}`, '');
}
const first = 'Wes';
const aboutMe = sanitize`I love to do evil <img src="http://unsplash.it/100/100?random" onload="alert('you got hacked');" />`;

const html = `
    <h3>${first}</h3>
    <p>${aboutMe}</p>
    `;
    
const bio = document.querySelector('.bio');
bio.innerHTML = html;
```


But instead, what we can do is add a new `const` called `dirty`, because our string is dirty because it still has it's `onload`, or iFrames, or any of the other nasty stuff people are trying to do. 

Now we'll use our library that we loaded, and it's called with `DOMPurify.sanitize`. We'll sanitize the dirty HTML:

```js
function sanitize(strings, ...values) {
    const dirty = strings.reduce((prev, next, i) => `${prev}${next}${values[i]} || ''}`, '');
    return DomPurify.sanitize(dirty);
}
const first = 'Wes';
const aboutMe = sanitize`I love to do evil <img src="http://unsplash.it/100/100?random" onload="alert('you got hacked');" />`;

const html = `
    <h3>${first}</h3>
    <p>${aboutMe}</p>
    `;
    
const bio = document.querySelector('.bio');
bio.innerHTML = html;
```

Now when you refresh, you can see see that the `onload` has been stripped out, along with any other nasty stuff. 

Obviously, you could do this just with a function as well. You could say `DOMPurify.sanitize` and just wrap your `html` variable with it to achieve the same results. I'm showing you how you could possibly create a tag template so that any time you are creating HTML. I've applied it to my `dirty` variable, but you could also put it within the `html` variable, for example. You can put this wherever you need to quickly sanitize something. Even if you're using variables or not, it's going to sanitize it for you.