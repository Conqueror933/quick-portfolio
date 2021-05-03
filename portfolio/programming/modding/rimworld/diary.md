## 20.04.2021

While looking for C# "Cheat Sheet" found https://goalkicker.com/
Now sifting through the 800 pages of the C# book.

## 19.04.2021

Make roadmap for Rimworld AI.
//todo Make roadmap more fine-grained, extend roadmap.

### What?
Rimworld AI Overhaul, make raiders more dangerous/smart.
### How?
Improving raider AI to use squad tactics.
	What does that mean?
	(1) Instead of charging into a killbox, stay back, observe the area, take potshots, break defensives from afar, avoid retaliate fire.
	(2) Keep melee units on alert behind ranged units and use them to block enemy charges, jump important targets or exploit weaknesses, dont just charge at the enemy.
	An overall slower, more methodical approach. It's not their first raid after all.
### How?
(1) Overlay a 'position value' grid.
	(3) Positions that are not in range of any enemy are valued higher. (Turrets, traps, people)
	(4) Positions that can shoot at enemies are valued higher. Shots not blocked by walls.
	(5) Move Units to high value squares and 'siege' the enemy. Destroy outer turrets. Kill enemies that stray to far from the safety of cover.
		(5.1) Break walls and move in over time. (Maybe just mark squares outside as impassable over time, to force the move in.)
(2) Overlay a 'threat value' grid.
	(6) Positions that are in range of enemies are valued lower.
	(7) Enemies on positions that threaten ranged units are valued higher.
### How?
(3, 4, 6, 7) Gain access to Rimworlds squares and add these values.
	(8) Tie new values into AI decision making.
(5) Gain access to Rimworlds raider AI (Job manager?).
	(9) Change it...
(5.1) Change wall pathfinding value to something lower.
### How?
(~) Get an idea of how Rimworld handles it's raider AI and squares.
### How?
(~) Good question.
	Learn C#. (Moderate level)
	dnSpy Rimworld and read a lot...


## 17.04.2021

Set up Diary.

Problem:
Trying to figure out how to tackle the giant codebase that is Unity+Rimworld,
without proper source code access, in a language Im not particularily familiar with, C#.

Approaches:
Read documentation: If there was any.
Talk to author of code: Well yeah...
Step through code from main function: Results may vary but worth a try.
Sit in a corner and cry? "Do you want fries with that?"

Solution?:
Make small program in C# to familiarize with the language. (TicTacToe?)