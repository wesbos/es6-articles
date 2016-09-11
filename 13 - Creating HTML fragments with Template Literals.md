Another feature of template literals or template strings is the ability to do multiple lines without any funny business. What I mean by that is, the funny business is, previously we had to do things like this:
 
```js
var text = "hello there,
 how are you +
  "
```
 
Any editor with a linter will freak out here. You can see I have entered onto a new line and we have to fix it by doing a forward slash and a forward slash and a semicolon there. 

```js
var text = "hello there, \
 how are you + \
  ";
```

That gets to be quite a task, to be able to do that. In ES6, we can now use template strings. I like to use these multiline strings when I'm creating HTML markup as a string. You can use backticks here, and use the object properties as variables. These look familiar because they're in the same format as I've been using in some of the previous posts. Let's define an object and I can show you what I mean:


```js
const person = {
    name: 'Wes',
    job: 'Web Developer',
    city: 'Hamilton',
    bio: 'Wes is a really cool guy that loves to teach web development!'
}


const markup = `
 <div class="person">
    <h2>
        ${person.name}
    </h2>
    <p class="location">${person.location}</p>
    <p class="bio">${person.bio}</p>
 </div>
`;
```

You can see how this is so much nicer to look at. You will get the whitespace included in the string, but since we're just creating HTML markup, it doesn't really matter.


Now, I can take this string and go ahead and dump it into an existing element in an HTML page. Just for sake of an example, you can use a blank page where the only existing element that we have on the page is the `document.body`, and then we can set our `markup` variable as the `inner.HTML`:

```js
const markup = `
 <div class="person">
    <h2>
        ${person.name}
    </h2>
    <p class="location">${person.location}</p>
    <p class="bio">${person.bio}</p>
 </div>
`;

document.body.innerHTML = markup;
```


You could also use `document.createElement`, set the `inner.HTML`, and then append that to the body, whatever you like, which is the same with markup. 

When we refresh the page you see "Wes, web developer, Hamilton, really cool guy," and so on, all on their own individual lines.
 
If you inspect that, you'll see it's all been processed as proper HTML without having to do any `document.createElement`. 

If you `console.log` the markup just to show you that the new lines are there. You can see that all of the new lines, all of the tabs and the spaces are included as part of that string.

Another amazing feature of template strings is that you can nest them inside of each other. What if I have an array of `dogs` and I want to loop over and get myself a list item for each one?
 
 ```js
const dogs = [
    { name: 'Snickers', age: 2 },
    { name: 'Hugo', age: 8 },
    { name: 'Sunny', age: 1 }
];
 ```

Let's use a template string, and create an unordered list with the class of `dogs`, and inside of that I want a list item for each one. But I can't, I can't just do something like `${dogs[0].name}` because that would be cheating and that's not really scalable. So how would I loop over every single one? 

We can nest template strings right inside of it. How do we do that? Let's take a look here:

```js
const markup = `
    <ul class="dogs">
    $dogs.map(dog => `<li>${dog.name} is ${dog.age * 7}</li>`)}
    </ul>
    `;
```

Here we are using a template string inside of a template string, so we're going to return a list item, inside of that actual list item we are going to use `${dog.name}` and we're going to say how old they are in dog years, which is `${dog.age *7}`.

Now we've got all this markup here. Let's use `console.log`, and see where we're at. You should see:

```html
<ul class="dogs">
    <li>Snickers is 14</li>,<li>Hugo is 56</li>,<li>Sunny is 7</li>
</ul>
```

But we have that comma in there, so how do you get rid of that? We know that `map` will return an array, so we can simply just use `join` and an empty string, which will join it without the commas:

```js
const markup = `
    <ul class="dogs">
    $dogs.map(dog => `<li>${dog.name} is ${dog.age * 7}</li>`).join('')}
    </ul>
    `;
```

If you want to test this, you can use `document.body.innerHTML = markup` to put it right onto the page for us. That's a good example of using `map` to loop through an array. 

Again, you could do this on their own lines if you prefer to do each on their own lines and indent it, which is just much more maintainable:

