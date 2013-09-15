---
layout: post
title: ".NET Dev on a mac book pro"
category: Development
tags: [c#]
summary: .NET Development on a Mac Book Pro
---


I was lucky enough to get a shiny new mac book pro at my new job - one of the new 2013 mac book pro retinas. Setting it up for (mostly) .NET development has been a bit of a task, so I thought I'd share my experiences here :) The challenge:

_Setting up an optimal environment for .net development environment on a retina mac, with a "normal" external monitor attached._

The laptop is a lovely thing, and the retina display is very nice indeed. In OSX everything looks super-crisp and everything runs as fast as you'd expect. When it comes to running windows on a mac you've got 2 options. You either go down the road of virtualisation and use something like Parallels or VMWare fusion or VirtualBox on a budget, or you use bootcamp to install windows natively and boot into it (you can actually do both, and create a bootcamp partition and use it within osx as a VM - most of the virtualisation tools support this now). Both virtualisation and native have their ups and downs, the biggest issue for me was resolution issues...

## Option 1 - Native windows via Bootcamp.

Ok, so you opt for windows natively. This is nice and fast, and arguably the best way to do .net dev when you're going to be using VS.NET 2012, ReSharper etc.. You just need to sort the display out. "Retina" basically just means "huge resolution". So in windows, because you're running a huge resolution, you need to address the fact that nothing is readable because it's soo small. The easiest way to do this is to set the DPI in windows to something like 150 - 200%. This works _ok_, most apps behave with a larger DPI but there are some (including some MS apps), that don't look so good and don't obey the DPI setting. Generally though, so this is great.... untill you throw in an external monitor. What happens is the DPI setting is applied to the second monitor which is running at a normal resolution. Cue enormous text and icons, and an unusable external display :(

To fix this, well there is no fix. The best I've been able to do is to "scale" the resolution on the retina display - I set the DPI back to 100% and on the laptop set the resolution at a non-native much lower setting. This works OK - I can use the external display now AND the laptop one together, but it does look a bit blurry / rubbish on the laptop. Which is a shame considering it's a super nice retina.

<s>Theres one more problem - buggy bootcamp. For me, running windows 8, everytime that I boot into windows my bootcamp control panel loses the settings and I have to re-apply them (to turn on 2-finger right mouse click for eg). Also, there is no 3-finger selection of text support, which is slightly naff. Maybe re-installing the bootcamp drivers will help here.. </s>

_UPDATE: I fixed this by re-installing the bootcamp drivers._

Apart from the bootcamp bugs, and the slightly fuzzy retina screen, all is well. Performance is impressive, and everything works well.

## Option 2 - Windows in a VM

This is the other option. Most of the modern virtualisation tools (I'm using parallels) allow you to run your bootcamp partition as a native partition, so I gave that a whirl.

I was surprised how well paralells handles running my windows partition. I gave it 4 gig and half the video ram, and it runs quite nicely. There is a small lag noticable compared to bootcamp, I think the video card could be the main culprit there though. Building in .net is fast too. 

The external display problem is kind of fixed - depending how you want to run... I tend to run windows full-screen on the external display, and osx on the laptop display. This gives me nice retina osx AND windows running at a normal dpi on the external display, so no blurry screens. If you want windows on both screens however you still have exactly the same issues as bootcamp.

So, the bootcamp niggles are gone, and the display sorting niggles are gone, but there are new niggles to replace the old niggles ;) The niggles that bother me in paralells are not performance but just getting the keyboard and mouse setup. The keyboard shortcuts can tend to clash a bit (osx shortcuts happen when you expected windows shortcuts), causing confusion when you're in visual studio expecting to bring up a resharper menu and instead osx does something funky! You can configure all this in paralells, I've just not yet found a good mix that stays out of the way enough. Also using a mouse can be tricky.. I'm not a fan of the osx mouse movement, it's horrible compared to windows (I think its the acceleration algorithm). So you're stuck with the osx mouse movement, and on top of that windows in a VM just doesn't handle the mouse as well as native.

Still, you do get to use both windows and OSX in this setup, which can be very nice.

## *UPDATE* - Windows 8.1 Might fix all the bootcamp resolution issues

So, it looks like windows 8.1 might well [fix all the above resolution related issues](http://www.theverge.com/2013/6/26/4465442/windows-8-1-will-finally-add-retina-display-support). This is awesome news, I hope it works !




