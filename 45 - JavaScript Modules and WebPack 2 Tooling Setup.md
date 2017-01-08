The way that we write JavaScript applications is changing. Previously, what we would do is have maybe a couple of script tags in the body of our document, and maybe load in jQuery in our own JavaScript, or maybe we had a build process where we would concatenate and minify all of JavaScript together. However, with ES6, it's starting to become popular to pick up **JavaScript modules**. Now, this is not new to JavaScript, however, with ES6 we're basilcally seeing everybody adopt it. 

What is a JavaScript module? It's a file with one or many functions inside of it, and then you can make those functions available to other files, and available to other applications. You can also use other people's existing modules. Instead of using a script tag and having a whole bunch of global variables, what we do is we import things from existing modules.

Right here, I've got three import statements:
```js
import slug from 'slug';
import {uniq, shuffle } from 'lodash';
import Flickity from 'flickity';
```
So here we need the `slug `library. Instead of finding one and using a script tag, we used an `npm install` and imported `slug` from `'slug'`.

We're going to be using a utility library, `Lodash`, and we need to pull in a couple of the methods that we explicitly need here, `uniq` and `shuffle`. We're just importing those two things.

We're also using a slider plug where you need to import `Flickity`, so we import it from the package that is called `flickity`.

Those are all **JavaScript modules**. We are going to dive into how this all works, the tooling of all it works, how we can create our own named exports versus default exports. It's a pretty big topic to cover, but this is really crucial to understanding how to write modern JavaScript.

Everything we've been doing so far, we've been simply writing JavaScript in HTML files and opening it up in the browser. That's because ES6 has fantastic support across all the browsers. The one thing where support is really lacking, and we're sort of seeing browser vendors look at how they're going to implement it, is ES6 modules. We do need some tooling in order to actually start using the ES6 syntax and using the idea of importing and exporting modules.

