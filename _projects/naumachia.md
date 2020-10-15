---
title: "Naumachia (multiplayer, virtual reality, naval battle survival game)"
excerpt: "Led a team of six in building an immersive and competetive online virtual reality experience for a third year university group project. We wrote over 2200 lines of C# code using an agile development method. We also implemented extensive use of version control, continuous integration and sprint planning through GitLab. Features included a robust and scalable enemy AI system, responsive voice control commands, immersive custom hardware controls, 3D models, textures and animations designed/created by us, multiplayer support both over LAN and the internet and an original music soundtrack.<br/><img src='/images/projects/naumachia/naumachia.png'>"
collection: projects
---

<div style="text-align: justify">
Led a team of six in building an immersive and competetive online virtual reality experience for a third year university group project. We wrote over 2200 lines of C# code using an agile development method. We also implemented extensive use of version control, continuous integration and sprint planning through GitLab. Features included a robust and scalable enemy AI system, responsive voice control commands, immersive custom hardware controls, 3D models, textures and animations designed/created by us, multiplayer support both over LAN and the internet and an original music soundtrack.
</div>
<br/><br/>

## Abstract 
<div style="text-align: justify">
Naumachia is a virtual reality, arcade, battle arena game between two opposing players. Each player controls the captain of a naval ship. Player ships are equipped with cannons on either side of the deck, which can be fired and reloaded using voice controls. Additionally, each player must steer their ship using a physical wheel built for this game. Player ships always move forwards without any player input. Other non-player ships are also present in the game which attack the players with cannonballs. All ships have a health meter and can be damaged by enemy ships. Player ships are larger than AI ships, making them easily distinguishable.

<br/><br/>
Naumachia takes its name from the very real naval arena battles that the Ancient Romans used to stage. They would flood gladiator arenas and bring in real ships to battle each other. The visual design of this game takes much influence from these historical events.

<br/><br/>
<img src='/images/projects/naumachia/naumachia_painting.jpg' width=450 height=400>

<br/><br/>
The aim of the game is to gain a higher score than the opposing player by the end of the round. The default duration of a round is 10 minutes. Points are gained by eliminating other ships, that being AI or player ships. Enemy ships can be damaged and destroyed by successful hits with cannonballs or by being rammed by player ships. AI ships grant a small amount of points and increase the score multiplier when killed. These ships respawn frequently and indefinitely so there will always be AI ships present in the arena. If an AI ship which has been attacked by both players dies, the score and score multiplier is gained by the player which delivered the final blow to the ship. Player ships grant a large amount of points when killed and respawn with full health after a short delay. When a player dies, they keep their total score so far, but their score multiplier is reset to 1.0x. The effect of a ship being damaged can be seen by fires which appear as its health becomes lower, as well as floating numbers indicating the amount of damage dealt or received. When a ship is destroyed, it shatters, leaving behind wreckage that disappears after a short delay.

<br/><br/>
Players can aim their cannons by looking at the side of the ship towards which they want to fire the cannons. A visual then appears representing the arc the cannonballs will follow when fired. The cannons can then be fired with the voice command “Fire!”. Since the cannons are on the sides, players cannot shoot directly forwards or backwards. Additionally, player cannons have a limited amount of shots that can be fired in quick succession and must be reloaded with the voice command “Reload!”. AI ships have only 2 cannons, one at the back and one at the front. These cannons have full range of motion and will be fired towards players when in range.

<br/><br/>
The battle takes place within a Roman coliseum with various isles and obstacles positioned inside the coliseum. These obstacles can block fired cannonballs and impede ship movement in the arena. If a ship collides with the environment, it will take damage. Around the arena, there are 12 gates from which player and AI ships will spawn throughout the battle. Furthermore, there are different power ups scattered throughout the arena which can be picked up by player ships to boost the efficacy of the players for a limited time. These powerups include:
- Increased movement speed
- Increased rate of fire for cannons
- Increased score multipliers

<br/><br/>
Additionally, there is a health pickup which restores health to player ships. Health pickups cannot increase the player’s health beyond their maximum health. These power ups respawn a few moments after their effects have expired.

<br/><br/>
Traditional UI elements such as score and health points are represented in in-game objects to further increase immersion. The score is represented on a paper scroll positioned next to the helm and the amount of health remaining can be seen by the amount of fires on the ship.

<br/><br/>
The inclusion of voice controls, a physical steering wheel and the integration of virtual reality make Naumachia an exciting, immersive, and competitive game which can be understood and enjoyed in short playing sessions. 

<br/><br/>
## Technical Content
