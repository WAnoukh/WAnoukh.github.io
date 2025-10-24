---
layout: post
title:  "Memory Arenas and Hot-reloading!"
date:   2025-10-24 
categories: jekyll update
excerpt_separator: <!--more-->
related: Onü
---

I just recently added __hot reloading__ to my game [Onü]({% link _projects/onu.md %}).
It was a functionality I wanted to add for a very long time and it also allowed me to finally implement __memory arenas__.

<video controls style="width:100%">
  <source src="/assets/video/hotreloading/hot-reloading.mp4" type="video/mp4">
  Your browser does not support the video tag.
</video>

<!--more-->

## Hot-Reloading

The core mechanism itself of my implementation isn't very complicated at all.
You need two types of build target: 
an *host executable* and a *DLL* that we will call respectively the "__EXE__" and the "__HOT__".

The *HOT* will contain all our code logic such as the engine, the gameplay or the editor's logic.
What is very practical is that the HOT is compiled in a dll, 
and that's allowing us to load it or unload it whenever we want 
\(and we most likely want that when a change has been made in the base code\).

The *EXE* is the part preserved between reloading.
So it needs to be minimal because we cannot reload the code that defines its logic. 
It's goal is only to check if the HOT dll has been changed, and to load it if it's the case.

This, as it is, is already very powerful. 
I inspired myself from Casey Muratori's [Handmade Hero](https://guide.handmadehero.org/) 
and from this wonderful article [Hot Reloading in C](https://www.bytesbeneath.com/p/hot-reloading-in-c) by Dylan Falconer.
But we still have one problem: every HOT dll __heap or global memory vanishes__ during reloading!

## Arenas

So this is the perfect occasion to use __arenas__!
Arenas are blocks of memory that we use to store this with similar life-times. 
We can then decide what allocation strategy we use inside the arenas, 
thus saving a lot of *malloc* cost, and simplifying *frees* by just freeing a bunch of data at once.

Arenas are by themselves very useful, but in our example it's even better!
We can malloc some arenas in our EXE and pass them to the HOT. 
If every allocation in the HOT is made inside those arenas every memory will be __saved between reloads__.

## Managing Libraries

My project uses a couple of libraries like GLFW, Glad or DearImgui.
I was building them as static libraries, but it causes issues with hot reloading.
Those libs __context aren't preserved across DLL boundaries__
\(so you have 2 "instance" one in EXE and the other in HOT\).

One solution can be to only statically link your lib to either EXE or HOT. 
But in my case I don't want the program window to be reloaded, 
so that means in need to link it to my EXE.
Doing that led to a lot of complications because my HOT calls a lot of GLFW or Glad functions to affect the game,
and it is pretty costly and cumbersome to call those functions across the DLL boundary.

So my final solution was to also build them as *shared libraries* \(DLL\) and to link them at compile time. 
Like that the context is shared with my EXE and HOT while being used as if it were statically linked.

Note: for the case of Dear ImGUI, it was even simpler. 
ImGUI is "immediate", so it is rebuilt every frame, 
so it is not very important if its memory was erased during a reload. 
So I just statically linked it to the HOT dll.

## Additional thinking 

I was using a lot \(too much\) global variables, 
especially for rendering where i needed to store some states or texture and shader indexes. 
And of course everything broke when hot-reloading.

So I gathered every global in a context struct.
I already passed some context structs to a lot of functions, and having to do the same for rendering was not my cup of tea.
It is quite tiring to have to scatter the call stack to add arguments for the rendering context when you realise that this function needs to call "draw_triangle()".

So i opted for... *global variables*!
But before throwing rocks at me, I decided to only use one global to store a pointer to the rendering context struct.
I only need to assign this pointer every reloading, but that is much better than having to pass it every call.
I may use this technique for isolated modules that need to keep track of a state or resources like my texture manager.


