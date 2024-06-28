# Exception

#### Raise an exception

```cpp
throw <<something>> // something can be any thing
```

Function will **stop** here, 

clean up the stack (**call destructors** of all local variables), 

and make its caller responsible

A good article of detailed demonstration of how exception works! [click](https://www.codeproject.com/Articles/2126/How-a-C-compiler-implements-exception-handling#figure4)

#### Handle an exception

* **Doesn't care**

  Function will **stop** here, 

  clean up the stack (**call destructors** of all local variables), 

  and propagate the exception to caller

  ```cpp
  void fun(){
  	vector<int> v[5];
  	v[40] = 1; -> exception
  	// return to caller. The following code not touched.
  	v[1] = 2;
  }
  ```

* **Cares deeply**

  ```cpp
  try{
   // ...
  }catch(ExceptionType &e){
  	// Handle the exception here
  }
  // Exception handled. The following code will be executed normally.
  cout << "Hello" << endl;
  ```

* **Mildly interested**

  ```cpp
  try{
   // ...
  }catch(ExceptionType &e){
  	// Do something
  	throw; // Throw this exception again to caller.
  }
  ```

* **Catch any type**

  ```cpp
  try{
  	// ...
  }catch(...){
  
  }
  ```

* **Selection from multiple handlers**

  ```cpp
  try{
  	// ...
  }catch(Type1 &e){
  
  }catch(Type2 &e){
  
  }catch(...){
  
  }
  ```

  Each handler is checked one by one.

  If:

  * exact match **or**
  * Can apply base class conversion (pointer or ref) **or**
  * Catch all (...)

  The handler is matched

#### Exception with 'new'

* If 'new' fails, no memory is allocated

* If there is an exception after 'new', the allocated memory will **not be freed**.

  **Solution: RAII** (Resource allocation is initialization)

  Wrap the allocation inside initialization of an object.

  Because local objects' destructors will be called automatically when exception !

#### Exception with Ctor and Dtor

* **Exception in Ctor**

  Dtors will not be called automatically.

  Dynamic allocated resource will not be freed automatically.

  Solution: RAII (smart pointers)

* **Exceptions in Dtor**

  If Dtor is called because of exception (when clean up the stack), exceptions in Dtor will trigger `std::terminate`.

  **Don't throw** exceptions in Dtors.