What we need to do is, first, set up a package adjacent file, which is going to let us install our external dependencies from [npm](http://npmjs.com), and then we're also going to to use [Webpack](https://webpack.github.io/), which is going to bundle up all of our JavaScript together.

If we import a couple libraries, export a couple things, and have maybe seven or eight files, it's going to bundle it up into a single or multiple files for us.

So let's head into the console, and set up our folder:

```bash
mkdir es6modules
cd es6modules
pwd
```

Triple-check you are in the right folder, and in a directory called 'es6modules'. Again, I this doesn't exist unless you create it. You should create that yourself.

Then we need to start things off. Now, there are lots of ways to get modules. There is [Bower](https://bower.io/) and there's something called [jspm](http://jspm.io/). But the pretty standard way to do it and what has won out is called [npm](http://npmjs.com). You probably have stumbled upon npm before and that's what we're going to use to install all of our dependencies.

What we need to do is first create ourselves an entry point. I like to call this `app.js`, so let's use the console to create our file and create our `package.json` file for npm:

```bash
touch app.js
npm init
```

It's going to ask you a series of questions here in terms of what would you like to call it. You can go ahead and fill this out to be whatever you want.

Really there is not any information that you need to put in so you can just hit Enter as you go on through, and leave everything blank.

We're going to go back into that `package.json`. If you actually take a look in our project directory we'll see that our `es6modules` directory now has our `app.js` and a `package.json` file. If you open up `package.json`, you'll see that it just has some information about it.

Why that is important is because this `package.json` is going to save all of the references to the packages that we want to install.

Let's go ahead and try and install a couple packages. 

In the console, type `npm install slug --save`. r

[Slug](https://www.npmjs.com/package/slug) is a library that I like to use if you ever need slugs. You can check out their example to see how it can convert, say, emojis in to text for a URL slug.

By doing that, and you can see the change in your `package.json` file:

```json
{
  "name": "es6modules",
  "version": "1.0.0",
  "description": "",
  "main": "app.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "slug": "^0.9.1"
  }
}

```
 
Now we have `slug` as a dependency, and also you should see a new `node_modules` folder pop up in our `es6modules` folder. You don't really have to look inside of here, but what we need to know is that it installed a library called `slug` in that folder.

I got a lot of emails with people asking me, "Why are there so many folders in here?" These other folders are just because the `slug` module has dependencies of its own and these install this way. Really, don't worry too, too much about what's inside of this `node_modules` here, think about it like a utility directory.

Let's go ahead install a couple more:
```bash
npm install lodash flickity --save
```
Now, if we go back to our `dependencies` in `package.json`, you see we have `flickity`, `lodash` and `slug`:

```json
{
  "name": "es6modules",
  "version": "1.0.0",
  "description": "",
  "main": "app.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "flickity": "^2.0.5",
    "lodash": "^4.17.2",
    "slug": "^0.9.1"
  }
}

```

If you're using jQuery or any other library like this, pretty much every single JavaScript library is available on npm. You can install it like this.

One thing I want to tell you is if you ever accidentally or on purpose delete the `node_modules` folder, and actually, I'll often delete the `node_modules` folder on purpose, because it is fairly large and I run out of computer space. 

Anyway, if you ever delete it, you simply just need to type `npm install` in your terminal. What that does is that it will read your dependencies in your `package.json` file, and will say, "Oh, I need to download `Flickity`, `Lodash` and `Slug` for it."

You never need to check this `node_modules` folder, or put it in your GitHub repository. Really, you never really need to hang on to it unless you're going on a plane and need offline access. You can always type `npm install` to get everything again.

Go ahead and install a couple more libraries. A couple that I use day to day are jQuery, and Insane, which is an HTML sanitization library, and something called JSONP, which we'll talk about in a minute:

```bash
npm install jquery --save
npm install insane --save
npm install jsonp --save
```

Now we want to go to our `app.js` file, and write some import statements, but we're only going to take a couple of methods from `lodash`, like in the example above:

```js
import {uniq, shuffle } from 'lodash';
import insane from 'insane';
import jsonp from 'jsonp';
```

What I'm doing with `lodash` is that we're importing a named export. What is that? We'll learn all about it soon, but you have to put curly brackets around it. We're also importing `insane`, and `jsonp`, which is something I want to show you, because we looked at `fetch` a while ago, but `fetch` doesn't work with `JSONP` API.

If you ever want to work with `JSONP` API, you need to install this really nice small package called `JSONP`. 

All right, let's put together a quick HTML page to try this out:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8"
    <title>JS Modules</title>
</head>
<body>
    <script src="app.js"></script>
</body>
</html>
```


If we open this file up in our browser and open the inspector, we'll see we get a console error `unexpected token import` on the first line. It doesn't know how to actually handle this import stuff because that's a one piece of ES6 that has not yet been implemented in browsers.

What we need to do is we need to use Webpack that's going to bundle it all together for us. This a little bit of a task to get up and running, but it's generally one thing that you do at the start of your project and you're golden. If you have multiple projects, you can re-use this set up. We'll go through it together.

You can find boiler plates online, however, they are so confusing, and it's a really overwhelming space. I really want to make sure that you understand what's happening with the module bundling here with Webpack. 


First things that we need to do is install some dependencies here. 

I'm going to be using **Webpack 2** right now because that's going to come out pretty soon. It's not yet available. We had to do `npm install webpack@beta --save-dev`. 

The way you can check if you need that app beta any longer is you can go to [the Webpack package listing on npm](http://npm.im/webpack), and then check the version of Webpack. See the current one for me is 1.13.3.

If that says 2.0 for you, you simply just need to `npm install Webpack`, with no need for the `@beta` add on. If it still says version 1, you should use the beta, because there's really no point in learning Webpack 1 if Webpack 2 is right around the corner.

The reason we did the `--save-dev` option is because Webpack is just a development tool, and not part of our application.

So now when we look at our `package.json`, we can see a new section included under `dependencies` called `devDependencies`:

```json
{
  "name": "es6modules",
  "version": "1.0.0",
  "description": "",
  "main": "app.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "flickity": "^2.0.5",
    "lodash": "^4.17.2",
    "slug": "^0.9.1"
  },
  "devDependencies": {
    "webpack": "^2.1.0-beta.27"
  }
}
```

There we go. Popped up, Webpack, this one is beta.27, it will probably a more recent version when you actually get around to doing the series.

Then we also need a couple things that's going to help us convert our code to ES5. Now we haven't talked about Babel, but the long and short of it right now is that it will take your ES6 code and convert it to the equivalent ES5 code and make it work in all of the browsers.

We will go much more into it, but this is the point when we actually integrate Babel into our build system so that it will work on all of our browsers.

We need to install a whole bunch of Babel add-ons, so just run this in your console, and I'll go over it:
 
`npm install webpack@beta babel-loader babel-core babel-preset-es2015-native-modules --save-dev`


We also need this thing called a Babel loader, we need the Babel core, and then we need this other thing, Babel preset ES2015 Native modules. 

What that does is it's going to convert all of our code from ES6 or ES2015 down to ES5, but this **Native modules** version is going to skip converting them to this older module version called common Js, because Webpack 2 is able to handle Native modules.

Now, if top of your head just blew off after everything I wrote don't worry about it. You'll catch on to it. 


Just take this one npm install line above, pop it into your, and let that install. You can check if it worked if you go to your `package.json` dev dependencies now includes Babel core, loader, preset and Webpack are all installed. While we're there, add a `"build"` script below our `"test"` script:

```json
{
  "name": "es6modules",
  "version": "1.0.0",
  "description": "",
  "main": "app.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack --progress --watch"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "flickity": "^2.0.5",
    "lodash": "^4.17.2",
    "slug": "^0.9.1"
  },
  "devDependencies": {
    "babel-core": "^6.18.2",
    "babel-loader": "^6.2.8",
    "babel-preset-es2015-native-modules": "^6.9.4",
    "webpack": "^2.1.0-beta.27"
  }
}
```

Finally, we need to create a Webpack file that's actually going to hold all the config for this, so let's create a `webpack.config.js` file to hold our Webpack configuration by typing `touch webpack.config.js` in the terminal to create the file, then add the following in your editor, which I'll go over:

```js
const webpack = require('webpack');
const nodeEnv = process.env.NODE_ENV || 'production';

module.exports = {
  devtool: 'source-map',
  entry: {
    filename: './app.js'
  },
  output: {
    filename: '_build/bundle.js'
  },
  module: {
    loaders: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        loader: 'babel',
        query: {
          presets: ['es2015-native-modules']
        }
      }
    ]
  },
  plugins: [
    new webpack.optimize.UglifyJsPlugin({
      compress: {
        warnings: false
      },
      output: {
        comments: false
      },
      sourceMap: true
    }),
    new webpack.DefinePlugin({
      'process.env': { NODE_ENV: JSON.stringify(nodeEnv) }
    })
  ]
};
```

Now you might be saying, "Why are we using `require` instead of `import` here?" The short answer is that Node.js doesn't support ES6's `import` yet, so we still need to use `require`. So we're adding in Webpack, and then we've added our nodeEnv for our production environment.

Next, we have `module.exports`, which is an object, containing a few things: We have a `devtool`, which is going to give us something called `source-map`. We have an `entry` point. With Webpack you have an entry point, and it essentially says, "Where do you want to start your application? We are going to start in `app.js`:

```js
module.exports = {
  devtool: 'source-map',
  entry: {
    filename: './app.js'
  },
```

Then we set the `output`m which is where do you want it to go, the name is going to be, let's make a folder called `_build` in our project, and point the output at `_build/bundle.js`:

```js
output: {
    filename: '_build/bundle.js'
  },
```

Then we have a `module` inside of here which has a `loaders` array. Loaders are essentially how should I handle specific types of files because Webpack is such a beast. You can use it for absolutely everything. It can pretty much do everything under the sun.

One of the loaders that we are going to do is we are going to take all of our JavaScript files and we are going to run it through Babel with our specific plug-ins. The first loader is going to be an object and the way that you tell it the entry point is that if it is a .js file. We'll also set up a test for our `.js` files using Regex. We want to exclude the `node_modules` directory from our test, because we don't want it to run against the entire node modules directory.

```js
module: {
    loaders: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        loader: 'babel-loader',
        query: {
          presets: ['es2015-native-modules']
        }
      }
    ]
  },
