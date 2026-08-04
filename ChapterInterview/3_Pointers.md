## 3. Pointers
* *Dangling pointer:* When the memory is released but the pointer still exists.
* *Memory leak:* When the pointer doesn't exist anymore, mostly because it goes out of scope, but the memory is still allocated.

Example for memory leak:
```c++
{
  MyClass* b = new MyClass();
}
```
As soon as the pointer `b` goes out of scope, it doesn't exist anymore but the memory is still allocated on heap and nobody can delete it.

One of the first and most basic solutions is the class `std::auto_ptr`. `std::auto_ptr` is a wrapper around the pointer. It gets the pointer as its input argument and at its destructor it makes sure that the pointer gets `delete`d. In other words, `std::auto_ptr` solves the problem of memory leak. **`std::auto_ptr` is obsolete and other smart pointers are encouraged to use!**

*Smart pointer* itself is not a pointer. It is a class whose object *mimics* the behavior of the pointer. Its input parameter is the pointer. Smart pointer handles the problem of memory leak by `delete`-ing the pointer at destruction. The three most common smart pointers in C++ are:
* `std::unique_ptr`: Use this by default.
* `std::shared_ptr`: Use if the ownership of pointer has to be shared.
* `std::weak_ptr`: ???
