### Questions and answers:

#### "identifier"
An "identifier" is simply the name given to something in a program. For example in below code, `age` is an identifier.
```c++
int age = 20;
```
Therefore, variable names, class names, function names, etc, are all *identifier*s.

#### Scope resolution operator `::`
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

