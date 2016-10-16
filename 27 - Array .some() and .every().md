There are two other array methods, `.some()` and `.every()`. These actually aren't part of ES6, but I feel like they don't get enough love and really not enough people know about them, so I'm going to show you how to actually use them.

`.some()` and `.every()` will check the data in an array to check if some of the items meet what you're looking for or all of the items meet what you're looking for.

I've got two examples right here. These are a whole bunch of ages. 

```js
const ages = [32, 15, 19, 12];
```

So we're looking to see if there is at least one adult in the group. In Canada, you're 18 and you are considered an adult, so:

```js
const adultPresent = ages.some(age => age >= 18);
console.log(adultPresent); // true
```

What it's going to do is check every single one, and as soon as it hits one of the items that is true, then it's going to return `true `in `adultPresent`. I'm going to say `console.log(adultPresent)` and we get `true`. 

If I add youngins:
 
```js
const youngins = [1,2,2,5]
const adultPresent = youngins.some(age => age >= 18);

console.log(adultPresent); // false
```

Checking to see if there are any adults in the younguns, we obviously will get `false`.

### .every() 

Next we want to know is everybody old enough to drink in this group? In Ontario, it's 19 to drink. We want to check if every single person in the array meets what we're looking for.

```js
const allOldEnough = ages.every(age => age >= 19);
console.log(allOldEnough);
```

If everybody is greater than or equal to 19, it will return `true`, otherwise we get `false`. Which, in this case, we have `false`.

Those are very simple, but can be very, very handy!