```

The `loader` that we are going to run our JavaScript file through is `babel-loader`, which is a recent change. This converts it to ES5, then there is a query which will allow us to set some presets with JavaScript. In our case here, we want the ES2015 dash native dash modules.

Again the reason I am using the `es2015-native-modules` here is because Webpack 2 doesn't need them converted to common JavaScript files. Webpack 2 knows how to handle ES6 modules, which is really, really nice.

So, that is basically my module up and running. 

The final parts of of our `webpack.config.js` file are plugins. We need some plug-ins that are going to actually handle everything


```js
plugins: [
    new webpack.optimize.UglifyJsPlugin({
      compress: {
        warnings: false
      },
      output: {
        comments: false
      },
      sourceMap: true
    }),
    new webpack.DefinePlugin({
      'process.env': { NODE_ENV: JSON.stringify(nodeEnv) }
    })
  ]
};
```

We're going to use `UglifyJsPlugin`, which is a common tool that's just going to compress our JavaScript so it is nice and small. Then we have the have the `process.env` plugin, which will set the actual process environment. So we call those in via `new webpack.optimize.UglifyJSPlugin`, That takes the object of UglifyJS, which let's us set some properties, like `compress:`, and `output`, and `sourceMap` like we have above. That's set for Webpack 1.
 
So now we have our `process.env` plug-in, which is set in `new webpack.DefinePlugin`. That will also take an object, `process.env` is the `NODE_ENV`, which we set to `JSON.Stringify`, and so on.

Essentially what that does is it passes the environment variable to our code and plug-ins, like Uglify, and they are going to be able to know if they should remove certain things or not depending on what environment we are actually in.


Remember earlier when we added that `"build"` line to our `package.json` file? Let's take a look at that now.

So we need to build our project with Webpack. We could run it from the command line. We could install Webpack globally using npm, but a better way is to use what is called an **npm script**.

If you head on over to your `package.json` file and scroll up to where it says `scripts`, take a look at our new `build` command.

Now, without having to globally install Webpack we should be able to run `npm run build`. We say `Run` and `Build` is the name of the actual script that we have here.

So what happened here is that it created us a Build file which is` bundle.js`, and our `bundle.js.map`.

If we go to our `index.html`, we need to swap out our `app.js` to `_build/bundle.js`. Why? Because that's where our build is.

If we go to our `app.js` and try to use these things, so let's just say:, 

```js
const ages = [1, 1, 4, 52, 12, 4];
console.log(uniq(ages));
```
 
I have these ages here, and I want to just get the unique versions of them, using that Lodash method we called in earler. When you save, it reruns this Webpack bundle, and everything updates.

If we open up our `index.html` in our browser and open up your JavaScript console here, you now see our unique array. It tells us that we wrote that on `app.js`, on line 7.

That was a very basic getting up-and-running with Webpack. I know it was a little bit of a slog to get through some of that. It's the tooling is a little bit tough right now, but if you get the hang of it, it's definitely worth implementing into your workflow.