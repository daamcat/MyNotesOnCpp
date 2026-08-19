## Templates
`Template` gives flexibility against different data types. A simple example of *function template*:

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
*Class template* lets us to write the logic once, and then to use it for different data types. A simple example from class template:
```c++
#include <iostream>
#include <string>

template <typename whatever>

class Box
{
  public:
  Box (whatever item)
  {
    m_item = item;
  }

  whatever getItem() const
  {
    return m_item;
  }

  private:
  whatever m_item;
};


int main (int argc, char* argv[])
{
  Box<int> boxInt(20);
  std::cout << "boxInt.getItem() = " << boxInt.getItem() << std::endl; // Prints: boxInt.getItem() = 20

  Box<std::string> boxString("Here is string");
  std::cout << "boxString.getItem() = " << boxString.getItem() << std::endl; // Prints: boxString.getItem() = Here is string 

  return 0;
}
```

Notice that `Box boxInt(20);`, without `<int>` would cause error. Because without `<int>` the compiler doesn't know how much memory it should allocate for object of class `Box`. In other words, the compiler can not use `Box` without knowing what type it should use with `Box`.

After we deliver the type with `<int>` The compiler generates a class like class `Box`, in which the type `whatever` has been replaced with `int`.

Here in this example:
* `Box` is *template class*.
* `<int>` is *template argument*.
* And the compiler generating the class by replacing `whatever` with `int` is *instantiation*.
