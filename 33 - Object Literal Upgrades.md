ES6 implemented a number of upgrades to the Object Literal notation.  Let's go over some of the most practical upgrades!

1. Deconstructing of variables into key and value

Let's say we have a series of variables:

```js
const first = 'snickers';
const last = 'bos';
const age = 2;
const breed = 'King Charles Cav';
```

If we wanted to create an object literal named `dog` to hold this data, we'd likely build something like this using ES5

```js
const first = 'snickers';
const last = 'bos';
const age = 2;
const breed = 'King Charles Cav';

const dog = {
    first: first,
    last: last,
    age: age,
    breed: breed
}
```

With this code, we could interact with the object literal `dog` in all the ways we would expect. In writing it though there is a fair amount of repetition creating the key and then calling the variable that contains the relevant information.

Object Literals in ES6 provide a nice method of reducing the amount of code.

```js
const first = 'snickers';
const last = 'bos';
const age = 2;
const breed = 'King Charles Cav';

const dog = {
    first,
    last,
    age,
    breed
}
```

Even though we're not explicitly writing the key/value pair syntax ES6 is actually deconstructing the constant from the value assigned to it.

When we run the code above and ask for the value of  `dog`, we'll see the object literal with the right key value pairs

```js
$ dog
=> {  first: 'snickers',
        last: 'bos',
        age: 2,
        breed: 'King Charles Cav'  }
```

The caveat to this is that it only works if your variable names also correspond to the keys that you want.

2. Deconstructing named functions into key and value

Perhaps a little curiously this deconstruction also follows for named functions:

Take this object literal with the following three functions:

```js
const modal = {
    create: function() {

    },
    open: function() {

    },
    close: function() {

    }
}
```

Using a similar pattern to our variable example, we can write these as named functions and ES6 will destructure the function reference from the name.

```js
const modal = {
    create() {

    },
    open() {

    },
    close() {

    }
}
```

This will provide us with the same behavior as in our first example:

```js
$ modal
=> {  create: [Function: create],
        open: [Function: open],
        close: [Function: close]  }
```


3. Computed property names

The third practical update to Object Literals in ES6 is the ability to dynamically create keys. Huh? What?

Imagine a situation where you want the key to be created on the fly

Consider this example:

```js
function invertColor(color) {
    return '#' + ("000000" + (0xFFFFFF ^ parseInt(color.substring(1), 16)).toString(16)).slice(-6)
}

const key = 'pocketColor';
const value = '#ffc600';

const tshirt = {
    [key]: value,
    [`${key}Opposite`]: invertColor(value)
}
```

```js
$ tshirt
=> { pocketColor: '#ffc600',       pocketColorOpposite: '#0039ff' }
```

In our tshirt object, we are generating the keys on instantiation.

Another example:

```js
const keys = ['size', 'color', 'weight']
const values = ['medium', 'red', 100]

const shirt = {
    [keys.shift()]: values.shift(),
    [keys.shift()]: values.shift(),
    [keys.shift()]: values.shift()
}
```

In this second example, when the object is instantiated each key value pair shifts through the array generating an object that has the three key value pairs hinted at in our variables.

```js
$ shirt
=> { size: 'medium', color: 'red', weight: 100 }
```

Pretty sweet!
