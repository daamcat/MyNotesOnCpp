## Questions and answers:

### Identifier
An "identifier" is simply the name given to something in a program. For example in below code, `age` is an identifier.
```c++
int age = 20;
```
Therefore, variable names, class names, function names, etc, are all *identifier*s.

### Namespace
Imagine an application using several libraries. If there are global variables with same name in different libraries, *name collision* will happen. `namespace` declares a named scope for each library and eliminates *name collision*.

In below code, *namespace identifier* is `blah::blih::bluh`:
```c++
namespace blah::blih::bluh
{

} 
```

### Scope resolution operator `::`
We use *scope resolution identifier* (`::`) to specify the *scope* to which an *identifier* belongs. We use *scope resolution identifier* (`::`) to show what we want from which *scope*. 

Simple example:
```c++
#include <iostream>
#include <string>

int x = 10;

int main(int argc, char* argv[])
{
    int x = 20;
    std::cout << "x = " << x << std::endl; // Prints x = 20
    std::cout << "::x = " << ::x << std::endl; // Prints x = 10
   
    return 0;
}
```
### Qualifier
A *qualifier* comes with *identifier* and adds extra information/limitation to the variable. Examples:
```c++
const int age = 10;
volatile int x = 30;
mutable bool enabled = false;
```
> Common *qualifier*s:
> * `const`: The value can not be modified.
> * `volatile`: The value may change unexpectedly.
> * `mutable`: Allows data member to be modified even inside a `const` object!

`volatile` tells the compiler: this variable can change anytime by something unexpected, for example some sensory info from hardware. So check the value before using it. Question: Doesn't the compiler do so for any normal variable? The answer seems to be negative! If the code is like:
```c++
int x = 5;
<some code without variable x>
std::cout << x << std::endl;
```
The compiler doesn't bother itself to make sure that x is still 5 and prints 5 at output. But if we write `volatile int x = 5;`, then the compiler checks the value every time the variable is used. So with `volatile` we tell compiler: Be careful of this variable and check it every time before using it, because it can change without you knowing it!

One of the major applications of `volatile` is when a function is running in another thread, for example using `QtConcurrent::run()` and we want to be able to cancel it anytime from main thread. In this scenario, `volatile bool* canceled` is passed as input argument to the function which is going to be run by `QtConcurrent::run()`. For example:
```c++
int32_t ClassName::convertImages(const std::string& path, volatile bool* canceled)
{
 ...
 if (*canceled)
 {
   return 1;
 }
 ...
}

volatile bool canceled {false};
std::string path {"path/to/folder"};
int32_t error = 0;

QFutureWatcher<int32_t> futureWatcher;
QObject::connect(&futureWatcher, &QFutureWatcher<int32_t>::finished, [&]()
  {
    error = futureWatcher.result();
  }
);

QFuture<int32_t> future = QtConcurrent::run(this, &ClassName::convertImages, path, &canceled);
futureWatcher.setFuture(future);
```
Now anytime we want to cancel `ClassName::convertImages`, we just set `canceled` to `true`. Using *qualifier* `volatile` here doesn't let the compiler to apply optimization on the variable, since optimization can cause the compiler to neglect the change of variable and assume it as a constant.

For more detailed explanation on using `volatile` for canceling a thread, refer to the book *Advanced Qt Programming* by *Mark Summerfield*, Chapter 7: *Threading with QtConcurrent*.

### Keyword `inline`
The keyword `inline` has two major applications:
1. Compilation optimization: Using `inline` for this purpose is not necessary in modern C++ anymore because the compiler itself decides whether to `inline` a function for optimization.
2. Solving the problem of "multiple definitions".

In C++ we can not have multiple function *definition*s for one function *declaration*. This is called "One Definition Rule" or ODR. Now imagine that we have a header file with a function in it:
```c++
int add (int a , int b)
{
  return a+b;
}
```
If we `#include` this header in multiple source files and somehow it ends up adding this header multiple times, the compiler will complain that we have multiple definitions. Now if we add the keyword/specifier `inline`, the compiler will accept that there is only one definition of function.

Normally we put the definition of `inline` function in header file as well.

### Keyword `static`
#### `static` member variable
Is a very interesting feature of a C++ class! A `static` member variable belongs to the class and not to the object. Two applications:
1. Counting objects of class.
2. Shared configuration for all objects of class.

