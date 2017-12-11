Let's make our own JavaScript modules which we can import and export into each other. We could make another JavaScript file alongside `app.js` but instead let's make a new folder called `src`. That's not anything special but it allows you to keep my modules in their own folder. We're going to start with very simple module where we `import` and `export` one function and two strings. We'll create a new file and let's call it `config.js`.

`config.js` is a place where I like to put all of my API keys and REST endpoints. Rather than scattering them through the entire application, we can put them in one single file so that if you ever have to reference any of the configuration for the application, you can just go to `config.js`.

Inside of that let's set up an API key:

```js
const apiKey = 'abc123';
```
 
Let's stop it at that and ask, "How do we get access to this API key variable when we're in another module in another file?" I'm in `app.js` in the root directory, and I want to know how to get access to our `config.js`.

Variables are not global with modules. Variables are always scoped either to their function or their block, or if they're not scoped to anything they are scoped to that module. That's the kind of benefit of having modules is you have this little files that are self-contained and they don't bleed into anything else.

There's no global variables that sit on window for you to be able to access everything. 


So how do we actually get API key from `config.js` into `app.js`? We need to first of all import it to `app.js`, and log it to the console. Because we're importing a local file and not a node module, you need to tell `import` where to find the file, and we can do that with `./src/config`:

```js
import { uniq, shuffle } from 'lodash';
import insane from 'insane';
import jsonp from 'jsonp';
import apiKey from './src/config';
console.log(apiKey);
```

Notice how we don't have the `.js` on `config`?. It's not necessary in this case. 

If we run this in our browser, we won't see `apiKey` logged to the console just yet. In order to make `apiKey` available outside of our `config.js` module, we need to export it. 

There are two types of exporting in the ES6. There is something called the default export which will allow you to export it as the default, which means that when you import it you can import it as any name that you like. Then we also have what's called named exports, which when you export it you export it as that variable name. When someone imports it on the other end they must know the name of the thing that they are importing.

Generally, a default export is the main thing that that module does. A named export is used for methods, variables, and things that you need to pluck off from that.

I'm going to show you the default export first, and then we'll change this one over to a named export. To make `apiKey` exported as default, we go our `config.js` and export it:

```js
const apiKey = 'abc123';

export default apiKey;
```

Now that we've exported it, we can refresh `app.js` in the browser and see our `apiKey` in the console.

The important part is because we exported it as a `default`, we can name it any possible thing that we want right here. We've called it apiKey because it's the same thing, but I could name it anything else, and still works exactly the same way:

```js
import { uniq, shuffle } from 'lodash';
import insane from 'insane';
import jsonp from 'jsonp';
import wesIsCool from './src/config';
console.log(wesIsCool);
```

Why is that? Because the `default` export gets renamed as whatever you import it as. So we've imported it as `wesIsCool`.

Every module that you have can only ever have one default export. However, you can have multiple named exports from a module. Let's take this `apiKey` and change it over to being a named export by removing our `default` export, and simply having:

```js
export const apiKey = 'abc123';
```

What that will do is it will export this variable, or the function, or the string, or whatever it is you'd like to export from the module and export it is as the same name. When I save the change, though, Webpack is going to get a little bit angry at me, because we have a bit of an error now.

It says, `export 'default' (imported as 'wesIsCool') was not found`. This is because we called `apiKey` as `wesIsCool` in our `app.js`.

You have to import it as whatever it was exported as, so let's switch it back over to `apiKey` in our `app.js` We have to use API key:
```js
import { uniq, shuffle } from 'lodash';
import insane from 'insane';
import jsonp from 'jsonp';
import apiKey from './src/config';
console.log(apiKey);
``` 

That's still not going to be enough, Webpack's going to give us an error here. It says, `export 'default' (imported as 'apiKey') was not found in './src/config`

Our syntax in `app.js` imports a thing from somewhere that is used for when that thing was exported as a default. 

