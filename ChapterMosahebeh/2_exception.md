### Exception
The following code is an example of how we can `throw` and `catch` exceptions:
```c++
#include <iostream>
#include <string>
#include <stdexcept>

class ExceptionCase1 : public std::exception
{
    public:
    ExceptionCase1() {}

    void setInfo(const std::string& info)
    {
        m_info = info;
    }

    // Custom exceptions inheriting from std::exception should override the 
    // virtual what() method to return the error message.
    virtual const char* what() const noexcept override
    {
        return m_info.c_str();
    }

    private:
    std::string m_info = "Not set yet.";
};

class ExceptionCase2 : public std::exception
{
    public:
    ExceptionCase2() {}

    void setInfo(const std::string& info)
    {
        m_info = info;
    }

    // Custom exceptions inheriting from std::exception should override the 
    // virtual what() method to return the error message.
    virtual const char* what() const noexcept override
    {
        return m_info.c_str();
    }

    private:
    std::string m_info = "Not set yet.";
};

void funcThrowException(int exceptionCase)
// Depending on input, we simulate throwing different exceptions
{
    // For testing purposes. We just want this function to throw an exception.
    // As if something has gone wrong!
    std::cout << "Input exception case is: " << exceptionCase << std::endl;
    if (exceptionCase == 1)
    {
        ExceptionCase1 myEx;
        myEx.setInfo("Initial error info for ExceptionCase1.");
        throw myEx;
    }
    else
    {
        ExceptionCase2 myEx;
        myEx.setInfo("Initial error info for ExceptionCase2.");
        throw myEx;
    }
}

void func(int exceptionCase)
// The job of this function is to catch the exception, and depending on its type,
// to add some info to it and to throw it further.
{
    try
    {
        funcThrowException(exceptionCase); // This will throw an exception.
    }
    // Depending on what kind of exception funcThrowException throws, we expect
    // different catches() to be triggered:
    catch(ExceptionCase1& e)
    // This catch only catches the type of exception specified in parenthesis.
    {
        e.setInfo("We caught the ExceptionCase1!"); // Add some info to exception.
        throw; // Throw the exception further!
    }
    catch(ExceptionCase2& e)
    // This catch only catches the type of exception specified in parenthesis.
    {
        e.setInfo("We caught the ExceptionCase2!"); // Add some info to exception.
        throw; // Throw the exception further!
    }
}

int main(int argc, char* argv[])
{
    // Depending on what is happenning inside funcThrowException(), 
    // we want to get different exception::what() messages.
    // Test 1: We call func so that exception of ExceptionCase1 is thrown:
    try
    {
        func(1);
    }
    catch(std::exception& e)
    {
        std::cout << "e.what(): " << e.what() << std::endl;
    }

    // Test 2: We call func so that exception of ExceptionCase2 is thrown:
    try
    {
        func(2);
    }
    catch(const std::exception& e)
    {
        std::cout << "e.what(): " << e.what() << std::endl;
    }

    return 0;
}
```
The output of this code is:
```
Input exception case is: 1
e.what(): We caught the ExceptionCase1!
Input exception case is: 2
e.what(): We caught the ExceptionCase2!
```
* We derive our exception class from `std::exception`.
* We reimplement std::exception::what().
* When we want to `throw` an exception, we create an object of our class and we `throw` it!
* We `try` what we want to do, wishing not to get any exception.
* After `try` we write a `catch(const std::exception& e)`.

#### Exception dispatcher
The idea is to have one function which handles different types of defined exceptions. This function is called in exception handler. Using the code `throw();`, it forwards the exception toward different customized `catch()`es, based on their input object type. 

```c++
#include <iostream>
#include <string>
#include <stdexcept>

class ExceptionCase1 : public std::exception
{
    public:
    ExceptionCase1() {}

    void setInfo(const std::string& info)
    {
        m_info = info;
    }

    // Custom exceptions inheriting from std::exception should override the 
    // virtual what() method to return the error message.
    virtual const char* what() const noexcept override
    {
        return m_info.c_str();
    }

    private:
    std::string m_info = "Not set yet.";
};

class ExceptionCase2 : public std::exception
{
    public:
    ExceptionCase2() {}

    void setInfo(const std::string& info)
    {
        m_info = info;
    }

    // Custom exceptions inheriting from std::exception should override the 
    // virtual what() method to return the error message.
    virtual const char* what() const noexcept override
    {
        return m_info.c_str();
    }

    private:
    std::string m_info = "Not set yet.";
};

void funcThrowException(int exceptionCase)
// Depending on input, we simulate throwing different exceptions
{
    // For testing purposes. We just want this function to throw an exception.
    // As if something has gone wrong!
    std::cout << "Input exception case is: " << exceptionCase << std::endl;
    if (exceptionCase == 1)
    {
        ExceptionCase1 myEx;
        myEx.setInfo("Initial error info for ExceptionCase1.");
        throw myEx;
    }
    else
    {
        ExceptionCase2 myEx;
        myEx.setInfo("Initial error info for ExceptionCase2.");
        throw myEx;
    }
}

void exceptionDispatcher()
// This is an exception handler. Different exceptions will be handled differently.
// Plus, it is shown how we simply throw an exception further.
{
    try
    // We know we are here because of catching an exception. We throw it further 
    {
        throw; // Throw the current exception further
    }
    catch (const ExceptionCase1& e)
    {
        std::cout << "In exception handler of ExceptionCase1. e.what(): " << e.what() << std::endl;
    }
    catch (const ExceptionCase2& e)
    {
        std::cout << "In exception handler of ExceptionCase2. e.what(): " << e.what() << std::endl;        
    }
}

int main(int argc, char* argv[])
{
    // Depending on what is happenning inside funcThrowException(), 
    // we want to get different exception::what() messages.
    // Test 1: We call funcThrowException so that exception of ExceptionCase1 is thrown:
    try
    {
        funcThrowException(1);
    }
    catch(std::exception& e)
    {
        exceptionDispatcher();
    }

    // Test 2: We call funcThrowException so that exception of ExceptionCase2 is thrown:
    try
    {
        funcThrowException(2);
    }
    catch(const std::exception& e)
    {
        exceptionDispatcher();
    }

    return 0;
}
```
Prints:
```
Input exception case is: 1
In exception handler of ExceptionCase1. e.what(): Initial error info for ExceptionCase1.
Input exception case is: 2
In exception handler of ExceptionCase2. e.what(): Initial error info for ExceptionCase2.
```

