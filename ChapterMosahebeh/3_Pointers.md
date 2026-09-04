## 3. Pointers
An small example of a *pointer* and a *reference*:
```c++
include <iostream>
#include <string>

int main(int argc, char* argv[])
{
    int a = 5;
    // a as integer is saved in an address. We access this adress with &a.
    // A pointer always points to an address. The address of a variable.
    // "Pointer points to an address" means the pointer stores an adress.
    // So! We can say a pointer variables always stores an address on memory in it.
    // In below line we declare pointer p. We should set this pointer equal to an address
    // in memory. We access the address of variable a with &a:
    int* p = &a;
    std::cout<<"p="<< p << std::endl;
    std::cout<<"*p="<< *p << std::endl;
   
    int& c = a;
    std::cout<<"c="<< c << std::endl;
    std::cout<<"&c="<< &c << std::endl;

    return 0;
}
```
Prints:
```
p=0x7ffedfd57d9c
*p=5
c=5
&c=0x7ffedfd57d9c
```
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

An small example of `std::unique_ptr`:
```c++
#include <memory>
#include <iostream>
#include <string>

class Dog
{
    public:
    Dog()
    {
        std::cout << "Class Dog constructor" << std::endl;
    }
    ~Dog()
    {
        std::cout << "Class Dog destructor" << std::endl;
    }

    void bark()
    {
        std::cout << "Haap! Haap!" << std::endl;
    }
};

int main(int argc, char* argv[])
{
    std::unique_ptr dog = std::make_unique<Dog>();
    dog->bark();

    return 0;
}
```
[Run in Compiler Explorer](https://godbolt.org/z/39M1qn1jv)

Prints:
```
Class Dog constructor
Haap! Haap!
Class Dog destructor
```
An small example of `std::shared_ptr` (Notice how the number of owners change when we pass `shared_ptr` by value (pass a copy):
```c++
#include <iostream>
#include <string>
#include <memory>

void funA(std::shared_ptr<int> in)
{
    // Since funA takes a copy of smart pointer, funA will be one of the owners
    // of the object that the input argument points to.
    // std::shared_ptr<T>::use_count "Returns the number of different shared_ptr objects managing the current object":
    std::cout<<"Inside funA: in.use_count()="<< in.use_count() << std::endl;  
}

void funB(const std::shared_ptr<int>& in)
{
    std::cout<<"Inside funB: in.use_count()="<< in.use_count() << std::endl;  
}

int main(int argc, char* argv[])
{
    int a = 5;
    std::shared_ptr<int> sharedPtr = std::make_shared<int>(a);
   
    // Because funA takes a copy of shared_ptr as input argument, it (funA) will be one of the
    // owners of the object pointed by shared_ptr. In this case, inside funA we expect the number
    // of owners to be increased by one:
    std::cout<<"Before funA: sharedPtr.use_count()="<< sharedPtr.use_count() << std::endl;  
    funA(sharedPtr);
    std::cout<<"After funA: sharedPtr.use_count()="<< sharedPtr.use_count() << std::endl;
    
    // Because funB takes a reference to shared_ptr, it (funB) doesn't add to the number of owners:
    funB(sharedPtr);
    std::cout<<"After funB: sharedPtr.use_count()="<< sharedPtr.use_count() << std::endl;

    return 0;
}
```
[Run in Compiler Explorer](https://godbolt.org/z/bfeon7Mqo)

This prints:
```
Before funA: sharedPtr.use_count()=1
Inside funA: in.use_count()=2
After funA: sharedPtr.use_count()=1
Inside funB: in.use_count()=1
After funB: sharedPtr.use_count()=1
```

#### Pointer vs reference
* Reference refers to an object. It must be initialized by referring to an object and after that, it can not refer to another object.
* Pointer can point to an object and then it can be modified to point to another object, or point to nothing, `nullptr` or `NULL`.
We use reference anytime possible. And we use pointer anytime we have to.
