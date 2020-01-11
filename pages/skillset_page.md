### In-depth:
- Syntax + Basics of C++ 00/11/14/17/20
	- included for completions sake
- everything std::
	- included for completions sake
- Unions
	- and other types of low level memory management [example](#Union)
- Pointer magic
	- Pointer arithmetics, Ownership-management with pointers [example](#Pointer)
- Templates
	- from using them to making them [example](/pages/engine_page)
- Run & Compile -time polymorphism
	- everything from dynamic dispatch, virtual function tables to template meta programming
- Design Patterns
	- from Singleton and Update Patterns to Data locality and Object Pools
- Data Structures
	- everything about O(n) and when to ignore it
- Algorithms
	- yes
- Debugging
	- search and fix
- Template Debugging
	- able to read and understand page-long template error messages
- Tests & Performance
	- from unit tests to finding performance bottlenecks, never assume anything, test it, try it, break it, fix it.
- Synchronous & Asynchronous program-flow
	- all things multithreading, advantages, disadvantages and most of all pitfalls
- Networking
	- while not used in practice yet, I have theoretical knowledge and know some pitfalls
- User Interface Design
	- the Do's and Dont's of what makes and breaks a UI
- 2D & 3D Graphics (programming)
	- Vector and Matrix math, Backface-culling, Transformations, ...
- Physics
	- Rebound angles, drag, impact-force, ...
- AI
	- State-Manager, Decision making
- Logistics
	- 600+ hours in [factorio](https://factorio.com/) which essentially is a logistics and automation simulator
- Pathfinding
	- A-Star, Grid based and the unrealthing

### Soft-Skills:
- Leadership ability
- Teamplayer & Lone wolf alike
	- I can work in and with a team, assign roles or just fulfill mine, but I can also do whatever is needed on my own
- Goal-oriented way of working
- Always calm and collected
- Extremly fast and thorough learner
- Phenomenal analyzing skills
- fluent english/german
- Drivers license

#### <a name="Union"></a> Union
```c++
template<class Union>
class StringSwitch
{
public:
	void AddEntry(std::string s, Union u)
	{
		assert(map.find(s) == map.end());
		map.emplace(s, u);
	}
	void ChangeEntry(std::string s, Union u)
	{
		assert(!(map.find(s) == map.end()));
		map.insert_or_assign(s, u);
	}
	Union GetValue(std::string s)
	{
		assert(!(map.find(s) == map.end()));
		return map.at(s);
	}
	void Flush()
	{
		map.clear();
	}

private:
	std::unordered_map<std::string, Union> map;
};

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
