# Menu

## Main menu
```c++
MenuHandler::MainMenu::MainMenu(MenuHandler& menuHandler) : Menu(menuHandler) 
{
	//Welcome Label
	vpLabels.emplace_back(std::make_unique<Label>(
		mH.gfx, mH.text, "Willkommen zur Arcade", Vec2<int>{ 200, 50 }, Vec2<int>{ 400, 100 }, letterspacing, border, Color(0u, 0u, 185u), Colors::White));

	vpButtons.emplace_back(std::make_unique<Button>(
		mH.text, mH.gfx, Vec2<int>{ 325, 200 }, Vec2<int>{ 150, 50 }, "Kaesekaestchen", 2, letterspacing, border, half_bordersize, Colors::White, Colors::Black));
	vpButtons.emplace_back(std::make_unique<Button>(
		mH.text, mH.gfx, Vec2<int>{ 325, 260 }, Vec2<int>{ 150, 50 }, "Snake", 3, letterspacing, border, half_bordersize, Colors::White, Colors::Black));
	vpButtons.emplace_back(std::make_unique<Button>(
		mH.text, mH.gfx, Vec2<int>{ 325, 320 }, Vec2<int>{ 150, 50 }, "Pong", 4, letterspacing, border, half_bordersize, Colors::White, Colors::Black));
	vpButtons.emplace_back(std::make_unique<Button>(
		mH.text, mH.gfx, Vec2<int>{ 325, 380 }, Vec2<int>{ 150, 50 }, "Memestone", 5, letterspacing, border, half_bordersize, Colors::White, Colors::Black));
	//End
	vpButtons.emplace_back(std::make_unique<Button>( 
		mH.text, mH.gfx, Vec2<int>{ 350, 500 }, Vec2<int>{ 100, 50 }, "Beenden", 1, letterspacing, border, half_bordersize, Colors::Red, Colors::White));
#ifdef _DEBUG
	vpButtons.emplace_back(std::make_unique<Button>( 
		mH.text, mH.gfx, Vec2<int>{ 360, 550 }, Vec2<int>{ 80, 40 }, "DevMode", 1000000, letterspacing, border, half_bordersize, Colors::Cyan, Colors::Black));
#endif
}
```
<br/>
Familiar buttons from the main menu.<br/>
<br/>
The menu is structured like the `Game`, having a stack to push and pop the menus on top of and calling the `Update()` and `Draw()` functions of the topmost element.<br/>
That's why every menuscreen is its own internal class, essentially functors. Each building the menu in their constructor, pushing the buttons and labels in their respective vectors.<br/>
Here is one of such:<br/>
```c++
MenuHandler::KaeseMenu::KaeseMenu(MenuHandler& menuHandler) : Menu(menuHandler)
{
	//Welcome Label
	vpLabels.emplace_back(std::make_unique<Label>(
		mH.gfx, mH.text, "Willkommen zum Kaesekaestchen", Vec2<int>{ 200, 50 }, Vec2<int>{ 400, 100 }, letterspacing, border, Color(0u, 0u, 185u), Colors::White));
	//2 Player
	vpButtons.emplace_back(std::make_unique<Button>(
			mH.text, mH.gfx, Vec2<int>{ 325, 200 }, Vec2<int>{ 150, 50 }, "Zwei Spieler", 2, letterspacing, border, half_bordersize, Colors::White, Colors::Black));
	//AI Level 1
	vpButtons.emplace_back(std::make_unique<Button>(
		mH.text, mH.gfx, Vec2<int>{ 325, 260 }, Vec2<int>{ 150, 50 }, "Einfach", 3, letterspacing, border, half_bordersize, Colors::White, Colors::Black));
	//AI Level 2
	vpButtons.emplace_back(std::make_unique<Button>(
		mH.text, mH.gfx, Vec2<int>{ 325, 320 }, Vec2<int>{ 150, 50 }, "Mittel", 4, letterspacing, border, half_bordersize, Colors::White, Colors::Black));
	//AI Level 3
	/*vpButtons.emplace_back(std::make_unique<Button>(
		mH.text, mH.gfx, Vec2<int>{ 325, 380 }, Vec2<int>{ 150, 50 }, "Schwer", 5, letterspacing, border, half_bordersize, Colors::White, Colors::Black));*/
	//Optionen
	vpButtons.emplace_back(std::make_unique<Button>(
		mH.text, mH.gfx, Vec2<int>{ 325, 460 }, Vec2<int>{ 150, 50 }, "Optionen", 6, letterspacing, border, half_bordersize, Colors::White, Colors::Black));
	//Back
	vpButtons.emplace_back(std::make_unique<Button>(
		mH.text, mH.gfx, Vec2<int>{ 25, Graphics::ScreenHeight - 65 }, Vec2<int>{ 75, 40 }, "Back", 1, letterspacing, border, half_bordersize, Colors::Red, Colors::White));

	//Set Default Values
	mH.data.AddEntry("Size", 10);
	mH.data.AddEntry("Square", 40);
	mH.data.AddEntry("Border", 0.25);
}
```
This is also adding entries to the `data` part for use in the respective game later on.
