---
name: Onü
layout: project 
tags: ["C", "Game", "2D", "OpenGL", "Custom Engine"]
is_article: true 
thumbnail: ["../assets/images/projects/onu/editor.png", "../assets/images/projects/onu/game.png"]
order: 0
---

Onü is a sokoban-inspired 2D game made in __pure C__, using __OpenGL__. 
It uses __GLFW__ for creating window and managing inputs and __Glad__ for communicating with OpenGL.
It also includes a custom level editor using __cimgui__ \(a C binding of the C++ __Dear ImGui__ librairy\).
So when building the game for editing level, It uses the C++ compiler.

The source code is available [here](https://github.com/WAnoukh/ONU).

  <img alt="Screenshot of the WIP game" src="../assets/images/projects/onu/game.png">
&nbsp; 
  <img alt="Screenshot of the level editor" src="../assets/images/projects/onu/editor.png">

## Origins

It is based on "Oui Non... Unless"\( playable [here](https://kyo-cz.itch.io/ouinon-unless) \), a game I programed and designer with [Kyo](https://kyo-cz.itch.io/) and [Ori](https://oribellame.itch.io/).
The original game was made in Unity for a 1-week game jam, organised by the univeristy's associtation: Arcadia.

It differs from classic sokoban-like games by having __in-game representations__ of your __keyboard keys__. 
Those *key blocks* can be pushed by your character, and when they are placed on top of an *action slots*, they whill be bound to it.
So if a `V key block` is placed on an `Open final door slot`, pressing the *V* key on your keyboard will open the door.

This concept is really pushed far by only using the key/slot system for every key bindings in the game :
- Player movement,
- Undo button,
- Main menu navigation...

<style>
  .responsive-images {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    gap: 1%; /* nice small space on desktop */
  }

  .responsive-images img {
    width: 49%;
    height: auto;
  }

  @media (max-width: 600px) {
    .responsive-images {
        flex-direction: column;  /* Stack vertically */
        align-items: center;     /* Center each image */
        gap: 10px;               /* Vertical space between them */
    }
    .responsive-images img {
      width: 100%;
    }
  }
</style>

<div class="responsive-images">
  <img alt="Second level of the base game" src="../assets/images/projects/onu/onu-base-1.png">
  <img alt="Fifth level of the base game" src="../assets/images/projects/onu/onu-base-2.png">
</div>

## A remake ?

My first reason leading to a remake of "Oui Non Unless..." was that I really liked the concept of the game, and I thought that there was still plenty of room to explore the key/slot mechanic. 

I was also looking for an oportunity to master C style game developement and to move away from *Object Oriented Programming*. 
I wanted to be more *memory aware* and to pay attention to *cache misses*.
That will led me to discover how to simplify memory management with __memory arenas__, thus reassuring me on my last concerns on my c programming abilities.

For the people not knowing what an arena is or still fearing C programming, I really recommand the following video.
I think it was the one that really motivated me to use C for my next project.

<iframe width="560" height="315" src="https://www.youtube.com/embed/9UIIMBqq1D4?si=mGSp9nXogj6FulGJ" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

## Conclusion

For the moment the developement of this game was extremely fun. The reason of that enjoyement are multiple, but I found that the *hand-made* philosophy procure a lot of joy.
Also, this was my first project I made using __Neovim__ and it was delightfull—appart from the couple of weeks of trying to make my config and understaining what a LSP is—spetialy when you became proeficient with the *vim motions*.
Forcing me to make my own build config with __CMake__ broadened my understaining of how program are builded. I inproved my comprehension of what *Release* or *Debug* build really are under the hood.

Moreover the simple C way of doing things, resulted in me doing lot more things rather than scratching my head on which abstraction or which convoluted *OOP black magic* I will use.
I think that this project is helping me being a better programmer and also being a better OOP user—yes because I still love this paradigm.

