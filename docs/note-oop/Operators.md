# Operator Overload

#### Anonymous function

```cpp
[vars...](args...){ // function body }
```

`vars` are contextual variables (temporary variables).

`args` are argument list of the fuction.

e.g.

```cpp
int a = 3;
auto f = [a](){cout << a << endl;}
f();
```

Actually **a class with `operator ()`  (functor) overloaded.**

`vars` is stored as **fields** in this class.

An example:

```cpp
class F{
private:
	int a;
public:
	F() : a(a) {}
	const int operator(const int &x) const {
		return x * a;
	}
};
/*
* F can be received as type : function<int(const int&)>
* #include<functional>
*/
```

#### Type Conversion Overload

* **Auto** type conversion

  When corresponding ctor is defined (implicitly).

  ```cpp
  class A{
  public:
  	A(B &b){}
      explicit A(C &c){}
  };
  
  void f(A _a){}
  
  A a;
  B b;
  a = b; // OK
  a = c; // Compile Error!
  a = A(c); // OK
  f(b); // OK
  ```

* Conversion Operator

  ```cpp
  class A{
  public:
  	operator double() const{
  		return 1.0;
  	}
  };
  
  double d = 1.3 * r; // OK r => double
  double d = r * 1.3; // OK
  ```

* Ambigous conversion

  ```cpp
  class B;
  
  class A{
  public:
      A(const B &b){} // B => A
  };
  
  class B{
  public:
      operator A(){} // B => A. Tow implements. Compile error!
  };
  ```

  
