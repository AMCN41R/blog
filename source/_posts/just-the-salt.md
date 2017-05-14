---
title: But I just need the salt?!
tags: []
photos: 
date: 2017-05-14 14:12:28
---


I recently came across [this xkcd comic](https://xkcd.com/974/) and absolutely loved it. It made me laugh because I could see myself, and so many others, in it.

![the general problem](/blog/images/just-the-salt/the-general-problem.png)

It's funny because it's true, and it got me thinking… this is something I've seen, not only during development, but through all stages of a project. It can be trivial, but it can also be something that slows a project, or even halts it completely.

The first thing that springs to mind, that I've seen and been guilty of many times, is something like the following example...

## Supreme Leader General Base Class

*"We need to validate that a product has a price."* - That's pretty easy. Oh, what if we need to validate other values later on. Oh yeah, and not just products either. Hang on, we'll obviously need to validate all the other things we haven't written yet. Don't worry, it'll save time in the long run!

*Will it?*

Before you know it, you end with some IGenericValidatorRepositoryBaseFactoryResolver&lt;T> and loads of supporting classes and interfaces, when all you probably needed was one simple function. 

It's an exaggeration to make the point, and I'm not saying that abstraction is a bad thing, far from it! But I think it is really important to find a balance and to keep emphasis on the needs of the customer. 

For instance, in our example it might be that we never end up needing to validate any other type of object. We have delivered the requirement, but at what cost? Costs we might not necessarily think out. It probably took longer than the simple implementation, there is not an extra maintenance overhead due to the more complex code, and increased number of tests that now have to be kept up to date etc.

The challenge is working out how to meet and deliver the requirement as efficiently as you can, but in a way that can easily be extended in the future - to borrow from Extreme Programming... 

> *"Do the simplest thing that could possibly work."*

As a good friend once told me, *"It may be the nicest code you've ever seen, but if it's late and doesn't get shipped it's useless."*

*On a side note, there is a really great video from MPJ on his channel *funfunfunction* about [the growth stages of a programmer](https://www.youtube.com/watch?v=2qYll837a_0&t=5s) that may be considered relevant - well worth a watch!*


## MVP - Fail Fast, Fail Smart
> *"To develop working ideas efficiently, I try to fail as fast as I can."* - Richard Feynman

As I said, we don't just see this in development. We can apply the same thinking to requirements gathering and design. In a very similar way, it's so easy to get carried away when designing new features, and sometimes lose sight of the actual requirement and immediate customer need.

*"We need a new feature to dispense paracetamol..."* Ok, no problem, that shouldn't take too long. But what if it did all painkillers? Yeah, great idea, but what about people with prescriptions for other drugs? Ooh good one! Let's make sure it can do that too.

Now your poor client, whose immediate need is just to be able to dispense paracetamol, has to wait months instead of weeks for something they might never need - or want!


## So, what do we do about it?
This is something that can so easily crop up in many parts of a project's lifecycle, and I think this is a very important and apt analogy to have in our minds when we start something new. I don't think there is a real solution other than every time we sit down to write a piece of code, or design a new feature or a new set of requirements, simply ask yourself, do they just need the salt?
