We know that `var` is **function scope**, and now we know that `let` and `const` are block scope, which means any time you've got a set of curly brackets. Now, we need to know you can only declare a variable inside of its scope once.

```
const key = 'abc123';
let points = 50;
let winner = false;
   ```
If I try to add

`let points = 60`

It's going to say **points has already been declared**.

However, with `var`, it will just go ahead and declare the variable, which can cause a lot of bugs, because you might accidentally use the same variable twice.

 You can **update** a `let` variable, and we'll take a look more at `let` and `const` but you cannot redeclare it twice in the same scope.

Now, what if I had this?

```
const key = 'abc123';
let points = 50;
let winner = false;

if(points > 40) {
   let winner = true
}
```


If we type in the console, `winner` will come back as `false`. We can add a `Console.log` line to prove that it runs, but why is `winner` still `false`, if we set `winner` to be `true`?


The important thing here is that these two `winner` variables are actually **two separate variables**. They have the same name, but they are both **scoped** differently:

* `let winner = false` outside of the if loop is scoped to the window.
* `let winner = true` inside the if loop is scoped to the block.


If I change our `let winner` to be `var winner`, they'll come back as `true`, because it's not inside of a function, it's not scoped to it, whereas a `let` variable is.



 The other thing we need to know about it is that the difference between `let` and `const` is that `const` variables **cannot be updated**.

 `Let` variables are made to be updated. I may say:
  ```
  const key = 'abc123';
  let points = 50;
  let winner = false;

  points = 60;
  ```


  ..and that will work just fine.

 However, I've got that `key` variable, maybe something that you do not want to ever change, we can use a `const`, which stands for **constant**.

```
const key = 'abc123';
```

If I try to update it like so:
  ```
  const key = 'abc123';
  let points = 50;
  let winner = false;

  key = abc1234;
  ```

That won't work because you cannot update a `const` variable, whereas you can update a `let` variable.


We're going to go more into examples of these and you're going to understand which one to use as we go through, but that's really all we need to know about it for now.


 One other quick thing is that sometimes people think that `const` means it's **immutable**, which means that if I have an object...

  ```
  const person = {
     name: 'Wes',
     age: 28
  }
  ```
 ...and if I try to update something in the `const` object by typing `person = {name = 'Wesley'}` it won't allow me to do that.

 However, the properties of a `const` variable *can* change. That's because the entire object is not immutable. It just can't be reassigned.


The way I like to think about it with an object is that the person is me. I'm not going to ever change, my entire life, but attributes about me are going to change.

Maybe my age, my hair color, where I live. Things about me are going to change. That's fine, as long as the object that is assigned to Wes is always the exact same object, I can go ahead and set a new age:

  ```
  const person = {
     name: 'Wes',
     age: 28
  }

  person.age = 29
  ```

That will work just fine, but I cannot ever wipe out the entire variable.

If you do need to freeze everything, we have this thing called `Object.freeze`. It's actually not part of ES6, [but you can take a look at it on MDN](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Object/freeze).

 So we can use it on our object:

 `const wes = Object.freeze(person);`

If I try to update `Wes.age = 30`, it will still say `28`.