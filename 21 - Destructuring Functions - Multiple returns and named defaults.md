There's a couple of other things we can do with destructuring, and they all are related to functions. They're actually very handy. 
Here's a convert currency function that takes an amount and returns an object that has U.S. dollars, British pounds, Australian dollars, and Mexican pesos. 


```js
function convertCurrency(amount) {
    const converted = {
        USD: amount * 0.76,
        GBP: amount * 0.53,
        AUD: amount * 1.01,
        MEX: amount * 13.30
    };
    return converted;
}

const hundo = convertCurrency(100);
console.log(hundo);
```


We run it here with $100 Canadian, and it's going to return an object, then when we `console.log` that object we'll see our values for each currency. 

Previously, if I wanted to get the Australian dollars I would call for `hundo.AUD` in the `console.log` and `hundo.MEX` for pesos. 

However with destructuring, you can sort of return multiple values from a function. You can't really return multiple values, but you can return an object and use destructuring to get your answer. Instead of using `console.log(hundo)`, I'm going to destructure immediately as it comes back, and say `USD`, `GBP`, `AUD`, and `MEX`, and `console.log` them individually:
 
```js
const { USD, GBP, AUD, MEX } = convertCurrency(100);
console.log(USD, GBP, AUD, MEX);
```
 
With that, you can use your console to return each currency individually, so we don't need that variable with all the individually properties for each currency on it. I can destructure them all into four separate values as needed.

Another cool thing about destructuring is that the order doesn't really matter because it's just an object, so I could put the currencies in any order that I would want and it would still work just fine. What's cool is that if you only need `AUD` and `USD`, you just don't use the currencies you don't need:

```js
const { AUD, USD } = convertCurrency(100);
```

`AUD` and `USD` are returned for this function and then immediately destructured, and the other two will just be thrown out. However, you can still use `console.log` to show `AUD` and `USD`.

That's two things there: where you can **return multiple values** as well as **pick and choose the pieces that you want** returned from a function. 


The other one is default named arguments. We already went through that last example with the tip calculator function:

```js
function tipCalc(total, tip = 0.15, tax = 0.13) {
    
}
```

That works well, because we know that the first thing is going to be `total`, second thing is going to be `tip`... or was it `tax`? Then the third one was either `tip` or `tax`. You see, We're already getting confused as to the order. Which order should I actually put them in? I might end up tipping too much, or maybe not enough.

There's some things we can do to make this order independent. 

The first thing I'm going to do is just finish off this function and add some destructuring to it:

```js
function tipCalc({total, tip = 0.15, tax = 0.13}) {
    return total + (tip * total) + (tax * total);
}
```

You can see that we've wrapped the variables in `tipCalc` in curly brackets. Now, when we pass in an object, it's going to destructure them into our three variables, `total`, `tip`, and `tax`, so it is not like we're going to have to say `options.total`, `options.tip`, and `options.tax`. It's going to destructure them into three variables available inside of the actual function. 

Then when we call it, we simply run `tipCalc` and we pass it an object with the total value, tip, and tax:

```js
function tipCalc({total, tip = 0.15, tax = 0.13}) {
    return total + (tip * total) + (tax * total);
}

const bill = tipCalc({total: 200, tip: 0.20, tax: 0.13});
console.log(bill);
```

If we refresh, we'll see in the console that we've got $266 as our total `bill`.

But what is really important here is, first of all, I can leave things off, and we don't have to pass that undefined sort of hole-filler if it's in the middle like we used to:

```js
function tipCalc({total, tip = 0.15, tax = 0.13}) {
    return total + (tip * total) + (tax * total);
}

const bill = tipCalc({total: 200, tax: 0.13});
console.log(bill);
```

The default `tip` is going to kick in, and you can put them in any order that you please. Maybe I want to put the tip first and the total goes second:

```js
function tipCalc({total, tip = 0.15, tax = 0.13}) {
    return total + (tip * total) + (tax * total);
}

const bill = tipCalc({tip: 0.20, total: 200});
console.log(bill);
```

That still works just as we want because passing in an object here, and it's getting destructured. What's more is that if we leave the one out from the object that we pass in, the defaults are filling themselves in. 

One last thing is, if you ever are to just pass in nothing. Let's say if we put a default for the `total` be $100. This is maybe a common order and we would want to just run `tipCalc`:

```js
function tipCalc({total = 100, tip = 0.15, tax = 0.13}) {
    return total + (tip * total) + (tax * total);
}

const bill = tipCalc();
console.log(bill);
```

What happens here? We get an error: 

`TypeError: Cannot match against 'undefined' or 'null'`

This is a destructuring error, and that's because we haven't passed in anything, so there's no object to destructure, so you have to give `tipCalc` a default argument:

```js
function tipCalc({total = 100, tip = 0.15, tax = 0.13} = {}) {
    return total + (tip * total) + (tax * total);
}

const bill = tipCalc();
console.log(bill);
```


If no object is passed it's going to default just to a blank object created by our curly brackets, and all of the three of our variables, `total`, `tip`, and `tax` are going to be included.

This is a little bit of a funky syntax, so it takes some times to get comfortable with it. Maybe try it out throughout your work day, but it's definitely worth getting used to, because it's going to really help you reduce a lot of your boilerplate code where you're checking for defaults.