ES6 has brought classes to the language. Now, this is not a new way to do object-oriented inheritance. It's the exact same prototypal inheritance that we've always had in the browser, it's just a new way to actually write them.

What I wanted to do in this video is do a quick review of how prototypal inheritance actually works because I understand not everyone has a good grasp on that. If you are totally comfortable with prototypal inheritance, skip this post and I'll see you in the next one. If not, stick around.

What I want to do here is let's make a dog function that will create puppies or create many dogs:

```js
function Dog(name, breed) {
    this.name = name;
    this.breed = breed;
}
```
Notice we're using a capital D on `Dog` because this is what's called a constructor function. The function itself will pass in whatever we get for name and breed are assigned as `name`, and `breed`.

I should be able to say something like:

```js
const snickers = new Dog('Snickers', 'King Charles');
```

Give that a save, refresh, and if you check in your browser's console, you'll see we have `snickers`, which is a dog, and the properties on it, `breed` and `name` like you expect.

Now, what is prototypal inheritance? Prototypal inheritance is when you put a method on the original constructor, here, it will be inherited by the rest of them.

Let's just talk about what is existing. If I create a new array, there are a few things we can do:

```js
const names = ['wes', 'kait']
// We can join the names...
names.join('')
// ...and we get "weskait"

// We can use pop(), too...
names.pop()
// and we get "kait"
```
We have all these methods on top of names.

Where did they come from? Well, we have the "Mama array", which is capital A, `Array`. If you were to look inside of `Array`, you'll know that `Array` has many, many prototype methods. What that means is that when you create an array or when you create a dog from the Mama array or from the Mama dog, every single instance of that inherits those methods.

We're going to take a look at how they don't just take their own, they actually share the same one. Let's add a prototype method to our `Dog`:

```js
function Dog(name, breed) {
    this.name = name;
    this.breed = breed;
}
Dog.prototype.bark = function() {
    console.log(`Bark Bark! My name is ${this.name}`);
}
const snickers = new Dog('Snickers', 'King Charles');
```

Remember, we're using template strings to call `this.name`, because we want to reference the name of the actual dog. If you run `snickers.bark()` in your console, it says, "Bark, bark. My name is Snickers." Makes sense.

If I were to make a second dog here, and we'll call that dog Sunny:

```js
const sunny = new Dog('Sunny', 'Golden Doodle');
```
If we run `sunny.bark()`, It returns "Bark, bark. My name is Sunny." You see how this one method that it declared on the prototype, `Dog`, that method is now inherited by every other `Dog`.

What's really cool about that is you can still change it after the fact:

```js
function Dog(name, breed) {
    this.name = name;
    this.breed = breed;
}
Dog.prototype.bark = function() {
    console.log(`Bark Bark! My name is ${this.name}`);
}
const snickers = new Dog('Snickers', 'King Charles');
const sunny = new Dog('Sunny', 'Golden Doodle');

Dog.prototype.bark = function() {
console.log(`Bark bark! my name is ${this.name} and I'm a ${this.breed}!`);
}
```

Even though I've created `snickers` before I've added this method should it still do this? Absolutely. What if I added a second method onto it after the fact? 

```js
Dog.prototype.cuddle = function() {
    console.log(`I love you owner!`);
}
```

Even though, after they were created, because you modified the Dog prototype, you can run `snickers.cuddle()`. 

If you just `snickers` in your console and you open it up and you won't see `bark` or `cuddle` here. That's because these things are specific to the instance. The `name` and the `breed` are associated with `snickers` or `sunny`.

But `bark` and `cuddle` are on the prototype, which means that they're not part of the actual object, `snickers`. But if you open that it up, you'll see `bark` and `cuddle` are right inside of `__proto__` in your DevTools. 

With that in mind, let's learn about how we can also do this sort of thing when we write classes.
