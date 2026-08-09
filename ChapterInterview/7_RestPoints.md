## Questions and answers:

### Identifier
An "identifier" is simply the name given to something in a program. For example in below code, `age` is an identifier.
```c++
int age = 20;
```
Therefore, variable names, class names, function names, etc, are all *identifier*s.

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
