Webpack isn't the only way to bundle up your JavaScript. We also have things like [Browserify](http://browserify.org/), [Rollup](http://rollupjs.org), and [SystemJS](https://github.com/systemjs/systemjs). These things all bundle up your ES6 modules, and it doesn't really matter which one you use. It all comes down to a personal preference. Webpack seems to be the most popular, and I'd recommend that you go with that. However, it's not like you're writing Webpack modules or Browserify modules. You're writing a standard ES6 module, so if you were to switch out your bundler at any time, you should be able to do that with no problem.

There's another one called SystemJS that works with something called [JSPM](http://jspm.io). JSPM is JavaScript Package Manager, and it sits on top of npm. It's not like an alternative to npm, it just sits on top of it.  One really cool feature of SystemJS is that you can run it in the browser, which means that you don't need any of the overhead of npm, or installing Webpack, and getting these bundlers up and running, all of that hard work that we just did in the last couple posts. 

Sometimes that can be a bit of a barrier to entry when you just want to try something out, or you got a couple of hours on a Friday night and you want to hack on something. You don't want the tooling to get in your way.

SystemJS, really how you do it is you load in a script tag:

```html
<script src="https://jspm.io/system@0.19.js"></script>
```
 
That's going to load the latest 19.x version. Then we also need to run this HTML through some sort of server, like a PHP server or a Python server,it just needs to not run as a file because a browser will think it's a security risk. It needs to run on a localhost or something like that.

You can spin up a browser-sync server pretty quickly if you remember from a couple posts ago. To do that, you can create a new systemjs folder and save your html file there, and use `npm init` to install a `package.json` file, and then use `npm install browser-sync --save-dev`, and add it to your `package.json`.


Then I'm going to install browser-sync--savedev. I'm going to go to my package.json and delete that test one that we had there, and create a server script, and paste in browser-sync, start sever. These need to be single quotes. Start directory, server, files, and then we're going to watch all of our JavaScript HTML and our CSS files.

If we've done that correctly, we should be able to head back to where we installed browser sync, which you see in our package.json. We have a dev dependency of browser sync, and we should be able to npm run server. 

That will then open up a little server for you. You should see index.html, which I have there for you, and then any other files that you have in there. That's just a quick and dirty little server if you ever need one. I prefer browser sync, because any time I make a save it's going to refresh it for me without any extra work.

Let's quickly setup our `index.html`:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8"
    <title>Systemjs</title>
</head>
<body>
    <script src="https://jspm.io/system@0.19.js"></script>
    <script>
      System.config({transpiler: 'babel'});
      System.import('./main.js');
    </script>
</body>
</html>
```

You can see that we've loaded up jspm to call in SystemJS, and then opened up another script tag to do two things:

First, it's going to configure SystemJS, and then it's going to enter our entry point, `main.js`, which is just something like:

```js
console.log('Hey, it works!');
```

So we have `System.config({transpiler: 'babel'});` `System.config` takes an object and in this case we're going to use Babel as our transpiler.

Next we call `System.import`, and then you import the actual application file, `main.js`, and so on, and keep writing our modules as we have been.

The really cool part about using SystemJS is that I don't need to do `npm install`, or do any of the stuff that we've previously done. Let's say I wanted to get some methods from `lodash`, so we'll say import, and this time let's get `sum` and `kebabCase` and import them into our `main.js`:

```js
import {sum, kebabCase} from 'npm:lodash';

console.log(kebabCase('Wes is soooo cool'))
```

What that's going to do is go to npm's registry and import `lodash` for us, and I've added in a console.log there to show what I mean, Once you refresh this page after a second or two you'll see the kebabCase version of our string. We didn't have to run `npm install`, you're just able to go ahead and import things.

If you had your own module, for example, let's call it `checkout.js`:
 
```js

export function addTax(amount, taxRate) {
  return amount + (amount * taxRate);
}


```

Remember that we want to export this function so that we can import it into our `main.js` which now looks something like:

```js
import {sum, kebabCase} from 'npm:lodash';
import { addTax } from './checkout';

console.log(kebabCase('Wes is soooo cool'))

console.log(addTax(100, 0.15));
```

You can see we've added in an `import` for our `addTax` function and use the relative path for checkout.js, and just run a simple tax function on a $100 bill. That is so much better than having to set up all of those tools that we did for 20 minutes in the last video, where you just need to import this one file. 

I think that is a great way to show someone how modules work. I think that's a great way, if you just need to get something working, or if you're trying to explain something and you need to set up a test case, and you don't have to make anyone else `npm install` all of your stuff. You just use it all client side. 

Obviously, it's not something you do for production as it is quite slow, but it's great for just testing things out.
