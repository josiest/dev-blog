---
layout: post
title: Simplifying and extracting modules from Unreal's Lyra project
tags: Unreal Lyra
---

I'm currently working on a small-scoped game, with the intent of adding 
a project made with Unreal to [my portfolio](https://josiest.github.io). I have
experience working with Unreal at a "Double-A" indie studio, where I became
very familiar working with tools introduced by Unreal's Lyra project.
Unfortunately, I had no hand in setting up those tools, so I'm now struggling
to import or even recreate them within the scope of my own project.

Over the past few weeks I've been in various states of researching, hacking
together code I don't quite understand, and banging my head against the wall
when it doesn't do what I want. I thought it would be a good idea to document
my process, just because it might be interesting or useful for others who might
be trying to do something similar.

# What is Lyra?

[Lyra](https://www.unrealengine.com/marketplace/en-US/product/lyra) is an
example project for Unreal Engine 5. It's a multiplayer shooter game, whose
purpose is to serve as foundational code for new game projects in Unreal.
However even if the game you're making isn't multiplayer or a shooter, Lyra
offers a plethera of incredibly useful tools such as the `CommonGame` plugin,
the data-oriented `GameplayExperience`s, as well as tons of examples of just
generally good design practices.

# Lyra is a use-as-is project. Why would you rewrite its tools?

While a few of Lyra's tools have already been abstracted into relatively
independent plugins, many of its most useful tools are still deeply ingrained
in with the rest of the project, and are tightly coupled with multiplayer and
Gameplay Ability System code. The Gameplay Ability System (GAS) seems like a
great tool for implementing player mechanics, but it takes a lot of effort to
set up within your game, and the system itself is also tightly coupled with
multiplayer code.

If you're like me, the project you're working on is an incredibly small-scoped
single player game with very simple mechanics, for which setting up GAS would
be overkill. Adding a bunch of multiplayer and GAS code that will go entirely
unused for the sole purpose of being able to use Lyra's utilities seems like a
bad idea to me (although maybe in my inexperience, I'm just being naive).

I could of course, just use Lyra as it's intended to be used and start my
project with the entire Lyra game as its base, but I have a couple reasons for
not doing this. The biggest reason is that there's still a lot about Unreal
that I'm unfamiliar with. One of the best ways I find to learn how to use a
tool is to follow existing examples, but rewrite them in my own words. In the
past, this is how I taught myself how to use SDL, and the code I wrote for that
eventually started culminating into [my own custom game
engine](https://github.com/josiest/ion).

And this is pretty much what I've been attempting to do these past few weeks
with trying to extract tools from the Lyra project. So far it's been a pretty
effective method of learning the basics of some of Unreal's most fundamental
building blocks, such as creating custom Characters and Gameplay Modes. And if
I'm successful in my endeavor, there will hopefully be some useful plugins for
small-scoped games on the Unreal marketplace for anyone to use!
