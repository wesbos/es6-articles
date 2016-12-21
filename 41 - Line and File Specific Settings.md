Sometimes there are ESLint settings that are specific to a single file rather than being for a project or for your global settings. We want these things to only be specific to that specific file, but we can even apply settings to specific lines that we are working with. 

Sometimes you have a setting where you're like, "There's no way around this for me, so I need to temporarily turn off that setting so I'm able to commit," or maybe you have an external library where they are introducing some global variables. 

Let's take a look at global variables first, and then we'll go in through other settings. 

If you have ever used Twitter or Google Analytics or Facebook, you know that the way that they work is they put a global variable in your window, like `ga()` is for Google Analytics, and if you want to use Google Analytics you just start using the function. 

But if we used, `ga.track()` or whatever the function is, like or Twitter's `twttr.trackConversion()`. If we start to use these things, and we were to ESLint our code, we'll run into an error that says that `ga` and `twttr` aren't defined, because we haven't defined them, they're set by Google Analytics and Twitter.

What we can do to prevent ESLint throwing an error is set globals inside of this specific file, and that's done by adding a block comment to declare your `globals` at the very beginning of your file.
```js
/* globals twttr ga */

ga.track();
twttr.trackConversion();

```

That tells ESLint what to expect in terms of global variables from that file, and so we no longer get those errors.

You could do that as a global setting, but stylistically, I prefer to just do it file by file and really just turn them off exactly wherever you need them.

Let's take a look at when you might want to turn off an ESLint setting in an entire file, or even just in a couple of lines. To illustrate this, I'm going to show you something called `array.includes()`. This is part of ES2016 or ES7. If we want to use this in our projects, because it's really handy, we have to polyfill it. 

We're going to learn all about polyfills, but essentially the way that it works is that if the `Array.prototype` does not have includes method, we will just add it to the prototype. Now in general it's a no-no to modify your prototype of the built-ins like array, and number, and string. 

However, if you're trying to be future-friendly, then you have to polyfill it, so it'll work in all of the current browsers, older browsers, and then when the browser does implement `array.includes`, we're going to be in good shape. 

If I were doing that, I would go ahead and [take the Polyfill code from the MDN page](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/includes), and I would paste that in to my JS file. Then we go back and rerun ESLint, and we get a whole bunch of errors. 

The first one we'll look at it `Array.prototype is read only, properties should not be added`, which is from the `no-extend-native` property. That makes sense. You shouldn't be just adding anything willy-nilly to the actual prototype of `Array`, but because this is a standard agreed-upon method from MDN, and it's definitely going to be added, then it's actually fine to do it.

What we need to do is we can turn it off entirely in the file. Like before with global variables, we add a comment to the beginning of our file:

```js
/* eslint-disable no-extend-native  */
```

If we run that file again through ESLint in our terminal, we'll get one less error.

What we just did there is that we disabled `no-extend-native` for the entire file, or all of the code that comes after that comment. If this was a big file, then I might not want to disable it for the entire file. I might want to just disable it for single line. I'll show you how to do that next.

I bet you're thinking, "I know, I know, I shouldn't be doing this, but I'm just going to disable it for this one line here, and then the line right after it, I'm going to enable that rule right again." 


```js
/* array.includes polyfill code */

if (!Array.prototype.includes) {
  /* eslint-disable no-extend-native  */
  Array.prototype.includes = function(searchElement /*, fromIndex*/) {
  /* eslint-enable no-extend-native  */
    'use strict';
    if (this == null) {
      throw new TypeError('Array.prototype.includes called on null or undefined');
    }

    var O = Object(this);
    var len = parseInt(O.length, 10) || 0;
    if (len === 0) {
      return false;
    }
    var n = parseInt(arguments[1], 10) || 0;
    var k;
    if (n >= 0) {
      k = n;
    } else {
      k = len + n;
      if (k < 0) {k = 0;}
    }
    var currentElement;
    while (k < len) {
      currentElement = O[k];
      if (searchElement === currentElement ||
         (searchElement !== searchElement && currentElement !== currentElement)) { // NaN !== NaN
        return true;
      }
      k++;
    }
    return false;
  };
}

```

What that does is it will stop it, ignore that the error that is in those lines, only ignore it this one time, and then keep going with the rule like normal, and still yell at me if I break the rule again. 

This will not really reduce the number of errors, but we get all kinds of different errors here, and that's because we've just included this polyfill. It's code that someone else wrote, and while maybe I should fix it, I know that it works fine in there's no issue with it. Again, we know this because we got the code from MDN.

What we could do is just ignore this entire block. The only way we would do that is, rather than just commenting out one line, you can up and you use `/* ESlint-disable */` before the polyfill, and then `/* ESlint-enable */` after the polyfill code.

That will just ignore absolutely every single rule in between the two set of comments. If re-run our ESLint code now, you see if we get no errors because I've ignored all the code in our polyfill.

It's really helpful to be able to ignore, either on a file, where we'd add the comments at the top of a file, or line by line, which we use `/* ESlint-disable */` and `/* ESlint-enable */` for rules around our code. 

It works the other way around. If you have the ESLint rule which you have disabled by default, you can enable them by for a couple lines, if you'd like that specific rule to kick in only for a bit of the file.