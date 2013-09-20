---
layout: post
title: "Javascript Template for a 'normal' web page"
category: Development
tags: [javascript, dev]
comments: true
---

A couple of years ago, if I were making a new page for a web app and wanted to stick some javascript on the page (nothing fancy, let's say a bunch of functions, some jquery plumbing in etc..) I would have just done something like:

{% highlight javascript %}

var thing = 1;

function doSomething() {
	// Do something exciting...
}

$(document).ready(function() {

	// Here comes all my jquery event plumbing
	$('#thing').click(....);
	...

});

{% endhighlight %}

Which is, well, OK. It's not great though, for a number of reasons, the main one being **it pollutes the global namespace**. What this means is that all those variables and functions are declared in the global scope. It means you can easily break things by writing code that changes your objects - perhaps you include another .JS file that also has a variable called `thing` in it (let's hope not eh!) - you can see the pain! 

Aside from this, it's also bad because it doesn't encourage well written code - functions are just thrown in there, there's no grouping of code into classes or modules or whatever would make sense.

Theses days I'm trying to structure my JS code in a way that avoids all these issues, and currently my default solution is to use the module pattern, creating an immediately invoked function that wraps the functions into a closure, and provides private/public scope. It looks like this:

{% highlight javascript %} 

var ThingPage = (function() {

	// Private scope..

	var thing = 1;

	function doSomething() {
		// Do something exciting...
	};

	return {
		init: function() {
			// Do page init stuff
		}

		doSomethingElse: function() {
			// Do something
		}
	};
}());

$(document).ready(function() {
	ThingPage.init();
});

{% endhighlight %}

Theres a nice write up of this pattern [here](http://www.adequatelygood.com/JavaScript-Module-Pattern-In-Depth.html).

I think this is a bit cleaner. I'm only putting `ThingPage` into the global scope, and I can decide what methods I want to expose, making it easier to work with and maintain. There's loads of rooms for improvement I'm sure - one of the biggest issue I have with this is that I can't really unit test it very easily - my JS logic is mixed up with all my jquery code.. Something like angular or backbone might help with that but it seems an extreme next step, especially if there is just a modest amount of JS code on the page.