Example for counting the number of objects. Notice that with each creation of an object, one is added to static variable `count`:
```c++
#include <iostream>
#include <string>

class Car
{
    public:
    Car()
    {
        count++;
    }
    int getCount() const // This doesn't make sense. "count" is about class not object.
    {
        return count;
    }

    static int getCountStatic() // Correct way to get static member variable.
    {
        return count;
    }
    private:
    static int count;
};

int Car::count = 0;

int main(int argc, char* argv[])
{
    Car vw;
    Car bmw;
    Car opel;

    std::cout << "bmw.getCount() = " << bmw.getCount() << std::endl; // Prints: bmw.getCount() = 3
    std::cout << "Car::getCountStatic() = " << Car::getCountStatic() << std::endl; // Prints: Car::getCountStatic() = 3

    return 0;
}
```
Example for shared configuration for all objects:
```c++
#include <iostream>
#include <string>

class Car
{
    public:
    Car(int productionExpense)
    {
        m_productionExpense = productionExpense;
    }
    int getPrice() const
    {
        return m_productionExpense + m_tax;
    }

    static void setTax(int tax)
    {
        m_tax = tax;
    }

    private:
    int m_productionExpense = 0;
    static int m_tax;
};

int Car::m_tax = 30;

int main(int argc, char* argv[])
{
    Car vw(100);
    Car bmw(120);
    Car opel(60);

    std::cout << "vw.getPrice() = " << vw.getPrice() << std::endl; // Prints: vw.getPrice() = 130
    std::cout << "bmw.getPrice() = " << bmw.getPrice() << std::endl; // Prints: bmw.getPrice() = 150
    Car::setTax(40);
    std::cout << "vw.getPrice() = " << vw.getPrice() << std::endl; // Prints: vw.getPrice() = 140
    std::cout << "bmw.getPrice() = " << bmw.getPrice() << std::endl; // Prints: bmw.getPrice() = 160

    return 0;
}
```

### Temporary variable
*Temporary variable*s are created when the compiler needs an unnamed intermediate object during execution of a function. When does the compiler need intermediate object?
* Type conversion
* Input parameter
* Return value
* ...

### Type size
When we say:
```
Blah y = Blah x + 1
```
this `1` here does not necessarily mean an integer. It depends how we interpret it. How we `cast` it.

The function `sizeof()` is a C++ operator which gives us the size of type objects in bytes. For example:
```
std::cout << "sizeof(int32_t): " << sizeof(int32_t) << ", sizeof(int64_t): " << sizeof(int64_t) << std::endl;
```
prints: `sizeof(int32_t): 4, sizeof(int64_t): 8`.

The idea is to measure the size of a type, for example `int` without using the operator `sizeof()`. 
```c++
#include <iostream>
#include <string>
#include <cstdint>

struct Car
{
    std::string name;
    int32_t price;
};

int main(int argc, char* argv[])
{
    int32_t* i;
    int32_t* j = i + 1;
    std::cout << "int32_t size: " << reinterpret_cast<std::uintptr_t>(j) - reinterpret_cast<std::uintptr_t>(i) << std::endl;
    std::cout << "sizeof(int32_t): " << sizeof(int32_t) << ", sizeof(int64_t): " << sizeof(int64_t) << std::endl; 
    std::cout << "sizeof(Car): " << sizeof(Car) << std::endl; 
    return 0;
}
```
prints:
```
int32_t size: 4
sizeof(int32_t): 4, sizeof(int64_t): 8
sizeof(Car): 40
```
Notice that the type of `i` is not `int32_t`. The type of `i` is `a pointer to int32_t`. So when we say `j = i + 1`, this is not that we add `1` to an `int`. The unit of `1` is not `int`. The unit of `1` is `int32_t*`. So `j` and `i` have one unit distance from each other. Unit `int32_t*`. Now in order to convert these addresses to int, we use the casting `reinterpret_cast<std::uintptr_t>`. We read `std::unitptr_t` _is an unsigned integer type that is capable of storing a data pointer_.

### typedef and using (none of them are really important or very useful!)
In C++ we rarely use `typedef` and prefer `using` more. But at the end, they both are not very useful! With `typedef` we give a data type another name, or as we may read in documentrations, with `typedef` and `using` we create *type alias*:
```c++
typedef int* intPtr; // Create a type alias
intPtr p; // As if we would write: int* p;
```
We could write:
```c++
using intPtr = int*; // Create a type alias
intPtr p; // As if we would write: int* p;
```
Example:
```c++
include <iostream>
#include <string>

using Blah = int;

int main(int argc, char* argv[])
{
   Blah a = 5;
   std::cout << "a = " << a << std::endl; // Prints: a = 5
   return 0;
}
```
One more useful use case of `using` is to avoid typing namespaces:
```c++
#include <iostream>
#include <string>

using namespace std; // This is NOT recommended!!! Don't do this! Just forget "typedef" and "using"!

int main(int argc, char* argv[])
{
   //std::cout << "Hello world" << std::endl;
   cout << "Hello world!" << endl; // Prints: Hello world!

    return 0;
}
```





### streams




