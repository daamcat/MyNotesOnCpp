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
Generating buildsystem (generating build files) and building:
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
#### Linux TO BE COMPLETED

### Adding Boost library to project

#### Windows 11
We download the boost library .zip file and extract in:

`G:\SenderReceiverTest\boost\boost_1_92_0`

Start VS developer command prompt: VS > Tools > Command line > Developer command prompt

Then the following commands:
```
G:\Projects_Damon\bvSysCenter\dev2_Build2022> cd G:\SenderReceiverTest\boost\boost_1_92_0
G:\SenderReceiverTest\boost\boost_1_92_0>bootstrap.bat
```
We see here the execution of these commands:
```
**********************************************************************
** Visual Studio 2022 Developer Command Prompt v17.14.25
** Copyright (c) 2025 Microsoft Corporation
**********************************************************************

G:\Projects_Damon\bvSysCenter\dev2_Build2022> cd G:\SenderReceiverTest\boost\boost_1_92_0

G:\SenderReceiverTest\boost\boost_1_92_0>bootstrap.bat
Building Boost.Build engine
LOCALAPPDATA=C:\Users\Damon\AppData\Local
Found with vswhere C:\Program Files\Microsoft Visual Studio\2022\Professional
Found with vswhere C:\Program Files\Microsoft Visual Studio\2022\Professional
###
### Using 'vc143' toolset.
###

G:\SenderReceiverTest\boost\boost_1_92_0\tools\build\src\engine>"cl" /nologo /MP /MT /TP /Feb2 /wd4996 /wd4675 /O2 /GL /EHsc /Zc:wchar_t /Gw   -DNDEBUG  bindjam.cpp builtins.cpp class.cpp command.cpp compile.cpp constants.cpp cwd.cpp debug.cpp debugger.cpp events.cpp execcmd.cpp execnt.cpp execunix.cpp filent.cpp filesys.cpp fileunix.cpp frames.cpp function.cpp glob.cpp hash.cpp hcache.cpp headers.cpp jam.cpp jamgram.cpp lists.cpp make.cpp make1.cpp md5.cpp modules.cpp native.cpp outerr.cpp output.cpp parse.cpp pathnt.cpp pathsys.cpp pathunix.cpp regexp.cpp rules.cpp scan.cpp search.cpp jam_strings.cpp startup.cpp tasks.cpp timestamp.cpp value.cpp variable.cpp w32_getreg.cpp mod_args.cpp mod_command_db.cpp mod_db.cpp mod_jam_builtin.cpp mod_jam_class.cpp mod_jam_errors.cpp mod_jam_modules.cpp mod_order.cpp mod_path.cpp mod_property_set.cpp mod_regex.cpp mod_sequence.cpp mod_set.cpp mod_string.cpp mod_summary.cpp mod_sysinfo.cpp mod_version.cpp /link kernel32.lib advapi32.lib user32.lib /MANIFEST:EMBED /MANIFESTINPUT:b2.exe.manifest
bindjam.cpp
builtins.cpp
class.cpp
command.cpp
compile.cpp
constants.cpp
cwd.cpp
debug.cpp
debugger.cpp
events.cpp
execcmd.cpp
execnt.cpp
execunix.cpp
filent.cpp
filesys.cpp
fileunix.cpp
frames.cpp
function.cpp
glob.cpp
hash.cpp
hcache.cpp
headers.cpp
jam.cpp
jamgram.cpp
lists.cpp
make.cpp
make1.cpp
md5.cpp
modules.cpp
native.cpp
outerr.cpp
output.cpp
parse.cpp
pathnt.cpp
pathsys.cpp
pathunix.cpp
regexp.cpp
rules.cpp
scan.cpp
search.cpp
jam_strings.cpp
startup.cpp
tasks.cpp
timestamp.cpp
value.cpp
variable.cpp
w32_getreg.cpp
mod_args.cpp
mod_command_db.cpp
mod_db.cpp
mod_jam_builtin.cpp
mod_jam_class.cpp
mod_jam_errors.cpp
mod_jam_modules.cpp
mod_order.cpp
mod_path.cpp
mod_property_set.cpp
mod_regex.cpp
mod_sequence.cpp
mod_set.cpp
mod_string.cpp
mod_summary.cpp
mod_sysinfo.cpp
mod_version.cpp
Generating code
Finished generating code

G:\SenderReceiverTest\boost\boost_1_92_0\tools\build\src\engine>dir *.exe
 Volume in drive G has no label.
 Volume Serial Number is 367D-FB9E

 Directory of G:\SenderReceiverTest\boost\boost_1_92_0\tools\build\src\engine

22.09.2026  08:42         1.005.056 b2.exe
               1 File(s)      1.005.056 bytes
               0 Dir(s)  81.595.674.624 bytes free

Generating Boost.Build configuration in project-config.jam for msvc...

Bootstrapping is done. To build, run:

    .\b2

To adjust configuration, edit 'project-config.jam'.
Further information:

    - Command line help:
    .\b2 --help

    - Getting started guide:
    http://boost.org/more/getting_started/windows.html

    - Boost.Build documentation:
    http://www.boost.org/build/


G:\SenderReceiverTest\boost\boost_1_92_0>b2
Performing configuration checks

    - default address-model    : 64-bit [1]
    - default architecture     : x86 [1]

Building the Boost C++ Libraries.
.
.
.
...updated 4314 targets...


The Boost C++ Libraries were successfully built!

The following directory should be added to compiler include paths:

    G:\SenderReceiverTest\boost\boost_1_92_0

The following directory should be added to linker library paths:

    G:\SenderReceiverTest\boost\boost_1_92_0\stage\lib


G:\SenderReceiverTest\boost\boost_1_92_0>
```
So far we have built Boost manually. We need to tell CMAKE in our `CMakeLists.txt` where to search for boost. We update `CMakeLists.txt` file:
```cmake
cmake_minimum_required(VERSION 3.5) # In order to get installed cmake version in Windows, in cmd type: cmake --version

project(SenderReceiverTest)

# 1. Point CMake directly to the generated config files inside your built Boost folder
set(Boost_DIR "G:/SenderReceiverTest/boost/boost_1_92_0/stage/lib/cmake/Boost-1.92.0")
set(Boost_USE_STATIC_LIBS ON) # Recommended for Windows to avoid missing .dll errors


find_package(Boost REQUIRED COMPONENTS "system")

set(moon "Moon")
set(mars "Mars")
add_executable(${moon} src/Moon/main.cpp)
add_executable(${mars} src/Mars/main.cpp)

target_include_directories(${moon} PRIVATE ${BOOST_INCLUDE_DIRS})
target_include_directories(${mars} PRIVATE ${BOOST_INCLUDE_DIRS})

target_link_libraries(${moon} PUBLIC Boost::system)
target_link_libraries(${mars} PUBLIC Boost::system) 

install(TARGETS ${moon} ${mars} DESTINATION "${CMAKE_BINARY_DIR}/install")
# When we generate buildsystem with "cmake -S src -B build", CMAKE_BINARY_DIR is set to "build".
```
Generate the buildsystem:
```
G:\SenderReceiverTest>cmake -S src -B build
-- Building for: Visual Studio 17 2022
CMake Deprecation Warning at CMakeLists.txt:2 (cmake_minimum_required):
  Compatibility with CMake < 3.10 will be removed from a future version of
  CMake.

  Update the VERSION argument <min> value.  Or, use the <min>...<max> syntax
  to tell CMake that the project requires at least <min> but has been updated
  to work with policies introduced by <max> or earlier.


-- Selecting Windows SDK version 10.0.26100.0 to target Windows 10.0.26200.
-- The C compiler identification is MSVC 19.44.35222.0
-- The CXX compiler identification is MSVC 19.44.35222.0
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
CMake Warning (dev) at CMakeLists.txt:11 (find_package):
  Policy CMP0167 is not set: The FindBoost module is removed.  Run "cmake
  --help-policy CMP0167" for policy details.  Use the cmake_policy command to
  set the policy and suppress this warning.

This warning is for project developers.  Use -Wno-dev to suppress it.

-- Found Boost: G:/SenderReceiverTest/boost/boost_1_92_0/stage/lib/cmake/Boost-1.92.0/BoostConfig.cmake (found version "1.92.0") found components: system
-- Configuring done (2.0s)
-- Generating done (0.0s)
-- Build files have been written to: G:/SenderReceiverTest/build
```
Build the project:
```
G:\SenderReceiverTest>cmake --build build --config Release
CMake is re-running because G:/SenderReceiverTest/build/CMakeFiles/generate.stamp is out-of-date.
  the file 'G:/SenderReceiverTest/src/CMakeLists.txt'
  is newer than 'G:/SenderReceiverTest/build/CMakeFiles/generate.stamp.depend'
  result='-1'
CMake Deprecation Warning at CMakeLists.txt:2 (cmake_minimum_required):
  Compatibility with CMake < 3.10 will be removed from a future version of
  CMake.

  Update the VERSION argument <min> value.  Or, use the <min>...<max> syntax
  to tell CMake that the project requires at least <min> but has been updated
  to work with policies introduced by <max> or earlier.


-- Selecting Windows SDK version 10.0.26100.0 to target Windows 10.0.26200.
CMake Warning (dev) at CMakeLists.txt:11 (find_package):
  Policy CMP0167 is not set: The FindBoost module is removed.  Run "cmake
  --help-policy CMP0167" for policy details.  Use the cmake_policy command to
  set the policy and suppress this warning.

This warning is for project developers.  Use -Wno-dev to suppress it.

-- Configuring done (0.0s)
-- Generating done (0.1s)
-- Build files have been written to: G:/SenderReceiverTest/build
MSBuild version 17.14.40+3e7442088 for .NET Framework

  1>Checking Build System
  Building Custom Rule G:/SenderReceiverTest/src/CMakeLists.txt
  main.cpp
  Mars.vcxproj -> G:\SenderReceiverTest\build\Release\Mars.exe
  Building Custom Rule G:/SenderReceiverTest/src/CMakeLists.txt
  main.cpp
  Moon.vcxproj -> G:\SenderReceiverTest\build\Release\Moon.exe
  Building Custom Rule G:/SenderReceiverTest/src/CMakeLists.txt

G:\SenderReceiverTest>
```
Now we can develop our code using VS Solution in: `G:\SenderReceiverTest\build\SenderReceiverTest.sln`

