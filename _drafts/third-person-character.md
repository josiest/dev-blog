---
layout: post
title: In search of the simplest third person character
tags: Unreal
---

# Or, how I spent two weeks learning how to customize one of Unreal's most elementary types 

If you've taken a look at the Lyra starter game project, you might have noticed
one of its core features: the Gameplay Experience. The Gameplay Experience is a
data-oriented alternative to the GameMode: a data asset that provides an access
point to configure various aspects of gameplay. This includes among other
things, what pawn type the player should spawn in as, any game features that
should be enabled, as well as "Feature Actions" which are essentially
independent scriptable objects that you can add to the Experience data. It
honestly seems like a really great design, and I'm excited to be able to use
it.

![Screenshot of the Lyra Default Gameplay
Experience](/assets/third-person-character/Gameplay-Experience.png)
*The Default Gameplay Experience in Lyra*

So I tried to copy the code over into my project and, long story short, it did
not go well. I don't think there's a whole lot of value in going into the
details of what I tried because honestly it was just a foolish endeavor.
I copied a bunch of code by hand while simultaneously trying to leave out parts
I didn't think were relevant to my project. It was a lot of tedious work and I
ended up with a broken system.

In particular, the player character was spawned into the world at the right
place, but the camera was stuck at the world origin and the player had no input.
The moral of the story is that you should probably understand the fundantal
components of a system, before you try to change how the system works! So let's
dive into understanding some of the fundamental components of the
GameplayExperience system, in particular the intricacies of how the player pawn
works.

# What even is a Pawn? And what's a Controller? How are they different? What's a Character? How is that different from a Pawn?

Okay, okay, slow down there a bit with the questions. Let's tackle these one at
a time. But really, these are the questions I started out with. If you're
curious, you can read [my research notes](https://github.com/josiest/UnrealNotes/blob/main/PawnAndControllerNotes.md),
but I'll also go ahead summarize them here. My process of researching is pretty
straightforward: I start with a handful of questions I want to answer, then I
set out to answer each of them. In the process of answering one question, I
usually end up asking a dozen more (and you can see this happening in my notes).

My first questions were motivated by the goals of my research:
- What happens when a pawn spawned into the world: how is input and the camera
  normally set up?
- What differentiates the PlayerController from the Pawn and the Character?
- Wasn't there some default character/pawn type that gets spawned in if you
  haven't set up any special character classes? How is that set up?

So let's dive in to each of these questions:

### What differentiates the PlayerController from the Pawn and the Character?

I first started out trying to answer my questions by reading Lyra code, but
I eventually found that, to my surprise, the official website actually has some
really great documentation on [the responsibilities of the Pawn and the
Controller](https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-framework-in-unreal-engine#gameplayframeworkclasses), among other core classes.
To summarize:
- A pawn is a physical representation of an entity like a player or an ai.
  It's also a more transient manifestation, as a pawn may "die" or otherwise be
  destroyed while the player controller persists

- A controller is a non-physical representation of the player that persists
  even when there may be no representing pawn. It may be a good place to store
  data about the player that you want to persist even when the player dies

- A character just seems to be a specialized pawn class that already has a
  couple common components set up, such as a mesh, a collision capsule and a
  movement component

### How does the DefaultPawn setup input and camera?

# Okay, I'm getting tired of researching. Time to get my hands dirty
