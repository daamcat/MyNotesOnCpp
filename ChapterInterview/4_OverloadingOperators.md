## Overloading operators


Examples for overloading operator:
```c++
#include <iostream>
#include <string>

class Dog
{
    public:
    Dog(const int val)
    {
        m_val = val;
    }
    ~Dog()
    {}

    int getVal() const
    {
        return m_val;
    }

    // operator + overload as member function:
    Dog operator+(const Dog& other) const
    {
       std::cout << "member operator + overload is called." << std::endl;
        return Dog(this->getVal() + other.getVal());
    }

    bool operator==(const Dog& other) const
    {
       std::cout << "member operator == overload is called." << std::endl;
       return this->getVal() == other.getVal();
    }
    
    Dog& operator+=(const Dog& rhs) // rhs: The object on the right hand side (b in a+=b;)
    {
       std::cout << "member operator += overload is called." << std::endl;
        m_val += rhs.getVal();
        return *this;
    }

    private:
    int m_val = 0;
};

// operator * overload as non-member function:
Dog operator*(const Dog& a , const Dog& b)
{
    std::cout << "non-member operator * overload is called. (a*b) + 5 !!!" << std::endl;
    return Dog(a.getVal() * b.getVal() + 5); // + 5 Just to make sure we don't call any default operator!
}

int main(int argc, char* argv[])
{
    Dog a(3);
    std::cout << "a.getVal() = " << a.getVal() << std::endl;
    Dog b(5);
    std::cout << "b.getVal() = " << b.getVal() << std::endl;
    Dog c = a + b;
    std::cout << "c = a+b; c.getVal() = " << c.getVal() << std::endl;

    Dog d = a * b;
    std::cout << "d=a*b; d.getVal() = " << d.getVal() << std::endl;
    
    bool areEqual = (a == b); // To call operator ==
    std::cout << "a == a : " << std::boolalpha << areEqual << std::endl;
    
    d += c;
    std::cout << "d+=c; d.getVal() : " << d.getVal() << std::endl;
    return 0;
}
```
[Run this code](https://godbolt.org/z/qnj1hcMe6)

Prints:
```
a.getVal() = 3
b.getVal() = 5
member operator + overload is called.
c = a+b; c.getVal() = 8
non-member operator * overload is called. (a*b) + 5 !!!
d=a*b; d.getVal() = 20
member operator == overload is called.
a == a : false
member operator += overload is called.
d+=c; d.getVal() : 28
```
* The format of function member operator overload declaration is like `<1> operator<2>(const <3>& other);`, in which:
  - <1>: what is the return type of operator? For example, the return of `*` or `+` in above example is class type `Dog`. But the return type of operator `==` is bool. The return type of `+=` is `Dog&` and it must be `return *this;`, because `+=` acts on the object itself, therefore the reference to object itself must be returned.
  - <2>: operator sign
  - <3>: Class type of the other element, that is engaged via operation.
* `operator`s can be overloaded as *member* or *non-member* functions. *member* implemented inside the class, using `this`, *non-member* function outside the class.
* Member operators that don't modify the object, like `+`, `==`, `*`, should be `const`.
* Member operators that modify the object, like `+=`, `-=`, `=`, should not be `const`.
* Non-member operators can not be `const`.
* Constructive operators, operators that *construct* new objects, like `c = a + b;`, should not change their operands. I.e. the member function must be `const` and the input argument must be ` const reference.
* Constructive operators should return their result by value.

