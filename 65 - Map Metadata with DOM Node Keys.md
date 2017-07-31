One of the unique properties of a map over just a regular old object, is that you can actually use an object as the key in a map. What I mean by that is, let's say we had an object right here and we wanted to use a object as the key. We can't do that. We can only use a string, where we say key, or wes, or hi, that's the only thing. 

But what if we were able to use an object here, where the value can be a number, a string, or another object itself? That's where maps come in really handy, because they can be used as sort of a metadata dictionary, where it holds information about an object but it doesn't necessarily put that information on the object.

Let's say we have a bunch of buttons:
```html
<button>Snakes</button>
<button>Cry</button>
<button>Ice Cream</button>
<button>Flamin'</button>
<button>Dancer</button>
```
 
Every time you click one of those buttons you want to record how many times that specific button was clicked:

You could put the click count right on the element itself, but I don't really want to do that because it's kind of dirty, and if you remove the element then that count is gone with it. You could also create an object where you take something unique about this button and use that. Maybe you give them each an ID. 

But in this case, we can actually use the button itself because when you select it it's going to be an object. We'll use the button as the actual key.

Let's make a new map called clickCounts, and then let's select every single button on the page. 

```js
const clickCounts = new Map();
const buttons = document.querySelectorAll('button');
```


Now let's loop over every single button and add it to this map, and then set it by default zero, the actual click count. 


```js
const clickCounts = new Map();
const buttons = document.querySelectorAll('button');

buttons.forEach(button => {
    clickCounts.set(button, 0);
});
```

Notice how the key is going to just be the button itself.

Over in the browser, use the console to call up `clickCounts` and you'll see our map, and if you open it up, you'll see every single key is a button. You can open up each button, up and you see that that's just a regular DOM node element right there. The key is an actual object. That's really handy.

Now when someone clicks one of these, we'll just increment the number by one:

```js
const clickCounts = new Map();
const buttons = document.querySelectorAll('button');

buttons.forEach(button => {
    clickCounts.set(button, 0);
    button.addEventListener('click', function() {
        const val = clickCounts.get(this);
        clickCounts.set(this, val + 1);
        console.log(clickCounts);
    });
});
```
Because we used a regular function there, `this` is going to be equal to the `button`. Then, our event listener will use `clickCounts.set` to increment `val` by one. Then we use `console.log` to show the the click counts, which will show the entire map, just to see what we're dealing with now.

In the HTML, click on snakes, open the console, and you'll see the first button which is this one, has a value of one. You could use an object or a string, or anything. I'm just using a regular old number. 

If you click it a few more times, every time you click one of the buttons, the click count is going on up. That's a useful case for using a map where you want to store metadata, not necessarily on the object, but you want to store metadata about an object. A map is a good use case for that.