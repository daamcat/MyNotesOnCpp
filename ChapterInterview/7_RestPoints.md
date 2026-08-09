### Questions and answers:

#### Identifier
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
#### Qualifier
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
