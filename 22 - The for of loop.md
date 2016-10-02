We have a new type of loop in JavaScript, and that is the `for of loop` which is used to loop over any type of data that is an iterable. We're going to learn all about different types of iterables, and we're going to learn why this `for of` loop is so useful when we hit things like generators and maps and sets and so on. For now we want to get an idea as to how this loop actually works.

What is an iterable? An iterable is anything that can be looped over. If you have an array, you can loop over an array, or a string, or a map, or a set, or a generator. We're going to look at all kinds of different examples as to when you can do it, but let's get going with looking at some of the existing loops first, and see what are the shortcomings of our existing loops, and why does `for of` shine above all of them in a couple of use cases.

For the first one, we've got a survey right here, which is just a regular array with four items in it.
 
```js
const cuts = ['Chuck', 'Brisket', 'Shank', 'Short Rib'];
 ```
 
 
We're going to look at the regular for loop. You've probably have written this one over and over:
  
```js
const cuts = ['Chuck', 'Brisket', 'Shank', 'Short Rib'];

for (let i = 0; i < cuts.length; i++) {
    console.log(cuts[i]);
}
```

You've done that one many times, so the console will log `Chuck`, `Brisket`, `Shank`, and `Short Rib`. No problem there.

What is the downside to something like this? The downside is that it uses kind of a confusing syntax. In all of my years of teaching JavaScript, this is probably one that trips people up the most. 

It does make sense when you break it down into the three parts, but people look at it and get anxious when you see it because there's a whole bunch going on here. Also, it doesn't read that well. 

You don't have a variable called `cut` as a singular of `cut`, you just have `cuts[i]`, and you have a problem here where if you use a `var` you don't have a closure, and so on. It's good in some cases, but not great because of the syntax that we have here.

What other kinds of loops do we have? We have the `.forEach` array method, which we can use in an arrow function:


```js
const cuts = ['Chuck', 'Brisket', 'Shank', 'Short Rib'];

cuts.forEach((cut) => {
    console.log(cut);
})
```
 
What's the downside to that? Well, we cannot abort this loop. We cannot skip one of the cuts. For example, if I wanted to stop once we hit `brisket`, I would say something like this:
 
```js
const cuts = ['Chuck', 'Brisket', 'Shank', 'Short Rib'];

cuts.forEach((cut) => {
    console.log(cut);
    if cut (=== `Brisket`) {
        break;
    }
})
```
 
But that doesn't work because you can't use a `break` inside of a `.forEach`. Same goes for `continue`. We can't `continue` for the same reason.

I like the `.forEach` in a couple of use cases, but if you need to be able to skip over something, then that's not what we want. 

So far we've got these two loops and you have to decide what you want. We also have the `for in` loop. We're going to learn about `for of`, but we have `for in`.

```js
const cuts = ['Chuck', 'Brisket', 'Shank', 'Short Rib'];
    

for ( const cut in cuts){  
    console.log(cut);
}

```

That doesn't work because it actually gives us the index, so you could say, maybe, `index` cut. 

```js
const cuts = ['Chuck', 'Brisket', 'Shank', 'Short Rib'];
    

for ( const i in cuts){  
    console.log(cut);
}

```

No, that doesn't work. It doesn't let us do that.

There's no way for us to actually do that either, but what we can do is this:


```js
const cuts = ['Chuck', 'Brisket', 'Shank', 'Short Rib'];
    

for ( const index in cuts){  
    console.log(cuts[index]);
}

```

That might be OK, but one thing is if you have ever worked with an array, or someone has monkeyed with the prototype, what does that mean for this case?


You may know that there are many methods on the prototype, like `.forEach` and `.filter` and `.map`. However, some people believe that you can just add things to the prototype of the array. Let's take a look at this example:

```js
    Array.prototype.shuffle = function() {
        var i = this.length, j, temp;
         if (i == 0 ) return this;
         while ( --i ) {
         j = Math.floor( Math.random() * (i + 1) ); 
         temp = this[i];
         this[i] = this[j];
         this[j] = temp;
         }
        return this;         
};

const cuts = ['Chuck', 'Brisket', 'Shank', 'Short Rib'];

for ( const index in cuts){  
    console.log(cuts[index]);
}
```

Now we have this new shuffle method that was just made, because we added it to the prototype of every single array, and that will just shuffle the actual array for us. 

