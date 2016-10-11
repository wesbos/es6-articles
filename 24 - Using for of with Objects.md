Now there is one thing that we have not been able to iterate over, and that is the regular, old, plain object. Plain objects are not an iterable. If we look at an example, you can see what I mean:

```js
const apple = {
    color: 'Red',
    size: 'Medium',
    weight: 50,
    sugar: 10,
    };
    
    for (const prop of apple){
        console.log(prop);
    }

```

When we try to run that that, it will error out and that `apple[symbol.iterator] is not a function`. We'll talk a lot more about what symbols are in those videos. For now, what we need to know is that you cannot iterate over an object.

What are some of our options? There is going to be an object method called `.entries`. Just like we used `.entries` against our array, we're going to be able to do `.entries` against an object:

```js
for (const prop of apple.entries()){
    console.log(prop);
}
```

We don't have this quite yet, but if you take a look at the [TC39 proposal on GitHub](https://github.com/tc39/proposal-object-values-entries), we'll see that `Object.values` and `Object.entries` and will be included in ES2017, and you can already [polyfill it](https://github.com/es-shims/Object.entries). We'll talk a lot more about polyfilling, but if you would like to use that simple solution, you can go ahead and you can use `.entries`.

If you cannot use a polyfill, let's take a look at some of our other options here. 

We could use `Object.keys`:
 
```js
for (const prop of Object.keys(apple){
    console.log(prop);
}
```
`Object.keys` will take in an object, like an apple, and it will return to us an array of all of the keys. We don't have values yet, and we don't have entries yet, but we can get the keys, and we can use that to get the values:
 
```js
for (const prop of Object.keys(apple){
    const value = apple[prop];
    console.log(value, prop);
}
```

This isn't the cleanest solution, because you sort of have to reach outside to get the object here, but that will work for you if that's exactly what you need to use. You can pick your own, of course you can always use your regular, old `for in` loop. That is going to give you a very similar one:
 
```js
for (const prop in apple){
    const value = apple[prop];
    console.log(value, prop);
}
```

My recommendation is until we have `Object.entries`, let's stick with just using a `for in` for objects.