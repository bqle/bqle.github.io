---
title: "3D Tic Tac Toe"
date: 2022-09-29T19:49:29-04:00
draft: true
author: "Benjamin Le"
image: /blogs/3dt.png
---

*What happens if you extend Tic Tac Toe to 3 dimension?*
<!--more-->

## I. The Age-Old Game
This project started from a curiosity I had about extending tic-tac-toe. In Vietnamese culture (and I am sure other asian cultures as well), tic-tac-toe is often played over an theoretically infinite board, where five (instead of three) moves in a row is required to win. As a kid, I would play this game with my dad, brother, friends, and even teachers when we had the chance. This version was so much more fun than the classic three-by-three because the game has not been theoretically solved. Even if it is solved for computers, the infinite board nature ensures possibility for creative work-arounds in human games and memorable circumstances. Once, for instance, a classroom rival and I played a game that stretched over 2 lessons and had to extending our game to extra pages of paper - flipping our notebook and forth to see where the other had moved - one page ago. 

However, as I embarked on this project, I was wondering how else I can expand this beloved game. It wasn't broke, so I added one dimension to it.
**How would it feel to play 3D tic tac toe?**
## II. Building 3D Graphics
I have had extensive experience building web apps, but they have largely been limited to plain CSS and HTML with occasional webkit transitions. At no time did I work with 3D graphics on the web. A couple of options were available to me like [BabylonJS](https://www.babylonjs.com/), [Unity WebGL](https://docs.unity3d.com/Manual/webgl-gettingstarted.html), and [ThreeJS](https://threejs.org/). While the first options required significant overhead of installment and having to learn new IDEs, ThreeJS provides flexible 3D graphics for simple use cases, good documentation, and a thriving community of developers and libraries.

ThreeJS became my first choice. Then, I dove into the documentation, threads, and projects to understand the basic building blocks of the ThreeJS 3D graphics world: the scene, mesh geometry, material, camera, and lighting. All aspects of 3D graphics but packed in the language of ThreeJS.

I also designed both a mouse-based and a key-based movement system, so that users can click or use their keyboard to highlight their choice. For the mouse, I used a simple ray-tracing library to decide on which object was clicked first, and ignored the signal for all the subsequence objects. One interesting aspect is that one cannot click on the center cube. To work around this, I attached the double-click action to selecting the center cube.

And voila, with the graphics and movement complete, two users could play on the same device by alternating their turns. However, that was not the end of it.

## III. Making It Multiplayer

Soon after I showed my friends the first demo, the idea of making it multiplayer came to my mind. This would allow friends to play across devices and for me to dive deep in the world of socket development. Unlike [Unity Engine](https://unity.com/), ThreeJS does not come with its own networked object syncing, so I incorporated my own event-driven networking using [SocketIO](https://socket.io/). SocketIO is a mature socket development library with cross-language support in Javascript/Typescript and Python - both of which I need for my central server and frontend. 

### Rethinking server purpose
Socket development required me thinking about the game in an event-driven way and program around signals being sent - a style of programming that I had not had extensive experience with. Server design also was important because I wanted to think about the sorts of information the server would store - inadvertently asking what role the server should play. Was the server going to contain the entire game state of every single game and perform checks on moves before passing them on? This would require significant process on its part. If not that, should the server only serve as a relayer - serving as a middleman to pass messages between players. Unsurprisingly, it had to be something in between. While I performed room managment on the server, it also serves as a relayer of messages between frontend users. Checks, thus, were done by the frontend before any message could be sent. Obviously, this approach falls prey to bad actors pretending to be users and sending inaccurate messages - corrupting game states - but network security was not a big concern for me at this point since only my friends were using the service.
Furthermore, adding multiplayer also meant rethinking design.

### Rethinking UX 
Turning the game online meant that the interface now take more responsibility over tasks that were once solved because players were in-person and using the same device. Now, since players are not in communication with each other, managing communication becomes important. For instance, players should be notified when another join or leave their room. The design also has to adapt to this change. For instance, what would happen if a player leaves the room before the game ended? Should the other still be kept in the room? Also, how should we handle deciding which player to make the first move - and let them know of their order? 

All these design questions came across my mind, and I had to make my own choices - weighing the effects on the intuitiveness of the design, convenience, and implementability.

## IV. Deployment 
After many iterations of experimentation and adjustment, the game was complete. With the advent of cloud technologies, I was able to host the entire game on AWS, serving both the game and as a central socket node. It was run in a docker container since I had other apps running on this EC2 instance as well and wanted to isolate this development environment.

Voila! Now my friends (and you) can see for themselves what 3D tic tac toe is like. It was an enlightening adventure in 3D graphics development and making a multiplayer game.

