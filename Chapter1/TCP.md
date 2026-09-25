# Network programming

## Synchronous TCP connection
TCP communication, unlike UDP, is reliable and guarantees data to be received in order with no loss.
In C++ for TCP network programming we use `boost::asio` library.

## A TCP example
### Building a simple solution with two executable files
The idea is to have two executable files, communicating with each other through TCP protocol:
* Mars.exe
* Moon.exe
The structure of files and folders should be like the following:
```
- SenderReceiverTest
---- CMakeLists.txt
---- src
------ Moon
-------- main.cpp
------ Mars
-------- main.cpp
```
Inside `Moon/main.cpp`:
```c++
#include <iostream>
int main()
{
  std::cout << "Hello World by Moon!" << std::endl;
  return 0;
}
```
Inside `Mars/main.cpp`:
```c++
#include <iostream>
int main()
{
  std::cout << "Hello World by Mars!" << std::endl;
  return 0;
}
```
Inside `CMakeLists.txt`:
```cmake
project(SenderReceiverTest)

cmake_minimum_required(VERSION 3.6) # In order to get installed cmake version in Windows, in cmd type: cmake --version

set(moon "Moon")
set(mars "Mars")
add_executable(${moon} src/Moon/main.cpp)
add_executable(${mars} src/Mars/main.cpp)
install(TARGETS ${moon} ${mars} DESTINATION "${CMAKE_BINARY_DIR}/install")
# When we generate buildsystem with "cmake -S src -B build", CMAKE_BINARY_DIR is set to "build".
```
#### Windows 11
Generating buildsystem (generating build files):
In our example, the two `main.cpp` files are located in:
* `G:\SenderReceiverTest\src\src\Moon\main.cpp`
* `G:\SenderReceiverTest\src\src\Mars\main.cpp`
We open `cmd` command prompt window in `G:\SenderReceiverTest` and insert:
```
G:\SenderReceiverTest>cmake -S src -B build
-- Building for: Visual Studio 17 2022
CMake Warning (dev) at CMakeLists.txt:3 (project):
  cmake_minimum_required() should be called prior to this top-level project()
  call.  Please see the cmake-commands(7) manual for usage documentation of
  both commands.
This warning is for project developers.  Use -Wno-dev to suppress it.

-- Selecting Windows SDK version 10.0.26100.0 to target Windows 10.0.19045.
-- The C compiler identification is MSVC 19.44.35208.0
-- The CXX compiler identification is MSVC 19.44.35208.0
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: C:/Program Files/Microsoft Visual Studio/2022/Professional/VC/Tools/MSVC/14.44.35207/bin/Hostx64/x64/cl.exe - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: C:/Program Files/Microsoft Visual Studio/2022/Professional/VC/Tools/MSVC/14.44.35207/bin/Hostx64/x64/cl.exe - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
CMake Deprecation Warning at CMakeLists.txt:5 (cmake_minimum_required):
  Compatibility with CMake < 3.10 will be removed from a future version of
  CMake.

  Update the VERSION argument <min> value.  Or, use the <min>...<max> syntax
  to tell CMake that the project requires at least <min> but has been updated
  to work with policies introduced by <max> or earlier.


-- Configuring done (2.7s)
-- Generating done (0.0s)
-- Build files have been written to: G:/SenderReceiverTest/build
```
Now that we generated buildsystem, we build the project (the executables) using this buildsystem:
```
G:\SenderReceiverTest>cmake --build build --config Release
MSBuild version 17.14.10+8b8e13593 for .NET Framework

  1>Checking Build System
  Building Custom Rule G:/SenderReceiverTest/src/CMakeLists.txt
  main.cpp
  Moon.vcxproj -> G:\SenderReceiverTest\build\Release\Moon.exe
  Building Custom Rule G:/SenderReceiverTest/src/CMakeLists.txt
  main.cpp
  Naghi.vcxproj -> G:\SenderReceiverTest\build\Release\Naghi.exe
  Building Custom Rule G:/SenderReceiverTest/src/CMakeLists.txt
```
Now check below two files:
* `G:\SenderReceiverTest\build\Release\Mars.exe`
* `G:\SenderReceiverTest\build\Release\Moon.exe`
Notice that there is still no install folder: `G:\SenderReceiverTest\build\install`

In order to install, we need to modify the way we build the project with an extra input argument:
```
G:\SenderReceiverTest>cmake --build build --config Release --target install
MSBuild version 17.14.10+8b8e13593 for .NET Framework

  Mars.vcxproj -> G:\SenderReceiverTest\build\Release\Mars.exe
  Moon.vcxproj -> G:\SenderReceiverTest\build\Release\Moon.exe
  1>
  -- Install configuration: "Release"
  -- Installing: G:/SenderReceiverTest/build/install/Moon.exe
  -- Installing: G:/SenderReceiverTest/build/install/Mars.exe
```
Now check the two files:
* `G:\SenderReceiverTest\build\install\Mars.exe`
* `G:\SenderReceiverTest\build\install\Moon.exe`

Now we can run these executables in `cmd`:
```
G:\SenderReceiverTest\build\install>Moon.exe
Hello World by Moon!

G:\SenderReceiverTest\build\install>Mars.exe
Hello World by Mars!
```
