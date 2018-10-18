## Quick Tip: Swap two variables with ES6 Destructuring

Let's look at a couple more examples of when you would actually use destructuring in the real world. Destrucuring is not just when you want to grab variables out of an object or out of an array - it has a few other uses too!

I'm building an application for a wrestling team. In wrestling, they are tag teaming. I don't really know that much about wrestling, but I know that there's one guy in the ring and one guy on the side!

Let's create two new variables. It will become obvious why I choose `let` over `const` here in a second. 

```js
let inRing = "Hulk Hogan";
let onSide = "The Rock";
```

I'm not sure if these guys ever wrestled together, but it seems feasible. They need to tag team each other. Right? 

What happens when you need to switch? The Rock is going in the ring and the Hulkster is going on the side.

How do you swap two variables before destructuring? 

If you use `inRing = onSide`, now `inRing` is The Rock, but then also `onSide` is also The Rock and we've lost the Hulkster altogether! Shit!

How do we get around that? The way we used to do it is we would make a `temp` variable. 

```js
var temp = inRing;
inRing = onSide;
onSide = temp;
```

They're totally switched now, and then you would delete your actual temp variable. That's really hard to follow. It's hard to look at, especially for someone coming back after six months of not working on this code, trying to understand what's going on.

With **JavaScript destructuring**, we can just swap them. We make use of an array here:

```js
console.log(inRing, onSide); // Hulk, Rock
[inRing, onSide] = [onSide, inRing];
console.log(inRing, onSide); // Rock, Hulk
```

What!? The above code creates an array of `[onSide, inRing]` and immediately destructures them into the opposite variables.

Now you see why I use let instead of const here, because we're actually updating the values of these variables.

Handy! Put that in your JavaScript tool belt because you'll need it sooner than later! 
