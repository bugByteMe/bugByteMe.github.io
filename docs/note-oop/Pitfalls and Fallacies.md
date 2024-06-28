# Pitfalls and Fallacies

1. Anonymous namespace 作用域为当前文件（相当于全局静态变量）

   ```cpp
   #include<iostream>
   
   using namespace std;
   
   int x = 5;
   
   namespace{
       int x = 10;
   }
   
   int main(){
       cout << "x = " << ::x << endl;
       return 0;
   }
   
   // ::x = 10
   ```

2. Equivocal function overload

   值传递与引用传递

   ```cpp
   void f(int x){}
   void f(int &x)
   ```

   默认参数

   ```cpp
   double f(double x, double y = 1.0){return 1;}
   double f(double x){return 2;}
   ```

   隐式转换

   ```cpp
   int f(unsigned int x) {return x;}
   double f(double x) {return x;}
   int a = 5;
   f(5);
   ```

   上述所有情况，不会编译错误。**只有产生调用时**，才会编译错误。

3. const

   下面两种都是 typename的常量类型

   ```cpp
   const <typename> <var name>;
   <typename> const <var name>;
   ```

   指针与常量同时出现，遵循左结合规则

   ```cpp
   const int * -> (const int) *
   int * const -> (int *) const
   ```

4. new

   with initialization

   ```cpp
   p=new T(init list);
   ```

5. 不能在类中对数据成员初始化

6. freind

   访问权限对友元函数无效，友元函数放在private, public, protected都行

7. const member and static member

   普通常量只能在初始化列表初始化

   ```cpp
   class A
   {
   	const int a;
   	static const int b;
   };
   
   const int A::b = 5;
   ```

8. const member function

   注意const是参数表的一部分，const A* this

   ```cpp
   void A::f() const {}
   ```

9. Derived class

   派生类中的同名函数，不会导致父类的东西消失。仍可通过Base::f 访问

10. Virtual inheritance

   Diamond inheritance construct：

   ```cpp
   class A{};
   class B_1: virtual public A{}
   class B_2: virtual public A{}
   class C: public B_1, public B_2{}
   ```

   1. A

   2. B_1

   3. B_2

   4. C

5. Derived class and Base class

   ```cpp
   Derived d;
   // value
   Base b = d; // truncate
   // 下列仅在public继承时可以
   // reference
   Base &br = d;
   // pointer
   Base *pb = &d;
   ```

6. ++ overload

   ```cpp
   a++;
   operator ++(int){}
   
   ++a;
   operator ++(){}
   ```

7. 重载类型转换时，不需要写返回值类型

8. 静态成员函数，不可以为虚函数，构造函数不可以时虚函数

9. Virtual function in derived class must have the same signature as in the base class

   If only differ from return values, **Compile error**.

   If differ from arg list, normal name hiding.

10. 当类中至少有一个纯虚函数，那么这个类就是抽象类

11. dynamic cast, cast from base pointer/reference to derived pointer/reference

    Pointer failed: return null pointer.

    Reference failed: exception.

12. Only fully specialization is allowed in function templates.

13. Operators can not be overloaded:

    `?` `.` `::` `sizeof` `.*`

15. 只要vtable还在，动态绑定就在，关键在于vtable是谁的

    ```cpp
    Derived d;
    Base *b = &d;
    (*b).f(); // Call derived f
    ```

16. Default parameters in dynamic binding

    ```cpp
    class Base{
    public:
        int x;
        virtual void f(int x = 1)
        {
            cout << "Base" << x << endl;
        };
    };
    
    class Derived : public Base{
    public:
        void f(int x = 2){
            cout << "Derived" << x << endl;
        }
    };
    ```

17. dynamic cast 的源类型必须是polymorphic 否则编译错误

    然后才是运行时检查是否能转换。及时转换目标完全不是源类型的派生类，也只是运行时返回NULL指针。

    



