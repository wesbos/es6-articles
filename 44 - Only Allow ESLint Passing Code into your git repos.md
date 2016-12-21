If you work on a team or an open source project with other people, it's helpful to have an ESLint file right in your Git repo, and then you can have what's called a Git hook, which will not allow anyone to commit any code, unless it first passes the ESLint rule. That's really important because than you're going to keep up code quality for everyone that touched that project.

Starting from scratch, let's say there's no predefined folder you're going to get. Go ahead and use the folder you've been testing out ESLint so far in, because we'll need that as a base. Open the folder in your terminal and type `git init ES6git`. It should make a folder called `ES6git`

Go ahead and create a file that actually has some bad code in it  by typing `touch code.js`, and open up that file in your editor. 

So let's give it some bad code:

```js
var x= 100
```

Go ahead and run ESlint on the file, and you'll see that it yells at you, and there are all the specific things that we just did.

You'll note that we don't have an ESLint file in this folder at all, but it's picking the one up from a parent folder. because that how it works. It will always check the parent until it finds one. 

Now we're inside of `ES6Git`, and I want to be able to commit these files, but before I do that, we want to make a hook for this. A lot of people don't know this, but if you were to open up the actual `.git` directory in your editor, check out what you get inside of there.

There are a few folders in there, like `branches` and other kinds of info, but there's a folder in here called `hooks`. If you open that up, there's all kinds of sample hooks. In git, hooks are essentially code that runs before things happen, and you can stop those things from happening unless they pass a specific use case.

In our case, if you open up the `commit-message.sample`, this is an example of something that will run before someone commits their code. What I want you to do is rename this file by removing the .sample so it's active, and go ahead and wipe it out.

Once you've done that, go ahead and copy-paste everything from [this Gist](https://gist.github.com/wesbos/8aec9d2ff7f7cf9dd65ca2c20d5dfc23) into the file. Make sure you get it all, not just forget to copy the last line. That happens to me a lot.

Then we're going to go to back to our terminal, and run a `git status` to see you have code.js as a new file. I want to be able to commit it, so run a quick `git add .` and `git commit -m "added new code"`

But as soon as I hit enter or return, we get our error message and our commit is denied. Why? This is a pre-commit message. I'm about to add a commit message to my commit, but before it does that, because we edited `commit-message` with the gist, it's going to run our check and say "ESLint failed. Git commit denied." 

So with our check, you get the same four errors here that we got in our manual lint earlier. What we have to do is go back to our code here, and we look at all the different errors. 

First error, unexpected var. I should have used const. Then next error, X is defined but never used, so I should have said console.log X. Infix operators must be spaced. I forgot the space there, and missing semicolon. I've got to put a semicolon on the end there.

So we get `Unexpected varm use let or const instead`, `'x' is defined but never used`, `Infix operators must be spaced`, and of course, our `Missing semicolon`.

So let's fix it up:

```js
const x = 100;
console.log(x);
```

If we go to add and commit, and include our message again, you'll see you didn't have any problems with our ESLint, so everything went ahead, and now your master branch is in a clean state.