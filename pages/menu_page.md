# Menu

## Main Menu
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
