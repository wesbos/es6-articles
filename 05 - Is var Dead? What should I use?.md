We've learned about `let` and `const` — what they do, and how they're scoped. We also know when they can be reassigned and when they cannot, but there's a question: What Should I actually use? 

That's a bit of a hot topic in the community right now, because some people prefer to still use `var`. Some people are saying, "var is dead!" Some say, "Use `let`." while others always use `const`. 

`var` isn't dead - it still does what it always has done — it's [function scoped](http://wesbos.com/javascript-scoping/) and [you can reassign or re-bind it](http://wesbos.com/let-vs-const/). You may very well continue to choose it. There isn't a _right_ answer here, just opinions. Check them out and make your own decisions.

I'm just going to go over two of the leading opinions here. These are both done by some very, very smart people in the JavaScript scene, so I'll let you pick your own.

[This one, by Mathias Bynens, is how I do it](https://mathiasbynens.be/notes/es6-const).

Later on in the article he talks about `let` vs. `const`... 
 
> * Use `const` by default 
> * Use `let` only if rebinding is needed. 
> * `var` should not be ever used in ES6.
 
Whenever you make a variable, assume it's `const`. Only use `let` if you need to update the value of the variable. You can use `const` to keep it the same value, but be warned that const is not about immutability. It is possible to modify the properties of an object declared through const .

[Another popular opinion here is from Kyle Simpson](http://blog.getify.com/constantly-confusing-const/), who also writes a whole bunch of awesome JavaScript books.

> * Use `var` for top-level variables that are shared across many (especially larger) scopes. 
> * Use `let` for localized variables in smaller scopes.
> * Refactor `let` to `const` only after some code has to be written, and you're reasonably sure that you've got a case where there shouldn't be variable reassignment.
 
He says, basically, use `var` to share larger scopes so you can put them inside of your function, and use `let` in smaller scopes. If you realize later that you do not need to update a variable using `let`, go back and refactor it to `const`.

Both of those are very valid opinions. I'll let you make your own choice on that, but through [the ES6.io series](https://es6.io), I'll be using `const` by default, `let` whenever I need to reassign a variable, and stay away from `var` entirely.
