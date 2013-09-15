---
layout: post
title: "Automated web app deployment with TeamCity and MSDeploy"
category: Development
tags: [teamcity, deployment, msdeploy]
summary: Getting started with teamcity and msdeploy.
---


Deployment. Don't you just love it. Well, we don't, which is why I needed to build something to make our asp.net mvc web application deployable, from a single click ideally. This post is about that, and what I did, and how it all works. A lot of what I did here was helped by a mammouth series of blog posts by Troy Hunt, which you can find [here](http://www.troyhunt.com/2010/11/you-deploying-it-wrong-teamcity.html). He goes into a lot of detail about setting it up, so if you're looking for a step-by-step guide, I'd recommend you head over that way. This post is more a summary explanation of what we did and how it works for us.

## What we had

I'd already set up a continous build server using the fantastic TeamCity, so we had that as a starting point. I'd previously created a NANT script that did the build, ran all the tests, and this was all plumbed nicely into TeamCity and was monitoring our subversion repository and running the builds. If you don't have a build server set up, then I seriously can't recommend enough that you check out TeamCity. It's free with some limitations (number of projects), but it's awesome, very easy to install and setup, and you'll be amazed you managed without it when you get it going.

So, as well as this, I'd also set-up Web Deploy which we were currently using for web-based deployments. More about this next.

## MSDeploy / Web Deploy

MSDeploy, is Microsoft's attempts to make web-deployment easier. It's actually "Web Deploy", MSDeploy is the name of the command line tool you get to work with it. You can find out more about it from [here](http://www.iis.net/download/webdeploy), but in a nutshell it has a number of nifty powers, such as the ability to perform transforms as it deploys, detect missing dependencies, and the ability to only send files that have changed. The downside of MSDeply IMO is that it just doesn't seem to get enough coverage on the internet, and as such as quite badly documented.

### Setting up Web Deploy

The first thing I did was to grab Web Deply (v2), from [here](http://www.iis.net/download/webdeploy). I installed that locally on my dev machine (which actualy doubles as the build server... for now), and also did the same on the production web server itself. When installed on the server, we only needed to install the remote agent service - more on that [here](http://learn.iis.net/page.aspx/421/installing-web-deploy/).

Ok, so once all installed, I can then deploy the web app from Visual Studio. Before I do that, one thing I want to share is this useful nuget:

![dependencies](/{{BASE_PATH}}images/msdeploy1.jpg)

So here, you can tell the web application that when you deploy it, you're also going to need to deploy additional assemblies for, for example "ASP.NET MVC". Very useful indeed. So, now I can manually deploy using web deploy, by right-clicking on the web project, and choosing "Publish..".

![dependencies](/{{BASE_PATH}}images/msdeploy2.jpg)

I won't dwell too much on this, but I'm publishing it to MyDomain.com, to the IIS website called "MonitoringAppTest", with the user account on the server of Administrator. I should be able to use a different account here, but didn't get that to work... I posted on SO and someone posted a great reply to assist - if you're worried about using the Admin account on the web server (and you have good reason to be) then check the SO post out [here](http://stackoverflow.com/questions/4428562/msdeploy-web-deploy-failing-with-401-auth-issues)

## Plumbing it into Nant

So, we have the ability to deploy using web deploy. That's great, but what I was after was a single button deploy I could trigger from the TeamCity. This turned out to be pretty simple, once I had worked out how to get Web Deploy to work over the command line (and turn that into a nant script). My resulting nant script looks like this:

First, to "package up" the web app:

	<exec program="${MSBuildPath}">
	  <arg line='"MonitoringApp/MonitoringApp.csproj"' />
	  <arg line="/property:Configuration=${SolutionConfiguration}" />
	  <arg value="/T:Package" />
	  <arg value="/verbosity:quiet" />
	  <arg value="/nologo" />
	</exec>

 Troy hunt goes into detail about what the package contains if you're interested. Once packaged, I need to fire it to the web server:

     <exec program="MonitoringApp/obj/${SolutionConfiguration}/Package/MonitoringApp.deploy.cmd">
      <arg value="/Y" />
      <arg value="/M:https://MyDomain.com:8172/msdeploy.axd" />
      <arg value="-allowUntrusted" />
      <arg value="/U:Administrator" />
      <arg value="/P:NotTellingYou" />
      <arg value="/A:Basic" />
      <arg value="-enableRule:DoNotDeleteRule" />
    </exec>

So, this command runs the "MonitoringApp.deploy.cmd" script that actually exists when you build the package. The arguments tell it where to publish, and also I set an additional rule (DoNotDeleteRule) that tells it not to delete anything already on the destination that wasn't deployed by web deploy (e.g. user uploads).

Almost done. You see, the script doesn't know which IIS site to target (e.g. MonitoringAppTest in my screenshot above), and I have a lot of them. It turns out that the package that is generated contains a ".SetParameters.xml" file, that contains additional parameters for the deployment. I needed to set name to my IIS web site name...
    
    <xmlpoke file="MonitoringApp/obj/${SolutionConfiguration}/Package/MonitoringApp.SetParameters.xml"
         xpath="parameters/setParameter[@name = 'IIS Web Application Name']/@value"
         value="${deploy.iisWebSite}" />

Nant to the rescue! So I "poke" the IIS web application name parameter, using the property $deploy.iisWebSite (kind of like a variable in a nant script). I'll come back to where that gets set in the next section...

## Plumbing it into TeamCity

This is really simple. First, I did all the simple stuff - created a new project, set the SVN repo, told it to ttrigger a NANT scipt target, and **set the build trigger to manual** - I only want to deploy when I say so!

Once all this is done, I needed some way to tell TeamCity which IIS site to deploy to. To do this, I added a bunch of command line parameters to my nant build step - like `-D:deploy.iisWebSite=%env.iisWebSite%` That tells TC to call nant and set the property "deploy.iisWebSite" to %env.iisWebSite%. Next, I go to the "Build Parameters" section in TeamCity, and set env.iisWebSite by adding an Environment variable with a value "MonitoringAppTest". All done!

The beauty of this setup is that if I want to deploy to another site on the same box, I just need to copy the old deployment project in TC to a new one, and just change the build parameter to whatever... So simple!

## Summary

![dependencies](/{{BASE_PATH}}images/msdeploy3.jpg)

There it is. Now I can see how many changes are waiting to be deployed from TC by looking at the "pending" dropdown, and when ready I hit "Run", and off we go. I missed a few bits out from this, such as how we store the packages as artifacts in TeamCity, but I hope this shows how productive using Web Deploy, TeamCity, and a bit of NANT is to automate the otherwise dull process of deployment.

I'll also talk about how we manage SQL updates automatically in another post.
