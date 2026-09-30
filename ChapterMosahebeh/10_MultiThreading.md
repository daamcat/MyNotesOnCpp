## Multithreading

References:
https://en.cppreference.com/w/cpp/thread/thread/join
https://stackoverflow.com/questions/200469/what-is-the-difference-between-a-process-and-a-thread

*Process* is an executing instance of a *program*. On each processor at a time, only one process can run. Each *process* is started with a single *thread*, called *primary thread*. 
But within a *process*, several threads can be created.

Each *process* is run on its separate memory. But threads run in a shared memory space.

**What does `join()` do?**
#### Example 1
In a process, you stop, you call multiple threads, they will run simultaneously. They end and then the process continues:
```c++
#include <iostream>
#include <thread>
#include <chrono>
 
void foo()
{
    // simulate expensive operation
    std::this_thread::sleep_for(std::chrono::seconds(3));
    std::cout << "Ending first helper...\n";
}
 
void bar()
{
    // simulate expensive operation
    std::this_thread::sleep_for(std::chrono::seconds(1));
    std::cout << "Ending second helper...\n";
}
 
int main()
{
    std::cout << "Starting first helper...\n";
    std::thread helper1(foo);
 
    std::cout << "Starting second helper...\n";
    std::thread helper2(bar);

    helper1.join();
    helper2.join();
    
    std::cout << "Waiting for helpers to finish..." << std::endl;
 
    std::cout << "Done!\n";
}
```
Prints:
```
Starting first helper...
Starting second helper...
Ending second helper...
Ending first helper...
Waiting for helpers to finish...
Done!
```
#### Example 2
Everywhere you put `join()` for a thread, it will keep the process in that line until the thread is finished!
```c++
#include <iostream>
#include <thread>
#include <chrono>
 
void foo()
{
    // simulate expensive operation
    std::this_thread::sleep_for(std::chrono::seconds(3));
    std::cout << "Ending first helper...\n";
}
 
void bar()
{
    // simulate expensive operation
    std::this_thread::sleep_for(std::chrono::seconds(1));
    std::cout << "Ending second helper...\n";
}
 
int main()
{
    std::cout << "Starting first helper...\n";
    std::thread helper1(foo);
    helper1.join();
 
    std::cout << "Starting second helper...\n";
    std::thread helper2(bar);
    helper2.join();
    
    std::cout << "Waiting for helpers to finish..." << std::endl;
    std::cout << "Done!\n";
}
```
Prints:
```
Starting first helper...
Ending first helper...
Starting second helper...
Ending second helper...
Waiting for helpers to finish...
Done!
```
This means `join()` keeps the major process waiting at the point of creating the thread till the thread finishes.

#### Example 3
```c++
#include <iostream>
#include <thread>
#include <chrono>
 
void foo()
{
    // simulate expensive operation
    std::this_thread::sleep_for(std::chrono::seconds(3));
    std::cout << "Ending first helper...\n";
}
 
void bar()
{
    // simulate expensive operation
    std::this_thread::sleep_for(std::chrono::seconds(1));
    std::cout << "Ending second helper...\n";
}
 
int main()
{
    std::cout << "Starting first helper...\n";
    std::thread helper1(foo);
 
    std::cout << "Starting second helper...\n";
    std::thread helper2(bar);
    helper2.join();
    
    std::cout << "Waiting for helpers to finish..." << std::endl;
    helper1.join();
 
    std::cout << "Done!\n";
}
```
Prints:
```
Starting first helper...
Starting second helper...
Ending second helper...
Waiting for helpers to finish...
Ending first helper...
Done!
```
**What does `detach()` do?**

`detach()` lets the thread run independent of the process. As `detach()` for a thread is called, the process will continue, not waiting for the thread to finish.
```c++
#include <iostream>
#include <thread>
#include <chrono>
 
void foo()
{
    // simulate expensive operation
    std::this_thread::sleep_for(std::chrono::seconds(3));
    std::cout << "Ending first helper...\n";
}
 
void bar()
{
    // simulate expensive operation
    std::this_thread::sleep_for(std::chrono::seconds(1));
    std::cout << "Ending second helper...\n";
}
 
int main()
{
    std::cout << "Starting first helper...\n";
    std::thread helper1(foo);
    helper1.detach();
 
    std::cout << "Starting second helper...\n";
    std::thread helper2(bar);
    helper2.join();
    
    std::cout << "Waiting for helpers to finish..." << std::endl;
 
    std::cout << "Done!\n";
}
```
Prints:
```
Starting first helper...
Starting second helper...
Ending second helper...
Waiting for helpers to finish...
Done!
```
