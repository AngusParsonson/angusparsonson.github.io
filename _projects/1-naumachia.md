---
title: "Naumachia (multiplayer, virtual reality, naval battle survival game)"
excerpt: "Led a team of six in building an immersive and competetive online virtual reality experience for a third year university group project. We wrote over 2200 lines of C# code using an agile development method. We also implemented extensive use of version control, continuous integration and sprint planning through GitLab. Features included a robust and scalable enemy AI system, responsive voice control commands, immersive custom hardware controls, 3D models, textures and animations designed/created by us, multiplayer support both over LAN and the internet and an original music soundtrack.<br/><img src='/images/projects/naumachia/naumachia.png'>"
collection: projects
---

<div style="text-align: justify">
Led a team of six in building an immersive and competetive online virtual reality experience for a third year university group project. We wrote over 2200 lines of C# code using an agile development method. We also implemented extensive use of version control, continuous integration and sprint planning through GitLab. Features included a robust and scalable enemy AI system, responsive voice control commands, immersive custom hardware controls, 3D models, textures and animations designed/created by us, multiplayer support both over LAN and the internet and an original music soundtrack.
</div>

## Abstract 
Naumachia is a virtual reality, arcade, battle arena game between two opposing players. Each player controls the captain of a naval ship. Player ships are equipped with cannons on either side of the deck, which can be fired and reloaded using voice controls. Additionally, each player must steer their ship using a physical wheel built for this game. Player ships always move forwards without any player input. Other non-player ships are also present in the game which attack the players with cannonballs. All ships have a health meter and can be damaged by enemy ships. Player ships are larger than AI ships, making them easily distinguishable.

Naumachia takes its name from the very real naval arena battles that the Ancient Romans used to stage. They would flood gladiator arenas and bring in real ships to battle each other. The visual design of this game takes much influence from these historical events.

<img src='/images/projects/naumachia/naumachia_painting.jpg' width=450>

The aim of the game is to gain a higher score than the opposing player by the end of the round. The default duration of a round is 10 minutes. Points are gained by eliminating other ships, that being AI or player ships. Enemy ships can be damaged and destroyed by successful hits with cannonballs or by being rammed by player ships. AI ships grant a small amount of points and increase the score multiplier when killed. These ships respawn frequently and indefinitely so there will always be AI ships present in the arena. If an AI ship which has been attacked by both players dies, the score and score multiplier is gained by the player which delivered the final blow to the ship. Player ships grant a large amount of points when killed and respawn with full health after a short delay. When a player dies, they keep their total score so far, but their score multiplier is reset to 1.0x. The effect of a ship being damaged can be seen by fires which appear as its health becomes lower, as well as floating numbers indicating the amount of damage dealt or received. When a ship is destroyed, it shatters, leaving behind wreckage that disappears after a short delay.

Players can aim their cannons by looking at the side of the ship towards which they want to fire the cannons. A visual then appears representing the arc the cannonballs will follow when fired. The cannons can then be fired with the voice command “Fire!”. Since the cannons are on the sides, players cannot shoot directly forwards or backwards. Additionally, player cannons have a limited amount of shots that can be fired in quick succession and must be reloaded with the voice command “Reload!”. AI ships have only 2 cannons, one at the back and one at the front. These cannons have full range of motion and will be fired towards players when in range.

The battle takes place within a Roman coliseum with various isles and obstacles positioned inside the coliseum. These obstacles can block fired cannonballs and impede ship movement in the arena. If a ship collides with the environment, it will take damage. Around the arena, there are 12 gates from which player and AI ships will spawn throughout the battle. Furthermore, there are different power ups scattered throughout the arena which can be picked up by player ships to boost the efficacy of the players for a limited time. These powerups include:
- Increased movement speed
- Increased rate of fire for cannons
- Increased score multipliers

Additionally, there is a health pickup which restores health to player ships. Health pickups cannot increase the player’s health beyond their maximum health. These power ups respawn a few moments after their effects have expired.

Traditional UI elements such as score and health points are represented in in-game objects to further increase immersion. The score is represented on a paper scroll positioned next to the helm and the amount of health remaining can be seen by the amount of fires on the ship.

The inclusion of voice controls, a physical steering wheel and the integration of virtual reality make Naumachia an exciting, immersive, and competitive game which can be understood and enjoyed in short playing sessions. 

## Technical Content

### Networking
The two main networking challenges for the project were finding and learning a suitable networking library for use with Unity, then secondly, choosing which components of the game were necessary to network, and which could be ignored without being noticeable to the players (in order to reduce network throughput and therefore lag.)

We used Unity’s Network Manager to implement our multiplayer functionality. This uses UDP by default enabling a low latency experience. The downside to this UNet is that it is a depreciated library, meaning it lacks support in some areas and has design issues. However, after researching the alternatives (eg. Photon), we decided the gains to be made in converting our project were not worthwhile and that all use cases could be fulfilled with the original choice.

Gameplay consists of three dynamic elements: the two players, enemyAI actions and map wide events.

Map events were the least network intensive of the three elements: after broadcasting that the event is happening in a single flag, no other detail must be passed through the network- meaning a simple, low-latency solution.

Networking player actions played out as being the most network intensive of all networking actions. This was necessary as any player action must be instantly communicated to the other to create a seamless multiplayer experience. We streamed all player actions using network transforms and networked variables. These streamed at roughly the same rate as screen refresh creating the illusion of instant response times.

Enemy movements were the most complicated part of networking. The obvious option was to stream the AI actions in the same way the player actions were streamed. However as there were many enemies this would cause a network overload. The solution was to create a state machine system where only changes in state were communicated across the network rather than streaming frame by frame positions and actions. This was possible as enemy action scripts/ai was determined by player positions and actions which are already available.