* Inside `catch(const std::exception& e)` we can call `e.what()` to see info of exception.
* We can also `catch` specific exception types, do things on the exception, and then to `throw` it further.

> **Some more common practices in working with exceptions:**
> * Derive/Inherit your exception class from `std::exception` or from one of its inherited classes, like `std::runtime_error`.
> * When `catch()`ing the exception, catch by reference. E.g. catch(const MyException& e). Catching by value can show different behavior and catching by pointer is not recommanded.

#### Stack unwinding
*unwinding* literally means “going backward” or “undoing step by step”. *Stack unwinding* is the process of destroying local objects and removing function calls from the call stack as an exception propagates toward **a matching catch** block.

Below example demonstrates how engaging object destructors are called when an exception is caught. This is a demonstration of *stack unwinding*:
```c++
#include <iostream>
#include <string>
#include <exception>

struct Blah
{
    void func()
    {
        std::cout<<"Blah::func() is going to throw an exception..." << std::endl;
        throw std::runtime_error("exception from Blah::func!");
        std::cout<<"Blah::func(), one line after throwing an exception..." << std::endl;
    }
   
   ~Blah()
   {
       std::cout<<"Blah destructor called."<<std::endl;
   }
};

struct Bluh
{
    void func()
    {
        Blah blah;
        blah.func();
    }
    ~Bluh()
    {
        std::cout<<"Bluh destructor called." << std::endl;
    }
};

struct Blih
{
    void func()
    {
        Bluh bluh;
        bluh.func();
    }
    ~Blih()
    {
        std::cout<<"Blih destructor called."<<std::endl;
    }
};

int main(int argc, char* argv[])
{
    Blih blih;
    try
    {
        blih.func();
    }
    catch(const std::exception& e )
    {
        std::cout<<"error message: " << e.what() << std::endl;
    }
   
  return 0;
}
```

prints:
```
Blah::func() is going to throw an exception...
Blah destructor called.
Bluh destructor called.
error message: exception from Blah::func!
Blih destructor called.
```
Notice how the destructors of `Blah` and then `Bluh` and then `Blih` are called. Till function `main()` where the matching exception is handled.

Notice that the message `Blah::func(), one line after throwing an exception...` never gets printed. As soon as the exception is thrown, C++ goes ghrough *call stack* to see if it can find the matching exception handler and on its way, it calls the destructor of all objects on its way. 

Moreover, notice that the exception being thrown from `Blah::func()` doesn't get caught till inside `main()`. For everything between these two (i. e. from `main()` to`Blah::func()`), the functions will be removed from *stack callback* or the objects will be destroyed due to *stack unwinding*. Below example is a modification of above example, but in below example, the matching exception will be caught and handled in `Bluh::func()`, way before in `main()`.

```c++
#include <iostream>
#include <string>
#include <exception>

struct Blah
{
    void func()
    {
        std::cout<<"Blah::func() is going to throw an exception..." << std::endl;
        throw std::runtime_error("exception from Blah::func!");
        std::cout<<"Blah::func(), one line after throwing an exception..." << std::endl;
    }
   
   ~Blah()
   {
       std::cout<<"Blah destructor called."<<std::endl;
   }
};

struct Bluh
{
    void func()
    {
        Blah blah;
        try
        {
            blah.func();
        }
        catch (const std::exception& e)
        {
            std::cout<<"exception inside Bluh::func(). Error message: " << e.what() << std::endl;
        }
    }
    ~Bluh()
    {
        std::cout<<"Bluh destructor called." << std::endl;
    }
};

struct Blih
{
    void func()
    {
        Bluh bluh;
        bluh.func();
    }
    ~Blih()
    {
        std::cout<<"Blih destructor called."<<std::endl;
    }
};

int main(int argc, char* argv[])
{
    Blih blih;
    try
    {
        blih.func();
    }
    catch(const std::exception& e )
    {
        std::cout<<"error message: " << e.what() << std::endl;
    }
   
  return 0;
}
```

prints:
```
Blah::func() is going to throw an exception...
exception inside Bluh::func(). Error message: exception from Blah::func!
Blah destructor called.
Bluh destructor called.
Blih destructor called.
```













