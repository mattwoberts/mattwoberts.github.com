---
layout: post
title: "Repository Reflections"
category: Development 
tags: [Architecture, Development]
summary: Reflecting on the use of the repository pattern.
---
{% include JB/setup %}

Se being a good .net developer, I use an ORM, like everyone else, right ;) Okay, so that's not entirely true, I've seen some pretty old fashioned dataset-based data layers still doing the rounds, and of course some people seem to have decided that an ORM is the worst thing since global variables - hell it's even an anti-pattern to [some people](http://seldo.com/weblog/2011/06/15/orm_is_an_antipattern).

Anyway, for a web application I started 2 years or so ago, I needed to create some sort of medium sized enterprise web application with an architecture that could take the current requirements and not be nightmare to expand in the future. For various reasons that made sense at the time, we decided to stick with Linq to sql, but with POCO entities, so kind of similar to EF code-first. 

## The simplest thing that could work

At the time, the repository pattern was a very popular approach - lots of sample apps were using it and most people were writing tutorials about the benefits of using it. So, to me it seemed a no brainer. To keep things simple, I decided that I would avoid a generic repository pattern, and just implement a simple repository class for each "major thing" that I could concieve of in the domain. I wanted my repository to offer me these things:

* A seperation of the data access code that I could use with my DI container to inject into my controllers. In short, unit testing was my main concern.
* To seperate the data access into something _like_ the aggregate roots of Domain Driven Design (DDD).

And conversely, I wasn't bothered at all about these things

* Some vague notion of swapping out the data layer for ORM x, or database Y. People don't do this generally.
* Some sort of querying / specification pattern. I was happy to return IQueryables from my repo, and then filter them in the controller classes or wherever directly. 

So my repository arhitecture was very simple. I'd have for example a UserRepository, which would contain various methods for dealing with users... Get, Find, Add, Delete, that kind of thing. I won't bother to post code examples, but it was all fairly straight-forwards.

## Experiences

So, I learnt a lot with this web application project...

Firstly, Linq to SQL was a bad choice, the technology was immature and never really done well when you want to come at it code-first with your own POCO obejct.. We are planning to swap in EF :)

### Testing

Repositories do/can IMO make unit testing much easier - you can mock out repository interfaces and test your controllers to your hearts content, although often the overhead in populating the mocked / fake repositories can be time-consuming.

BUT... Mocking out your repositories makes your controllers much more testable, but likely leads to you not testing the repository layer. I found quite often that all my tests would pass but when running I'd get errors. If my controller action contained some linq code to filter data fetched from a repo, my test passed because the object-LINQ is different from linq to sql, which actually evaluates to SQL... You can find out more about that in [this SO post](http://stackoverflow.com/questions/6904139/fake-dbcontext-of-entity-framework-4-1-to-test/6904479#6904479).

### Needlessly wrapping the ORM and making it useless?

Repositories risk you needlessly encapsuating your mature and powerful ORM framework to the point that you can't benefit from it. This is a big deal. Ayende talks about this in [this post](http://ayende.com/blog/3955/repository-is-the-new-singleton) (he makes some excellent observations about the pitfalls of the repository pattern and I agree with pretty much all of them). So my wrapping your ORM in a repository, you risk hiding away the power of the ORM, and then end up adding stuff to your repository just to be able to make use of it. After all, DataContext - that's a Unit of work right there, why you wanna code your own?

## If I had to it all again..

I wouldn't use a repository pattern. That's not to say I'm totally against them and think they're a waste of time, but I think carefull consideration is needed reagrding WHY you're using one, what you'll gain, and the trade-offs you'll encounter. 

For the same project, I'd strip things right back and use the ORM right there in the controller. "OMG WHAT ABOUT TESTING" I hear you scream. Well, one thing I found for this particular project was that it took a lot of effort to setup my fakes/mocks to provide the right test data so I could meaningfully test things. What I'd do instead is create a test database that was populated for the tests - ideally an in-memory database with SQLServerCe, which wouldn't be too slow. Think about it...

* No repositories to worry about
* Full power of ORM available where you need it
* The tests would test the ORM (db) logic too, and remove the need for seperate db tests
* Loads less code setting up mocks / stubs - just work off the test database.

In some situations I might find that I really don't want the database getting in the way - I could perhaps solve this by creating an interface wrapper for the DbSet / whatever, depending on the ORM. I'm not 100% sure that not using a repository would be the right move, but I think I'd end up with much simpler (and less) code, and I'd be more productive. 

If for whatever reason I decided to stick with a repository, I'd definately use a generic repository, and I'd pay closer attention to my domain, and make sure that my repositories mapped to my aggregate roots well, and wasn't just a data access object, which seems to be an easy trap to fall into.

Obviously, as with everything, it depends. It depends on the project, how big it is, the complexity, the team working on it, the user-load, and all sorts of other factors. But if anyone is interested, I'd recommend checking out Ayende's blog and looking at the back-log of repository stuff and general architecture simplification - it's very interesting and hard to argue against.

Hope this was useful to someone ;)




