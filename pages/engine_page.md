# Engine
---
Our little triangle in it's purest form.<br/>
```c++
	Model<float, 2> model;
	model.points.emplace_back(150.5f, 200.5f);
	model.points.emplace_back(250.3f, 250.6f);
	model.points.emplace_back(200.4f, 150.8f);
	Model<float, 2>::Triangle tri;
	tri.a = 0;
	tri.b = 1;
	tri.c = 2;
	model.indicies.emplace_back(tri);

	MoH.AddModel("triangle", model);

	ObjectToAdd<float, 2> obj;
	obj.transformation.loc = { 0.5f, 0.5f};
	obj.transformation.rot = { 1.f, 1.f };
	obj.transformation.sca = { 1.f, 1.f};
	obj.direction.vel = { 0.01f, -0.02f };
	obj.model_name = "triangle";
	obj.material_name = "red";
	AddObject(obj);
```	
---
This project once finished will include:
* Plug&Play style modules for
    * Graphics
    * Sound
    * Physics
    * AI
    * UserInput
    * UserInterface<br/>
 so you can test and play around with each component independent of the others.
* Fully to-the-user-non-visible asynchronous multithreading
    * This includes using **all** cores of a CPU at 100% load if so desired.
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

---

### The Vector class making infinite dimensions possible
Up to 5D we get the nice x,y,z,w,v syntax while wasting no memory and enabling interdimension compatibility.<br/>
While for 6D+ we have to sacrifice the syntax, using arr[dimensions_over_6] instead, it ensures that all dimensions are supported.<br/>
The specification for 5 dimensions: (shortened)<br/>
```c++
template<typename T> class Vector<T, 5> : public Vector<T, 4>
{
public:
	///BIG 3
	Vector<T, 5>() = default;
	Vector<T, 5>(T x, T y, T z, T w, T v) : Vector<T, 4>(x, y, z, w), v(v) {}
	Vector<T, 5>(Vector<T, 4> base, T v) : Vector<T, 4>(base), v(v) {}
	template<typename S>
	explicit Vector<T, 5>(const Vector<S, 5>& rhs) : Vector<T, 4>(rhs), v((T)rhs.v) {}
	Vector<T, 5>(const Vector<T, 5>& rhs) : Vector<T, 4>(rhs), v(rhs.v) {}
	Vector<T, 5>& operator=(const Vector<T, 5>& rhs)
	{
		v = rhs.v;
		Vector<T, 4>::operator=(rhs);
		return *this;
	}
	~Vector<T, 5>() = default;

	///Math
	// +
	inline Vector<T, 5> operator+(const Vector<T, 5>& rhs) const
	{
		return Vector<T, 5>{Vector<T, 4>::operator+(rhs), v + rhs.v};
	}
	inline Vector<T, 5>& operator+=(const Vector<T, 5>& rhs)
	{
		Vector<T, 4>::operator+=(rhs);
		v += rhs.v;
		return *this;
	}
	inline Vector<T, 5> operator+(const T& rhs) const
	{
		return Vector<T, 5>{Vector<T, 4>::operator+(rhs), v + rhs};
	}
	inline Vector<T, 5>& operator+=(const T& rhs)
	{
		Vector<T, 4>::operator+=(rhs);
		v += rhs;
		return *this;
	}
	// ... -,*,/ are all the same ...
	
	/// comparison
	inline bool operator==(const Vector<T, 5>& rhs) const
	{
		return Vector<T, 4>::operator==(rhs) && v == rhs.v;
	}
	inline bool operator!=(const Vector<T, 5>& rhs) const
	{
		return Vector<T, 4>::operator!=(rhs) || v != rhs.v;
	}
	inline T highestValue() const
	{
		auto temp = Vector<T, 4>::highestValue();
		return v > temp ? v : temp;
	}
	inline T lowestValue() const
	{
		auto temp = Vector<T, 4>::lowestValue();
		return v < temp ? v : temp;
	}
	/// Vector Operations
	inline T dot(const Vector<T, 5>& rhs) const
	{
		return Vector<T, 4>::dot(rhs) + v * rhs.v;
	}
	inline T getLengthSquared() const
	{
		return Vector<T, 5>::getLengthSquared() + v * v;
	}
	inline T getLength() const
	{
		return std::sqrt(getLengthSquared());
	}
	inline Vector<T, 5>& normalize()
	{
		Vector<T, 4>::normalize(); //please dont sqrt 5 times compiler
		v /= getLength();
		return *this;
	}
	inline Vector<T, 5> getNormalized() const
	{
		Vector<T, 5> norm = *this;
		norm.normalize();
		return norm;
	}
	inline T getDistanceSquared(const Vector<T, 5>& rhs) const
	{
		return Vector<T, 4>::getDistanceSquared(rhs) + (v - rhs.v) * (v - rhs.v); //theres gotta be a better way
	}
	inline T getDistance(const Vector<T, 5>& rhs) const
	{
		return std::sqrt(getDistanceSquared());
	}

	/// variable
	T v;
};
```
