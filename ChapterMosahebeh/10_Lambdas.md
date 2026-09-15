### Lambdas
Ref: https://leanpub.com/cpplambda

The syntax for lambdas is like the following:
```
     capture list. Here we specify what the lambda body can have from its enclosing scope
     |             and how: [&]: capture all by ref. [=]: capture all by value. [a, &b]: capture variable "a" by value and "b" by ref.
     |
     |    parameter list. Here we specify which input arguments to pass when calling lambda.
     |    |
     |    |                                         lambda body
     |    |                                             |
     ▼    ▼                                             ▼
     []  ()  specifiers exception attr  -> <type> {  /* code */ }
                ▲                           ▲                            
                |                           |
                |                           |
                |                           trailing return type
                |                           
    	   mutable, exception specification, noexcept, ...
```

A simple example of lambda can be seen below:
```c++
#include <iostream>
#include <string>

int main(int argc, char* argv[])
{
  int in1 = 5;
  int in2 = 7;
  auto lambda1 = [](int a , int b)
  {
    return a + b;
  };
  auto lambda2 = [&in1, &in2]()
  {
    return in1 + in2;
  };

  std::cout << "lambda1(in1,in2): " << lambda1(in1,in2) << std::endl; // Prints: lambda1(in1,in2): 12
  std::cout << "lambda2(): " << lambda2() << std::endl; // Prints: lambda2(): 12

  return 0;
}
```
Prints:
```
lambda1(in1,in2): 12
lambda2(): 12
```
By *capture list* we can decide to what lambda can have access from its enclosing scope. It can have a `const` copy of all variables using `[=]`, or reference to all variables using `[&]`, or a mix of reference and `const` copy 
of specified variables. Below example shows this:
```c++
#include <iostream>
#include <string>

int main(int argc, char* argv[])
{
  int in1 = 5;
  int in2 = 7;

  auto lambda3 = [=]() mutable // Lambda gets a copy of variables. mutable helps the copies not to be const.
  {
    in1 = 8; // We would get compiler error "assignment of read-only variable" here without "mutable"
  };
  lambda3();
  std::cout << "in1: " << in1 << std::endl; // Prints: in1: 5

  auto lambda4 = [&]()
  {
    in1 = 8;
  };
  lambda4();
  std::cout << "in1: " << in1 << std::endl; // Prints: in1: 8

  return 0;
}
```
Prints:
```c++
in1: 5
in1: 8
```
**Tip:** The capture list per default takes a `const` copy of a variable. But if we want to be able to modify it inside the lambda body, we use the keyword `mutable`.  


