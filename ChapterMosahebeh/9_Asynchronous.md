### Asynchronous
Ref: https://www.cppstories.com/2014/01/tasks-with-stdfuture-and-stdasync/#did-stdasync-fulfilled-its-promises

INCOMPLETE

The following example shows how asynchronous programming can increase code efficiency:
```c++
#include <iostream>
#include <string>
#include <chrono>
#include <thread>
#include <future>

bool func()
{
  std::this_thread::sleep_for(std::chrono::milliseconds(100));
  // Do something in here!
  return true;
}
int main(int argc, char* argv[])
{
  std::chrono::steady_clock::time_point begin = std::chrono::steady_clock::now();
  func();
  func();
  func();
  std::chrono::steady_clock::time_point end = std::chrono::steady_clock::now();

  std::cout<< "Execution time: " << std::chrono::duration_cast<std::chrono::milliseconds>(end - begin).count() << std::endl; // Prints: Execution time: 300

  begin = std::chrono::steady_clock::now();
  std::future<bool> future1 = std::async(std::launch::async, func);
  std::future<bool> future2 = std::async(std::launch::async, func);
  std::future<bool> future3 = std::async(std::launch::async, func);
  
  future1.get();
  future2.get();
  future3.get();
  end = std::chrono::steady_clock::now();
  std::cout<< "Execution time: " << std::chrono::duration_cast<std::chrono::milliseconds>(end - begin).count() << std::endl; // Prints: Execution time: 100
  return 0;
}
```

prints:
```
Execution time: 300
Execution time: 100
```
