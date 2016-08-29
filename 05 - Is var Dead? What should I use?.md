We've learned about `let` and `const` and what they do, and how they're scoped. We also know when they can be reassigned and when they cannot, but there's a question: What Shoud lI actually use? 

That's a bit of a hot topic in the community right now, because some people prefer to still use `var`. Some people are saying, "Var is dead, entirely." Some people say, "Use `let`." Some people always use `const`.

I'm just going to go over two of the leading opinions here. These are both done by some very, very smart people in the JavaScript scene, so I'll let you pick your own.

[This one, by Mathias Bynens, is how I do it](https://mathiasbynens.be/notes/es6-const). He says that "`Const` is not about immutability where you can change the properties."

Later on in the article he talks about `let` vs. `const`... 
 
> * Use `const` by default 
> * Use `let` only if rebinding is needed. 
> * `var` should not be ever used in ES6.
 
Whenever you make a variable, assume it's `const`.  Only use `let` if you need to update the value of the variable. You can use `const` to keep it the same value.

[Another popular opinion here is from Kyle Simpson](http://blog.getify.com/constantly-confusing-const/), and he also writes a whole bunch of awesome JavaScript books.

> * Use `var` for top-level variables that are shared across many (especially larger) scopes. 
> * Use `let` for localized variables in smaller scopes.
> * Refactor `let` to `const` only after some code has to be written, and you're reasonably sure that you've got a case where there shouldn't be variable reassignment.
 
He says, basically, Use `var` to share larger scopes so you can put them inside of your function, and use `let` in smaller scopes. If you realize later that you need to update a value, you'd have to go back and make it a `let` instead of a `const`. If you use `let`, it's easier to go back and refactor them to `const`. 

Both of those are very valid opinions. I'll let you make your own choice on that, but through the series, I'm going to using `let` and `const` instead of `var`.