```js
const markup = `
    <ul class="dogs">
    $dogs.map(dog => 
    `<li>${dog.name}
    is 
    ${dog.age * 7}
    </li>`).join('')}
    </ul>
    `;
```
 
You don't have to worry about your white space or anything else like that.

Let's look at an example where we need an `if` statement inside of our template string. This is taken straight from how you do `if` statements inside of a React render template, and that is with a ternary operator. 

Here I've got a song, some data with a name and an artist:

```js
const song = {
    name: 'Dying to live',
    artist: 'Tupac',
    featuring: 'Biggie Smalls'
};
```

We always have a `name` of the song and the `artist` of the song, but sometimes we've got a `featuring` artist. If there is a `featuring`, we need to include it in our markup so we can add it to our `document.body.innerHTML` like this:

```js
const markup = `
    <div class="song">
        <p>
            ${song.name} - ${song.artist}
            (Featuring ${song.featuring})
        </p>
    </div>
`;

document.body.innerHTML = markup;
```

If you load that, you'll see `Dying to Live - Tupac (Featuring Biggie Smalls). 

Our HTML is working fine. It looks kind of funny here if you look at it in the inspector because we've got all of these white space, and that's because we've got all this white space in our `markup` variable. It really doesn't matter, because the white space is ignored as soon as it is converted to HTML. 

That works, but what if we delete `Biggie Smalls`, we get  `(Featuring undefined). 

If it's not there, we don't want the parentheses or the word "Featuring," or anything there. A way we can get around that is by using a **ternary operator**. Our ternary operator will say if "this" then "that", otherwise nothing:


```js
const markup = `
    <div class="song">
        <p>
            ${song.name} - ${song.artist}
            ${song.featuring ? `(Featuring ${song.featuring})` : ''}
        </p>
    </div>
`;

document.body.innerHTML = markup;
```

Using the ternary operator, we can use that to add any featured artist if there is one, otherwise it will just use a blank string, which will remove the featured artist brackets.  That's a nice little way to do an if statement right inside.



The first few examples were pretty simple, but what happens when your data starts to get a little bit complex? 

With nesting inside of nesting, inside of nesting, it starts to get a little bit hairy and harder to maintain your code. What I like to do is create what I call a **render function**. I've sort of taken that from React, where we create separate components that will handle different complex data and different components in our markup.

```js
const beer = {
    name: 'Belgian Wit',
    brewery: `Steam Whistle Brewery`,
    keywords: ['pale', 'cloudy', 'spiced', 'crisp']
};

const markup = `
<div class="beer">
    <h2>${beer.name}</h2>
    <p class="brewery">${beer.brewery}</p>
</div>
`;

document.body.innerHTML = markup;
```

Just like you'd expect, in your HTML you'll get the beer name and the beer brewery, and that's all wrapped appropriately in `<h2>` and a `<p>` tags, and it's looking pretty good. But what if I want to implement our array of keywords that's nested inside of the actual `beer` object? I could just go ahead and do `map` right on one line, but I'd rather kick it off to a separate function, which I'll call `renderKeywords`:

```js
const beer = {
    name: 'Belgian Wit',
    brewery: `Steam Whistle Brewery`,
    keywords: ['pale', 'cloudy', 'spiced', 'crisp']
};

function renderKeywords(keywords) {
    return `
    <ul>
    ${keywords.map(keyword => `<li>${keyword}</li>`)}
    </ul>
    `;
}

const markup = `
<div class="beer">
    <h2>${beer.name}</h2>
    <p class="brewery">${beer.brewery}</p>
    ${renderKeywords(beer.keywords).join('')}
</div>
`;

document.body.innerHTML = markup;
```

You can see that our function an unordered list, and then uses the map function to fill in the keywords from our array as list items. It's going to give us the keyword, and that is going to return, again, another template string that has the key word inside of it. We're also using the `join` function to make sure that our keywords don't include the commas from the array.

Now this function should just be able to pass it off. It's only one line, and it should be able to create the unordered list, and the list item, any other HTML that we need to have created inside of this. 

If you take a look at our HTML you've got your unordered list with all of the list items inside, and you can see that any time you need to render out a unordered list of keywords, whether it's tied to this particular beer or not, it can simply just use `renderKeywords` to get the markup it needs.