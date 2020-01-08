#Engine
---
This project once finished will include:
* Plug&Play style modules for
 * Graphics
 * Sound
 * Physics
 * AI
 * UserInput
 * UserInterface
 so you can test and play around with each component independent of the others.
* Fully to-the-user-non-visible asynchronous multithreading
 * This includes using all cores of a CPU at 100% load if so desired.
 * Allowing for multithreading in the modules for load/save operation or whatever else it may be desired for. Using all cores that are free at the moment
 * An easy to use Userinterface that doesn't know about the multithreading, everything happens behind the scenes.
* Support up to infinite dimensions (1D, 2D, 3D, 4D, 5D, ...)

* Physics Module that works on Atom-level.
 * Objects will not be coded, but build! inside of the engine.
  * A Car for example won't be a `class car` but all the nuts and bolts that are actually required to make a car function. And it will function.
 * Objects will shatter based on their material and form.

* 4D Graphics
* Simple easy to use "It just works" UserInterface with as few buttons as possible.
* Advanced UserInterface in C++ for unlimited power.
