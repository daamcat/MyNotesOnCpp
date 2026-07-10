## void main(int argc, char* argv[])

What is *main* function?

Function *main()* is the entry point of an application.
*main()* gets called by execution of application.

Input arguments:
* argc: Number of input arguments.
* argv: Name of program. Plus input arguments:
  - argv[0] ---> "program"
  - argv[1] ---> "arg1"
  - argv[2] ---> "arg2"
  - ...
  - argv[argc] == NULL

C++ and CMake must be learnt simultaneously.

Cmake:

## CMD
Microsoft Windows [Version 10.0.26200.8655]
(c) Microsoft Corporation. Alle Rechte vorbehalten.

C:\Users\diana\OneDrive\Dokumente\MyNotesOnCpp\HelloWorld>cmake -S src -B build  
-- Building for: Visual Studio 18 2026  
CMake Deprecation Warning at CMakeLists.txt:1 (cmake_minimum_required):  
  Compatibility with CMake < 3.10 will be removed from a future version of CMake.  

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
-- Configuring done (4.5s)  
-- Generating done (0.1s)  
-- Build files have been written to: C:/Users/diana/OneDrive/Dokumente/MyNotesOnCpp/HelloWorld/build  

C:\Users\diana\OneDrive\Dokumente\MyNotesOnCpp\HelloWorld>cmake --build build --config Release  
MSBuild-Version 18.4.0+6e61e96ac für .NET Framework  
  1>Checking Build System  
  Building Custom Rule C:/Users/diana/OneDrive/Dokumente/MyNotesOnCpp/HelloWorld/src/CMakeLists.txt  
  main.cpp  
  HelloWorld.vcxproj -> C:\Users\diana\OneDrive\Dokumente\MyNotesOnCpp\HelloWorld\build\Release\HelloWorld.exe  
  Building Custom Rule C:/Users/diana/OneDrive/Dokumente/MyNotesOnCpp/HelloWorld/src/CMakeLists.txt  

C:\Users\diana\OneDrive\Dokumente\MyNotesOnCpp\HelloWorld>cd build/Release  

C:\Users\diana\OneDrive\Dokumente\MyNotesOnCpp\HelloWorld\build\Release>HelloWorld.exe param1 param2 param3  
Total argument count (argc): 4  
argv[0]: HelloWorld.exe  
argv[1]: param1  
argv[2]: param2  
argv[3]: param3  
C:\Users\diana\OneDrive\Dokumente\MyNotesOnCpp\HelloWorld\build\Release>  
