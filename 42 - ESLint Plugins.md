One really cool thing about ESLint is that there is a fantastic community that has built great plugins for ESLint, and if you take a look at [this awesome ESLint GitHub repo](https://github.com/dustinspecker/awesome-eslint), you'll see that there is a listing of all kinds of different plugins that you can use depending on what type of JavaScript that you write every single day.

Whether you're writing Angular or Backbone, or you need to specifically lint your Jasmine docs, every time you write stuff that is for a different framework, you can have a little bit of difference in your style rules. Chances are that someone has already built some sort of linting plugin for that specific framework.

A plugin I use a lot is for when I need to lint my JavaScript that is inside of script tags in HTML files, because that's often how I teach. It doesn't require extra JS files, and it doesn't require any extra tooling to get up and running. Secondly, I often will write JavaScript inside of markdown, which is very often the case when I'm writing docs or notes for a course.

How do I lint code that as not inside of a JS file? If you try to run ESLint code right through HTML, it's going to give you an error and say "This ESLint doesn't understand HTML." We need a plugin that's going to strip your JavaScript out of your script tags, lint it, and tell you what's wrong with it. Same goes for markdown.

Let's take a look at how to use those two specific plugins here. I'm going to go into here and find the HTML. Looks like the HTML is actually not in here, so I'm going to say ESLint HTML, [which is actually hosted over on NPM as a package](https://www.npmjs.com/package/eslint-plugin-html). As a note, this also works in `.erb` files for Ruby on Rails, `.php` files, and a whole lot of other file formats as well. 

Remember, to install an npm package, you can use `npm install eslint-plugin-html -g` to install it globally, or  however you need to install the plugin.

To install an ESLint plugin, open up your `.eslintrc` file and and you have a property called `plugins`, and that itself is an array, and you simply pass the name of all of your plugins that you want.

```json
{
    "env": {
     "es6": true,
     "browser": true
},
"extends": "airbnb",
  "rules": {
    "no-console": 0,
    "no-unused-vars": 1
  },
  "plugins": ["html"]
}
```

In my case, I installed it globally on my system, and now any time I have an ESLint file, I simply just say I have the HTML file, give that a save. Now if I run my ESLint against an HTML file, it actually tells me what's going on. It doesn't trip up on any of my actual HTML, but it's going to tell me things how my variables might not be reassigned and I should  use const instead, or I have an unexpected alert.
 
Let's do the same for markdown, [which can be found over on GitHub](https://github.com/eslint/eslint-plugin-markdown). So again, we're going to use `npm install eslint-plugin-markdown -g` to install it globally. It'll take a minute or two to install.

Once again, you add the plugin you want to use in your array, which will look something like this:
 
```json
{
    "plugins": ["html", "markdown"]
}
```
Like before, you can run ESLint against your markdown files using something like `eslint *.md` to look at all of your markdown files in a specific directory. 

Using that `*.md` is something called "glob pattern". If you have ever used gulp-matching or any other sort of matching, you can pass a list of `*.md`, or `*.js`, or `*.html` or whatever. You can also pass it `--ext`. If you wanted to know all of the options for ESLint, simply type `eslint --help` and it will give you a list of all of the possible ways where you can search for different kinds of extensions, and pass it a specific ESLint file.

Those are plugins. Take a look through it and see which plugins probably work best with your workflow, but those are the two that you probably will run into at some point, and they're very helpful to use. 

One little thing I should mention that is currently, at the time of recording, you cannot use `--fix` on HTML or markdown files. `--fix` only works on pure `.js` files. Hopefully that will get fixed soon, but don't spend any time trying to get it to work because it's not possible.
