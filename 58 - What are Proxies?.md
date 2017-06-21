Proxies allow you to overwrite the default behavior for many of an object's default operations. What I mean by that is, with an object you can get things, you can set things, you can check if it has an item, you can delete a property.

There's a whole bunch of methods that come along with a default object. What if you wanted to override these? We've looked previously at custom setters and getters, which are actually not part of the ES6, but what if you wanted to overwrite absolutely everything, or what if you wanted to overwrite some of the other properties in an object?


That's what proxies do. They allow you to overwrite the default behavior of an operation on an object.

To explain that, let's take a look here. Let's say we have a person that is named Wes, and age is 100.

```js
const person = {name: 'Wes', age: 100};
```


When I get the name, let's say I wanted to return something other than `'Wes'`.

What we can do is create a proxy for that object:

```js
const personProxy = new Proxy();
```

Now, a proxy it takes two things. First it takes a target, which is, what object would you like to proxy? We'll give it the person:

```js
const personProxy = new Proxy(person);
```


Then the second is something called the handler, and the handler is where you specify all of the operations which you wish to rewrite. That is an object inside of our proxy:

```js
const personProxy = new Proxy(person, {
    
});
```


I'm going to bring you over to the [MDN docs for Proxy](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Proxy). When you want to overwrite one of the default operation, those things are called traps. Essentially what it does is it sits between you and the object.

If I had something like:
```js
personProxy.name = 'Wesley';
```

What that does is set the property of `name` on `personProxy`. What I want to do is trap that from happening. You sort of want to go in the middle and implement your own logic for when your want to set it.

[MDN outlines all of the methods that you can actually trap each other](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Proxy#Methods). The object contained within our new Proxy is called a handler, and everything inside of the handler is a trap. I can trap a get, a set, a delete, and apply a contract.

There's a whole bunch of them that we can go through, and you can look through the MDN documentation, but let's do the very basics of a `.get()` with what we have so far:

```js
const person = {name: 'Wes', age: 100};

const personProxy = new Proxy(person, {
   get(target, name) {
       console.log('someone is asking for', target, name);
   }
});

personProxy.name = 'Wesley';
```
Because part of the new Proxy is just a regular object with methods on it, I can use our shorthand here. When you `get` it, you get the target, and you get the name that the person is requesting, and from that, we can just do a `console.log` to make it say what we're looking for.

Let's take a look at what that does here in the browser.

If you call `personProxy`, you'll get `name: "Wesley", age: 100`, because we have `personProxy.name` setting the name to Wesley. 
 
 Then we call `.name` on it. We'll see, "someone is asking for", it gives me the entire object as well as the name or the key.

What could we do there? We could just return "Nahhhhh", and maybe some fun emojis. 

```js
const personProxy = new Proxy(person, {
   get(target, name) {
       console.log('someone is asking for', target, name);
       return 'Nahhhhh';
   }
});
```

Now when I ask for `personProxy.name`, we get "Nahhhhh" back, right? We've sort of jumped in the middle of our `get`, and hijacked it, and returned our own.

Obviously, that's not what you want to do, but you can modify, you can check if things are meeting your standards, or you can do something like this:

```js
const personProxy = new Proxy(person, {
   get(target, name) {
       console.log('someone is asking for', target, name);
       return target[name].toUpperCase();
   }
});
```
 
This will `console.log` out WESLEY, because we've included `.toUpperCase`, because we're returning a string. It always returns me upper case.

That's really helpful for a variety of reasons. We're going to look at a whole bunch of examples, but let's quickly do another example of when you might want to use another trap, let's say a `set`. Something that might happen where you are setting, and there's a whole bunch of white space in the string that you're setting.

```js
const personProxy = new Proxy(person, {
   get(target, name) {
       console.log('someone is asking for', target, name);
       return target[name].toUpperCase();
   },
   set(target, name, value) {
       if(typeof value === 'string') {
           target[name] = value.trim();
       }
   }
});
```

Now if I go to the browser and call `personProxy.cool = "        ohh yeah     "`;, with a whole bunch of white space in there, it shows us all of the white space. However, if we take a look at just `personProxy` now, you'll see that we now have `cool: ohh yeah` with the white space trimmed out. It's been trimmed as we then brought it in.

We can also store like `.toUpperCase`, and then maybe we can concatenate on some scissors because it was trimmed, just do something silly like that. 

```js
const personProxy = new Proxy(person, {
   get(target, name) {
       console.log('someone is asking for', target, name);
       return target[name].toUpperCase();
   },
   set(target, name, value) {
       if(typeof value === 'string') {
           target[name] = value.trim() + '✂️';
       }
   }
});
```

So now we can set `personProxy.wes = "I love Wes     "`, lots of space on the end there. Now if we take a look at `personProxy.Wes`, it should give us trimmed, upper cased, and tacked on with an actual scissors.

Again, with a proxy you can jump in-between and overwrite the default operations. You notice that I'm not using any special methods, I'm not using any special functions here to set those things. I'm simply just using the object as I normally would. Then the custom logic, which is our handler, and the traps, will take over.

If you do not specify one of these traps that we have here, obviously there's a whole bunch of them, the default for the object will take over.