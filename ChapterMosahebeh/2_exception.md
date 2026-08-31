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

