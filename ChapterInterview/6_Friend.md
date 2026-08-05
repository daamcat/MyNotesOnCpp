## Friend

One of the major applications of `friend` classes is breaking a big class into several smaller classes, where these classes are all `friend`s of each other. Proper use of `friend` classes not only
doesn't harm encapsulation, even helps to improve it. In below example, notice how we hide the function member `FuelSystem::consumeFuel` from `main` and let the car only to consume fuel!

An example of `friend` classes. In this example we see how we can divide one complete class to two smaller classes. These two classes being `friend` of each other makes it possible for the classes to 
communicate with each other and access their `private` member variables, at the same time that their private members are hidden from outside the classes:

(Sidenote: Notice how we use `std::unique_ptr` and `std::move` its ownership to an object)
```c++
#include <iostream>
#include <string>
#include <memory>

class CarOld
// In this class the car includes its fuel system
{
    public:
    CarOld();
    void drive()
    {
        std::cout << "Car is driving ..." << std::endl;
        m_fuelLevel -= 2;
        m_speed = 5;
    }

    void refuel(int newFuelLevel)
    {
        std::cout << "Car is refueling ..." << std::endl;
        m_fuelLevel = newFuelLevel;
    }

    private:
    int m_speed = 0;
    int m_fuelLevel = 0;
};

class FuelSystem
{
    public:
    FuelSystem() {};

    private:
    int getFuelLevel() const
    {
        return m_fuelLevel;
    }
    void refuel(int newFuelLevel)
    {
        m_fuelLevel = newFuelLevel;
    }
    void consumeFuel(int consumedFuel)
    {
        m_fuelLevel -= consumedFuel;
    }
    
    int m_fuelLevel = 0;

    friend class CarModern;
};

class CarModern
{
    public:
    CarModern(std::unique_ptr<FuelSystem>& fuelSystem) : m_fuelSystem(std::move(fuelSystem)) {}
    // Get the ownership of unique pointer with std::move

    void drive()
    {
        std::cout << "Car is driving ..." << std::endl;
        m_fuelSystem->consumeFuel(2);
        m_speed = 5;
    }
    int getFuelLevel()
    {
        std::cout << "Get fuel level ..." << std::endl;
        return m_fuelSystem->getFuelLevel();
    }
    void refuel(int newFuelLevel)
    {
        std::cout << "Car is refueling ..." << std::endl;
        m_fuelSystem->refuel(newFuelLevel);
    }

    private:
    int m_speed = 0;
    std::unique_ptr<FuelSystem> m_fuelSystem;

    friend class FuelSystem;
};

int main(int argc, char* argv[])
{
    // Create a unique pointer of FuelSystem:
    std::unique_ptr<FuelSystem> fuelSystem = std::make_unique<FuelSystem>();
    CarModern carModern(fuelSystem); // The ownership of fuelSystem will be "std::move"d to carModern.
    carModern.drive();

    // Notice that everything with fuelSystem is private. We can not touch them, but carModern can,
    // because CarModern and FuelSystem are "friend"s!!!
    
    return 0;
}
```
Prints:
```
Car is driving ...
```


