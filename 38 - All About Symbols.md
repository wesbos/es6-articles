ES6 has added a seventh primitive to JavaScript called a Symbol.

Symbols are unique identifiers that we can use to help us avoid naming collisions because symbols are always unique. So, we could create two Symbols that have the same descriptor—in our example, we'll use the descriptor `'Wes'`—and be able to distinguish the two as separate objects.

```javascript
const wes = Symbol('Wes');
const person = Symbol('Wes');
```

By passing the string, `'Wes'` as an argument to `Symbol()` we create a Symbol with the descriptor of `Wes`, effectively attaching a unique id to the String `Wes`. Let's compare the two objects.

```javascript
$ wes
=> Symbol(Wes)
$ person
=> Symbol(Wes)
```
The output of both looks the same but if we actually compare the two Symbols:

```javascript
$ wes === person
=> false
$ wes == person
=> false
```

We see that the variables are not equal. Why is that interesting? Well, imagine we hadn't created Symbols and had left them as strings.

```javascript
const wes = 'Wes';
const person = 'Wes';
```

This time, when we compare the variable `wes` and `person`, we see that they pass our truth test; they hold the same value.

```javascript
$ wes === person
=> true
$ wes == person
=> true
```

What is our takeaway here? Symbols are 100% unique!

Let's look at an example of where we might benefit from using symbols. Imagine we have a `classRoom` object that has three students:

```javascript
const classRoom = {
    'Mark': { grade: 50, gender: 'male' },
    'olivia': { grade: 80, gender: 'female' },
    'olivia': { grade: 80, gender: 'female' }
}
```

We can't control the names of our students, and it is entirely possible that some of the students in this class might share a first name. If we try to instantiate this object, it will fail due to the naming collision in our two `olivia` key/value pairs.

However, if we turn our three keys into Symbols using the new computed properties behavior in objects, we can circumvent this naming collision:

```javascript
const classRoom = {
    [Symbol('Mark')]: { grade: 50, gender: 'male' },
    [Symbol('olivia')]: { grade: 80, gender: 'female' },
    [Symbol('olivia')]: { grade: 80, gender: 'female' }
}
```

Our keys are now symbols and `olivia`, `olivia` and `Mark` are descriptors!

```javascript
$ classRoom
=> {  Symbol(Mark): Object,
         Symbol(olivia): Object,
         Symbol(olivia): Object
}
```

The gotcha with Symbols is that they're not Enumerable, which means we can not loop over them. Go ahead and try using a for in loop on our classRoom object and console.log the item. You'll notice that you get undefined.

```javascript
$ for (person in classRoom) {
   console.log(person);
}
=> undefined
```



For this reason, Symbols can be used to store private data in an object. However, our classRoom object contains student information that is clearly not private. So how do we access it dynamically?

```javascript
$ Object.getOwnPropertySymbols(classRoom);
=> [Symbol(Mark), Symbol(olivia), Symbol(olivia)]
```

What we get back is an Array of the Symbols in this object that we can then use to map over our `classRoom` object to access the values. This pattern is very similar to using `Object.keys(classRoom)`.

```javascript
const syms = Object.getOwnPropertySymbols(classRoom);
const data = syms.map(sym => classRoom[sym]);
```

Why does this work? Well, remember, each symbol is unique and is given a specific id. In asking for each symbol in our loop, we're asking for that specific id rather than just the String.

Try it out!

```javascript
$ const syms = Object.getOwnPropertySymbols(classRoom);
=> undefined
$ const data = syms.map(sym => classRoom[sym]);
=> undefined
$ data
=> [   { grade: 50, gender: 'male' },
         { grade: 80, gender: 'female' },
         { grade: 80, gender: 'female' }
]
```
