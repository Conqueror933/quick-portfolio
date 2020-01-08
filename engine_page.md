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


### The Vector class making infinite dimensions possible
Showing only the template class and the specialisation for 5 dimensions, the entire thing is 1000+ lines of code
```c++
template<typename T, size_t dimension> class Vector : Vector<T, 5>
{
public:
	///BIG 3
	Vector<T, dimension>() = default;
	Vector<T, dimension>(T x, T y, T z, T w, T v, std::array<T, dimension - 5> arr) : Vector<T, 5>(x,y,z,w,v), arr(arr) {}
	Vector<T, dimension>(std::array<T, dimension> arr) : Vector<T, 5>(arr[0], arr[1], arr[2], arr[3], arr[4]), arr({*(arr.data() + 5)}) {}
	Vector<T, dimension>(Vector<T, 5> base, std::array<T, dimension - 5> arr) : Vector<T, 5>(base), arr(arr) {}
	template<typename S>
	explicit Vector<T, dimension>(const Vector<S, dimension>& rhs) : Vector<T, 5>(rhs)
	{
		for (size_t i = 0; i < arr.size(); i++) arr[i] = (T)rhs.arr[i];
	}
	Vector<T, dimension>(const Vector<T, dimension>& rhs) : Vector<T, 5>(rhs)
	{
		for (size_t i = 0; i < arr.size(); i++) arr[i] = rhs.arr[i];
	}
	Vector<T, dimension>& operator=(const Vector<T, dimension>& rhs)
	{
		return *this = Vector<T, dimension>(rhs);
	}
	~Vector<T, dimension>() = default;

	///Math
	// +
	inline Vector<T, dimension> operator+(const Vector<T, dimension>& rhs) const
	{
		std::array<T, dimension - 5> temp = {};
		for (size_t i = 0; i < arr.size(); i++) temp = arr[i] + rhs.arr[i];
		return Vector<T, dimension>{Vector<T, 5>::operator+(rhs), temp};
	}
	inline Vector<T, dimension>& operator+=(const Vector<T, dimension>& rhs)
	{
		Vector<T, 5>::operator+=(rhs);
		for (size_t i = 0; i < arr.size(); i++) arr[i] += rhs.arr[i];
		return *this;
	}
	inline Vector<T, dimension> operator+(const T& rhs) const
	{
		std::array<T, dimension - 5> temp = {};
		for (size_t i = 0; i < arr.size(); i++) temp = arr[i] + rhs;
		return Vector<T, dimension>{Vector<T, 5>::operator+(rhs), temp};
	}
	inline Vector<T, dimension>& operator+=(const T& rhs)
	{
		Vector<T, 5>::operator+=(rhs);
		for (size_t i = 0; i < arr.size(); i++) arr[i] += rhs;
		return *this;
	}
	// -
	inline Vector<T, dimension> operator-(const Vector<T, dimension>& rhs) const
	{
		std::array<T, dimension - 5> temp = {};
		for (size_t i = 0; i < arr.size(); i++) temp = arr[i] - rhs.arr[i];
		return Vector<T, dimension>{Vector<T, 5>::operator-(rhs), temp};
	}
	inline Vector<T, dimension>& operator-=(const Vector<T, dimension>& rhs)
	{
		Vector<T, 5>::operator-=(rhs);
		for (size_t i = 0; i < arr.size(); i++) arr[i] -= rhs.arr[i];
		return *this;
	}
	inline Vector<T, dimension> operator-(const T& rhs) const
	{
		std::array<T, dimension - 5> temp = {};
		for (size_t i = 0; i < arr.size(); i++) temp = arr[i] - rhs;
		return Vector<T, dimension>{Vector<T, 5>::operator-(rhs), temp};
	}
	inline Vector<T, dimension>& operator-=(const T& rhs)
	{
		Vector<T, 5>::operator-=(rhs);
		for (size_t i = 0; i < arr.size(); i++) arr[i] -= rhs;
		return *this;
	}
	// *
	inline Vector<T, dimension> operator*(const Vector<T, dimension>& rhs) const
	{
		std::array<T, dimension * 5> temp = {};
		for (size_t i = 0; i < arr.size(); i++) temp = arr[i] * rhs.arr[i];
		return Vector<T, dimension>{Vector<T, 5>::operator*(rhs), temp};
	}
	inline Vector<T, dimension>& operator*=(const Vector<T, dimension>& rhs)
	{
		Vector<T, 5>::operator*=(rhs);
		for (size_t i = 0; i < arr.size(); i++) arr[i] *= rhs.arr[i];
		return *this;
	}
	inline Vector<T, dimension> operator*(const T& rhs) const
	{
		std::array<T, dimension * 5> temp = {};
		for (size_t i = 0; i < arr.size(); i++) temp = arr[i] * rhs;
		return Vector<T, dimension>{Vector<T, 5>::operator*(rhs), temp};
	}
	inline Vector<T, dimension>& operator*=(const T& rhs)
	{
		Vector<T, 5>::operator*=(rhs);
		for (size_t i = 0; i < arr.size(); i++) arr[i] *= rhs;
		return *this;
	}
	// /
	inline Vector<T, dimension> operator/(const Vector<T, dimension>& rhs) const
	{
		std::array<T, dimension / 5> temp = {};
		for (size_t i = 0; i < arr.size(); i++) temp = arr[i] / rhs.arr[i];
		return Vector<T, dimension>{Vector<T, 5>::operator/(rhs), temp};
	}
	inline Vector<T, dimension>& operator/=(const Vector<T, dimension>& rhs)
	{
		Vector<T, 5>::operator/=(rhs);
		for (size_t i = 0; i < arr.size(); i++) arr[i] /= rhs.arr[i];
		return *this;
	}
	inline Vector<T, dimension> operator/(const T& rhs) const
	{
		std::array<T, dimension / 5> temp = {};
		for (size_t i = 0; i < arr.size(); i++) temp = arr[i] / rhs;
		return Vector<T, dimension>{Vector<T, 5>::operator/(rhs), temp};
	}
	inline Vector<T, dimension>& operator/=(const T& rhs)
	{
		Vector<T, 5>::operator/=(rhs);
		for (size_t i = 0; i < arr.size(); i++) arr[i] /= rhs;
		return *this;
	}
	/// comparison
	inline bool operator==(const Vector<T, dimension>& rhs) const
	{
		for (size_t i = 0; i < arr.size(); i++) if (arr[i] != rhs.arr[i]) return false;
		return Vector<T, 5>::operator==(rhs);
	}
	inline bool operator!=(const Vector<T, dimension>& rhs) const
	{
		for (size_t i = 0; i < arr.size(); i++) if (arr[i] == rhs.arr[i]) return false;
		return Vector<T, 5>::operator!=(rhs);
	}
	inline T highestValue() const
	{
		auto temp = Vector<T, 5>::highestValue();
		for (size_t i = 0; i < arr.size(); i++) if(arr[i] > temp) temp = arr[i];
		return temp;
	}
	inline T lowestValue() const
	{
		auto temp = Vector<T, 5>::lowestValue();
		for (size_t i = 0; i < arr.size(); i++) if (arr[i] < temp) temp = arr[i];
		return temp;
	}
	/// Vector Operations
	inline T dot(const Vector<T, dimension>& rhs) const
	{
		auto temp = Vector<T, 5>::dot(rhs);
		for (size_t i = 0; i < arr.size(); i++) temp += arr[i] * rhs.arr[i];
		return temp;
	}
	inline T getLengthSquared() const
	{
		auto temp = Vector<T, 5>::getLengthSquared();
		for (size_t i = 0; i < arr.size(); i++) temp += arr[i] * arr[i];
		return temp;
	}
	inline T getLength() const
	{
		return std::sqrt(getLengthSquared());
	}
	inline Vector<T, dimension>& normalize()
	{
		Vector<T, 5>::normalize(); //please dont sqrt dimension times compiler
		for (size_t i = 0; i < arr.size(); i++) arr[i] /= getLength();
		return *this;
	}
	inline Vector<T, dimension> getNormalized() const
	{
		Vector<T, dimension> norm = *this;
		norm.normalize();
		return norm;
	}
	inline T getDistanceSquared(const Vector<T, dimension>& rhs) const
	{
		auto temp = Vector<T, 5>::getDistanceSquared(rhs);
		for (size_t i = 0; i < arr.size(); i++) temp += (arr[i] - rhs.arr[i]) * (arr[i] - rhs.arr[i]); //theres gotta be a better way
		return temp;
	}
	inline T getDistance(const Vector<T, dimension>& rhs) const
	{
		return std::sqrt(getDistanceSquared());
	}

	// arriable
	std::array<T, dimension - 5> arr;
};

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
		return *this = Vector<T, 5>(rhs);
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
	// -
	inline Vector<T, 5> operator-(const Vector<T, 5>& rhs) const
	{
		return Vector<T, 5>{Vector<T, 4>::operator-(rhs), v - rhs.v};
	}
	inline Vector<T, 5>& operator-=(const Vector<T, 5>& rhs)
	{
		Vector<T, 4>::operator-=(rhs);
		v -= rhs.v;
		return *this;
	}
	inline Vector<T, 5> operator-(const T& rhs) const
	{
		return Vector<T, 5>{Vector<T, 4>::operator-(rhs), v - rhs};
	}
	inline Vector<T, 5>& operator-=(const T& rhs)
	{
		Vector<T, 4>::operator-=(rhs);
		v -= rhs;
		return *this;
	}
	// *
	inline Vector<T, 5> operator*(const Vector<T, 5>& rhs) const
	{
		return Vector<T, 5>{Vector<T, 4>::operator*(rhs), v * rhs.v};
	}
	inline Vector<T, 5>& operator*=(const Vector<T, 5>& rhs)
	{
		Vector<T, 4>::operator*=(rhs);
		v *= rhs.v;
		return *this;
	}
	inline Vector<T, 5> operator*(const T& rhs) const
	{
		return Vector<T, 5>{Vector<T, 4>::operator*(rhs), v * rhs};
	}
	inline Vector<T, 5>& operator*=(const T& rhs)
	{
		Vector<T, 4>::operator*=(rhs);
		v *= rhs;
		return *this;
	}
	// /
	inline Vector<T, 5> operator/(const Vector<T, 5>& rhs) const
	{
		return Vector<T, 5>{Vector<T, 4>::operator/(rhs), v / rhs.v};
	}
	inline Vector<T, 5>& operator/=(const Vector<T, 5>& rhs)
	{
		Vector<T, 4>::operator/=(rhs);
		v /= rhs.v;
		return *this;
	}
	inline Vector<T, 5> operator/(const T& rhs) const
	{
		return Vector<T, 5>{Vector<T, 4>::operator/(rhs), v / rhs};
	}
	inline Vector<T, 5>& operator/=(const T& rhs)
	{
		Vector<T, 4>::operator/=(rhs);
		v /= rhs;
		return *this;
	}
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