#### Linux TO BE COMPLETED

### First data exchange
Here we have an initial working implementation in which `Moon` sends a Hello World message to `Mars`. The message has to end with a `\n` in this example.

`G:\SenderReceiverTest\src\src\Mars\main.cpp`:
```c++
#include <iostream>
#include <string>
#include <boost/asio.hpp>


int main()
{
  std::cout << "Hello World by Mars!" << std::endl;

  /*
  Synchronous TCP server:
  Refs: 
  https://www.boost.org/doc/libs/1_37_0/doc/html/boost_asio/overview/core/basics.html
  https://www.boost.org/doc/libs/1_92_0/doc/html/boost_asio/tutorial/tutdaytime2.html
  https://theboostcpplibraries.com/boost.asio-network-programming
  */

  try
  {
    int32_t portNumber = 12345;

    // boost::asio::io_context is the modern replacement for boost::asio::io_service. It links our executable to I/O services of OS.
    boost::asio::io_context ioContext;

    // An endpoint is the destination from which we receive message. The destination we use in our TCP connection.
    // This endpoint is specified with two things: 1. IP address, 2. Port number.
    // Socket will get connected to this endpoint.
    // Listen to ANY available IP version 4 IP interface (boost::asio::ip::tcp::v4()), but only on port 12345:
    boost::asio::ip::tcp::endpoint endpoint(boost::asio::ip::tcp::v4(), portNumber);

    // boost::asio::ip::tcp::acceptor sits on endpoint and waits for connection request to accept. It doesn't send or receive data.
    // It only manages the initial connection:
    boost::asio::ip::tcp::acceptor acceptor(ioContext, endpoint);

    // boost::asio::ip::tcp::socket is the connection pipeline through which data is exchanged. It performs I/O operations.
    // First we create a blank disconnected socket:
    boost::asio::ip::tcp::socket socket(ioContext);

    std::cout << "Mars server online. Waiting for Moon to connect on port " << portNumber << " ..." << std::endl;
    acceptor.accept(socket); // Here acceptor waits for connection to be established (blocks the program) and the socket to get activated...
    // As soon as the connection is established, the acceptor polulates the connection details with socket and activates it.
    // The acceptor has nothing more to do! Data exchange is carried out by socket.
    std::cout << "Moon connected successfully." << std::endl;

    // boost::asio::streambuf provides a memory buffer which can be resized dynamically. It makes the life easier and we have no buffer overflow.
    boost::asio::streambuf buffer;

    // Get the data from the socket and put it in buffer till you reach the given delimiter.
    // boost::asio::read_until blocks execution till it receives the given delimiter.
    boost::asio::read_until(socket, buffer, "\n");

    std::string message;
    {
      std::istream stream(&buffer);
      std::getline(stream, message); // Reads until new line
    }
    std::cout << "Mars received message from Moon: \"" << message << "\"" << std::endl;
  }
  catch (std::exception& e)
  {
    std::cout << "Mars exception. Error message: " << e.what() << std::endl;
  }

  return 0;
}
```
`G:\SenderReceiverTest\src\src\Moon\main.cpp`:
```c++
#include <iostream>
#include <boost/asio.hpp>

int main()
{
  std::cout << "Hello World by Moon!" << std::endl;

  try
  {
    boost::asio::io_context ioContext;

    // boost::asio::ip::tcp::resolver creates a list of endpoints from given IP and port number.
    boost::asio::ip::tcp::resolver resolver(ioContext);
    // Get the IP and port number and return a list of matching endpoints:
    boost::asio::ip::basic_resolver_results<boost::asio::ip::tcp> endpoints = resolver.resolve("localhost", "12345");

    boost::asio::ip::tcp::socket socket(ioContext);
    std::cout << "Attemptring to connect to Mars..." << std::endl;
    
    // boost::asio::connect tries every endpoint till one gets connected. This will try for a while and if no success, will throw an exception:
    // "No connection could be made because the target machine actively refused it"
    boost::asio::connect(socket, endpoints);
    std::cout << "Connected to Mars successfully." << std::endl;

    std::string message = "Hello to Mars from Moon\n"; // The \n is important and expected from our implementation for receiver.
    // boost::asio::buffer: 1. Checks where the input message on memory is. 2. Checks the input message size. 3. Converts it to something that 
    // can be sent via Boost.Asio.
    // boost::asio::write: Puts/Writes the contents of the buffer byte-by-byte in the socket to send.
    // boost::asio::write guarantees that execution will not move to the next line of code until your message is entirely sent 
    // (or an unrecoverable network error occurs).
    boost::asio::write(socket, boost::asio::buffer(message));
    std::cout << "Sent message to Mars successfully." << std::endl;
  }
  catch (const std::exception& e)
  {
    std::cout << "Moon exception. Error message: " << e.what() << std::endl;
  }

  return 0;
}
```
Now after building in Release, we see two executables in here:
* `G:\SenderReceiverTest\build\Release\Mars.exe`
* `G:\SenderReceiverTest\build\Release\Moon.exe`

Now we open two cmd s in folder `G:\SenderReceiverTest\build\Release\`
In the first one we start `Mars.exe` (message receiver):
```
Microsoft Windows [Version 10.0.26200.9457]
(c) Microsoft Corporation. All rights reserved.

G:\SenderReceiverTest\build\Release>Mars.exe
Hello World by Mars!
Mars server online. Waiting for Moon to connect on port 12345 ...
```
In the second one `Moon.exe`:
```
Microsoft Windows [Version 10.0.26200.9457]
(c) Microsoft Corporation. All rights reserved.

G:\SenderReceiverTest\build\Release>Moon.exe
Hello World by Moon!
Attemptring to connect to Mars...
Connected to Mars successfully.
Sent message to Mars successfully.

G:\SenderReceiverTest\build\Release>
```
And now if we check the cmd of Mars again we see the added part:
```
Microsoft Windows [Version 10.0.26200.9457]
(c) Microsoft Corporation. All rights reserved.

G:\SenderReceiverTest\build\Release>Mars.exe
Hello World by Mars!
Mars server online. Waiting for Moon to connect on port 12345 ...
Moon connected successfully.
Mars received message from Moon: "Hello to Mars from Moon"

G:\SenderReceiverTest\build\Release>
```



