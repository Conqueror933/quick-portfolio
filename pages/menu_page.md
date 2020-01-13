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

## Structure
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

<br/>
This is also adding entries to the `StringSwitch<DataPass>& GetData() { return data; }` part for use in the respective game later on. The use of the string to identify, while needing more memory than an int/enum in most cases, gives a lot more flexibility. The actual data part being able to take in whatever type is needed is achieved by using a Union<br/>
```c++
union DataPass
{
	DataPass(char c) : c(c) {}
	DataPass(int i) : i(i) {}
	DataPass(double d) : d(d) {}
	int i;
	char c;
	double d;
};
```
## Letters
How I got the letters on the screen. `Text text;` is where the magic is hidden. Let's have a look.
```c++
#include "Bitmap.h"

Bitmap::Bitmap(Graphics& gfx, std::string filename)
	:
	gfx(gfx)
{
	readBMP(filename);
}

void Bitmap::readBMP(const std::string &file)
{
	static constexpr size_t HEADER_SIZE = 54;

	std::ifstream bmp(file, std::ios::binary);
	if (!bmp.is_open()) return;

	std::array<char, HEADER_SIZE> header;
	bmp.read(header.data(), HEADER_SIZE);
	
	//Header
	bH.hdr.bfType = *reinterpret_cast<WORD *>(&header[0]);
	if (bH.hdr.bfType != 19778) throw "BMP could not be loaded"; //19778 = 'BM' //Windows 3.x bmp file
	bH.hdr.bfSizeHeader = *reinterpret_cast<DWORD *>(&header[2]);
	bH.hdr.bfReserved1 = *reinterpret_cast<WORD *>(&header[6]);
	bH.hdr.bfReserved2 = *reinterpret_cast<WORD *>(&header[8]);
	bH.hdr.bfOffsetBits = *reinterpret_cast<DWORD *>(&header[10]);
	
	//Bitmap header
	bH.info.biSizeHeader = *reinterpret_cast<DWORD *>(&header[14]);
	bH.info.biWidth = *reinterpret_cast<LONG *>(&header[18]); //actually 4 byte
	bH.info.biHeight = *reinterpret_cast<LONG *>(&header[22]); //actually 4 byte
	bH.info.biPlanes = *reinterpret_cast<WORD *>(&header[26]);
	bH.info.biBitCount = *reinterpret_cast<WORD *>(&header[28]);	//check file format
	if (bH.info.biBitCount != 4) throw "BMP wrong bit format";
	bH.info.biCompression = *reinterpret_cast<DWORD *>(&header[30]);	//if 0 -> Windows 3.x //if !0 -> Windows NT bmp
	bH.info.biSizeImage = *reinterpret_cast<DWORD *>(&header[34]);
	bH.info.biXPelsPerMeter = *reinterpret_cast<LONG *>(&header[38]); //actually 4 byte
	bH.info.biYPelsPerMeter = *reinterpret_cast<LONG *>(&header[42]); //actually 4 byte
	bH.info.biClrUsed = *reinterpret_cast<DWORD *>(&header[46]);
	bH.info.biClrImportant = *reinterpret_cast<DWORD *>(&header[50]);
	
	//Extract Color palette
	auto temp_SIZE = (bH.hdr.bfOffsetBits - HEADER_SIZE);
	std::vector<char> temp(temp_SIZE);
	bmp.seekg(HEADER_SIZE);
	bmp.read(temp.data(), temp_SIZE);
	//convert from char to Color (int)
	std::vector<Color> ColorPalette; ColorPalette.reserve(temp_SIZE / 4);
	for (auto i = 0u; i < temp_SIZE; i += 4)
	{
		ColorPalette.emplace_back(Color(
			static_cast<unsigned char>(temp[i + 3]),
			static_cast<unsigned char>(temp[i + 2]),
			static_cast<unsigned char>(temp[i + 1]),
			static_cast<unsigned char>(temp[i + 0])));
	}

	auto ghostbytes = bH.info.biSizeImage * 2 - bH.info.biWidth * bH.info.biHeight;
	auto ghostbytesperline = ghostbytes / 2 / bH.info.biHeight;

	//Read raw Bitmap data
	std::vector<char> img(bH.info.biSizeImage);
	bmp.seekg(bH.hdr.bfOffsetBits);
	bmp.read(img.data(), bH.info.biSizeImage);
	//Reserve space to avoid reallocation
	bitmapColors.reserve(bH.info.biWidth * bH.info.biHeight);
	//for every BYTE in the image... 
	if (bH.info.biWidth % 2 % sizeof(int) != 0)	// 2 has to do with the 2 pixels stored per BYTE
	{
		auto j = 0u;
		for (auto i = 0u; i < bH.info.biSizeImage; i++)
		{
			if (j >= bH.info.biWidth / 2) //avoid the fucking padding crap, jeez
			{
				if (j == bH.info.biWidth / 2 + ghostbytesperline - 1)
				{
					j = 0;
					continue;
				}
				j++;
				continue;
			}
			j++;
			//...extract PixelColor stored in first 4 BITS pointing to a location in the ColorPalette and...
			bitmapColors.emplace_back(Color(ColorPalette[(img[i] & 0xF0) >> 4]));
			//...extract PixelColor stored in second 4 BITS pointing to a location in the ColorPalette...
			bitmapColors.emplace_back(Color(ColorPalette[img[i] & 0x0F]));
			//...and store them as full fledged Color Objects in the bitmapColors vector
		}
	}
	else
	{
		for (auto i = 0u; i < bH.info.biSizeImage; i++)
		{
			//...extract PixelColor stored in first 4 BITS pointing to a location in the ColorPalette and...
			bitmapColors.emplace_back(Color(ColorPalette[(img[i] & 0xF0) >> 4]));
			//...extract PixelColor stored in second 4 BITS pointing to a location in the ColorPalette...
			bitmapColors.emplace_back(Color(ColorPalette[img[i] & 0x0F]));
			//...and store them as full fledged Color Objects in the bitmapColors vector
		}
	}
}

void Text::DrawLetter(const char letter, Vec2<int> position, Color c)
{
	//read bitmap
	auto xpos = letters[letter].first;
	auto ypos = bH.info.biHeight;
	auto width = letters[letter].second;
	auto xend = xpos + width;

	for (auto i = xpos; i < xend; i++)
	{
		for (auto j = 0u; j < ypos; j++)
		{
			if (bitmapColors[i * ypos + j] != Colors::Magenta)
				gfx.PutPixel(position.x + i-xpos, position.y + j, c);
		}
	}
}

Text::Text(Graphics& gfx, std::string filename) 
	: 
	Bitmap(gfx, filename)
{
	auto hw = bH.info.biHeight * bH.info.biWidth;
	std::vector<Color> temp;
	temp.reserve(hw);
	for (auto i = 0u; i < bH.info.biWidth; i++)
	{
		for (auto j = bH.info.biHeight; j > 0; j--)
		{
			temp.emplace_back(bitmapColors[(j-1)*bH.info.biWidth+i]);
		}
	}
	bitmapColors = temp;
	//fill the letters map
	if (filename == "Letters2.bmp")
	{
		letters.emplace(std::make_pair('A', std::make_pair(0, 8)));
		// ... letters ...
		letters.emplace(std::make_pair('z', std::make_pair(422, 6)));
		letters.emplace(std::make_pair('!', std::make_pair(430, 2)));
		letters.emplace(std::make_pair('?', std::make_pair(434, 6)));
		letters.emplace(std::make_pair('.', std::make_pair(442, 2)));
		letters.emplace(std::make_pair(',', std::make_pair(445, 3)));
	}
}

void Text::Draw(std::string str, Vec2<int> position, int letterspace, Color c)
{
	Draw(str.c_str(), position, letterspace, c);
}

void Text::Draw(const char* str, Vec2<int> position, int letterspace, Color c)
{
	for (auto i = 0; str[i] != 0; i++)
	{
		if (str[i] == ' ')
		{ 
			position.x = position.x + space;
		}
		else
		{
			auto width = letters[str[i]].second;
			DrawLetter(str[i], position, c);
			position.x = position.x + width + letterspace;
		}
	}
}
```
<br/>
Here we have the rather simple `Bitmap` class that reads from file, handles the bitmap file format and stores the colours in a vector formated how the `Graphics` expects it. Then as a derived class `Text` maps the letters in the bitmap according to their offset from 0. For ease of use `Text` just constructs with a `std::string` and handles the rest internally.

## Labels & Buttons

