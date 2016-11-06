[ESLint](http://eslint.org) is an indispensable code quality tool when you're writing any type of JavaScript, especially ES6. You may have used JSHint or JSLint in the past. However, it seems to be that most of the community has moved over to ESLint, specifically because they have support for all of the new ES6 features.

Now what is ESLint? 

ESLint essentially, it hurts your feelings. It looks at your code and tells you what exactly is wrong with it. 

At first it seems a little bit overwhelming because it tells you everything is wrong with your code, but it's all a matter of:

- Getting it configured properly for your coding style 
- Actually understanding why are these errors happening. 


In the long run, it's going to make you a much better developer. It includes a [demo](http://eslint.org/demo/) that you can try out and see what comes back. For example, "Strings must use doublequote" is a habit I have that it doesn't like, or it finds extra spaces and stuff like that.

ESLint finds all kinds of different stuff and catch code quality problems before they actually cause an issue.

I'm going to go through setting it up, getting it installed, and then I'm going to show you how do you configure it and what is your process for actually understanding how it works. 

In my experience, the first time someone uses it takes maybe an hour or two to get it really where they like it, but you're going to be writing better code and you're going to earn that time back after no time.

There's a few things we need to in order to get started. 

First of all, you need nodejs and npm installed. Chances are, most of you already have this installed. If you don't know, head on over to your terminal and check your versions of node and npm:
 
```bash
$ node -v
$ npm -v
```
In order to continue, you'll need to make sure that you have version 4.0 or higher. If you have an old version or you get an error, like `node is not a command`, go to [nodejs.org](http://nodejs.org) and download the latest version. 

 Once you have that installed, we need to install ESLint. The way that ESLint is going to work is we're going to type `eslint` at the command line and point it at a file and give us our errors and problems in the file.

To install that, use your terminal:

```bash
$ npm install -g eslint
```

If you get a permission error, if you're using OSX for example, you can override any permission errors by using `sudo`.

If you have that properly installed, you should be able to type `ESLint --version` and it should tell you that you have at least version 2.0 or up, but do yourself a favor and just download the latest version of ESLint. 

We're going to start by working from the command line. I'm going to show you how to integrate it into your text editor in just a bit, but let's get it up and running.

How do you actually get it to work? Ideally what's going to happen is we're going to have something that runs on our command line, something that runs in our editor, or something that runs on a git hook before you commit your actual code to a repo.

Let's start via the command line. 

So let's say for example that we have a `bad-code.js` file that we're working with.


So from the command line, you'd just type:
```bash
$ eslint bad-code.js
```

What that's going to do is it's going to scan our file and tell us what is actually wrong with it. One of the things you might want to do is configure ESLint to account for arrow functions, because it doesn't do that right away. This is where the settings for ESLint actually start to come in handy.

ESLint settings can be doneglobally on your computer and or you can do it project by project. 

A lot of people prefer to do it project by project because there's different coding styles depending on which project or team you are working with. We are going to go ahead and create what's called a `.eslintrc` file in the folder right here, and that's going to hold all of our actual settings.

```bash
$ touch .eslintrc
```

That will have created a new file called `.eslintrc`. Sometimes some computers hide files that start with a dot, so you might not immediately see it in your Finder or Windows Explorer. However, if you go to your editor and open that folder you should see the `.eslintrc` file.

Inside of the empty `.eslintrc` file we are going to write json, which is full of all of our settings. To write json, you open up an object and we now need to specify a whole bunch of options that we need. What are the possible options? Well, ESLint is full of options, and it can be a little bit overwhelming.
 
If we head back over to the [ESLint demo page](http://eslint.org/demo/) and scroll down a bit, you can see some of the options, like Environments. Are you writing node, Mocha, Jasmine, PhantomJS, QUnit, or whatever. There's also all these different rules that you can have. We'll look at a shortcut way, because there's no way you have time to make a decision about every single one of the possible rules.

So let's set up some rules in our `.eslintrc` file.

```json
{
    "env": {
            "es6": true,
            "browser": true
    
    }
    
}
```

Since we're writing json here, you must use double quotes for your keys For the sake of this example, we're only going to use ES6 and browser. 

However, you can always go to the [ESLint.org's documentation and check out the possible rules](http://eslint.org/docs/rules),as well as if you check out [Configuring ESLint](http://eslint.org/docs/user-guide/configuring), you're going to see a list of all of the possible environments.

Now, I've set the environment ES6 to be true, and now we should be able to run ESLint on our file.
 
When we do that, though ,it doesn't tell us anything. Why is that? Because by default ESLint doesn't have any rules enabled. You have to turn on all the rules.  Rather than you having to go to user guide, rules and just wade through all of these possible rules, what happens is that there are handy presets out there for other people that have put together.

There's one that is the ESLint recommended. The way that you pick up someone else's preset is to add to your `.eslintrc` file:

```json
{
    "env": {
            "es6": true,
            "browser": true
    
    },
    "extends": "eslint:recommended"
}
```

Now if we go back to our terminal and re-run that that on your code file, you'll start to see some errors.

As you go back through your code, you'll be able to resolve each error reported by ESLint and make decisions on how to refactor your code. After each resolution, you'll be able to reduce your errors.

By default, ESLint recommends against using stuff like `console.log`, which makes sense. However, we're saying, "It's fine that I'm using console, I know what I'm doing. I'll make sure that it take them out, or maybe I'll have another ESLint that that role will be set on development and not in production." 

But to turn it off, we can add custom rules to our `.eslintrc` file.

Before we do that, how do you actually work with these rules? For `console.log`, the rule is `no-console`, which ESLint will tell us.
 
 
So if we [check out the rule](http://eslint.org/docs/rules/no-console), spend some time reading why this rule is set. Don't just turn it off immediately because it's annoying and you think you're right. Maybe just spend some time and say, "Maybe I shouldn't be using this rule, or maybe I shouldn't be writing my code in this way. Why is this possibly bad?" 

Read through why they think that maybe this rule should be turned on. It looks at some examples as to what you shouldn't be using, and then finally there is when not to use it.

How do you turn it off if you don't like it? 

There are a couple ways you can turn it off. At the very high level there's three stages of rules. They can be "off", they can be "warning", or they can be "error". Sometimes I like to turn things on just to "warning", so I know that that possibly shouldn't be there. However, an "error" will go red.

So in our `.eslintrc` file:

```json
{
    "env": {
            "es6": true,
            "browser": true
    
    },
    "extends": "eslint:recommended",
    "rules": {
      "no-console": 0
    }
    
}
```

You have a few options on how to set your rule. you can use `"off"`, `"warn"`, or `"error"`, or use `0` for off, `1` for warning, or `2` for error. There's also the ability to pass options here. We'll look at that in just a second, but that's very basic.

What our `"rules"` line is doing is it's taking all the rules from the ESLint recommended set, and then we're taking the no-console rule and applying our own setting. It's like CSS, where you have a base framework here and then you overwrite your own specific ones.

There's all kinds of stuff that you can do to tell ESLint to warn you about, especially stylistic ones. In the next post we'll look at Airbnb's default rules, which are a little bit more strict but will allow you to write much better code.