Let's do another example of proxy with phone numbers. Phone numbers are a tricky thing because sometimes people put dashes in them, sometimes people put parenthesis or spaces or neither or pluses. There's all kinds of different characters that could go into a phone number when sometimes you might just want to store it as a raw actual number.

Now, I'm going to assume that these are US numbers. Obviously this would be a little bit more involved if it's an international numbers, but we'll keep it simple here. I'm going to first create what's called a phone handler, and that's going to do the trapping of the `set` and `get` methods.

Let's first start with set, and the set will take in the target, the name, and the value, and create a phone number proxy. 

```js
const phoneHandler = {
    set(target, name, value){
      target[name] = value; 
    }
};

const phoneNumbers = new Proxy({}, phoneHandler);
```


We're going to start off with an empty object because there's no phone numbers in it to start, but we'll plan to pass it to phone handler.

Let's add a couple numbers using our browser, we can assign `phoneNumbers.home = +234 -234-2343` and `phoneNumbers.work = (234) 234 2343`

What we've done here is we've set the home phone number. We've got a plus on one, a space that someone accidentally put in. We've got a dash here, sometimes there's going to be parenthesis. 

`phoneNumber.work` only has spaces in it in that case, and parentheses. Now we have `phoneNumbers`, which is an object, with some not nicely formatted data inside of it. 

What we can then do is, when someone sets it, we can clean it of all these characters that are non-numbers using a `.match` method and RegExp, and `.join` the numbers together once we have it.

```js
const phoneHandler = {
    set(target, name, value){
      target[name] = value.match(/[0-9]/g).join(''); 
    }
};

const phoneNumbers = new Proxy({}, phoneHandler);
```

After we refresh, we should be able to assign our work number for `phoneNumbers.work` in the browser console with the spaces and parentheses, then pull up `phoneNumbers.work`, which is just an actual number. Same goes for the `phoneNumbers.home`. When I pull up `phoneNumber.home` in the browser console, it will now be stripped of all of your dashes and spaces and any other characters that might have then come on in with it.

We could go one further and actually store it as number if we wanted to, but let's do it on the flip side of when I actually pull it out of this object, we want it to be consistently formatted. That will be our `get`, and from this we're going to return the `target[name]`. That's the default operation.  What we can do is use a `.replace` RegExp here to format it in a standard phone number fashion.

```js
const phoneHandler = {
    set(target, name, value){
      target[name] = value.match(/[0-9]/g).join(''); 
    },
    get(target, name){
            return target[name].replace(/(\d{3})(\d{3})(\d{4})/, '($1)-$2-$3');
    }
        
};

const phoneNumbers = new Proxy({}, phoneHandler);
```
 
I just pulled the RegExp off the Internet. 

Now when you pull the work out, we intercept it with this `get` trap and then we format it properly as it comes out. 

No matter how you pass in the values, it's always going to pull it out and format it very nicely for me. That's another good example where you might want to use a handler. Again, we're using `set` and `get` to step in between on the proxy. 

One other thing we did here is we didn't start with an existing object in our `phoneNumbers` variable, we just passed it a blank object and then use that to set values on.