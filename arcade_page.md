# [Arcade](https://github.com/Conqueror933/Arcade)
This project was created about 3 month after I started learning c++ through [internet resources](https://www.youtube.com/watch?v=PwuIEMUFUnQ&list=PLqCJpWy5FohcehaXlCIt8sVBHBFFRVWsx&index=1).<br/>
I consider it legacy by now as there are a ton of bad design decisions in the code that I wouldn't repeat today. But until my Engine is finished it stays as my biggest __finished__ project.<br/>
<img src="ArcadeMainMenu.PNG?raw=true"/><br/>

## [Kaesekaestchen](/kaesekaestchen_page)
The original project that evolved into the Arcade.
## [Snake](/snake_page)
It's the game Snake.
## Pong (under construction)
Teaching a friend of mine who's studying at university how to c++.
## Memestone (under construction)
A cardgame idea that should it ever get finished will be made in my Engine instead.
## [Menu](/menu_page)
Despite being in plain sight, a somewhat hidden part you often don't think about.<br/>
An explanation on how the menu works.

---

### The thing connecting it all
The entire project is based on the [Chili Framework](https://github.com/planetchili/chili_framework).<br/>
The framework handles the Windows-API and provides the user with an easy to use interface to put pixels on the screen. 
`gfx.PutPixel(x_coordinate, y_coordinate, red, green, blue);`
It also provides a way to get mouse&keyboard input.
`if(kbd.KeyIsPressed('A) || mouse.LeftIsPressed())`
In addition to keeping the game running
```c++
			Game theGame( wnd );
			while( wnd.ProcessMessage() )
			{
				theGame.Go();
			}
```
it handles errors and gives diagnostics.
```c++
		catch( const ChiliException& e )
		{
			const std::wstring eMsg = e.GetFullMessage() + 
				L"\n\nException caught at Windows message loop.";
			wnd.ShowMessageBox( e.GetExceptionType(),eMsg,MB_ICONERROR );
		}
```
