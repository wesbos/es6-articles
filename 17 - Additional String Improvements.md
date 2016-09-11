Strings in ES6 come with four new methods that are really handy, help us write a little bit more readable code, as well as reduce our reliance on using regular expressions, or RegExp, for certain things. 

```js
const course = 'RFB1';
```

So I have a `const` called `course`, which has the value of `RFB1`, this stands for React for Beginners. 

Sometimes I have `RFB1` which is the starter package, `RFB2` which is the master package, `RFB3` which is the team package. I don't really care too much about that in certain cases, I just want to know if `course` starts with `RFB`, and not something like `STPU`, which is Sublime Text Power User or `ES6`, which is this series.

Here, `RFB` means React for Beginners, I need to know if the string starts with it. We can use the console to check this out by typing `course.startsWith('RFB')`, it will return `true`, because obviously it does start with it. 

If I did `rfb`, in lowercase letters, it says, `false`, because there is no way to make this case insensitive. If you do need case sensitivity you do need to still use RegExp. One other thing that `.startsWith()`  will do is it'll allow you to skip a certain number of characters and start looking at a set point.

```js
const flightnumber = '20-AC2018-jz';
```

This flight number here, I want to see if it starts with `AC`. Over in the console, I'm going to type `flightNumber.startsWith('AC')`. It's false, because the variable starts with `20-`. This also happens if we have SKU numbers, and they start with a bunch of junk and then it gets to actually what we want. 

What you can do is you can use `flightnumber.startsWith('AC', 3);`, which says start after three characters. That is returning `true`, because it ignores the first three and then starts at AC and checks against that.

`EndsWith` works fairly similar. Here is an example where we have `jz` at the end of the `flightNumber`, and I want to know if it's an Air Canada Jazz flight. We can say `flightnumber.endsWith(jz)`, which will be `true`, obviously, because it ends with it. 

There's another option that we can pass `.endsWith()`, and I'm going to use an account number variable as an example here:

```js 
const accountnumber = '825242631RT0001';
```
In Canada we have business numbers that are nine digits long. They're always nine digits long, and that is your actual business number. 

Then you have a tax number, which is RT0001, or RT0002. You also might have RP0001 for your first employee, RP0002 for your second employee, and so on. We also have RC for your corporate taxes. Often you need to know, is this number a tax number or is this number a corporate tax number, or is this a payroll number? Is it **RC**, or **RP**, or **RT**?

I need to check if this number, which ends with RT, and I want to ignore this right here. What you can do is you can tell the account number to just take a certain number of characters, and ignore the rest. 

Just like with our `flightNumber`, we can use the console to put in `accountNumber.endsWith('RT')`, which will be false. What I can tell it, though, is only take the first 11 numbers, by using `accountNumber.endsWith('RT', 11);` which will be `true`.
 
 Essentially you're just going to take the first 11 numbers of `accountNumber`, ignore the rest, and then see if it ends in RT or whatever else it might be.

Then next up we have `.includes()` which will just check if that string is anywhere in it. If I wanted to see if my flight number includesthe letters AC, then I could use `flightNumber.includes('AC'), which is `true`. 

Again, it is not case sensitive so you cannot use lower case letters here. You have to know that. 

Includes checks to see if your string has something in it. As a bit of an aside, it was originally supposed to be called `.contains()`, but it got changed to includes because of some conflicts with the MooTool libraries and the way that they modified the prototype.

Next up we have make, model and colour here:

```js
 const make = 'BMW';
 const model = 'x5'
 const colour = 'Royal Blue';
``` 

I'm going to show you where that would be useful for using `.repeat()`, which allows you to take a string, and repeat it. You can just call `.repeat()` and it's going to repeat that string over and over and over again.

Where is that useful? Sometimes we have some words here. I'm going to take my `BMW` `x5` and `royal blue`, and if I wanted to display the variables in a terminal or something, but I want to right align them. How would that work? I'd have to just put a whole bunch of padding in, depending on how long this was and how much space will be used, kind of like this:

```js
          BMW
          x5
          Royal Blue
```

What we can do, instead of hitting the space bar each time, we can use a left pad function, and you can use `.repeat()`to code a nice little left padding function. 

Here we can `return` a string, and we need to then pad it with however many characters we need. We'll take a space and repeat it 10 times. 

```js
leftPad.repeat(str, length = 10){
    return `${' '.repeat(length - str.length)}${str}`;
}

console.log(leftPad(make));
console.log(leftPad(model));
console.log(leftPad(colour));
```

However, if we want it right aligned, we'll need to subtract however many characters are in the string, so we subtract `str.length` here before we return the actual string itself in `${str}`;


I can take make, model, and color, put them in here. It's going to console.log each one of them out. Console.log leftPad model. I'm going to leave out the length because we're going to default it to 20, and it should just pass in the make, model, and color.

There we go. See how all these are perfectly left aligned? BMW X5 in royal blue, whereas all of this is however much padding we actually need. That's a nice little use for repeat. 

Another little funny one that you can do is, we can take an ES6 template string, and inside of that let's do a statement where we take a string and multiply it:

```js
${'woof' * 5}
```

What happens when you take a string and multiply it by a number? You get `NaN`. Let's call repeat on that 10 times, and then tack on 'Batman!'. 

```js
`${'woof' * 5}`.repeat(10) + " Batman!";
```


Go ahead and run that in your console, and I you [should get the joke](https://www.youtube.com/watch?v=VSaDPc1Cs5U). I saw it online somewhere, but I thought it was pretty cool because what's happening here, you're giving yourself essentially a string of NaN, repeat it 10 times, and then you're tacking on Batman on to the end to make a silly joke. 

Those are some four new functions. Put them in your back pocket, and hopefully enjoy them.