As this project was focused around the final games day event, special effort was put into networking the two computers around the MVB’s network. We initially had difficulty in streaming across the network due to external security constraints, so our final solution was to log into user profiles through eduroam then disconnect and directly connect the two computers which ran the game to avoid the universities restrictions. This lan connection helped provide a low-latency experience. 

Below is a picture of two applications running on the same computer. One of the applications runs as a client and the other as a host (server and client at the same time). This functionality was possible by setting up a lan connection between the two applications, and made testing drastically more efficient by bypassing the majority of the set-up. By running different roles in the unity editor it was possible to debug both experiences.

<img src='/images/projects/naumachia/multiplayer_screenshot.png'>
*Two players in the same game*

### Wheel Controller
No one in the team had previous experience in electronics or Computer Aided Design and Manufacturing (CAD/CAM). We also had a small budget. The controller was an essential part of the game that needed to be integrated early, so rapid prototyping was required and we would have to find cheap solutions. We had an idea to use two wheels attached to a stand: one main wheel outside the stand for users to turn, and a smaller,  inner wheel inside the stand. The idea was that if we had some sensor fixed in parallel to one point on the inner wheel, we could measure movement along an axis as the inner wheel turns with the main wheel.

<img src='/images/projects/naumachia/wheel_sketch.jpg'>
*Sketch of idea for wheel mechanics*

First, we built a proof of concept for this idea. It made use of a paper plate (main wheel), an old CD (inner wheel), a cardboard box (stand), a kitchen roll tube (axel) and the optical sensor of an old USB mouse. The stripped-apart mouse worked well so - when we were unable to be inducted into the electronics area of the Hackspace in time for our MVP - we decided to continue using it as our sensor rather making one from scratch.

Then, once we had access to the Hackspace, the next prototypes were built in wood. Experience was gained in using Laser Cutters (and their design software) to build the stand, and 3D printers (and their software) to produce a cuskjtom piece required to attach the main wheel to the axel.

Rather than trying to build our own model helm (ship wheel)  - given our little time and no experience in detailed woodwork - we chose to buy a scale replica. This decision turned out to be very good as the textured feel and heavy weight of the wheel felt great to play with. A lot of effort was then spent in making the wheel’s control feel responsive in-game (like you were actually controlling the ship). Most of those who tested our MVP demo noted that the replica wheel and its responsiveness created great immersion for them.

Unfortunately, due to COVID-19, we were unable to finalise the design and build two final controllers. Only one working prototype exists. Also, we were unable to complete the final task of using 3D scanning technology to produce a realistic digital rendering of the wheel. It would have been used in-game and moved in sync with the real world wheel to add even more immersion.

### Enemy AI
The First AI system implemented was very problematic. It hard-coded all decision making into a single script. During gameplay, an enemies behaviour was uninterpretable during gameplay and the performance did not scale well with multiple enemies at once.

After our MVP, it was decided to completely rebuild our AI on top of Unity’s in-built NavMesh library. This AI library allows developers to quickly add movement to their characters. A NavMeshAgent component will automatically calculate a shortest path and move to a specified destination. A NavMeshSurface defines where this Agent can go, and NavMeshObstacles can be added to the Surface which the Agent will avoid.

While very useful and scalable - thanks to the ability to pre-calculate and bake shortest path navigation to a surface to avoid repeated calculations -  the enemy ships could move in any direction and even jump! This is because the NavMesh library was designed for controlling human-like characters. It turned out that it was not straightforward to customise the internal behaviours of such Agents. There was also quite poor documentation on the library.

To achieve more realistic movement, a Finite State Machine like system was built over the NavMeshAgent layer. This also solved our interpretability problem. Now, the enemy AI could be in one of the following states:
1. **Patrol**: moving around a set of points in the arena, searching for players to fight.
2. **Chase**: moving directly towards a player to fight them.
3. **Attack**: aiming and firing at a nearby player's broadside.
4. **FightOrFlight**: deviding whether to flee from or attach a player who just damaged it.
5. **Flee**: moving in the opposite direction of the player attacking it.

The current state of the FSM would determine what destination is given to the NavMeshAgent - however, the Agent cannot actually move its enemy ship. Instead, we disable its ability to change its own position, and just extract the path along the NavMeshSurface that it wants to take towards the destination. Another component, AIEnemy, interprets this path and moves the enemy in a ship-like manner accordingly.

In the end, this design was very successful. It was very easy to debug enemy AI during development as we could always trace bugs to specific states. Also, this AI system scaled well enough that we could support over 12 enemies at once with no performance issues.

### Firing System
The firing system started as a basic aiming and firing using mouse controls and eventually evolved into using VR input, such as head tracking to control the aiming of the cannon balls and voice commands to initiate firing sequences. 

One of the main technical challenges was developing realistic physics principles for the movement of the cannon balls themselves. This became a primary objective after feedback from our initial alpha review as it would help develop immersion into the game. 
To do this we utilised parabola equations, and Unity’s raycasting features, to predict the end point of the cannon ball’s trajectory. This allowed accurate modelling of trajectory and realistic physics which helped solidify cannon firing in relation to the movement of the player ship. This was improved when we added rigidbodys to the ships to allow the impact of forces on their movement.

This also allowed the cannons to utilise Unity’s lookAt function so as to swivel to the point where the player is aiming. This enabled our game to feel responsive, with added object animation and immersion.

To visualise the firing arcs we initially attempted to use the in-built unity line rendering scene object. However, we quickly realised this was not an ideal solution due to the functionality being deprecated. We therefore created our own bespoke solution which involved utilising a trajectory model and splitting it up into individual coloured line segments. The overall effect was that of a dotted line which was both more user friendly (easier to see where you were aiming) and more in keeping with our game’s cartoonish aesthetic than the line renderer.
