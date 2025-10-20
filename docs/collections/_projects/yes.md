---
name: YES (Yellow Exploration School) 
layout: project 
tags: ["Unity", "Game Jam", "2D"]
---

Yes is a week-end-long game jam submission made with Unity.
It is a 2D horizontal-scroller game where you need to manage a school of fishes to avoid obstacles.
It was made by me, [Kumpo](https://itch.io/profile/kumpo), [Ori](Ori) and [Oroshibu](https://oroshibu.itch.io/) and is playable [__here__](https://anoukh.itch.io/yes) on Itch.

The game is about ever-growing school of fish that you can control with your mouse.
To *grow* it you must collide with "rogue" fishes to rally them to your group.
The game ends when you don't have a single controlled fish left \(because they were blowed out by mines, or by being kicked of the screen by an obstacle\).

<img alt="Screenshot of the game" src="../assets/images/projects/yes/yes.png">

## Fish Behaviour

This game was fun to make.
The most notable part I made was the __school behaviour of the fishes__, and that why I think that it could be interesting to write about it here.

We wanted to have a really __organic__ (maybe realistic) behaviour for the fishes. 
The goal was to have the sensation of controlling a __swarm__ rather than just controlling a single fish followed by many.
I belived that it was the perfect occasion for me to implement a [__flocking__](https://en.wikipedia.org/wiki/Flocking) algorithm that I was really wanting to investigate for a long time.

#### Flocking and Boids

We wanted to investigate Reynolds' Models for simulating flock of __boids__ \(bird-like entities\) that have a behaviour really close to a flock of birds or a school of fishes.

The simulation can be realised by having agents that moves following 3 simple rules:

- __Cohesion__: The agent tries to move toward the average position of its neighbors within a certain range.
- __Separation__: The agent tries to move away from the neighbors within a very closes range.
- __Alignement__: The agent tries to move in the average direction of its neighbors.

Thoses rules are simple and easy to understain their contribution but are totaly enought to make a realistic flocking simulation.

#### Our Additions

We added somes rules to adapt this flocking mecanism to our game.

- __Line of sight__: We made all agents only apply the *alignement* and *cohesion* rule for neighbors inside a foward __cone of vision__.
- __Mouse attraction__: Agents with very few neighbor in their sight will follow the mouse.
    This allows front fishes to control the school toward the mouse.

    > Note: the combination of the __line of site__ and __mouse attraction__ rule allows fishes to focus more on the front fishes of the school following the mouse, 
    rather than being distracted by late or bloqued fishes.
    It also make the school kinda elastic and satisfying.

- __Obstacle avoidance__: Agents will move away from environemental obstacles inside a close range.
- __Rogue blindness__: A rogue agents \(a fish not yet in your school\) have it *cohesion* and *separation* range highly reduced and will ignore mouse. 
    So it will roam around minding is own buisness, unless your controlled fish come very close.
    When a rogue agent joins the school, it will no longer be rogue and its viewing range return to normal.