Basically, if you want to import a named export, you need to stick some curly brackets around it:

```js
import { uniq, shuffle } from 'lodash';
import insane from 'insane';
import jsonp from 'jsonp';
import { apiKey } from './src/config';
console.log(apiKey);

const ages = [1, 1, 4, 52, 12, 4];

console.log(uniq(ages));
```

It looks kind of like destructuring, but it's not. This is just a syntax for importing things that were a named export from a file. Then if we gave that a save, Webpack should finally be happy with us, and we go back to our browser and refresh, we get our `apiKey` in the console.

Let's do it again. Go back to `config.js`, let's create a variable called `url` and send it to `wesbos.com`, and make it available to our `app.js`:

```js
// named exports
export const apiKey = 'abc123';
export const url = 'wesbos.com';
```
 
Then we have that URL variable available to be imported in `app.js`:

```js
import { uniq, shuffle } from 'lodash';
import insane from 'insane';
import jsonp from 'jsonp';
import { apiKey, url } from './src/config';
console.log(apiKey, url);
```

You see how you just pick and choose the things that you want?

At first that might seem like a little bit of a pain because you don't have access to everything, but that really helps enforced these small modules that do one thing, and do that one thing well. If you make these self-contained modules, chances are you'd be able to use them on multiple projects, and you can just trade them around and they're easily testable, and all kinds of other benefits that we have there.

Here, we've just been exporting variables but you can also export functions. Let's say I have a function called `sayHi`: 

```js
export const apiKey = 'abc123';
export const url = 'http://wesbos.com';

function sayHi(name) {
  console.log(`Hello there ${name}`);
}
```

That could very well just be an internal function that I use inside of this module. They don't all have to be exported, but if for whatever reason I wanted it available in another module. I would have to stick an `export` on it. That would be exported as `sayHi`, which we can then import, and pass in a name, `'wes'`:

```js
import { uniq, shuffle } from 'lodash';
import insane from 'insane';
import jsonp from 'jsonp';
import { apiKey, url, sayHi } from './src/config';
console.log(apiKey, url);
sayHi('wes')
```
Give that a save and a refresh in your browser, and you get your `sayHi` function working perfectly.

There's all kinds of really nice syntax for ES6 modules and I encourage you ake a look at the [Mozilla developer network docs for export](https://developer.mozilla.org/en/docs/web/javascript/reference/statements/export). 

What we've been doing is we have been exporting something like, `export variable`, `export expression`, `export function`.

You can export multiple things at once. If we had a bunch of variables
 
```js
const age = 100;
const dog = snickers;
```
 
 
I could export both of those at once, I would use:
 
```js
const age = 100;
const dog = snickers;
export {age, dog}
```
 
 
Then I will be able to import those into `app.js` just like we've been importing `apiKey`, `url`, and `sayHi`.

There's also a couple of other things where you can rename them using the `as` keyword. That's really important because imagine you had a variable called `url`, or imagine you already had a variable called `apiKey`, you need to rename it. 

What you can do is you can use `as` to change the variable name. Let's change `apikey` over to `key` using the `as` keyword and then use the new variable name in our `console.log`:

```js
import { uniq, shuffle } from 'lodash';
import insane from 'insane';
import jsonp from 'jsonp';
import { apiKey as key, url, sayHi } from './src/config';
console.log(key, url);
sayHi('wes')
```

The same goes for when you export things:

```js
const age = 100;
const dog = snickers;
export {age as old, dog}
```

Then when you import `age` it's no longer able to be imported. You have to import it as `old` not as `age`.

You have some flexibility there. If you'd like to rename them you can also put this on their own lines just for readability sake:

```js
import { uniq, shuffle } from 'lodash';
import insane from 'insane';
import jsonp from 'jsonp';
import { apiKey as key, 
url, 
sayHi, 
old,
dog } 
from './src/config';
```


That can be really handy when you're importing maybe five or six different methods from it. That's just a personal preference as to how you like to write your code.
