One of the reasons people haven't picked ES6 just yet is they say, "Well, it doesn't work in all the browsers." However, that is not true for the latest browsers, but it is true if you have to support browsers that are a little older, which is why they just don't know how to interpret ES6. 

That should not stop you from using ES6 today because it is possible with Babel and polyfilling to get probably about 99 percent of all ES6 working across all the browsers that you need to support. 

What does that mean? I'm going to explain to you what Babel is, how we can set it up, and in the next video I'm going to talk all about using polyfills to polyfill the new methods that we haven't used yet. 

First up, Babel, what is it? I've been talking about it a couple of videos and I've referenced it here and there, but I haven't actually talked about what it is. Essentially, it is a JavaScript compiler, and what that means is you put JavaScript in and you get ES5 or a browser-compliant ES5 out.

It initially started with just ES6 stuff, however it's really expanded to future JavaScript stuff. Stuff like experimental JavaScript, a lot of React stuff is also in it. Let's take a quick look at how it works, and then we'll step back. I'm just going to go to [babeljs.io](https://babeljs.io) and I'm going to go to **Try It Out**.

This is really nice, because if you're ever trying to understand how something works you can just go to the babeljs website and try it out. 

Right here, I've got this function called `logPet`. It takes in a `petName` and it takes in a `petType`:
 
```js
function logPet(petName, petType = 'dog') {
  console.log(`${petName} is a ${petType}`);
}
logPet('snickers');
```
 
However, we've got a couple of ES6 things going on here. first up, I've got this `petType = dog`, where I've set that as a default in the function parameters, then I'm also using a couple template strings here. 

Now, that might work great if you're using a modern browser, but what if everyone isn't using the latest Chrome? What Babel will do is compile it down to what the ES5 equivalent is, and it does a fantastic job at doing that. We'll take a quick look at the ES5 code in a moment, but you generally don't need to worry about what the ES5 code actually looks, because you're just concerned with how your ES6 code looks.

```js
'use strict';

function logPet(petName) {
  var petType = arguments.length > 1 && arguments[1] !== undefined ? arguments[1] : 'dog';

  console.log(petName + ' is a ' + petType);
}
logPet('snickers');
```

You can see that it's set up a `petType` variable a little differently, and it does a quick check if someone has passed a second argument to our `logPet` function, and if not, the ES5 sets `petType` to be `dog`. Same thing goes for when we did the concatenation with our template strings. It still uses template strings, but it adds `+ ' is a ' +`, our good old style of concatenation.

Babel can be used pretty much in every single project that you need ES6 to work with. It sort of goes hand and hand with something Webpack or a SystemJS.

I've already shown you how to use Babel with Webpack. You only need Webpack, SystemJS or Browserify if you are using modules. If you're using absolutely everything else, there's no need for a Webpack or a Browserify or anything like that. If you just want to write ES6 code and get it to compile down without having to use modules, then you don't need Webpack or anything like that. You can have a much simpler setup.

I'm going to show you how to do that now. Let's take a look. 

In your terminal, let's create a Babel directory in our workspace, and move into that directory:

```bash
$ mkdir babel
$ cd babel
```

Now, I need to set up an npm package, we're going to do an `npm init`-- you can just hit enter here to get it filled in, and now we have our `package.json` file. Now we need to install just two things: We need to grab  babel-cli, which allows us to run Babel via the command line, but in this case, we're actually going to do an npm script. Then we also need what's called a preset because Babel has these plugins, if we go to the [Plugins listing on babeljs.io](http://babeljs.io/docs/plugins/), every single possible feature that JavaScript has has a plugin.

It's sorted out by different chunks, so you can find arrow functions, block scoping, classes, our `for...of` from earlier, and, destructuring, spread, and every single possible feature of ES6 we've talked about so far is available right here.

Then it also supports modules. It also supports some experimental like stuff that might be added to the language but the community isn't quite sure just yet. It supports some minification stuff, and like I said earlier, it supports a whole bunch of React stuff.

There's all kinds of stuff, and probably you don't need all of these. You just need all of the ES6 stuff, so rather than you having to download every single plugin that you need, they also have these things called presets. A preset is simple, it's just a collection of plugins, so we don't need to install every single ES6 plugin, we just need to install one ES2015 preset, so that's what we'll do. 


```bash
$ npm install babel-cli babel-preset-es2015 --save
```
Once it's installed you can actually check your `package.json` and you should see that we have `babel-cli` and `babel-preset` installed.

Now that we have that, we have this command called Babel. You won't be able to run it via the command line unless you have installed that globally, but we can run it via an npm script. 

That's actually what we're going to go ahead and do, which means we need to edit `scripts` in our `package.json` and add in our script to run Babel:

```json
{
  "name": "babel",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "babel": "babel app.js --watch --out-file app-compiled.js"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "babel-cli": "^6.18.0",
    "babel-preset-es2015": "^6.18.0"
  }
}

```

What we've done here is to set babel to run against our `app.js`. The `--watch` works like you might expect, it will watch `app.js` for changes, and `--out-file` tells babel to output our compiled file to `app-compiled.js`.


Now, we need a couple of things here: First of all, we need an app.js, so let's go into our Babel and create a file called app.js, and we'll fill it out with something like:

```js
const age = 100;
const people = ['Wes', 'Kait'];

const fullNames = people.map(name => `${name} Bos`);
```

What ES6 features did we use? We used `const`, we used an arrow function, and we used implicit return. So we used a whole bunch of different ES6 features here, and ideally what should happen if we take that and try it out in the [Babel editor](http://babeljs.io/repl), it should compile that down to using `var` isntead of `const`. It should properly scope our new `var`s if I'm inside of any blocks, and it should make it a proper function and use that annoying concatenation we know so well.

But how do we do it locally? Let's go over to our `package.json`. Now we have our script here. If we were to try run it, it wouldn't work just yet, and if you run `npm run babel`, it kicks out our JavaScript file, but none of it has changed at all because Babel, by default, just is a file mover. 

It doesn't actually do anything because we haven't told at which plugins or which collections of plugins, which preset, or anything, so it doesn't know to use. 

To tell it, we could have done it on the command line, or better is to have what's called a `.babelrc` file. A `.babelrc` file is very similar to an ESLint file, where it will check your file for your specific settings. 

I actually don't like to use a `.babelrc` file in some of my tutorials because it's a file that tends to get lost. Many people don't have it showing in Windows Explorer, or Finder, so one little thing you can do is you can effectively put your `.babelrc` file inside of `package.json`, which is really cool. What we're doing is pretty simple, we're going to add our `presets` array to `package.json` like so:

```json
{
  "name": "babel",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "babel": "babel app.js --watch --out-file app-compiled.js"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "babel-cli": "^6.18.0",
    "babel-preset-es2015": "^6.18.0"
  },
  "babel": {
    "presets": ["es2015"]
  }
}
``` 

What we've done there is set up `"babel"` and told it to use `"presets": ["es2015"]`. If you were using multiple presets or React or something like that, you could call up `"presets": ["es2015", "react"]` and so on, but in this case, we're just using ES2015.

That is our entire Babel configuration. There's a whole bunch of other stuff that you could possibly put in there, and to find out more about that you go check out the docs for [cli usage](http://babeljs.io/docs/usage/cli/), or go to or [babelrc usage](http://babeljs.io/docs/usage/babelrc/), and it's going to tell you information that can possibly go into it and you can check it all out via the options on there. 

So now we should be able to run. Let's save our `package.json`, and head back over to our terminal to do our `npm run babel`. If you still have `npm run` going, you can hit Control-C to kill it, and then rerun `npm run babel` again. 

If you go and take a look, you'll see that `app-compiled.js` has compiled like we'd expect. It uses `var` instead of `const`, it's using a proper `return`, and our good friend, concatenation.
```js
'use strict';

var age = 100;
var people = ['Wes', 'Kait'];

var fullNames = people.map(function (name) {
  return name + ' Bos';
});
```
Now any time that I write any sort of ES6 in here, like this:

```js
const age = 100;
const people = ['Wes', 'Kait'];

const fullNames = people.map(name => `${name} Bos`);

for(const person of people) {
    console.log(person);
}
```

We'll get all sorts of crazy stuff:

```js
'use strict';

var age = 100;
var people = ['Wes', 'Kait'];

var fullNames = people.map(function (name) {
  return name + ' Bos';
});

var _iteratorNormalCompletion = true;
var _didIteratorError = false;
var _iteratorError = undefined;

try {
  for (var _iterator = people[Symbol.iterator](), _step; !(_iteratorNormalCompletion = (_step = _iterator.next()).done); _iteratorNormalCompletion = true) {
    var person = _step.value;

    console.log(person);
  }
} catch (err) {
  _didIteratorError = true;
  _iteratorError = err;
} finally {
  try {
    if (!_iteratorNormalCompletion && _iterator.return) {
      _iterator.return();
    }
  } finally {
    if (_didIteratorError) {
      throw _iteratorError;
    }
  }
}
```

It uses our for, it uses the symbol iterator, it does all of the hard work behind the scenes to get this to actually work, where we just have to write the nice, clean ES6 and it works extremely well. Companies like Facebook are using Babel to compile their entire code base, so if it's good enough for them it's probably also good enough for you. 

One other thing is if you would like to add additional plugins, remember, because our `package.json` Babel preset is only ES2015, but let's say you were interested in trying some of the other plugins out. One that I am a big fan of which is still experimental is this `object-rest-spread`. 

Remember you learned about `rest` and `spread`, the `...`, I mentioned earlier this only works for arrays. It can also work for objects, but it's not in the language yet. It hasn't been totally adopted yet. What we would do is run the [npm install that's specified on the Babel site](http://babeljs.io/docs/plugins/transform-object-rest-spread/), which is: 

```bash
$npm install babel-plugin-transform-object-rest-spread --save
```
 
Now that we've installed this right here we can go to our `package.json`, and instead of also using presets we can use plugins, which is going to be an array in itself:

```json
{
  "babel": {
    "presets": ["es2015"],
    "plugins": ["transorm-object-rest-spread"]
  }
}
```

So now if we go into go into app.js and use some of their rest properties or their spreads properties from the docs:

```js
// Rest properties
let { x, y, ...z } = { x: 1, y: 2, a: 3, b: 4 };
console.log(x); // 1
console.log(y); // 2
console.log(z); // { a: 3, b: 4 }

// Spread properties
let n = { x, y, ...z };
console.log(n); // { x: 1, y: 2, a: 3, b: 4 }

```

Once you rerun `npm run babel` you see the compiled version:

```js
var _extends = Object.assign || function (target) { for (var i = 1; i < arguments.length; i++) { var source = arguments[i]; for (var key in source) { if (Object.prototype.hasOwnProperty.call(source, key)) { target[key] = source[key]; } } } return target; };

function _objectWithoutProperties(obj, keys) { var target = {}; for (var i in obj) { if (keys.indexOf(i) >= 0) continue; if (!Object.prototype.hasOwnProperty.call(obj, i)) continue; target[i] = obj[i]; } return target; }

// Rest properties
var _x$y$a$b = { x: 1, y: 2, a: 3, b: 4 },
    x = _x$y$a$b.x,
    y = _x$y$a$b.y,
    z = _objectWithoutProperties(_x$y$a$b, ["x", "y"]);

console.log(x); // 1
console.log(y); // 2
console.log(z); // { a: 3, b: 4 }

// Spread properties
var n = _extends({ x: x, y: y }, z);
console.log(n); // { x: 1, y: 2, a: 3, b: 4 }
```
 

They put in some helper functions often object without properties, and this is our actual `...` which is the `var _x$y$a$b`. It looks a little bit funky sometimes, but they know what they're doing and the code actually turns out working really nicely.
