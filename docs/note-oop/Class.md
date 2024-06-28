# Class

#### Resolvers (::)

Pure '::' resolves to global

```cpp
void f(){} // Global f();

class A{
public:
	void f(){
		::f(); // Call global f();
	}
};
```

#### Ctors

If a ctor with argument is defined, default ctor will not be generated automatically.

```cpp
class A{
public:
	A(int x){}
};

A a; // Compile error. No default ctor
```

#### Constant object

Constant object can only call constant functions:

```cpp
void func() const ;
```

#### Constant members

Has to be initialized in the ctor's list.

#### Compile time constants

Needs to be **static and const**.

static const member **needs not** be declared outside the class and can be assigned inside the class.

```cpp
class A{
    static const int size = 10;
    int arr[size];
};
```

#### Inline

inline in c++ means multiple definition is allowed (**in linking** multiple compile units).

multiple definition still **not allowed in compiling.**

Put inline functions' **body** in the header file.

