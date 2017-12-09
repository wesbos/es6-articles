ES6 classes can also be extended, and to show you that, let's make an animal class:

```js
class Animal {
    constructor(name) {
        this.name = name;
        this.thirst = 100;
        this.belly = [];
    }
    drink() {
        this.thirst -= 10;
        return this.thirst;
    }
    eat(food) {
        this.belly.push(food);
        return this.belly;
    }
 }
```


Here is an animal class, where it's just any animal drinks and eats. Any animal has thirst and has a belly that things can go into. We can make an animal off of that class, like a rhino.

```js
const rhino = new Animal('Rhiney');
```

We'll look at the rhino over in our console to make sure that Rhiney is there, and let's let them eat a burger, because that's what rhinos eat.

```js
rhino.eat('burger')
```

A rhino can also eat some leaves, and it can also eat a zebra.

```js
rhino.eat('leaves')
rhino.eat('zebra')
```

If we double check `rhino.drink `,  Rhiney drinks, the thirst goes down.

That is an example of a base class that we have here. It really works on its own. However, sometimes you want to get a little more specific, like what if we had a dog? Dogs also bark. How do I add that? Do we do if this is the dog, then not?

What we'll do is we'll extend this class of animal and create a more specific class called `Dog`, and we're going to name our dog `'Snickers'` and pass in a breed of `'King Charles'` which you might think goes like this:

```js
class Dog extends Animal {
    constructor (name, breed) {
        this.name = name;
        this.breed = breed;
    }
}
```

However, if we try to run that, it'll yell at us saying something like `this is not defined`. Why is `this` not defined? It should be okay because it's inside a method here.

The problem here is that when you create a `Dog` it is, in turn, extending an `Animal`, so we need to create an `Animal` before we can create a `Dog`.

How do we do that?

We call what's called `super()`. When I first was learning how to do this stuff, it was really confusing to me. I was like, "What does super mean? I don't really understand that." In this case `super` means just call the thing that you are extending, first. We need to create an animal before we can layer on our additional stuff, and to do that, we do something like this:

```js
class Dog extends Animal {
    constructor (name, breed) {
        super(name);
        this.breed = breed;
    }
}
```


You have to pay attention to what our inital class needs, though, because we need to pass that to `super`. You can think of it like calling `Animal` but the keyword super will call whatever you are extending, and it needs a `name`, which is why we passed it in and removed `this.name`, because that's taken care of with `super`. Our `Dog` will also have thirst and belly and drink and eat, but we do need to set `this.breed`. So if we run this now, you'll see that you can call up `Snickers`, and see that Snickers has a belly, breed, thirst, and if you inspect it closer, you can see that it was created off of our `Animal` prototype.

Now, we can also add our own methods on top of that, like `bark`:

```js
class Dog extends Animal {
    constructor (name, breed) {
        super(name);
        this.breed = breed;
    }
    bark() {
        console.log('Bark bark I\'m a dog');
    }
}
```

And we'll see that in the console we can call `Snickers.bark` What does he like to eat? We can use `Snickers.eat('burgers');`, and that will work too.

All of the methods that we defined on the initial `Animal` class have been inherited because we extended it.

The same things go if you were to change any of these prototypes on `Animal`, they would filter on down to `Dog`, and filter on down to Snickers where we actually created it.

That is extending classes based off of another. Generally, the rule of thumb I've heard is don't go deeper than two or three, because you can imagine how this would start to get hellish as you extend and extend and extend away.

One other thing you can do is you can actually extend native things, like the array. We'll take a look at that next video.