That's cool, but now what happens when I `console.log` the `index` of `cuts`?

We'll get everything in the array, sure, but we'll also return our `shuffle` function, because it iterates over absolutely everything in the array, including things that have been added to the prototype.


It doesn't just iterate over the items in `cuts`, because what are the `cuts`? The `cuts` are `Chuck`, `Brisket`, `Shank`, and `Short Rib`, but it also iterates over additional things that have been added, including our function.

```js
    Array.prototype.shuffle = function() {
        var i = this.length, j, temp;
         if (i == 0 ) return this;
         while ( --i ) {
         j = Math.floor( Math.random() * (i + 1) ); 
         temp = this[i];
         this[i] = this[j];
         this[j] = temp;
         }
        return this;         
};

const cuts = ['Chuck', 'Brisket', 'Shank', 'Short Rib'];

for ( const index in cuts){  
    console.log(cuts[index]);
}

cuts.shop = 'MM Meats';

```

By adding that method, we've also added a property to the array. Now, all of a sudden, any time you add a property, or a method, or anything to the array, it's also going to show up in our `console.log`. That's kind of a pain. 

You may say, "That's fine but I never do that," but there are a lot of libraries that actually modify the prototype. 

For example, MooTools is known for modifying the prototype here. 

If I am on a website; I'm not even going to use MooTools here, but I'm going to make an array with `var` names, and I want to iterate over them. I would say:

```js
var names = ['wes', 'lux']
for (name in names) { console.log(name);

}

```
 
We'll get our index. That's going to be the `name` we're looking for, actually, but then you get a lot of other stuff because the prototype has been modified, and that's what shows up when you use a `for in` loop.

Those three examble aside, we now have the `for of` loop, which gives us the best of all three worlds. You're able to use the `for of` loop for absolutely any type of data except objects. We can't use it with objects. We'll look at that in a second.


```js
    Array.prototype.shuffle = function() {
        var i = this.length, j, temp;
         if (i == 0 ) return this;
         while ( --i ) {
         j = Math.floor( Math.random() * (i + 1) ); 
         temp = this[i];
         this[i] = this[j];
         this[j] = temp;
         }
        return this;         
};

const cuts = ['Chuck', 'Brisket', 'Shank', 'Short Rib'];

for ( const index of cuts){  
    console.log(cuts);
}

```


Even though I've monkeyed with the prototype and added a weird property on to the array, we still only get actual items in it with the `for of` loop. You're able to use `break` and `continue`, too. 

If cut = brisket, I want to break. I want to stop the entire loop from going. We only stop. We get chuck, brisket, and then the rest should not show because we broke the loop. Here we go, chuck and brisket.

```js
    Array.prototype.shuffle = function() {
        var i = this.length, j, temp;
         if (i == 0 ) return this;
         while ( --i ) {
         j = Math.floor( Math.random() * (i + 1) ); 
         temp = this[i];
         this[i] = this[j];
         this[j] = temp;
         }
        return this;         
};

const cuts = ['Chuck', 'Brisket', 'Shank', 'Short Rib'];

for ( const index of cuts){
    console.log(cuts);
    if(cut === 'Brisket') {
        break;
    }
}

```

Similarly, if we wanted to just skip a whole bunch of stuff, if we wanted to skip `console.log` `Brisket`, we could just simply say `continue`. That's not going to `break` the entire loop, but it's going to skip over this one iteration, so we should see `Chuck`, `Shank`, and Short Rib:
 
 ```js
     Array.prototype.shuffle = function() {
         var i = this.length, j, temp;
          if (i == 0 ) return this;
          while ( --i ) {
          j = Math.floor( Math.random() * (i + 1) ); 
          temp = this[i];
          this[i] = this[j];
          this[j] = temp;
          }
         return this;         
 };
 
 const cuts = ['Chuck', 'Brisket', 'Shank', 'Short Rib'];
 
 for ( const index of cuts){
     console.log(cuts);
     if(cut === 'Brisket') {
         continue;
     }
 }
 
 ```

Why? Because when it was brisket, we hit `continue`, which is like having a `return` from a function. It just ignores everything underneath it, and goes on to the next iteration of our item. 

That's introduction to the `for of` loop. Let's dive in a little bit deeper and see where else we can use it.