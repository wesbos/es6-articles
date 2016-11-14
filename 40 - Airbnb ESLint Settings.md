The engineers at Airbnb have created a really nice JavaScript style guide that shows you how to write ES6 in a way that is consistent across all of your projects. It does things like tell you to use `const` and when to use `let`, and how to write your `if` statements, and where do you semicolons and that kind of stuff. Their ESLint file is a way to enforce their style guide.

A lot of people adopt the Airbnb style of coding and then they'll use their own settings on top of it, and that's exactly what we're going to do. We're going to implement the Airbnb style guide and then we're going to go through in and pick off little things where we think that our style differs a little bit.

If you [go to Airbnb's GitHub repository](https://github.com/airbnb/javascript/tree/master/packages/eslint-config-airbnb), they provide a readme to tell you how to install their style guide. 

To install someone else's style guide you can go to your `.eslintrc` file, and instead of saying `"extends": "eslint:recommended"`, we can use `"airbnb"`:

```json
{
    "env": {
        "es6": true,
        "browser": true
},
    "extends": "airbnb",
    "rules": {
    "no-console": 0
    }
}
```
If we run ESLint against our `badcode.js` example file, it will tell you `Error: Cannot find module 'eslint-config-airbnb'` Why not? Because we haven't actually installed it yet. Even though we declared `airbnb` in in the `.eslintrc` file, we need to install the config files that we're going to use. 

You can install this locally to your project if you only need it on one project, but since, I like to use it on every single project that I'm working on I like to install it globally.

Over on the [GitHub repo](https://github.com/airbnb/javascript/tree/master/packages/eslint-config-airbnb), there's some install instructions right in the readme. It should look something like this:

```bash
npm install --g eslint-config-airbnb eslint@^#.#.# eslint-plugin-jsx-a11y@^#.#.# eslint-plugin-import@^#.#.# eslint-plugin-react@^#.#.#
```

There are a few packages included in there that we might not need, like `eslint`, or the `eslint-plugin-react` if we're not writing React. We do need to install some of the peer dependencies, and Airbnb's install process changes slightly over time, so you might be better off just following their instructions. 

However, we want to install it globally, so we're going to run our command in the console:

 ```bash
 npm install -g eslint-config-airbnb eslint@^#.#.# eslint-plugin-jsx-a11y@^#.#.# eslint-plugin-import@^#.#.# eslint-plugin-react@^#.#.#
 ```
 
Here we've used `-g` instead `--save-dev` because we're going to install it globally. Remember that you might need to use `sudo` to run `npm install`, especially if you're using macOS, unless you've fixed your npm. 

Once you have that installed, we can run ESLint against our `badcode.js` file, and it's going to tell us all kinds of new errors that are happening in our code. 

Many of our errors in `badcode.js` have to do with spacing, where we get `A space is required after ','`, or `A space is required before '}'`, Those are just such minor little things that ESLint will actually fix immediately for you if you run `eslint bad-code.js --fix`. Instead of having to go through our file and fix each spacing one by one, it's taken care of.
 
By running that, we're able to go from 12 problems down to 2 problems. It's going to fix almost all of those little spacing issues for us. Howeever, now we've got two other problems here that we need to take care of.

First of all `unexpected var use let or const instead`, which is pretty straight forward. In our example, I have `var weather = new Promise`. So instead of `var`, should I use `let` or should I use `const`? Now the question here is will I ever reassign `weather`, does it need to update itself or is it always going to hold this `new Promise`?

In the case of us we're always going to be holding the `promise`, so we can use `const`.

Our other error is `Unexpected block statement surrounding arrow body`, which is `arrow-body-style`. It may not actually what the problem is here, let's go check it out:

```js
Promise
    .all([postsPromise, streetCarsPromise])
    .then(responses => {
        return Promise.all(responses.map(res => res.json()));
    })
    .then(responses => {
        console.log(responses);
    });
```
The issue here is that we have a block statement and then all that block is doing is that `return Promise.all`. What it actually should be telling is that you should use the implicit return if all that you're going to be doing is opening up a block and returning one thing from it.

If we check out our [ESLint documentation](http://eslint.org/docs/rules/arrow-body-style), it says: "Arrow functions have two formats. They may be defined with a block or it's a single expression" and it shows us some examples of when you wouldn't want to do it and when you would want to use it. I like that error, so I'm going to not turn it off in here but I'm going to actually go ahead and fix that myself. We don't need a block here or a return, we don't need the opening block.

```js
Promise
    .all([postsPromise, streetCarsPromise])
    .then(responses => Promise.all(responses.map(res => res.json())))
    .then(responses => {
        console.log(responses);
    })
```

Back over in our terminal, we can rerun ESLint, and it should like celebrate or something and tell you that you did a good job but it's simply just gives you your command prompt, and nothing else.

So far we've been working with the local `.eslintrc` file and that means that we have an ESLint file for every single project that we work on. That can be really handy when you have different coding styles amongst different projects or different teams that you work with. However, it's also helpful to have a global `.eslintrc` file, which if you do not have one anywhere inside of your project, it's going to use your global one as sort of a default.

I really like to do that because it means I don't have to create an ESLint file for every single project or I have one that I can copy/paste into new projects that I'm going to put up to GitHub and expect others to follow that coding style. I'm going to open up my global one and it lives in your home directory.

What's your home directory? Well, it depends. On a mac, it's generally your username directory, on Windows machine it's under `C:\users\username`, where user name. 

Your home directory is `~`, If you do not have one you can use your terminal to create one by typing `touch .eslintrc`, but if you already do have one you can just open it up in your editor.

I'm going to show you exactly what is inside of mine:

```json
{
    "extends": "airbnb",
    "env": {
        "browser": true,
        "node": true,
        "jquery": true
},
   
    "rules": {
    "no-unused-vars": [1,{"argsIgnorePartern": "res|next|~err"}],
    "arrow-body-style": [2, "as-needed"],
    "no-param-reassign": [2, { "props": false }],
    "no-console": 0,
    "import": 0,
    "func-names": 0,
    "space-before-function-paren": 0,
    "comma-dangle": 0,
    "max-len": 0,
    "no-underscore-dangle": 0,
    "react/prefer-es6-class": 0,
    "radix": 0
    }
}
```

So usinging Airbnb work in the browser, Node.js, and sometimes I use jQuery so I've turned all of those on. 

Then I have all of these rules that I have been working with, and I take all the rules from Airbnb and then these are the ones where I don't necessarily agree a hundred percent with Airbnb's implementation. These are just the once that I've sort of taken over.


Looking at `"comma-dangle"` as our example, without my rules, I can write some code like:

```js
const wes = {
age: 100,
cool: true
}
```
So if we run ESlint, it's going to yell us with a whole bunch of stuff, like `no-unused-vars`, that's good because I've created a variable, `wes`, and haven't yet used that.

That's actually one that I have turned to a warning in my regular ESLint rules, and you can see `no-unused-vars` as my first rule above, because I say, "Yeah, I should know when I don't use a `var` but often I'm coding and like I'm not done my application yet and it's already yelling at me for my errors." 

What I would do is I turn that to an actual warning, which I've done by setting it to `1`

Back to this `comma-dangle here`, what is the actual error? Sometimes people will say that you should absolutely always put a comma even if you don't have another line, like here on the end of `cool: true`:

```js
const wes = {
age: 100,
cool: true,
}
```

That's because if you have checked something into Git, and you add a new line, you're actually changing two lines without that dangling comma. if I wanted to add another thing to my object, like `dog: "Snickers"`, I'd need to add the comma to the above line if it wasn't there.

A lot of people put that dangling comma on the end and I actually generally agree with that but I don't use it every single time and I find it to be a bit of a pain that it errors out on me, so I've turned it off in my ESLint. You might see, like on my `arrow-body-style` where I've passed in array of `[2, "as needed"]`instead of just a 0, 1 or 2, and this is where you can pass options.

Some of these options here have the ability to pass things on like `as-needed`, or `props`. In this case I said `no-unused-vars`, but I've added on there, where it lets me have them when the variable name is `res`, `next` or `^err`. And where is that helpful? For me I write a lot of Node.js where I'll do something like:
 
```js
app.get('/accounts', (req, res, next) => {
    
});
```
 I like to set up all my routes like that and I might not necessarily use `res` or `next` but I like to have them there in case I need to use them again. What I've done is I've set them to simply just give me a warning when I haven't use a variable. However, if I use res, next or anything that starts with error then don't worry about that at all.

I'm not going to sit here and go through every single rule in all of ESLint and sort of explaining what you should and shouldn't use. What I honestly think that you should do is start with the Airbnb set, and just start writing code and over the next like couple of days what's going to happen is you're going to come out, you're going to trip over errors, you're going to say, "Why is that error there?"

Go ahead and go to the ESLint docs, research it, see if it's actually a rule that makes sense for you and your team. If it is, make sure you keep it in your rules. If it doesn't, take it out of your rules or set it to warn instead of actual error. A lot of times people ask me, "Hey, Wes can I just take your ESLint?" I think that is OK, however I maybe even prefer that you just start with nothing and just learn and build your own as you start to go through this.