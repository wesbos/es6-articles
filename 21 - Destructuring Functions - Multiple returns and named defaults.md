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

### Destructure a function's returned value immediately 

However with destructuring, you can sort of return multiple values from a function. You can't really return multiple values, but you can return an object and use destructuring to get your answer. Instead of using `console.log(hundo)`, I'm going to destructure immediately as it comes back, and say `USD`, `GBP`, `AUD`, and `MEX`, and `console.log` them individually:
 
```js
const { USD, GBP, AUD, MEX } = convertCurrency(100);
console.log(USD, GBP, AUD, MEX);
```
 
With that, you can use your console to return each currency individually, so we don't need that variable with all the individually properties for each currency on it. I can destructure them all into four separate values as needed.

## Pick and Choose

Another cool thing about destructuring is that the order doesn't really matter because it's just an object, so I could put the currencies in any order that I would want and it would still work just fine. What's cool is that if you only need `AUD` and `USD`, you just don't use the currencies you don't need:

```js
const { AUD, USD } = convertCurrency(100);
```

`AUD` and `USD` are returned for this function and then immediately destructured, and the other two will just be thrown out. However, you can still use `console.log` to show `AUD` and `USD`.

That's two things there: where you can **return multiple values** as well as **pick and choose the pieces that you want** returned from a function. 

