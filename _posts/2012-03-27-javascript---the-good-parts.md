---
layout: post
title: "JavaScript - the good parts"
tagline: "Improving my JavaScript understanding"
category: Development
tags: [javascript]
summary: Learning some JavaScript goodess, with Crockford's help.
---


I reckon I've been coding bits of JavaScript pushing on 14 years. In all that time I've gotten better at it, but was guilty of never really caring about it until about 4 years ago. At first it was a necessary evil (I even used vbscript circa 1999 for some sites because I thought it was easier!), then it became just something I did without thinking too much about the code I was writing. But as it grew and grew, and more impressive and elaborate libraries started appearing, I realised I should pay more attention.

### Just use functions?!

So, for the last couple of years, I've been using things like jQuery and other JavaScript libs quite a lot, and whenever I look at the source code for these libs, I'm finding all sorts of crazy stuff that actually looks quite terse and readible. I've also been looking back at my code with a more critical eye. I mean, what's wrong with

	var x,y;

	function doSomething() {
		// Blah blah blah
		x = 10;
	}

	function doSomethingElse() {
		// Blah blah...
		y = 10;
	}

Oh. Turns out that plenty is wrong with that, actually. 

Fast-forward to 2010 and I started at a new company, with the main responsibility of making a _responsive real time_ web application. Obvously if it's real-time it's going to be big with the javascript and ajax, right? We need instant updates in the web app so we need to plumb in all sorts of good stuff to make that happen. All this good stuff that needs lots and lots of javascript code.

### Time to learn me some JavaScript coding patterns

![BookCover](https://encrypted-tbn3.google.com/images?q=tbn:ANd9GcRfoJw8n3AgSjhk_RcwX7zUbwr6dmcxXaISwPuYFZ7t-i-Vmcazqg)

So, one book I'd seen referenced a lot on stackoverflow and mentioned in blogs was [JavaScript - the good parts](http://www.amazon.com/JavaScript-Good-Parts-Douglas-Crockford/dp/0596517742/ref=sr_1_1?s=books&ie=UTF8&qid=1332845139&sr=1-1), by Douglas Crockford.  

I can't recommend this book enough. It's pretty obvious that web applications are getting more complex with richer features now. Look at Gmail or Trello, these web applications rely on JavaScript to make all the good stuff happen. A good grounding in JavaScript is essential for a web developer, _especially_ when you start to make use of JavaScript libraries like BackBone.js to manage your code. Sure, you can probably get away without this grounding, and you can use things like coffeescript to help you hide from the javascript, but you're adding stuff ontop of a JavaScript foundation, and if you don't know how that foundation works, you're going to suffer. Let me talk a little more about what is wrong with the above code snippet..

### this. And that.

The syntax `function nameOfFunction() {...}` is shorthand for `var nameOfFunction = function() {...}`. The standard seems to be to use the latter, it makes it more obvious that the function is an object.

First of all, those functions are scoped to the global object. Functions are objects just like everything else in JavaScript, and they havw a scope. If you define functions like that well then you're defining then at the global scope level. That's evil. You could in-advertadly create global methods or variables that clash with other sub-components or libraries you're using, creating all sorts of bugs. 

Every function you create in JavaScript also recieves 2 additional parameters: _this_ and _arguments_. The value of _this_ depends on the way you invoke the method. Now, in this global context, _this_ is set to the global object, which isn't much use.

Going back to the example code, perhaps the easiest way to fix that code is to place it inside an _object literal_, which is very simple:

	var pageHandlers = {
		
		x: 1,
		y: 1,
		
		doSomething: function() {
			// Do something
			this.x = 2;
		},

		doSomethingElse: function() {
			// Do something
		},

	}

So now all that gloabl stuff has been moved into `pageHandlers`. You can call `PageHandlers.doSomething()` for example. This moves the functions and the variables x & y out of the global scope, and into the pageHandlers object. Since this is an object, _this_ is now scoped to the object, which is why you can safely call this.x inside that code.

One thing I wanted to know from this book was how to do the equivilent of classes in JavaScript, I had my C# head on and wanted to do the same thing in JavaScript. Oops. Got that wrong too. JavaScript is a _prototypal language_. There is no such thing as classes, it's class-free. Objects inherit from other objects, not from classes.

What I was looking for in my code, was the module pattern, which I'll quickly try to explain.

### Module Pattern

The problem with the code now is that although it's encapsulated inside an object, there is no security in place - all of my variables defined are accessible outside the object. For example you can `pageHandlers.x = 10;` and while that might be fine for some situations, for others it might be a serious flaw. You can solve that with the module pattern:

	var pageHandler = (function() {
		
		var x = 1;
		var y = 2;

		return {
			get_x: function() {
				return x;
			},

			set_x: function(val) {
				x = val;
			},

			doSomething: function() {
				// Blah...
			}
		}
	}());

So, plenty going on in this code example..

First, pageHandler is not set to a function, it's set to the __result of calling a function__ - see the brackets at the end that call this function immediately. So this returns an object literal that contains these 3 methods. Each of the 3 methods has access to x & y because functions defined inside functions have access to objects/variables defined in the outer scope. It is possible with this code to do `pageHandler.set_x(10)` but not `pageHandler.x = 1`, because x & y are just variables defined inside that function - they are not accessable outside of it.

Now, the really cool thing about this code is that the inner object (the object literal we are returning) has a longer lifetime than the function inside which it is defined. Because the inner function makes use of the outer function, everything in that outer function continues to live, this is called a _closure_. At least, this is my quite possibly somewhat simplified view of a closure. It's a great way to create modular objects with private/public methods.

### Did I mention the book's really good?

There's a lot more such as dealing with inheritence effectively in the book. I'm pretty excited by this now, and my view of javascript as a dull and extremely basic langauge that can't even do classes has been changed thanks to this book! 








