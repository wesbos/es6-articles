Let's take a look at another simple use case where we create a file that is going to have both a named export and a default export. We're going to create a user, where this module will allow us to create a new user, but then it will also have some utility methods to get the URL for that user's profile and generate a base64 encoded Gravatar URL.

Let's go and make a new file, and we will call that `user.js`. 

Inside of that, we have a function called `user`. That will take in a `name`, an `email`, and a `website` for the person. Then we're simply going to return an object that has their name, has their email, and has their website.

```js
function User(name, email, website) {
    return {
        name: name,
        email: email,
        website: website
    }
}
```

Hopefully by now you're yelling at me and saying, "Wes, if the property name and the variable that it's being sent to are the same thing, we do not need to do that." This is an improvement on object literals. We can just put them all on a single line and return it there:

```js
function User(name, email, website){
    return {name, email, website}
}
```

Now we have that function for the user. Then let's create a function called `createURL`. That is going to take in someone's name, and it's going to return to us something like `site.com/wes-bos`, assuming your name was Wes Bos. That is a function, and we need that `slugify` plugin that we installed earlier.

If you look at your `package.json`, you can see from earlier posts we installed `slugify` Now I'm not inside of `app.js` here, but if a module has a dependency on it own, you just `import` it.

We also need that base URL. Where's that base URL? It's in our `config.js`. 

How do you decide what's needed? Go into the file that we need and just pluck the one item that we need, so we wind up with something like this, where we create our path by calling in `url`, and then passing `name` to the `slug` function within a template string: 

```js
import slug from 'slug';
import {url} from './config';
function User(name, email, website){
    return {name, email, website}
}

function createURL(name) {
    return `${url}/users/${slug(name)}`;
}
```

You see how we have a function here that pulls in a variable from another file, and a function from an existing package? We use that right inside of there.

If we don't import these to our new package, what happens? Even though we've imported these same variables into our `app.js`, including our API key and URL, doesn't mean that it's available in every other single module. If you need something twice, you need to import it twice. What's great is that Webpack won't duplicate the code, as it's smart enough to just import it once.

Finally, let's use a Gravatar. A Gravatar is a globally recognized avatar. You can get anyone's avatar if you know their email. You just have to base64 encode it. 

How is this going to work? Well, the way that a Gravatar URL looks like in our function is something like this:
 

```js
import slug from 'slug';
import {url} from './config';
function User(name, email, website){
    return {name, email, website}
}

function createURL(name) {
    return `${url}/users/${slug(name)}`;
}

function gravatar(email) {
    const photoURL = 'https://www.gravatar.com/avatar/12345!@#$%qwertQWERTasdfgASDFGzxcvbZXCVB'
}
``` 
 
On the end we have a base64 hash of their actual user name, so we need a base64 library to do that. Luckily, that's no problem, we'll go grab one from npm.

Head into your command line and run `npm install base-64 --save`, and we're able to import it into our file, just like `slug` and `{url}`:

```js
import slug from 'slug';
import {url} from './config';
import base64 from 'base-64';
function User(name, email, website){
    return {name, email, website}
}

function createURL(name) {
    return `${url}/users/${slug(name)}`;
}

function gravatar(email) {
    const photoURL = 'https://www.gravatar.com/avatar/12345!@#$%qwertQWERTasdfgASDFGzxcvbZXCVB'
}
``` 

Now let's take a second. What's going on here? Well, the creator of `base-64` package has created it for us, so that they have exported it as a default. In our project, we can import it as anything we like, and in this case we're using base64.
 
So let's go ahead and make this a template string and pop in our actual hash. 

```js
import slug from 'slug';
import {url} from './config';
import base64 from 'base-64';
function User(name, email, website){
    return {name, email, website}
}

function createURL(name) {
    return `${url}/users/${slug(name)}`;
}

function gravatar(email) {
    const hash = base64(email);
    const photoURL = `https://www.gravatar.com/avatar/${hash}`
}
``` 

Cool, so we have two utility functions, as well as the ability to create our user name. 

Now we've made this module that does a bunch of stuff for us. If we want to be able to use it in other files, we simply stick `exports` in front of them. 

This is the main one, so I want that to be default. I'm going to say export default function user, good. Then these ones are just export and export. These ones are named exports, and this one is the default.
```js
import slug from 'slug';
import {url} from './config';
import base64 from 'base-64';
export default function User(name, email, website){
    return {name, email, website}
}

export function createURL(name) {
    return `${url}/users/${slug(name)}`;
}

export function gravatar(email) {
    const hash = base64(email);
    const photoURL = `https://www.gravatar.com/avatar/${hash}`
}
```

Now if I go back to app.js we can go ahead and start using it. 

We've got our imports and we are going to import. Now how do we do a default?

```js
import {uniq, shuffle } from 'lodash';
import insane from 'insane';
import jsonp from 'jsonp';
import { apiKey, url, sayHi } from './src/config';

import User from './src/user';
 
```
 
Good, but what if I want to do named exports as well? You could simply do this: 

```js
import {uniq, shuffle } from 'lodash';
import insane from 'insane';
import jsonp from 'jsonp';
import { apiKey, url, sayHi } from './src/config';

import User, {createURL, gravatar} from './src/user';
 
```
Now we're able to go ahead and start to use them just as we want.

Let's try it out. Let's first create a user object at the end of `app.js` now that we have everything imported:
 
```js
const wes = new User('Wes Bos', 'wesbos@gmail.com', 'wesbos.com')
console.log(wes);
```

Now, you may notice that this takes a little bit longer this to build our app in Webpack using `npm run build` , and that is because that `slug` library is fairly large because it does all of the languages. You may want to find a slimmer slug library if you don't need all of the features for it.

If we open `index.html` now, we should see an object in our console which has `email`, `name,` and `website`. Good, I'm happy with that. Now back here, let's try the `createURL` function:

```js
const wes = new User('Wes Bos', 'wesbos@gmail.com', 'wesbos.com');
const profile = createURL(wes.name);
console.log(profile);
``` 

Refresh, now we have the actual URL. That base is pulling from our `config.js` file, and this `slug` is pulling from the slug package that we used.

Let's try the last one. 

```js
const wes = new User('Wes Bos', 'wesbos@gmail.com', 'wesbos.com');
const profile = createURL(wes.name);
const image = gravatar(wes.email)
console.log(image);

```

We actually get an error here in `user.js`, and it's because the method is supposed to actually be `base64.encode`, and we need to return the actual photo so that section should look something like this once we're done editing:

```js
export function gravatar(email) {
    const hash = base64.encode(email);
    const photoURL = `https://www.gravatar.com/avatar/${hash}`
    return photoURL;
}
```
Refresh our index.html, and we should see our gravatar.

Hopefully that shows you how you can use modules to make nice and tidy code bases. Put your functions inside of their own files. Then if you ever want to use these functions in other files, you can put them on npm yourself, or you could share them amongst your coworkers, and they are much more easily testable.