---
layout: post
title: "Learnings from a failed experiment: how are pawn spawning points chosen?"
tags: Unreal
---

### How is the pawn spawned into the world?

You won't find much of anything about this in my research notes, because by the
time I started researching I had already gleaned a bit about how Lyra spawns
players into the world. In particular, Lyra overrides the default spawning
mechanism becaues it cares about not spawning two players in the same place at
the same time. I don't wanna deal with multiplayer stuff at all, so this aspect
of Lyra's Gameplay Experiences isn't really something I need. But we're still
curious about how this happens so let's figure out how to figure this out.

If you pull up any starter map and inspect the objects that have been placed in
it, you'll likely notice an object called a `PlayerStart`. You might also
notice that when you play the starter level, you'll spawn in where the player
start is.

![The Player Start in the default
ThirdPersonMap](/assets/third-person-character/Player-Start-In-Level.png)

Now, I used to be a console cronie, a bash buff, a terminally terminal typist
if you will. I wrote all my code in vim and compiled it by hand. But nowadays I
use an IDE, and let me tell you, Rider's "Find Usages" command sure comes in
handy. If you _really_ want to be hardcore you could of course use grep, but
the IDE is at least _little_ bit smarter here since it does know about syntax
and semantics. It would be pretty rad if there was some command line tool that 
did this sort of usage search, but I digress.

My typical path if I want to understand how a tool is used, is just finding
where it's used in code! In this case we can just head to the `PlayerStart.h`,
find usages and start digging.

| How to find usages in Rider | Result of found usages of Player Start |
|-----------------------------|----------------------------------------|
|![Find usages of `PlayerStart`](/assets/third-person-character/Find-Usages-Player-Start.png) | ![Naive usage found result](/assets/third-person-character/Find-Usages-Player-Start-Naive.png) |

Now this can potentially be a bit intimidating at first: my search gives me
around 250 results. And it definitely doesn't help that the first result is
always the expanded one, when it's likely not what I wanna look at. But with a
little strategy and a little insider knowledge, we can quickly narrow down the
right place to look.

The first thing we can see is that most of those results are within plugins,
but we want to see the "main" way these are used within the engine. So then by
digging a little deeper in the source subdirectory we can see that it's used
among in a couple files, including the game mode base. Now, from my foray into
copying code from the LyraGameplayExperience, I know that the Game Mode seems to
be largely the class responsible for spawning. So let's start looking at the
code there.

![Found usages of PlayerStart in the Engine Runtime
folder](/assets/third-person-character/Found-Usages-PlayerStart.png)
_Found usages of APlayerStart within the Engine Runtime directory_

Inspecting the code that uses the PlayerStart this class, we can see most of it
is part of the `ChoosePlayerStart` Implementation. It essentially boils down
to:
- iterate over all the PlayerStart instances in the current world
- use some basic collision testing to find the all the currently unoccupied
  starting points
- chose a random point within the unoccupied points if any are available

This is the main logic that the Lyra Gameplay Experience changes, but I think
it works perfectly fine for what I wanna do, so I'm fine without the changes
that Lyra adds.

