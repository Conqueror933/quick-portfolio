# Arcade
[This project](https://github.com/Conqueror933/Arcade) was created about 3 month after I started learning c++, in november 2018, through [internet resources](https://www.youtube.com/watch?v=PwuIEMUFUnQ&list=PLqCJpWy5FohcehaXlCIt8sVBHBFFRVWsx&index=1).<br/>
I consider it legacy by now as there are a ton of bad design decisions in the code that I wouldn't repeat today. But until my Engine is finished it stays as my biggest __finished__ project.<br/>
<img src="/images/ArcadeMainMenu.png?raw=true"/><br/>

## Willkommen zur Arcade
Welcomes you in german.
## [Kaesekaestchen](/portfolio/programming/cpp/arcade/kaesekaestchen)
The original project that evolved into the Arcade.
## [Snake](/portfolio/programming/cpp/arcade/snake)
It's the game Snake.
## Pong (under construction)
Teaching a friend of mine who's studying computer engineering at university how to c++.
## Memestone (under construction)
A cardgame idea that should it ever get finished will be made in my Engine instead.
## Beenden
End button.
## DevMode
Only exists in the debug-build, serves as a platform to test things.
## [Menu](/portfolio/programming/cpp/arcade/menu)
Despite being in plain sight, a somewhat hidden part you often don't think about.<br/>
<br/>
---
---
<br/>
### The Base
The entire project is based on the [Chili Framework](https://github.com/planetchili/chili_framework).<br/>
The framework handles the Windows-API and provides the user with an easy to use interface to put pixels on the screen.<br/>
`gfx.PutPixel(x_coordinate, y_coordinate, red, green, blue);`<br/>
It also provides a way to get mouse&keyboard input.<br/>
`if(kbd.KeyIsPressed('A) || mouse.LeftIsPressed())`<br/>
In addition to keeping the game running<br/>
```c++
			Game theGame( wnd );
			while( wnd.ProcessMessage() )
			{
				theGame.Go();
			}
```
it handles errors and gives diagnostics.<br/>
```c++
		catch( const ChiliException& e )
		{
			const std::wstring eMsg = e.GetFullMessage() + 
				L"\n\nException caught at Windows message loop.";
			wnd.ShowMessageBox( e.GetExceptionType(),eMsg,MB_ICONERROR );
		}
```

### The thing connecting it all
Game.h<br/>
```c++
#pragma once

#include "Keyboard.h"
#include "Mouse.h"
#include "Graphics.h"
#include "Menu.h"
#include "Kaesekaestchen.h"
#include "Snake.h"
#include "Pong.h"
#include "DevMode.h"
#include "MemestoneHandler.h"

class Game
{
	friend class Menu;
public:
	Game(class MainWindow& wnd);
	Game(const Game&) = delete;
	Game& operator=(const Game&) = delete;
	~Game();
	void Go();

private:
	void ComposeFrame();
	void UpdateModel();
	/********************************/
	/*  User Functions              */
	/********************************/
private:
	MainWindow & wnd;
	Graphics gfx;
	FrameTimer ft;
	/********************************/
	/*  User Variables              */
	std::stack<std::unique_ptr<Interface>> spInterface;
	class IQuit : public Interface
	{
	public:
		int Update() { return 1; }
	};
	/********************************/
};
```
The name `Game` comes from the Chili Framework as this is the class that was intended for the game.<br/>
It got outgrown by the project.<br/>
A more appropriate name would probably be `Engine`.<br/>
<br/>
But let's start at the beginning.
```c++
void Game::Go()
{
	gfx.BeginFrame();
	UpdateModel();
	ComposeFrame();
	gfx.EndFrame();
}
```
`Go()` is the function that gets called by the mainloop every frame.<br/>
It clears the Graphics buffer, does the ~~game~~ engine logic, draws the new frame and renders it.<br/>
<br/>
To explain `UpdateModel()` we have to take a step back and explain how the architecture works.<br/>
In order to avoid having all the (potentially thousands) of games in memory at the same time and being able to go back to the previous screen, we use a stack on which the topmost object gets updated and once it finishes it gets popped off and the one below takes it's place. The first object on the stack being the `IQuit` object, ending the program. The stack allows to keep the menu in memory and quickly take over once the current game finishes.<br/>
`UpdateModel()` calls the `Update()` function of the game/menu that is currently on the top of the stack.<br/>
`ComposeFrame()` calls the `Draw()` function of the game/menu that is currently on the top of the stack.<br/>
<br/>
The stack mechanic is also used in the `Menu` where it is the easiest way to implement the "Back"-button.<br/>
