Another really great use case for proxies is when you are building a library that other developers are going to use, and you need to implement some sort of ergonomics so that they are going to use it with the greatest ease possible. This is something that I run into all the time.

```js
map.longditiude = 79.3423; // wrong spelling
map.longditude = 79.3423; // full spelling
map.long = 79.3423; // wrong key
map.lon = 79.3423; // nope
map.lng = 79.3423; // correct
```

It drives me bonkers that, when you have a map and you want to set the longitude, people have a hard time deciding on if the property's going to be longitude, or they're going to spell it wrong, or you use L-O-N-G or L-O-N. Finally at the end of the day they realize "OK, it's LNG," but that might be half an hour of debugging to realize that's such a silly mistake.

Same thing goes with camel casing or something like that where you have a property like ID. People don't know if identification should be camel cased -- it shouldn't -- or if it's all uppercase -- it shouldn't -- or if it's just lower case. Something like this:

```js
const person = { name: 'Wes' };
person.ID = 123; // no
person.iD = 123; // no
person.id = 123; // correct
```
 
It really is helpful when you're making something to check.

Are people making any of these common mistakes, or is someone setting a property where there is an existing property already that is in a different case like ID? Then maybe we should actually warn them. Let's make an example with this. We're going to make a new safety object.

I'm going to start with an object, and we'll put an ID of 100 in there. Then we also need a handler, so we'll create the handler and call it `safeHandler`

```js
const safeHandler = {
    };

const safety = new Proxy({ id: 100}, safeHandler);
```

 We'll take that object and pop it into here. Now the way we're going to test this is we're going to go down here and change the ID.
 
```js
const safeHandler = {
     };
 
const safety = new Proxy({ id: 100}, safeHandler);

safety.ID = 200;
```

If someone tries to change the ID and they're using a different case than what we initially intended, then we should throw an error in actually one of them. We only need to trap the set here. The get doesn't really matter to us, and neither do any of the other traps. We just want the set value, which will take a target, a name, and a value.

```js
const safeHandler = {
    set(target, name, value){
        
    }
     };
 
const safety = new Proxy({ id: 100}, safeHandler);

safety.ID = 200;
```

Now here what we want to do is make a list of `likeKeys`, so we're going to make them all lower case. We can use `Object.keys` to get a list of all of the keys off of the current object.

Then I'm just going to find all of the like keys that are equal to the one that the person is looking for but in lowercase.

 ```js
 const safeHandler = {
     set(target, name, value){
         const likeKey = Object.keys(target).find(k => k.toLowerCase() === name.toLowerCase());
     }
      };
  
 const safety = new Proxy({ id: 100}, safeHandler);
 
 safety.ID = 200;
 ```

The reason I did `toLowerCase` on both of them is so that we have apples to apples here, and we're not trying to check against all of the other ones. If someone passes `Wes` and our key is `wes`, then we know that there's a difference, or if someone passes `wEs` and our key is `WES`, you know that there's a difference.

That just covers every single possible use case for us if we normalize them all to lower case here. 

Now we say, if there is no name in the target, like if they are trying to set a new property, which setting `ID` would be a new property because the existing property is `id`, and there is a `likeKey`.

  ```js
const safeHandler = {
    set(target, name, value){
        const likeKey = Object.keys(target).find(k => k.toLowerCase() === name.toLowerCase());
        
        if(!(name in target) && likeKey){
            throw new Error(`Oops! Looks like we already have a(n) ${name} property but with the case of ${likeKey}.`)
        }
    }
     };
 
const safety = new Proxy({ id: 100}, safeHandler);

safety.ID = 200;
```

We check for that, and if that's the case, then we just want to `throw` a new error. We'll tell them, otherwise, if that doesn't happen, then we'll just go along with our lives and set the key for them because that's what we actually wanted.

Now I should be able to refresh this in the browser, and we get an error: "Oops! Looks like we already have a(n) ID property but with the case of id". 

Let's try it again. We'll say, `safety.name = wes`, which works just fine. Then if you try to update the name to Wesley, but I use `safety.Name = `wesley`, it's going to yell at you.

It's not going to go through and update it because we already have a property called `name`. You can really help out other developers that are using this kind of code. 

By thinking ahead of these things, you can cut down on GitHub issues, emails, and all that kind of stuff if you make your proxies a little bit more resilient.