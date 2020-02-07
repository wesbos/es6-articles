Default function arguments in JavaScript are here. This is one of those features that's going to make your life much easier, and make your code much more readable and maintainable.

Let's say we have a function called `calculateBill` which is going to take in three things: the `total` cost of your meal, how much `tax` we need to pay, and then how much you would like to `tip` your waiter:
 
```js
function calculateBill(total, tax, tip) {
    return total + (total * tax) + (total * tip);
}
```

From that, we can return the total amount of the bill. 

Then we'll make a new variable called `totalBill`, and we'll use our new function to figure out how much all of that costs. 

```js
function calculateBill(total, tax, tip) {
    return total + (total * tax) + (total * tip);
}

const totalBill = calculateBill(100, 0.13, 0.15);
console.log(totalBill); // 128
```

On a $100 meal, with 13% tax and a 15% tip, the console tells us `128`, or $128.

What happens if we want to automatically assume 13% tax rate, and we want to assume a 15% tip rate?

What we could do is just pass in our $100 meal and get our result. If we omit `tax` and `tip` — `calculateBill(100)` our function returns us `NaN`, or "not a number". That's because we're trying to do math against things that aren't passed in.

```js
function calculateBill(total, tax, tip) {
    console.log(tax);
    return total + (total * tax) + (total * tip);
}

const totalBill = calculateBill(100);
console.log(totalBill); // NaN
```

If you use `console.log` to show the value of `tax` or `tip`, you'll see that they show up as `undefined`.

What we would normally do is we would add a number of conditional checks with `if` statements:

```js
function calculateBill(total, tax, tip) {
    if(tax === undefined) {
        tax = 0.13;
    }
    if(tip === undefined) {
        tip = 0.15;
    }
    return total + (total * tax) + (total * tip);
}

const totalBill = calculateBill(100);
console.log(totalBill); // 128
```

It works fine there, but there's a whole lot of code that we've added.

You might be saying, "Yeah, you can do this, tax equals tax," or you can do this little trick here where you can say it equals itself. 

```js
function calculateBill(total, tax, tip) {
    tax = tax || 0.13;
    tip = tip || 0.15;
    return total + (total * tax) + (total * tip);
}

const totalBill = calculateBill(100);
console.log(totalBill); // 128
```

I don't mind that, but it can be a little bit hard for your team to have to read it. If you've got four or five different arguments coming in, then you've got to do all this crazy code at the top.

Another thing to note is the `||` trick will fallback to the default if you pass in `0`, `null`, `false` or any other falsy value. Not exactly what you want when you are trying to tip $0!

So let's just do without that entirely. What we can do now in ES6 is just simply set the value when we define our function, and those things will be assumed. 

```js
function calculateBill(total, tax = 0.13, tip = 0.15) {      
    return total + (total * tax) + (total * tip);
}

const totalBill = calculateBill(100);
console.log(totalBill); // 128
```

If nothing is passed in for that one, then the defaults are going to go ahead. There we're good to go.

What else we can do?

What if I wanted to pass only the total amount, as well as the tip amount, because I'm a big tipper. Let's say I'm tipping 25% here, and the tax rate isn't going to change because I'm still in Ontario. 

However, if I just leave the tax empty, we'll get a syntax error, like I would here:

```js
function calculateBill(total, tax = 0.13, tip = 0.15) {      
    return total + (total * tax) + (total * tip);
}

const totalBill = calculateBill(100, , 0.25);
console.log(totalBill); // Unexpected token ','
```

How do you leave a hole in here? 

Well, what does this essentially check for? It checks for tax being passed as `undefined`, so you can just explicitly pass `undefined`:

```js
function calculateBill(total, tax = 0.13, tip = 0.15) {      
    return total + (total * tax) + (total * tip);
}

const totalBill = calculateBill(100, undefined, 0.25);
console.log(totalBill); // 138
```


The function is going to say, "Oh, no one passed any tax." our default 13% and then the big tip is going to kick in here, and we get `138`, or $138.


I've got another example similar to this one, and when we hit destructuring, I'm going to show you how to use an object so that you could pass the arguments in any order, as well as have default arguments. But more on that later.
