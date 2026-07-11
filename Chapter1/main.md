# ???
**C++ and CMake must be learnt simultaneously.**

First choose a name for your test project. I choose `HelloWorld`.
Now you need a new directory on your computer for your project. In order to create a test project, your files need to be organized. Create a folder for your project. I create:  
`C:\Users\diana\OneDrive\Dokumente\MyNotesOnCpp\HelloWorld`  
Inside this directory, I will have everything related to `HelloWorld`. Normally, all can be categorized in two groups:
* Source files: in `HelloWorld\src`
* Build files: in `HelloWorld\build`

Create `HelloWorld\src`!

```
HelloWorld/
├── src/
│   ├── CMakeLists.txt
│   └── src/
│       ├── HelloWorld.cpp
├── build/
```
 
## Source files
Inside `HelloWorld\src`, you will have all your source files. These source files can be different groups of files:
* .h and .cpp files: in `HelloWorld\src\src`
* Test files: in `HelloWorld\src\Tests`
* Data files: in `HelloWorld\src\Data`
* ...  

For now, we only create:  
`C:\Users\diana\OneDrive\Dokumente\MyNotesOnCpp\HelloWorld\src\src`  
Now we have all directories we need. We will create two files inside these directories:
* HelloWorld.cpp: in `HelloWorld\src\src` (It is better that the function `main()` to be included in a file whose name is the same as project name. In our case `HelloWorld.cpp`. What is `main()`?! You will learn!!!)
* CMakeLists.txt: in `HelloWorld\src`

### HelloWorld.cpp
Create the file `HelloWorld\src\src\HelloWorld.cpp`. The contents of this file:  
(In code, notice that the lines starting with `//` are comments)

```cpp
#include <iostream>
// With #include you ask the compiler to include the given library. You add these includes during coding, depending on what you need.
// For example, we included <iostream> because we wanted to use std::cout and std::endl, functions that are in <iostream>.

int main(int argc, char* argv[])
{
  std::cout << "Total argument count (argc): " << argc << std::endl;
	
  for (int i = 0; i < argc; i++)
  {
    std::cout << "argv[" << i << "]: " << argv[i] << std::endl;
  }
	
  return 0;
}
```
#### int main(int argc, char* argv[])

What is *main* function?

Function `main()` is the entry point of an application.
`main()` gets called by execution of application.

Input arguments:
* argc: Number of input arguments.
* argv: Name of program. Plus input arguments:
  - argv[0] ---> "program"
  - argv[1] ---> "arg1"
  - argv[2] ---> "arg2"
  - ...
  - argv[argc] ---> NULL


Cmake:

### CMakeLists.txt
Create the file `HelloWorld\src\CMakeLists.txt`. The contents of this file:
```cmake
cmake_minimum_required (VERSION 3.5)
project(HelloWorld) # Name for project. Can be accessed with variable ${PROJECT_NAME}.
add_executable(${PROJECT_NAME} src/HelloWorld.cpp)
```

## Build and run
```bash
Microsoft Windows [Version 10.0.26200.8655]  
(c) Microsoft Corporation. Alle Rechte vorbehalten.  
  
C:\Users\diana\OneDrive\Dokumente\MyNotesOnCpp\HelloWorld>cmake -S src -B build  
-- Building for: Visual Studio 18 2026  
CMake Deprecation Warning at CMakeLists.txt:1 (cmake_minimum_required):  
  Compatibility with CMake < 3.10 will be removed from a future version of  
  CMake.  
  
  Update the VERSION argument <min> value.  Or, use the <min>...<max> syntax  
  to tell CMake that the project requires at least <min> but has been updated  
  to work with policies introduced by <max> or earlier.  
  
  
-- Selecting Windows SDK version 10.0.26100.0 to target Windows 10.0.26200.  
-- The C compiler identification is MSVC 19.50.35728.0  
-- The CXX compiler identification is MSVC 19.50.35728.0  
-- Detecting C compiler ABI info  
-- Detecting C compiler ABI info - done  
-- Check for working C compiler: C:/Program Files/Microsoft Visual Studio/18/Community/VC/Tools/MSVC/14.50.35717/bin/Hostx64/x64/cl.exe - skipped  
-- Detecting C compile features  
-- Detecting C compile features - done  
-- Detecting CXX compiler ABI info  
-- Detecting CXX compiler ABI info - done  
-- Check for working CXX compiler: C:/Program Files/Microsoft Visual Studio/18/Community/VC/Tools/MSVC/14.50.35717/bin/Hostx64/x64/cl.exe - skipped  
-- Detecting CXX compile features  
-- Detecting CXX compile features - done  
-- Configuring done (7.2s)  
-- Generating done (0.2s)  
-- Build files have been written to: C:/Users/diana/OneDrive/Dokumente/MyNotesOnCpp/HelloWorld/build  
```
Build:
```bash
C:\Users\diana\OneDrive\Dokumente\MyNotesOnCpp\HelloWorld>cmake --build build --config Release  
MSBuild-Version 18.4.0+6e61e96ac für .NET Framework  
  
  1>Checking Build System  
  Building Custom Rule C:/Users/diana/OneDrive/Dokumente/MyNotesOnCpp/HelloWorld/src/CMakeLists.txt  
  HelloWorld.cpp  
  HelloWorld.vcxproj -> C:\Users\diana\OneDrive\Dokumente\MyNotesOnCpp\HelloWorld\build\Release\HelloWorld.exe  
  Building Custom Rule C:/Users/diana/OneDrive/Dokumente/MyNotesOnCpp/HelloWorld/src/CMakeLists.txt  
```
Change the current directory:
```bash
C:\Users\diana\OneDrive\Dokumente\MyNotesOnCpp\HelloWorld>cd build/Release  
```
Execute the executable:
```bash
C:\Users\diana\OneDrive\Dokumente\MyNotesOnCpp\HelloWorld\build\Release>HelloWorld.exe param1 param2 param3  
Total argument count (argc): 4  
argv[0]: HelloWorld.exe  
argv[1]: param1  
argv[2]: param2  
argv[3]: param3  
```


# Drafts
```

my_project/
├── .gitignore             # Files to ignore in Git (e.g., build folders)
├── CMakeLists.txt         # Main build configuration file
├── README.md              # Project documentation
├── cmake/                 # Custom CMake modules and scripts
├── doc/                   # Detailed documentation and design files
├── include/               # Public header files (.h, .hpp)
│   └── my_project/        # Subfolder matching project name (prevents naming clashes)
│       ├── core.hpp
│       └── utils.hpp
├── src/                   # Private source files (.cpp) and private headers
│   ├── core.cpp
│   ├── main.cpp           # Program entry point
│   └── private_helper.hpp # Headers not needed by external users
├── tests/                 # Unit tests and test data
│   ├── CMakeLists.txt     # Test-specific build file
│   └── test_core.cpp
└── third_party/           # External libraries and dependencies
    └── external_lib/

```

