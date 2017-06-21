 Remember we talked about the `for of` loop, and how it pretty much works with absolutely everything except for objects? Well, it actually works on looping through generators as well.

Let's take a quick look at an example right here:

```js
function* lyrics() {
    yield `But don't tell my heart`;
    yield `My achy breaky heart`;
    yield `I just don't think he'd understand`;
    yield `And if you tell my heart`;
    yield `My achy breaky heart`;
    yield `He might blow up and kill this man`;
}

const achy = lyrics();
```
 
This `lyrics` function is just going to yield a line of "My Achy Breaky Heart" every single time that we go through to it. To go through this, we could create a new generator

We see we have achy.next.next.next.next.next.

Over in the console, we have to call `achy.next()` over and over again for each line, and we don't want to write some sort of recursive function. 

You can use the `for of` loop:

```js
for (const line of achy) {
    console.log(line);
}
```

Notice how we're not going to call `next` anywhere in here. It just logs it all for us.

That's really useful if you've got a generator that has either some hard-coded stuff that you need to loop through, or even better yet, you have a dynamic function that would generate some code for you, or some lines, or some numbers, or some objects.

Whatever type of content it is, you can just use the `for of` loop to loop right through it, just like it works on arrays, and sets, and maps, and everything else we talked about with the `for of` loop.