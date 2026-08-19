## Templates
*Template* gives flexibility against different data types. A simple example of template function:

```c++
#include <iostream>
#include <string>

template <typename whatever> // We used "whatever", but normally we use "T"!

whatever findMax(whatever a , whatever b)
{
  if (a > b)
  {
    return a;
  }
  return b;
}

int main (int argc, char* argv[])
{
  {
    int a = 4;
    int b = 6;
    std::cout << "findMax(" << a << "," << b << ") = " << findMax(a,b) << std::endl; // prints: findMax(4,6) = 6
  }

  {
    double a = 4.9;
    double b = 6.1;
    std::cout << "findMax(" << a << "," << b << ") = " << findMax(a,b) << std::endl; // prints: findMax(4.9,6.1) = 6.1
  }

  return 0;
}
```
