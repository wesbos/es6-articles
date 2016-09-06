In JavaScript, when you want to put a variable inside of a string, it's a pain in the ass because you have to stop your string, concatenate on the variable, and then open your string again and keep going.

```js
var name = 'Snickers';
var age = 2;
var sentence = 'My dog ' + name + ' is ' + age * 7 + ' years old.';
```

Every other language has the ability to just drop your variables right inside of a string and just interpolate it like that, but not JavaScript.

What ends up happening is you forget one of your closing quotes, and you get an error and it tells you it's on line 3. You say "Of course it's on line 3, but I don't know where it is!"

We can fix all of that with what's called **template strings**, or **template literals**. In JavaScript we have double quotes and single quotes to make a string. Now we have a third way to make a string, and that is with back-ticks, which I've been using in most of my examples so far.

We're going to take this code right here, and first of all I'm going to make all those `const`, as they do not change, so there's no need to have those as a `var` or a `let`:
 
 ```js
 const name = 'Snickers';
 const age = 2;
 const sentence = 'My dog ' + name + ' is ' + age * 7 + ' years old.';
 ```

Now we have a `name` variable, we're setting that to `'Snickers'`, we have an `age` variable that's set to `2`, and we have the `sentence` variable that puts it all together.

How do we change this now?  I'm going to actually use back-ticks here:

  ```js
  const name = 'Snickers';
  const age = 2;
  const sentence = `My dog ${name} is ${age * 7} years old.'`;
  console.log(sentence);
  ```
 
That is going to pop in, or _interpolate_, the `name` and `age` variables that we previously had set there by using curly brackets and the variable name. You can also run JavaScript straight away inside of these curly brackets, too. You can add in a variable, you can run a function, you can run any JavaScript inside of the curlys — including more template strings, but we will talk more about that later. 

I'm calculating the age in dog years right inside of `sentence` by using `${age * 7}`, so our `console.log`  will just show a regular sentence, `My dog Snickers is 14 years old.` 

That's a very basic overview of template strings. However there's quite a bit more that we can do with them. It can be very handy other than just being able to stick variables inside of it — see you in the next one!
