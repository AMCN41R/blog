---
title: But I just need the salt?!
tags: [agile, mvp, minimum viable product, iterative development, software delivery, customer centered design, extreme programming]
date: 2017-05-19 14:12:28
---

I recently came across [this xkcd comic](https://xkcd.com/974/) and absolutely loved it. It made me laugh because I could see myself, and so many others, in it.

![the general problem](/blog/images/just-the-salt/the-general-problem.png)

It's funny because it's true, and it got me thinking… this is something I've seen, not only during development, but through all stages of a project. It can be trivial, but it can also be something that slows a project, or even halts it completely.

The first thing that springs to mind, that I've seen and been guilty of many times, is something like the following example...

## Supreme Leader General Base Class

*'Yo! We need to write something to validate that a product has a price. That should be pretty easy right?'* 
'Yeah, but we should make it validate other values too, in case we need that later on.' 
*'Oh yeah, and not just products either.'* 
'Of course! We'll definitely need to validate all the other things we haven't written yet.' 
*'It'll save time in the long run!'*

#### *Will it?*

Before you know it, you have some GenericValidatorRepositoryBaseFactoryResolver&lt;T> and loads of supporting classes and interfaces, when all you probably needed was one simple function. 

It's an exaggeration to make the point, and I'm not saying that abstraction is a bad thing, far from it! But I think it is really important to find a balance that keeps emphasis on the needs of the customer.

For instance, in our example, it might be that we never end up needing to validate any other type of object. We have delivered the requirement, but at what cost? Costs we might not necessarily think about. It probably took longer than the simple implementation, there will be an extra maintenance overhead due to the more complex code, and increased number of tests that now have to be kept up to date etc.

As a good friend once told me, *'It may be the nicest code you've ever seen, but if it never gets shipped it's useless!'*

Of course, this isn't an excuse to hack out *any* old code as quickly as possible. We still have responsibilities as developers to adhere to good coding practices, otherwise we end up going too far the other way, and actually getting slower in the long run.

The challenge is working out how to meet and deliver the requirement as efficiently as you can, but in a way that can easily be extended in the future - to borrow from Extreme Programming... 

> *'Do the simplest thing that could possibly work.'*

*On a side note, there is a really great video from MPJ on his channel funfunfunction about [the growth stages of a programmer](https://www.youtube.com/watch?v=2qYll837a_0&t=5s) that is relevant here, and well worth a watch!*


## Minimum Viable Product
Or, in other words...

> *Deliver value quickly!*

[Techopedia](https://www.techopedia.com/definition/27809/minimum-viable-product-mvp) explains a minimum viable product (MVP) as...

*... the most pared down version of a product that can still be released. An MVP has three key characteristics:*
* *It has enough value that people are willing to use it or buy it initially* 
* *It demonstrates enough future benefit to retain early adopters* 
* *It provides a feedback loop to guide future development* 

The idea with creating an MVP is not only to speed up the learning process, but to also put emphasis on the needs of the customer.

As I said, we don't just see this issue in development, and can apply the same thinking to requirements gathering and design. It's so easy to get carried away when designing new features, and sometimes lose sight of the actual requirement and immediate customer need.

Extending the original example...
*'We need a new feature to dispense salt...'* 
'Ok, no problem, that shouldn't take too long. But what if it did all condiments?' 
*'Yeah, great idea, but what about people with allergies?'* 
'Ooh good one! Let's make sure it can do track allergy information. Don't worry, it'll save time in the long run!'

Now your poor client, whose immediate need is just to be able to dispense salt, has to wait months instead of weeks for something they might never need - or want!


## So, what do we do about it?
This is something that can so easily crop up in many parts of a project's lifecycle, and there is not a simple *'just do this'* solution. However, I think this is a very important and apt analogy to have in our minds every time we start something new.

As a software developer, or member of a project team, your goal should be to deliver value throughout the process. Start small and build iteratively!

So, every time we sit down to write a piece of code, or design a new feature or a new set of requirements, simply ask yourself...

> *Do they just need the salt?*
