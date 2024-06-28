```cpp
class A{
    public:
    virtual void f(){ cout << "A::f()"; }
    virtual void f(int x){ cout << "A::f(int)"; }
};

class B : public A{
    public:
    void f() override { cout << "B::f()"; }
};

A *a = new A();
B *b = reinterpret_cast<B*>(a);
b->f(1);
```

