Ideally you should have ESLint set up right in your editor, so as you write code you can just have it in real time telling you what your errors and what your warnings are. Now I'm going to show how to set it up in both Sublime Text and Atom, since those are probably the two most popular editors used by JavaScript developers. However, every editor worth its salt has some sort of ESLint or linting integration so you can feel free to check that out with your specific editor. 

If you don't use Atom or you don't use Sublime, feel free to skip these posts, or if you use Atom skip the Sublime section, and vice-versa.

With Sublime Text we need to use something called SublimeLinter. SublimeLinter is sort of like a framework for all linting because there's PHPLint, CSSLint, SASSLint, and there's all different kinds of linting that you can do for different languages. Rather than installing an ESLint plugin directly, we can install a linting framework, and then we can install a plugin for that linting framework for ES6.

First of all, you need to have package control installed. I assume you know how to do that, if not, you can take a [quick look on how to do it](https://packagecontrol.io/installation) or [check out my Sublime Text Power User series](https://sublimetextbook.com/)

I'm going to use the command palate (Command+Shift+P in MacOS, Control+Shift+P in Windows) and type `install package`. From that type, `SublimeLinter`. If you do not see it, that probably means you already have it installed as a package. After you've installed it, go ahead and use `install package` again and you want the SublimeLinter and you just search for ESLint. Again, I don't see it as well, but it's going to say SublimeLinter-contrib-eslint, and  you go ahead and install that one. 

If you want to know if you have them both installed you can just type list packages and you should look for `lint`, you see that I have `SublimeLinter` and `SublimeLinter-contrib-eslint`. Those two are the ones that I use to lint my JavaScript and you need to make sure that you have them installed.

The third thing you need to do, and this is very, very important, is that you have to have ESLint globally installed. These plugins don't come with ESLint. They use your system's ESLint. 

Again, if you've been doing these tutorials with me you already have it installed, but double check by checking in your terminal with `eslint -- version` and see that you have ESLint actually installed. In my experience, you have to give Sublime Text a full shutdown, so do a Sublime Text quit. Then open up Sublime Text again to get it to run. 

Then what you need to do is turn it on so that it lints as to your preference. I like to have my linting every time I hit save, it's going to lint it. Sometimes you can also just lint it on demand, or people have it linting every single time that the type a character. 

I find that when you are typing and you're not done writing your code, it will give you errors before you're done. Then on the other end, you just forget to lint, so I like to do it on save. The way that you do that is open up your command palette, and type `SublimeLinter`. 

Then, type `Choose Lint Mode`, and here we have a few options: 

- **Background**, which is "Lint whenever text is modified", that's as you type. 
- **Load/save**, "Lint only dowhen the file is loaded or saved", that's what I have it. 
- **Save Only**, "Lint only when a file is saved", I do it on load, because when you open a new file I also want to lint it.
- **Manually**, "manually lint only when requested." I could do it manually here, and then I could just write all the code not knowing there's any errors, and then I could just say "Lint this view," and it's going to lint it.

You'll start seeing that you have dots show up in the sidebar here. That means that there are errors. Then it will actually underline the actual characters that are giving me grief.

Sublime Text shows your errors in the status bar at the very bottom here. It's kind of small and it's a little bit out of the way, so you have to look back at it. What I like to do is just click on it and then read errors as they come up. You see stuff like "Use const", or "Prefer const" That's the error that I have a lot of. It will show all of your errors, like `a space is required after {`, which are the same kinds of errors we got via the command line, only now we have them without having to run ESLint from the command line, and go back and edit and run it again. This does it in every single JS file that you edit with Sublime.

That's what Sublime is. It's pretty simple to get set up. Sometimes people have trouble getting it up and running, and it's really important that you do those three things: 

1. Make sure you have ESLink globally installed, 
2. Make sure you have SublimeLinter installed first, 
3. Then install the contrib-eslint-package for SublimeLinter. 

If you get all those three lined up perfectly, you should be in really good shape to get linting working. You don't have to do it via the command line any longer.

Atom works very much the same way, except they don't use the `sublime-contrib-eslint` package, it uses its own package. 

One cool thing about Atom is that, first of all, it will give you the errors nicely down above, and because of that, it's a little bit nicer than Sublime Text. 

Also, when you click on an error, it will give you the actual name of the variable or whatever that's causing a problem, you can click on it, it will bring you to the documentation. The implementation in Atom is actually much better if you're using Atom. 

How do we get this installed? First of all you need to go to your settings and install two packages: `linter`, which is like the linting framework, and then `linter-eslint`. 

Once you have those installed, you simply go ahead and start writing your .JS code and you are up and running. No further configuration needed.