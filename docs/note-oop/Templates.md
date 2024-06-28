# Templates:yum:

#### Ambiguous Function Overload

```cpp
void foo(int i) {}
void foo(double d){}
foo(10L); // Compile error. Ambiguous! long can convert to both i and d implicitly
```

#### Template Instantiation

Compilers help you to overload a function.

Only when the template function/class is called/instantiated with specialized types, will it be created to a specialized type of code by compilers.

```cpp
template <class T>
void print(T a, T b){
	cout << a << " " << b << endl;
}

void print(string a, string b){
    cout << a << " " << b << endl;
}

int a = 1, b = 2, c = 3;
float d = 1.0, e = 2.0;
print(a, b); // Create a int type my_swap
print(b, c); // Use the already created int type my_swap
print(d, e); // Create a float type my_swap

print(a, d); // Compile error. Implicit type conversion is not allowed in templates.
print<double>(a, d); // Good!

print("a", "b"); // Call the normal funtion!
```

* **Which function to use?** :innocent:

  1. Compilers will **exact match** for normal function first (no implicit type conversion).

  2. Then match for templates.

  3. Then match for normal function. (with implicit type conversion).

  example:

  ```cpp
  template <class T1, class T2>
  void foo(T1 a, T2 b){
      cout << "foo(T1, T2)" << endl;
  }
  
  void foo(int a, double b){
      cout << "foo(int, double)" << endl;
  }
  
  int main(){
      foo(1, 1.0);
      foo(1, 1.0f);
  }
  
  /* output:
      foo(int, double)
      foo(T1, T2)
  */
  ```

#### None-type Template Parameters

example

```cpp
template<class T, int N = 100>
class Array{
public:
	int size() const {return N;}
	// ... ...
};

Array<int> arr1;
Array<int, 50> arr2; // Not the same type as arr1
```

#### Template Member Function

```cpp
template <class T>
class A{
public:
    T val;
    A(T v): val(v){}
    template <class U> A(const A<U>& other) : val(other.val){}
    // A(const A& other) : val(other.val) {} Error ! (*this) is A<double>, (other) is A<int>
};

int main(){
    A<int> a(2);
    A<double> b = a;
}
```

#### Template & Inheritance :sleeping:

* Template class can inherits from non-template class.

* Template class can inherits from template class.

  ```cpp
  template <class C>
  class A : public
    B<C> { /* ... */ } // Here B's template parameter can be a fake type.
  ```

* Non-template class can inherits from instantiated template class.

  ```cpp
  class A : public
    B<int> { /* ... */ } // Here B's template parameter is a real type.
  ```

#### Recurring Template

manually implement virtual function :joy:, just work as virtual function.

There are no vptr-table anymore, more efficient than virtual functions!

```cpp
template <class T>
struct Base{
    void interface(){
        static_cast<T*>(this)->implementation_a();
        static_cast<T*>(this)->implementation_b();
    }
};

struct Derived1 : Base<Derived1>{
    void implementation_a(){
        std::cout << "Derived1 implementation_a\n";
    }
    void implementation_b(){
        std::cout << "Derived1 implementation_b\n";
    }
};

struct Derived2 : Base<Derived2>{
    void implementation_a(){
        std::cout << "Derived2 implementation_a\n";
    }
    void implementation_b(){
        std::cout << "Derived2 implementation_b\n";
    }
};

template <class T>
void foo(Base<T>& b){
    b.interface();
}

int main(){
    Derived1 d1;
    Derived2 d2;
    foo(d1);
    foo(d2);
    return 0;
}
```



