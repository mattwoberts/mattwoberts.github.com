---
layout: post
title: "Making command line bearable on windows - part 1"
category: Development
tags: [command]
comments: true
---

Windows has an awesome set of command line tools, doesn't it? I mean, you can, erm, umm, run batch files, and erm, that's about it really. Ok, so it sucks. it's the worst thing about windows for a developer, a stinking pile of 20-year-old-at-least badger poop.

There are a lot of things you can do however to drag it out of the abyss into something that you wouldn't be too embarrassed to show to a *nix developer. Scot Hanselman has posted a couple of things about it like [this](http://www.hanselman.com/blog/MakingABetterSomewhatPrettierButDefinitelyMoreFunctionalWindowsCommandLine.aspx "this") and [this older one](http://www.hanselman.com/blog/Console2ABetterWindowsCommandPrompt.aspx "this older one")

Anyway, since I re-installed windows (again) on my laptop, I thought I'd show off my current command prompt setup. I'll probably do a lot more to it, but for now this is working really well for me...

Introducing...

![dependencies](/{{BASE_PATH}}images/console_1.jpg)

So this gives me things like:

* Tabbed command prompts
* Copy/Paste that works like a bash shell on *nix
* Colour output
* Proper git integration, (notice the branch name in the prompt)
* Persistent command history
* Unix commands (ls, grep, sed, etc) in addition to the normal windows ones

It's pretty easy to set up, here's what I have:

## [Console2](http://sourceforge.net/projects/console/files/ "Console2")

Console2 is what gives you tabbed windows, sorts out select/copy/paste, and a generally much nicer command prompt experience. It just wraps your command line tool (like cmd.exe), which takes me on to...

## [PyCmd](http://sourceforge.net/projects/console/files/ "PyCmd") 

PyCmd is a replacement command line extension for cmd.exe. It wraps it, and adds in lots of lovely features, like the persistent command history between sessions, and proper tab-completion. It's a lovely little tool and worth playing around with. It also did that git branch highlighting you see in the screenshot.

You can tell console2 to use PyCmd as the command line tool and they both work together to give you a better experience all round.

## Git

I just install the standard windows git installer (msysgit), and when it installs and asks where I want to put the *nix tools, I just tell it to merge them with the windows tools (basically it's the option with the red warning text!). Personally, I like this option because it means I always have access to the *nix tools it installs without having to load a different shell. 

That's it! I'm planning on looking at SSH tools next.

Happy command-lining !


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

