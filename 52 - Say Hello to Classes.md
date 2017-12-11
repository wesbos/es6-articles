To understand classes, let's start by taking take this dog function here, which we used to create Snickers and Sunny, and turn them into an ES6 class:

```js
function Dog(name, breed) {
    this.name = name;
    this.breed = breed;
}

Dog.prototype.bark = function() {
    console.log(`Bark Bark! My name is ${this.name}`)
};

Dog.prototype.cuddle = function() {
    console.log(`I love you owner!`);
};

const snickers = new Dog('Snickers', 'King Charles');
const sunny = new Dog('Sunny', 'Golden Doodle');
```

There are two ways to make a class in ES6, class **declaration** and a class **expression**. 

For a class declaration, write the keyword `class`, and then you type the name of the class, in our case `Dog`, and then you open up a class block:

```js
class Dog {

}
```

A class expression looks more like this:

```js
const Dog = class {

}
```
It's very similar to how you can stick a function inside of a variable, or you can just go ahead and declare the actual function itself. I much prefer to do declarations. There are use cases for expressions, but we'll go ahead with declarations for now.
 
Inside of the body of your class, there are usually bunch of methods. The only method that is absolutely required when you're building a class is what's called a constructor. That is what happens when someone creates a new version: 

```js
class Dog {
    constructor () {
    
    }
}
```

Now, you notice that I didn't say `constructor: function()`. It's simply `constructor()` and then you open and close a block.

All of your methods that live inside of a class are going to look like that. It's exactly the same as our object shorthand from methods, which we were talking about earlier in the object improvement ES6.io video.

In this case here, that constructor gets called when someone makes a new `Dog`.

What is it going to take? It's going to take `name`, because we pass it the name of our dog, and it's going to take `breed`, because we pass it the breed. 

```js
class Dog {
    constructor (name, breed) {
        this.name = name;
        this.breed = breed; 
    }
    
}
```

We can just take our two parameters, and pop them in, and the same goes for our original code which sets `this.name` and `this.breed` to whatever we pass in.

Now for our prototype methods, `bark` and `cuddle`, we can basically move them into our class like this:

```js
class Dog {
    constructor (name, breed) {
        this.name = name;
        this.breed = breed;  
    }
    
    bark() {
        console.log(`Bark Bark! My name is ${this.name}`)
    }
    
    cuddle() {
        console.log(`I love you owner!`)
    }
    
}
```
 
There is a gotcha here, though. Notice how we don't put a comma after each block in the example. This might seem really weird to you, but that's just how the syntax is in this case. If you include commas after each block, it's going to break.

If your method had any parameters, you could put them in the brackets after each method, but we don't in this case.

That should create a dog, which we are allowed to call with a `new`:

```js
const snickers = new Dog('Snickers', 'King Charles');
const sunny = new Dog('Sunny', 'Golden Doodle');
```

And if we go ahead and take `snickers` for a run in the console, we get our `name` and `breed`, the same goes for `sunny`, and if we run `snickers.bark()`, or `sunny.cuddle()`, they should all work.

Those are the very basics of a class. However there are quite a few more things that we can do with them.

There's one thing called a static method. In order to understand what a static method is, think back to when we were working with `Array.from` and `Array.of`. You'll remember that we have `Array.from` which will create an array out of something like a NodeList or arguments. You know that we have `Array.of`, which only lives on top of `Array`. We can go ahead and use `Array.of` to create an array:

```js
Array.of(1,2,3,4)
```

Which will create an array of `[1, 2, 3, 4]` like you expect.

However, that `of` is not a method on every single array, remember. It only exists on `Array` directly. If we create a new array:

```js
const names = ['Wes', 'Kait']
```

You don't have `names.of`, because it's not a method. It's not inherited by all arrays, it only exists on `Array`.

If we also wanted something similar to that for our `dog` example, maybe like `info`:

```js
class Dog {
   
    ... 
    static info() {
        console.log('A dog is better than a cat by 10 times');
    }
    
}
```

Now, if I say Sunny and I say `sunny.info`, it doesn't work. Why not? Because `sunny` is an instance of dog. How do I get this info? Because it is a static method, we can only call it on dog directly. Then we get the information. That's a static method that lives inside of it. 

We can also use `get` and `set` on classes, just as you would an object. 

Let's use the description as an example:

```js
    get description() {
        return `${this.name} is a ${this.breed} type of dog`;
    }
```

We used a `get` there, or getter. It's not a method, it's a property that is computed when you pull on it. Similarly, if you wanted to also work with a  `set`, or a setter,

```js
    set nicknames(value) {
        this.nick = value.trim();
    }
```


We have to use `this.nick` because we can't use nicknames again, we're using that as a setter, which is why we use another variable called `nick`, and we included the `.trim` in case people want to include some spaces.


So now if we use the console to run `snickers.nicknames = '    snicky   '`, with a whole bunch of spaces, it'll return as `"   snicky   "` with all of our spaces.

There we go. Now we can also have a getter. Let's say `snickers.nicknames` won't give us anything. Why not? We haven't set a `get` for it, so we have to include that:

```js
    get nicknames() {
        return this.nick;
    }
```


In this case it will just return `"snicky"` because we put a setter where we trimmed it as it came in, and then a getter where we removed it.

We could also say like `this.nick.toUppercase()` with a whole bunch of spaces, and it will simply return `"Snicky"`. That's an example of a setter and getter where you have the same property value.

That is the basics of a class. Let's look at some more advanced examples of what you can do with it.
