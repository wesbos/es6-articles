We just learned that Babel takes our ES6 code and compiles it down to ES5 code, but that really only gets us about half the way there. Babel only works on syntax, meaning that it only works on things like an arrow function, or using template strings, or any of the `const` stuff that we've been using. Those are all syntax. That's just what our code looks like and how it acts. The other side of things is there is a whole bunch of new methods that we've learned about, where those things aren't included in Babel.

What am I talking about?

Well, we've got here `Array.from()`. That is a new method and it's part of ES6. We have that added to arrays and we've looked at a whole bunch of other methods that are new. However, Babel doesn't convert the `Array.from()`to something else. It just assumes that you have `.from` available on all of your arrays.

What you need to do in a situation like this is use what's called a polyfill. A polyfill is very simple. Essentially, it says if the browser does not have it, we must recreate it with regular JavaScript.

What I like to do is, whenever I'm looking at a method, MDN will always have a polyfill for it inside of the documentation. If you [take a look at the documentation](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/from) it says if there is no `Array.from()` method available, then they just instantiate `Array.from()`, or they put it on the prototype if it's something that gets inherited by every single array.

We can take a look and see that someone really smart has figured out how to just re-implement `Array.from()` without actually having it in the browser. What that allows us to do is back port it to older browsers, and that's really, really nice.

It's sort of a pain to say, "What browsers do I support? Do I need to use this method or do I need to polyfill it?" You can just implement a polyfill that will just figure it out all for you.

There are two major polyfills that I like to tell people to use. The first one is using the Babel polyfill. This uses something called `core-js` which has a polyfill for every single ES6 feature. All you need to do, if you're using modules, you just use `import "babel-polyfill";` in your initial file. What that's going to do is include a whole bunch of code which will polyfill for all of the older browsers that you don't have.

That's great, but if you aren't using modules, or if you really don't need to polyfill all that much, that can be a little bit more code overhead that you don't necessarily need, so there's this other really great service called [polyfill.io](http://polyfill.io).

What polyfil.io does is, by including it with a script tag on your page it detects which version browser is doing the requesting. You don't need decide, "All right, who's visiting my website and what do I need to polyfill?" Polyfill.io decides, "All right, who's visiting and what do they need polyfilled?"

If someone's visiting on IE9, it's going to polyfill a whole bunch of stuff. If someone's visiting on latest Chrome, they're probably not going to polyfill all that much. It's pretty cool.

If you head over to [Polyfil.io's documentation](http://polyfill.io/v2/docs) and copy the URL from the script tag there:

```html
<script src="https://cdn.polyfill.io/v2/polyfill.min.js"></script>
```

If you put it in your URL bar and take off the `.min.js` and take a look. What this is, is a dynamically-generated JavaScript file for your specific browser.

If you take a look, if you're using an up to date browser, it's actually really not polyfilling all that much. In my case, it was polyfilling a bunch of new properties, like `.after`, `.append`, `.before`, and all the new methods that are coming to our elements.

If I were to fake this, if you were to user your Chrome DevTools' Network Conditions tab to emulate IE9, you can see that it'll have a whole lot more to polyfill including `Array.from()`, the one we were just looking at.

If we take a look for `Array.from` in the script it will show us the code there. Basically, it says if there is no `Array.from()` it's going to polyfill that for us. It's going to polyfill `Array.of()`, `Array.fill()`, all of the element stuff, all of the string `.endsWith()`, `.startsWith()`, `.includes()`. We went over all these, and now you can use them in your older browsers.

What I would recommend is go ahead and just stick that script tag in an HTML file, and you don't have to worry about anything anymore. If you are worried about multiple requests, where you have to polyfill it, and then load your JavaScript, then I might look at using the Babel polyfill, where you just include it all into one bundle. It's totally up to you.

One other thing with Polyfill.io, you can also tailor your response so if you know you're only using three or four ES6 methods and you don't want to polyfill the entire ES6 feature set for your IE9 visitors, you can go ahead and tailor your own.

 There's a whole bunch of options that you can use with Polyfill.io to specifically request some of the features that you want. You can take a look here, and you can go to the browser and the features and look at every possible thing that you want.  Polyfilling, along with transpiling with Babel, is going to give you very good coverage of all of your ES6 features.