### Binary compatibility
#### C++ library
Imagine we have a C++ code:
```c++
int add(int a, int b)
{
    return a + b;
}
```
And we want to use this code in form of a *library* in an application. The C++ compiler can not use our source code of *library* directly in our *application*. The library must first be converted to machine language. We say the compiler converts our source code to machine language, called *binary* or *object file* or *library*.
This compiler-generated binary library file is:
* Linux: lib<LibraryName>.so
* Windows: <LibraryName>.dll

This binary library can be used in our application, if we *link against* it.

#### Binary compatibility
Imagine we have a C++ application, app.exe, which uses our library, libMyLibV1.so:
```
             compile
app.cpp ───────────────► app.exe
                           │
                           │ uses
                           ▼
                     libMyLibV1.so
```
Now imagine we make some changes in libMyLibV1.so and build the second version of binary library, libMyLibV2.so.

If we can replace libMyLibV1.so with libMyLibV2.so without needing to recompile the app.cpp to app.exe, our changes in library is *binary compatible*. Otherwise the changes are *binary incompatible*. If the changes are *binary incompatible*, the whole application must be compiled and built again. This means the process from app.cpp to app.exe.

*Binary compatibility* can be important because for example we have already sold app.exe to customer. In this case we only replace libMyLibV1.so with libMyLibV2.so and app.exe will keep working. With having *binary incompatible* changes in libMyLibV2.so, the application app.exe will crash
as soon as the client starts to run it.

####
An example of binary incompatible change in class would be having a class like below in library:
```c++
class Person {
public:
    int age;
};
```
now: `sizeof(Person) = 4`.

If we change the class into:
```c++
class Person {
public:
    int age;
    int salary;
};
```
we will have `sizeof(Person) = 8`. This change is binary incompatible, because the compiler has built the application with the assumption that the size of class `Person` is 4.
> **Don't confuse these:**
> * API/Source compatibility: Can I compile my source code, app.cpp, against new version of library?
> * ABI/Binary compatibility: Can my already-compiled application, app.exe, continue to work with the new version of library?
