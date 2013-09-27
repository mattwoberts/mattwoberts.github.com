---
layout: post
title: "Making command line bearable on windows"
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

**Note:** Looks like there are some interesting alternatives to Console2 that Scott talks about - ConEnu looks especially promising...
{:.notice}

Console2 is what gives you tabbed windows, sorts out select/copy/paste, and a generally much nicer command prompt experience. It just wraps your command line tool (like cmd.exe), which takes me on to...

## [PyCmd](http://sourceforge.net/projects/console/files/ "PyCmd") 

PyCmd is a replacement command line extension for cmd.exe. It wraps it, and adds in lots of lovely features, like the persistent command history between sessions, and proper tab-completion. It's a lovely little tool and worth playing around with. It also did that git branch highlighting you see in the screenshot.

You can tell console2 to use PyCmd as the command line tool and they both work together to give you a better experience all round.

## Git

I just install the standard windows git installer (msysgit), and when it installs and asks where I want to put the *nix tools, I just tell it to merge them with the windows tools (basically it's the option with the red warning text!). Personally, I like this option because it means I always have access to the *nix tools it installs without having to load a different shell. 

That's it! I'm planning on looking at SSH tools next.

Happy command-lining !
