# Copy Constructor

Only necessary to be written manually when there exists member of pointer type.

#### When Copy Ctors are called

* Initialize from other object

  ```cpp
  A a(b);
  A a = b; // equivalent to A a(b)
  ```

* Function call by value

  ```cpp
  void f(A a);
  ```

* Function return

  ```cpp
  A f(){
  	A a;
  	return a;
  }
  
  A f2(){
  	return A();
  }
  A b = f(); // Copy Ctor is called
  A c = f2(); // Copy Ctor not called
  ```

#### Forbid Copy Ctor

```cpp
A(const A& other) = delete;
```

