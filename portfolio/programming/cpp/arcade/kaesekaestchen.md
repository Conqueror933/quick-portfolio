## The Main Menu
<img src="/images/ArcadeMainMenu.png?raw=true"/><br/>

### The Kaesekaestchen Menu
<img src="/images/KäsekästchenMainMenu.png?raw=true"/><br/>
#### Willkommen zum Kaesekaestchen
Welcomes you to the game in german
#### Zwei Spieler
Two player versus mode
#### Einfach
Easy AI that just clicks at random without making obvious mistakes
#### Mittel
Medium AI that finds a bar to click without giving you a square until there are no more
#### Optionen
Optionsmenu, see below
#### Back
Brings you back to the mainmenu

### The Kaesekaestchen Options Menu
<img src="/images/KäsekästchenOptionsMenu.png?raw=true"/><br/>
#### Optionen
Optionslabel in german
#### Small & Medium & Default & Big
Sets the playingfieldsize to 4,8,10,12 respectivly
#### Square & Fullscreen
Sets wether or not black bars on the edges or filling the whole screen is desired
#### Thick & Slim Borders
Makes the borders of the squares, the bars, bigger so they are easier to click on but sacrifices the aestethic look for it

### What a game could look like
The very start.<br/>
<img src="/images/kaesekaestchen_start.png?raw=true"/><br/>
<br/>
After some more or less random setting.<br/>
<img src="/images/kaesekaestchen_phase1.png?raw=true"/><br/>
<br/>
It appears red gave blue an opportunity to strike!<br/>
<img src="/images/kaesekaestchen_phase2.png?raw=true"/><br/>
<br/>
Blue couldn't find a "free" spot anymore, so red gets to take ...a lot.<br/>
<img src="/images/kaesekaestchen_phase3.png?raw=true"/><br/>
<br/>
Red had to give some back, will it be enough?<br/>
<img src="/images/kaesekaestchen_phase4.png?raw=true"/><br/>
<br/>
Red wins.<br/>
<img src="/images/kaesekaestchen_redwins.png?raw=true"/><br/>
<br/>
One more click brings you back to the menu so you can play again!<br/>

---
## Technical Explanation
For an explanation on how the menu works click [here](/pages/menu_page).<br/>
<br/>
### Highlights
Board.cpp - the mother class for all Kaesekaestchen game modes.<br/>
The constructor working out the sizes of various things that can be changed in the options menu.<br/>
```c++
Board::Board(Graphics& gfx, const BoardColors brdclr, const Vec2<int> cellcount, const double borderthicknessratio)
	:
	gfx(gfx),
	brdclr(brdclr),
	cellcount(cellcount),
	cellborderwidth(CalculateCellBorderWidth1(cellcount, borderthicknessratio)),
	cellsize(CalculateCellSize(cellcount, cellborderwidth)),
	topleft(CalculateTopLeft(cellcount, cellsize, cellborderwidth)),
	text(gfx, "Letters2.bmp"),
	label(gfx, text, ".", Vec2<int>(700, 10), Vec2<int>(0, 0), 3, 6, Colors::Magenta)
{
	Init(*this);
}
```
<br/>
Overloaded `Draw` function. `Draw(bool b)` exists for the green "last clicked" bar, which just gets drawn over what was there before.<br/>

```c++
void Board::Cell::Draw() const
{
	//decide on Color
	Color c;
	switch (playerflag)
	{
	case None:
		c = brd.brdclr.foreground; break;
	case Player1:
		c = brd.brdclr.player1; break;
	case Player2:
		c = brd.brdclr.player2; break;
	}
	//Draw inner Cell
	brd.gfx.DrawRectangleDim(pos.x + brd.cellborderwidth, pos.y + brd.cellborderwidth,
		brd.cellsize.x - brd.cellborderwidth, brd.cellsize.y - brd.cellborderwidth, c);
	//Draw Edges
	if (top)
		brd.gfx.DrawRectangle(pos.x, pos.y, pos.x + brd.cellborderwidth + brd.cellsize.x, pos.y + brd.cellborderwidth, brd.brdclr.clicked);
	if (left)
		brd.gfx.DrawRectangle(pos.x, pos.y, pos.x + brd.cellborderwidth, pos.y + brd.cellborderwidth + brd.cellsize.y, brd.brdclr.clicked);
}

void Board::Cell::Draw(bool b) const
{
	if(b)
		//top
		brd.gfx.DrawRectangle(pos.x, pos.y, pos.x + brd.cellborderwidth + brd.cellsize.x, pos.y + brd.cellborderwidth, brd.brdclr.lastclicked);
	else
		//left
		brd.gfx.DrawRectangle(pos.x, pos.y, pos.x + brd.cellborderwidth, pos.y + brd.cellborderwidth + brd.cellsize.y, brd.brdclr.lastclicked);
}
```
AI.cpp<br/>
The function to find a cell that has 3 sides set already, so the AI can claim it for itself<br/>
```c++
int AI::FindCellWith3()
{
	unsigned int index = 0;
	for (; index < cellstate.size(); index++)
		if (cellstate[index] == 3)
			break;
	if (index != cellstate.size())
	{
		int temp = 0;
		if (cells[index].top)
			if (cells[index].left)
				if (!CheckRight(index)) //NOT LeftClick, its + not -
					temp += cells[index + 1].Update(leftclick.x, leftclick.y, Player2); //set right
				else
					temp += cells[index + cellcount.x].Update(topclick.x, topclick.y, Player2); //set bottom
			else
				temp += LeftClick(index); //set left
		else
			temp += TopClick(index); //set top
		return temp;
	}
	return -1;
}
```
As you might be able to tell there are a lot of pieces to make it all work, for the entire classes (and project) click [here](https://github.com/Conqueror933/Arcade/tree/master/Engine).
