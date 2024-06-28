# Composition and Inheritance

#### Composition

* $A$ has $B,C$.

  $A$ is composed of $B$ and $C$ 

  $B,C$ is the embedded object of $A$, usually made **private**.

* **Ctors**

  ```cpp
  class A{
  	B b;
      C c;
  public:
      A(args...) : b(...), c(...) {}
  };
  ```

  If sub-constructors not exist in init-list, default ctor will be called.

#### Inheritance

* $B$ is a $A$

  Accessibility of members in $A$

  <img src="Composition and Inheritance.assets/image-20240516134426785.png" alt="image-20240516134426785" style="zoom:33%;" />

  Accessibility inheritance.

  <img src="Composition and Inheritance.assets/image-20240516135701362.png" alt="image-20240516135701362" style="zoom:33%;" />

* **Ctors**

  ```cpp
  class B : public A{
  public:
  	B(...) : A(...) {}
  };
  ```

  Base class always **constructed first**.

  **Order:**

  Base class ctors -> members (w.r.t. members' definition order, not order in init-list) -> My ctor body. 

* **Call base class functions**

  **Only public and protected** base class functions can be called.

  ```cpp
  A::func();
  ```

  ```cpp
  // Compile error
  class A{
  public:
      A(){cout << "A::A()" << endl;}
      int f1(){}
  private:
      int f2(){}
  };
  
  class B : public A{
  public:
      int f1() {A::f2();}
  };
  ```

  If you redefine a member function in the derived class, all the **other overloaded functions** in the base class are **inaccessible**.

  ```cpp
  class A{
  public:
      A(){cout << "A::A()" << endl;}
      int f1(){}
      int f1(int x){}
  };
  
  class B : public A{
  public:
      void f1() {A::f1(1);}
  };
  
  int main(){
      B b;
      b.f(2); // Compile error !!!
  }
  ```

* **Friends**

  Grant access to a function or class to access my members.

  ```cpp
  class A{
      int x;
      friend int get_x(A a);
      friend class B;
  };
  
  class B{
  public:
      void f(A a){ a.x = 1; }
  };
  
  int get_x(A a){ return a.x; }
  ```

* **Virtual Inheritance**

  ```cpp
  class A{
  public:
      A(int x) : a(x) { cout << "A:A(int)" << x << endl;}
  protected:
      int a;
  };
  
  class B : virtual public A{
  public:
      B() : A(5) /*Not called*/ { cout << "B:B()" << A::a << endl;}
  };
  
  class C : virtual public A{
  public:
      C() : A(10) /*Not called*/ { cout << "C:C()" << A::a << endl;}
  };
  
  class D : public A{
  public:
      D() : A(15) { cout << "D:D()" << endl;}
  };
  
  class E : public B, public C, public D{
  public:
      E() : A(25), B(), C(), D()  { cout << "E:E()" << B::a << " " << C::a << " " << D::a << endl;}
  };
  int main(){
      E e;
      return 0;
  }
  /*B,C,E shares the same A*/
  /*D has its own A*/
  /*
  A:A(int)25
  B:B()25
  C:C()25
  A:A(int)15
  D:D()
  E:E()25 25 15
  */
  ```

  